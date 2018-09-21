# Command

```
openstack \
--os-identity-api-version 3 \
--os-auth-type v3samlpassword \
--os-auth-url http://$(hostname):5000/v3 \
--os-identity-provider testshib \
--os-identity-provider-url https://idp.testshib.org/idp/profile/SAML2/SOAP/ECP \
--os-protocol saml2 \
--os-username myself \
--os-password myself \
--os-user-domain-name '' \
--os-project-domain-name '' \
--os-project-name '' \
token issue
```

# Output

```
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field   | Value                                                                                                                                                                                                        |
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| expires | 2018-09-21T15:36:47+0000                                                                                                                                                                                     |
| id      | gAAAAABbpQH_a8na5o_k9qRQFvijZqp5R2Qfenad2GIg7rDP5WuTTE-rz2z2wUIteHSQN2XwtGMMss5rvMBIGRg9A6QJHGu4EAU8rYyMS4t6R9pGqo-VlFJve3qXl-EFIOxWj85zK8bb6fSLFSMFbWg5iXuccMefd3-IdiRCvY1GJi0bR4fBJojz9FTH2v9VrrbDEmZkUfV4 |
| user_id | 7c06314fc2dc4bb78084edfb2bce1037                                                                                                                                                                             |
+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

