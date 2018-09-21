# Environment

- SP: Keystone + Shibboleth (mod\_shib)
- IdP: TestShib

# WebSSO

- [HTTP/SAML Flow #1](flow1.md) (Horizon => Keystone)
- [HTTP/SAML Flow #2](flow2.md) (Keystone => TestShib)
- [HTTP/SAML Flow #3](flow3.md) (TestShib => Keystone)
- [HTTP/SAML Flow #4](flow4.md) (Keystone => Horizon)
- [IdP (TestShib) log](testshib_log_websso.txt)

# ECP

- [Get unscoped token](ecp_unscoped_token_issue.md)

  - [Debug output](ecp_unscoped_token_issue_debug.txt) with [debug-print patch](../patches/keystoneauth1_saml2.py.diff)
  - [IdP (TestShib) log](testshib_log_ecp.txt)

- [Get scoped token and list volumes](ecp_volume_list.md)
