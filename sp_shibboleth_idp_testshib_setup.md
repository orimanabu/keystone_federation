# Keystone Federationの設定

# あらかじめ決めておくもの

|項目|設定値|備考|
|---|---|---|
|SAMLプロトコル名|saml2||
|IdPの名前|testshib||
|IdPのEntity ID|https://idp.testshib.org/idp/shibboleth||
|SPのEntity ID|https://$(hostname)/shibboleth|TestShibのサイトでshibboleth2.xmlを生成するとこのIDになります|
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
[root@osp13ps-aio ~]# crudini --set ${conf} federation trusted_dashboard "http://$(hostname)/dashboard/auth/websso/"
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

## Apacheの設定

UseCanonicalNameを有効にします。

```
[root@osp13ps-aio ~]# diff -u /etc/httpd/conf/httpd.conf.orig /etc/httpd/conf/httpd.conf
--- /etc/httpd/conf/httpd.conf.orig     2018-09-12 05:43:58.138169095 +0900
+++ /etc/httpd/conf/httpd.conf  2018-09-12 05:43:58.139169126 +0900
@@ -48,3 +48,4 @@

 IncludeOptional "/etc/httpd/conf.d/*.conf"

+UseCanonicalName On
```

Keystoneのvhost設定で、SAMLのエントリーポイントの設定をします。
"/v3/auth/OS-FEDERATION/websso/saml2"がHorizonからのSP initiated HTTPの設定、"/v3/OS-FEDERATION/identity_providers/testshib/protocols/saml2/auth"がCLIからのECPの設定となります。

```
[root@osp13ps-aio ~]# diff -u /etc/httpd/conf.d/10-keystone_wsgi_main.conf.orig /etc/httpd/conf.d/10-keystone_wsgi_main.conf
--- /etc/httpd/conf.d/10-keystone_wsgi_main.conf.orig   2018-09-12 05:43:58.130168852 +0900
+++ /etc/httpd/conf.d/10-keystone_wsgi_main.conf        2018-09-12 12:08:22.593266409 +0900
@@ -2,9 +2,10 @@
 # Vhost template in module puppetlabs-apache
 # Managed by Puppet
 # ************************************
+LoadModule mod_shib /usr/lib64/shibboleth/mod_shib_24.so

 <VirtualHost *:5000>
-  ServerName osp13ps-aio.osptest.local
+  ServerName osp13ps-aio.osptest.local:5000

   ## Vhost docroot
   DocumentRoot "/var/www/cgi-bin/keystone"
@@ -26,4 +27,24 @@
   WSGIProcessGroup keystone_main
   WSGIScriptAlias / "/var/www/cgi-bin/keystone/keystone-public"
   WSGIPassAuthorization On
+
+<Location /Shibboleth.sso>
+  SetHandler shib
+</Location>
+
+<Location /v3/auth/OS-FEDERATION/websso/saml2>
+  ShibRequestSetting requireSession 1
+  AuthType shibboleth
+  ShibRequireSession On
+  ShibExportAssertion Off
+  Require valid-user
+</Location>
+
+<Location /v3/OS-FEDERATION/identity_providers/testshib/protocols/saml2/auth>
+  ShibRequestSetting requireSession 1
+  AuthType shibboleth
+  ShibExportAssertion Off
+  Require valid-user
+</Location>
+
 </VirtualHost>
```

## Horizonの設定

下記を設定します。

|パラメータ|設定値|備考|
|---|---|---|
|OPENSTACK\_KEYSTONE\_MULTIDOMAIN\_SUPPORT|True|ドメインを有効にしたいので|
|OPENSTACK\_KEYSTONE\_DEFAULT\_DOMAIN|'Default'|特に何でもいいです|
|OPENSTACK\_KEYSTONE\_URL|"http://osp13ps-aio.osptest.local:5000/v3"|UseCanonicalNameを有効にするので、ホスト名に変更します|
|WEBSSO\_ENABLED|True|これにより認証方式を選べるようになります|
|WEBSSO\_INITIAL\_CHOICE|"saml2"|認証方式のデフォルトの選択肢|
|WEBSSO\_CHOICES|(("credentials", _("Keystone Credentials")),<br>("saml2", _("TestShib SAML")),)||

```
[root@osp13ps-aio ~]# diff -u /etc/openstack-dashboard/local_settings.orig /etc/openstack-dashboard/local_settings
--- /etc/openstack-dashboard/local_settings.orig        2018-09-12 05:32:21.883492284 +0900
+++ /etc/openstack-dashboard/local_settings     2018-09-12 05:42:54.274963995 +0900
@@ -71,12 +71,12 @@

 # Set this to True if running on multi-domain model. When this is enabled, it
 # will require user to enter the Domain name in addition to username for login.
-#OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = False
+OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT = True


 # Overrides the default domain used when running on single-domain model
 # with Keystone V3. All entities will be created in the default domain.
-#OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = 'Default'
+OPENSTACK_KEYSTONE_DEFAULT_DOMAIN = 'Default'


 # Set Console type:
@@ -206,14 +206,14 @@
 #OPENSTACK_HOST = "127.0.0.1"
 #OPENSTACK_KEYSTONE_URL = "http://%s:5000/v2.0" % OPENSTACK_HOST
 #OPENSTACK_KEYSTONE_DEFAULT_ROLE = "_member_"
-OPENSTACK_KEYSTONE_URL = "http://192.168.13.11:5000/v3"
+OPENSTACK_KEYSTONE_URL = "http://osp13ps-aio.osptest.local:5000/v3"
 OPENSTACK_KEYSTONE_DEFAULT_ROLE = "_member_"

 # Enables keystone web single-sign-on if set to True.
-#WEBSSO_ENABLED = False
+WEBSSO_ENABLED = True

 # Determines which authentication choice to show as default.
-#WEBSSO_INITIAL_CHOICE = "credentials"
+WEBSSO_INITIAL_CHOICE = "saml2"

 # The list of authentication mechanisms which include keystone
 # federation protocols and identity provider/federation protocol
@@ -223,13 +223,13 @@
 # Do not remove the mandatory credentials mechanism.
 # Note: The last two tuples are sample mapping keys to a identity provider
 # and federation protocol combination (WEBSSO_IDP_MAPPING).
-#WEBSSO_CHOICES = (
-#    ("credentials", _("Keystone Credentials")),
+WEBSSO_CHOICES = (
+    ("credentials", _("Keystone Credentials")),
 #    ("oidc", _("OpenID Connect")),
-#    ("saml2", _("Security Assertion Markup Language")),
+    ("saml2", _("TestShib SAML")),
 #    ("acme_oidc", "ACME - OpenID Connect"),
 #    ("acme_saml2", "ACME - SAML2")
-#)
+)

 # A dictionary of specific identity provider and federation protocol
 # combinations. From the selected authentication mechanism, the value
```

## Shibbolethの設定

SP用の設定ファイルshibboleth2.xmlをTestShibの[Configureページ](http://www.testshib.org/configure.html)からダウンロードできます ("Post Install SP Config"のところ)。
もしくは下記コマンドでも取得できます。
```
[root@osp13ps-aio ~]# curl -s -o /etc/shibboleth/shibboleth2.xml "https://www.testshib.org/cgi-bin/sp2config.cgi?dist=Others&hostname=$(hostname)"
```

IdPのメタデータを取得します。

SPのメタデータをIdP(TestShib)に登録します。
まずSPのメタデータを下記コマンドで取得します。
```
[root@osp13ps-aio ~]# sp_metadata_file=sp_metadata_$(hostname).xml
[root@osp13ps-aio ~]# curl -s -o ${sp_metadata_file} http://localhost:5000/Shibboleth.sso/Metadata
```

取得したメタデータをTestShibの[Registerページ](http://www.testshib.org/register.html)からTestShibにアップロードします ("Upload"のところ)。
```
[root@osp13ps-aio ~]# curl -s --form userfile=@"${sp_metadata_file}" https://www.testshib.org/procupload.php
```

