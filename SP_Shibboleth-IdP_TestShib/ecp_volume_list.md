# Command

```sh
#!/bin/bash

function do_cmd {
	local cmd=$1; shift
	echo "=> ${cmd}"
	#cmd=$(echo "${cmd}" | sed -e 's/\\//g')
	#${cmd}
	eval ${cmd}
}

output=$(
openstack \
--os-identity-api-version 3 \
--os-auth-type v3samlpassword \
--os-auth-url http://osp13ps-sp-shib.osptest.local:5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-username myself \
--os-password myself \
--os-user-domain-name '' \
--os-project-domain-name '' \
--os-project-name '' \
token issue)

echo "=> unscoped token"
echo "${output}"
unscoped_token=$(echo "${output}" | awk '/ id / {print $4}')
echo "unscoped token: ${unscoped_token}"

echo "=> federation domain list"
openstack \
--os-identity-api-version 3 \
--os-auth-type token \
--os-auth-url http://osp13ps-sp-shib.osptest.local:5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-token ${unscoped_token} \
--os-user-domain-name '' \
--os-project-domain-name '' \
--os-project-name '' \
federation domain list

echo "=> federation project list"
openstack \
--os-identity-api-version 3 \
--os-auth-type token \
--os-auth-url http://osp13ps-sp-shib.osptest.local:5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-token ${unscoped_token} \
--os-user-domain-name '' \
--os-project-domain-name '' \
--os-project-name '' \
federation project list

echo "=> scoped token"
output=$(openstack \
--os-identity-api-version 3 \
--os-auth-type token \
--os-auth-url http://osp13ps-sp-shib.osptest.local:5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-token ${unscoped_token} \
--os-user-domain-name '' \
--os-project-domain-name federated_domain \
--os-project-name federated_project \
token issue)
echo "${output}"
token=$(echo "${output}" | awk '/ id / {print $4}')
echo "scoped token: ${token}"

echo "=> volume list"
openstack \
--os-identity-api-version 3 \
--os-auth-type token \
--os-auth-url http://osp13ps-sp-shib.osptest.local:5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-token ${token} \
--os-user-domain-name '' \
--os-project-domain-name federated_domain \
--os-project-name federated_project \
volume list

```

# Output

```
=> unscoped token
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field   | Value                                                                                                                                                                                                        |
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| expires | 2018-09-21T15:44:51+0000                                                                                                                                                                                     |
| id      | gAAAAABbpQPj1pnBehkGhWLp88NZgj-3mGe6pSCILG_C1GUG-hl9RgZBo4CsX84vQcax636aHHGULZACZnY8uy0ysko1rPj9yxek9pGbVol5x5vvLXYtBzqDEI_KwtLmZmMLuaafzqxOjHpyj3TSd9m01bifHrypSLbovYIHm5HHOBaVv9uFi2yJnSBB_DwBGmiLP4wbxLZk |
| user_id | 7c06314fc2dc4bb78084edfb2bce1037                                                                                                                                                                             |
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
unscoped token: gAAAAABbpQPj1pnBehkGhWLp88NZgj-3mGe6pSCILG_C1GUG-hl9RgZBo4CsX84vQcax636aHHGULZACZnY8uy0ysko1rPj9yxek9pGbVol5x5vvLXYtBzqDEI_KwtLmZmMLuaafzqxOjHpyj3TSd9m01bifHrypSLbovYIHm5HHOBaVv9uFi2yJnSBB_DwBGmiLP4wbxLZk
=> federation domain list
+----------------------------------+---------+------------------+-------------+
| ID                               | Enabled | Name             | Description |
+----------------------------------+---------+------------------+-------------+
| bd86daad00a8437b9d03ef25003c597d | True    | federated_domain |             |
+----------------------------------+---------+------------------+-------------+
=> federation project list
+----------------------------------+----------------------------------+---------+-------------------+
| ID                               | Domain ID                        | Enabled | Name              |
+----------------------------------+----------------------------------+---------+-------------------+
| 6d1d744a26ef46fe8eac8de9f08313ec | bd86daad00a8437b9d03ef25003c597d | True    | federated_project |
+----------------------------------+----------------------------------+---------+-------------------+
=> scoped token
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field      | Value                                                                                                                                                                                                                              |
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| expires    | 2018-09-21T15:44:54+0000                                                                                                                                                                                                           |
| id         | gAAAAABbpQPm73DIexukODDPcEq97QiwE0-9xKTIsPsBDCGRSwcRG1LRJE5_n0n1LBd1ix4pOeehntjs2sWaNtGQ4vd0d13hiCKmILTxzX5mSF2_1zOzkmvPNI22RZje9ntYL5gBx75XfuUI0plXD4NmmVfwXB_mVoMnqctcxxJUgYzrhzIlPWgqBsLJ4rqoEYNSuCelAM84dmXnIGlwHB94E8skhKgV8w |
| project_id | 6d1d744a26ef46fe8eac8de9f08313ec                                                                                                                                                                                                   |
| user_id    | 7c06314fc2dc4bb78084edfb2bce1037                                                                                                                                                                                                   |
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
scoped token: gAAAAABbpQPm73DIexukODDPcEq97QiwE0-9xKTIsPsBDCGRSwcRG1LRJE5_n0n1LBd1ix4pOeehntjs2sWaNtGQ4vd0d13hiCKmILTxzX5mSF2_1zOzkmvPNI22RZje9ntYL5gBx75XfuUI0plXD4NmmVfwXB_mVoMnqctcxxJUgYzrhzIlPWgqBsLJ4rqoEYNSuCelAM84dmXnIGlwHB94E8skhKgV8w
=> volume list
+--------------------------------------+-----------+-----------+------+-------------+
| ID                                   | Name      | Status    | Size | Attached to |
+--------------------------------------+-----------+-----------+------+-------------+
| 59a4b1ea-49e9-4655-8ded-9a4f480bb7ff | vol1-shib | available |    1 |             |
+--------------------------------------+-----------+-----------+------+-------------+
```
