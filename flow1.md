# Horizon

## Select auth type of `saml2` protocol, then press `Connect` button

### General

|||
|---|---|
|Request URL: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/|
|Request Method: |POST|
|Status Code: |302 Found|
|Remote Address: |192.168.13.31:80|
|Referrer Policy: |no-referrer-when-downgrade|

### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store, must-revalidate, max-age=0|
|Connection: |Keep-Alive|
|Content-Language: |en|
|Content-Length: |0|
|Content-Type: |text/html; charset=utf-8|
|Date: |Fri, 21 Sep 2018 10:26:08 GMT|
|Expires: |Fri, 21 Sep 2018 10:26:08 GMT|
|Keep-Alive: |timeout=15, max=100|
|Location: |http://osp13ps-sp-shib.osptest.local:5000/v3/auth/OS-FEDERATION/websso/saml2?origin=http://osp13ps-sp-shib.osptest.local/dashboard/auth/websso/|
|Server: |Apache/2.4.6 (Red Hat Enterprise Linux)|
|Vary: |Accept-Language,Cookie|
|X-Frame-Options: |SAMEORIGIN|

### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Content-Length: |238|
|Content-Type: |application/x-www-form-urlencoded|
|Cookie: |csrftoken=SRGdmPwTTpBYX4E8yMjFk1T4eqyjk3HCTWHkq1DnbksZ51EG9grFUUwH69yO9Lec|
|Host: |osp13ps-sp-shib.osptest.local|
|Origin: |http://osp13ps-sp-shib.osptest.local|
|Referer: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/?next=/dashboard/|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|

### From Data

|||
|---|---|
|csrfmiddlewaretoken: |st9GBFyllwgcii6KSh0zPitqoKXitft5tyaNFRFPDr7dqf6itL8zpb63gtXNiX0F|
|fake_email: 
|fake_password: |
|next: |/dashboard/|
|region: |http://osp13ps-sp-shib.osptest.local:5000/v3|
|auth_type: |saml2|
|domain: 
|username: |
|password: 


## Redirected to Keystone Federation URL for WebSSO, then redirect again to TestShib SSO endpoint with SAML AuthnRequest

### General

|||
|---|---|
|Request URL: |http://osp13ps-sp-shib.osptest.local:5000/v3/auth/OS-FEDERATION/websso/saml2?origin=http://osp13ps-sp-shib.osptest.local/dashboard/auth/websso/|
|Request Method: |GET|
|Status Code: |302 Found|
|Remote Address: |192.168.13.31:5000|
|Referrer Policy: |no-referrer-when-downgrade|

### Response Headers

|||
|---|---|
|Cache-Control: |private,no-store,no-cache,max-age=0|
|Connection: |Keep-Alive|
|Content-Length: |817|
|Content-Type: |text/html; charset=iso-8859-1|
|Date: |Fri, 21 Sep 2018 10:26:08 GMT|
|Expires: |Wed, 01 Jan 1997 12:00:00 GMT|
|Keep-Alive: |timeout=15, max=100|
|Location: |https://idp.testshib.org/idp/profile/SAML2/Redirect/SSO?SAMLRequest=fZLNbsIwEIRfJfI9sRPaQi2CROFQJFoQoT30UjnJhlhy7NTr9Oft6xCo6IWj17Pz7Y49RdGols87V%2BsdfHSALvhulEZ%2BvEhJZzU3AiVyLRpA7gqezZ%2FWPIkYb61xpjCKBHNEsE4avTAauwZsBvZTFvCyW6ekdq7llBps41GLIbYh1jKP%2FNl5XKRMIRS%2FZYzRzNdzo8DVEaKhPSih2022J8HSS6UWPWNwRG8pyzbqPQY%2Fe%2BgL1E9VSQWn7h2U0kLhaJZtSLBapuSdjUvBCgajEpK8ElU8LstkUo2qMdwXcMO8DLGDlUYntEtJwuJJyO7DJN7HjCd3nE3eSLA9Lf8gdSn14XpS%2BSBC%2Frjfb8Nho1eweNzGC8hs2ufNj2B78QLXbcU5djI7R3I1ZYp%2FAU%2FpBXCgt%2FzZE1bLrVGy%2BAnmSpmvhQXhICUxobOh5f9nmf0C&RelayState=ss%3Amem%3Ade9128ec4313662480bf8f27ab63bdf70855ab48507107b17d16ae9759304292|
|Server: |Apache/2.4.6 (Red Hat Enterprise Linux)|

### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Cookie: |csrftoken=SRGdmPwTTpBYX4E8yMjFk1T4eqyjk3HCTWHkq1DnbksZ51EG9grFUUwH69yO9Lec|
|Host: |osp13ps-sp-shib.osptest.local:5000|
|Referer: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/?next=/dashboard/|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|

### Query String Parameters

|||
|---|---|
|origin: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/websso/|


