# Keystone Federationの設定

# あらかじめ決めておくもの

|項目|設定値|備考|
|SAMLプロトコル名|saml2||
|IdPの名前|testshib||
|IdPのEntity ID|https://idp.testshib.org/idp/shibboleth||

|FederationしたドメインのHorizon上の表示名|\_federated\_domain||
|Federation用ドメイン|federated\_domain||
|Federation用プロジェクト|federated\_project||
|Federation用したユーザーをマッピングするグループ名をマッピングするグループ|federated\_users||
|Federation用したユーザーのロール|\_member\_|既存のロールを流用|
|Federation用したユーザーのマッピング名|testshib_mapping||

# Keystoneの設定

ホスト名を確認しておきます (検証環境の場合 "osp13ps-aio.osptest.local")。
```
[root@osp13ps-aio ~]# hostname
osp13ps-aio.osptest.local
```

```
[root@osp13ps-aio ~]# conf=/etc/keystone/keystone.conf
[root@osp13ps-aio ~]# crudini --set ${conf} auth methods 'external,password,token,saml2,oauth1'
[root@osp13ps-aio ~]# crudini --set ${conf} federation remote_id_attribute 'Shib-Identity-Provider'
[root@osp13ps-aio ~]# crudini --set ${conf} federation federated_domain_name 'federated_domain'
[root@osp13ps-aio ~]# crudini --set ${conf} federation trusted_dashboard "http://${hostname}/dashboard/auth/websso/"
```

# Federateion用のドメイン、プロジェクト等の作成

## ドメインの作成
```
[root@osp13ps-aio ~(keystone_admin)]# openstack domain create federated_domain
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description |                                  |
| enabled     | True                             |
| id          | 28ed9755c6e94a41a30b650ad258cd74 |
| name        | federated_domain                 |
| tags        | []                               |
+-------------+----------------------------------+
```

## プロジェクトの作成
```
[root@osp13ps-aio ~(keystone_admin)]# openstack project create federated_project --domain federated_domain
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description |                                  |
| domain_id   | 28ed9755c6e94a41a30b650ad258cd74 |
| enabled     | True                             |
| id          | 123068fbb9e4450494041f2b9efb4a22 |
| is_domain   | False                            |
| name        | federated_project                |
| parent_id   | 28ed9755c6e94a41a30b650ad258cd74 |
| tags        | []                               |
+-------------+----------------------------------+
```

## グループの作成
```
[root@osp13ps-aio ~(keystone_admin)]# openstack group create federated_users --domain federated_domain
+-------------+----------------------------------+
| Field       | Value                            |
+-------------+----------------------------------+
| description |                                  |
| domain_id   | 28ed9755c6e94a41a30b650ad258cd74 |
| id          | 72fb4dcf9f0c4181a1ba90d145484558 |
| name        | federated_users                  |
+-------------+----------------------------------+
```

## ロールのアサイン
```
[root@osp13ps-aio ~(keystone_admin)]# openstack role add --group federated_users --domain federated_domain _member_
[root@osp13ps-aio ~(keystone_admin)]# openstack role add --group federated_users --project federated_project _member_
```

## Federationしたユーザーのマッピングルールの作成

まず、マッピングルールをjson形式のファイルで記述します。
```
[root@osp13ps-aio ~(keystone_admin)]# cat testshib_mapping.json
[
  {
    "local": [
      {
        "user": {
          "name": "{0}"
        }
      },
      {
        "group": {
          "name": "federated_users",
          "domain": {"name": "federated_domain"}
        }
      }
    ],
    "remote": [
      {
        "type": "REMOTE_USER"
      }
    ]
  }
]
```

次に、そのファイルを入力にし、testshib\_mappingという名前でルールを作成します。
```
[root@osp13ps-aio ~(keystone_admin)]# openstack mapping create --rules testshib_mapping.json testshib_mapping
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field | Value                                                                                                                                                                      |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| id    | testshib_mapping                                                                                                                                                           |
| rules | [{u'remote': [{u'type': u'REMOTE_USER'}], u'local': [{u'user': {u'name': u'{0}'}}, {u'group': {u'domain': {u'name': u'federated_domain'}, u'name': u'federated_users'}}]}] |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

## IdPの定義

testshibという名前で、IdPを定義します。--remote-idオプションにIdPのEntity IDを指定します。

```
[root@osp13ps-aio ~(keystone_admin)]# openstack identity provider create --remote-id https://idp.testshib.org/idp/shibboleth testshib
+-------------+-----------------------------------------+
| Field       | Value                                   |
+-------------+-----------------------------------------+
| description | None                                    |
| domain_id   | 0d9e62a375944d108aaa55195d9c6550        |
| enabled     | True                                    |
| id          | testshib                                |
| remote_ids  | https://idp.testshib.org/idp/shibboleth |
+-------------+-----------------------------------------+
```

## プロトコルの定義

定義したIdPとマッピングルールから、saml2という名前でプロトコルを定義します。

```
[root@osp13ps-aio ~(keystone_admin)]# openstack federation protocol create --identity-provider testshib --mapping testshib_mapping saml2
+-------------------+------------------+
| Field             | Value            |
+-------------------+------------------+
| id                | saml2            |
| identity_provider | testshib         |
| mapping           | testshib_mapping |
+-------------------+------------------+
```
