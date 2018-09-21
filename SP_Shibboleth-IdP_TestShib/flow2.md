# TestShib

Keystone as a SP with Shibboleth sends SAML AuthnRequest to TestShib.

## Keystone SP sends SAML AuthnRequest to TestShib SSO Endpoint

### General

|||
|---|---|
|Request URL: |https://idp.testshib.org/idp/profile/SAML2/Redirect/SSO?SAMLRequest=fZLNbsIwEIRfJfI9sRPaQi2CROFQJFoQoT30UjnJhlhy7NTr9Oft6xCo6IWj17Pz7Y49RdGols87V%2BsdfHSALvhulEZ%2BvEhJZzU3AiVyLRpA7gqezZ%2FWPIkYb61xpjCKBHNEsE4avTAauwZsBvZTFvCyW6ekdq7llBps41GLIbYh1jKP%2FNl5XKRMIRS%2FZYzRzNdzo8DVEaKhPSih2022J8HSS6UWPWNwRG8pyzbqPQY%2Fe%2BgL1E9VSQWn7h2U0kLhaJZtSLBapuSdjUvBCgajEpK8ElU8LstkUo2qMdwXcMO8DLGDlUYntEtJwuJJyO7DJN7HjCd3nE3eSLA9Lf8gdSn14XpS%2BSBC%2Frjfb8Nho1eweNzGC8hs2ufNj2B78QLXbcU5djI7R3I1ZYp%2FAU%2FpBXCgt%2FzZE1bLrVGy%2BAnmSpmvhQXhICUxobOh5f9nmf0C&RelayState=ss%3Amem%3Ade9128ec4313662480bf8f27ab63bdf70855ab48507107b17d16ae9759304292|
|Request Method: |GET|
|Status Code: |302 Found|
|Remote Address: |128.118.137.210:443|
|Referrer Policy: |no-referrer-when-downgrade|


### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store, must-revalidate, max-age=0|
|Connection: |close|
|Content-Length: |0|
|Content-Type: |text/plain; charset=UTF-8|
|Date: |Fri, 21 Sep 2018 10:26:08 GMT|
|Expires: |0|
|Location: |https://idp.testshib.org:443/idp/AuthnEngine|
|Pragma: |no-cache|
|Set-Cookie: |JSESSIONID=02E969898F2A3D8895F7BED8BE60CCC7; Path=/idp; Secure|
|Set-Cookie: |_idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9; Version=1; Path=/idp; Secure|


### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate, br|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Host: |idp.testshib.org|
|Referer: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/?next=/dashboard/|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|


### Query String Parameters

|||
|---|---|
|SAMLRequest: |fZLNbsIwEIRfJfI9sRPaQi2CROFQJFoQoT30UjnJhlhy7NTr9Oft6xCo6IWj17Pz7Y49RdGols87V+sdfHSALvhulEZ+vEhJZzU3AiVyLRpA7gqezZ/WPIkYb61xpjCKBHNEsE4avTAauwZsBvZTFvCyW6ekdq7llBps41GLIbYh1jKP/Nl5XKRMIRS/ZYzRzNdzo8DVEaKhPSih2022J8HSS6UWPWNwRG8pyzbqPQY/e+gL1E9VSQWn7h2U0kLhaJZtSLBapuSdjUvBCgajEpK8ElU8LstkUo2qMdwXcMO8DLGDlUYntEtJwuJJyO7DJN7HjCd3nE3eSLA9Lf8gdSn14XpS+SBC/rjfb8Nho1eweNzGC8hs2ufNj2B78QLXbcU5djI7R3I1ZYp/AU/pBXCgt/zZE1bLrVGy+AnmSpmvhQXhICUxobOh5f9nmf0C|
|RelayState: |ss:mem:de9128ec4313662480bf8f27ab63bdf70855ab48507107b17d16ae9759304292|


#### SAML AuthnRequest

<!---
```xml
<samlp:AuthnRequest
    AssertionConsumerServiceURL="http://osp13ps-sp-shib.osptest.local:5000/Shibboleth.sso/SAML2/POST"
    Destination="https://idp.testshib.org/idp/profile/SAML2/Redirect/SSO"
    ID="_07da0c0e3de2bfaf17dd28f3f7e9ce40" IssueInstant="2018-09-21T10:26:08Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://osp13ps-sp-shib.osptest.local/shibboleth</saml:Issuer><samlp:NameIDPolicy AllowCreate="1"/></samlp:AuthnRequest>
```
--->

```xml
<samlp:AuthnRequest
        AssertionConsumerServiceURL="http://osp13ps-sp-shib.osptest.local:5000/Shibboleth.sso/SAML2/POST"
        Destination="https://idp.testshib.org/idp/profile/SAML2/Redirect/SSO"
        ID="_07da0c0e3de2bfaf17dd28f3f7e9ce40"
        IssueInstant="2018-09-21T10:26:08Z"
        ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
        Version="2.0"
        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://osp13ps-sp-shib.osptest.local/shibboleth</saml:Issuer>
    <samlp:NameIDPolicy AllowCreate="1"/>
</samlp:AuthnRequest>
```

## Redirected to `AuthnEngine`

### General

|||
|---|---|
|Request URL: |https://idp.testshib.org/idp/AuthnEngine|
|Request Method: |GET|
|Status Code: |302 Found|
|Remote Address: |128.118.137.210:443|
|Referrer Policy: |no-referrer-when-downgrade|


### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store, must-revalidate, max-age=0|
|Connection: |close|
|Content-Length: |0|
|Content-Type: |text/plain; charset=UTF-8|
|Date: |Fri, 21 Sep 2018 10:26:09 GMT|
|Expires: |0|
|Location: |https://idp.testshib.org:443/idp/Authn/UserPassword|
|Pragma: |no-cache|


### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate, br|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Cookie: |JSESSIONID=02E969898F2A3D8895F7BED8BE60CCC7; _idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9|
|Host: |idp.testshib.org|
|Referer: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/?next=/dashboard/|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|



## Redirected to `UserPassword` page

This is TestShib authentication page, in which users input username and password in a form.

![auth idp](images/110_idp_login.png "auth idp")

### General

|||
|---|---|
|Request URL: |https://idp.testshib.org/idp/Authn/UserPassword|
|Request Method: |GET|
|Status Code: |200 OK|
|Remote Address: |128.118.137.210:443|
|Referrer Policy: |no-referrer-when-downgrade|

### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store, must-revalidate, max-age=0|
|Connection: |close|
|Content-Length: |1594|
|Content-Type: |text/html; charset=UTF-8|
|Date: |Fri, 21 Sep 2018 10:26:09 GMT|
|Expires: |0|
|Pragma: |no-cache|


### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate, br|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Cookie: |JSESSIONID=02E969898F2A3D8895F7BED8BE60CCC7; _idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9|
|Host: |idp.testshib.org|
|Referer: |http://osp13ps-sp-shib.osptest.local/dashboard/auth/login/?next=/dashboard/|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|



## Submit username and password

In this page, I input username (`myself`) and password (`myself`), then press `login` button.

### General

|||
|---|---|
|Request URL: |https://idp.testshib.org/idp/Authn/UserPassword|
|Request Method: |POST|
|Status Code: |302 Found|
|Remote Address: |128.118.137.210:443|
|Referrer Policy: |no-referrer-when-downgrade|


### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store, must-revalidate, max-age=0|
|Connection: |close|
|Content-Length: |0|
|Content-Type: |text/plain; charset=UTF-8|
|Date: |Fri, 21 Sep 2018 10:26:34 GMT|
|Expires: |0|
|Location: |https://idp.testshib.org:443/idp/profile/SAML2/Redirect/SSO|
|Pragma: |no-cache|
|Set-Cookie: |_idp_session=MTI1LjU0LjY3LjI0Mg%3D%3D%7CLQ%3D%3D%7CY2E1NWJiNGRlZWIxODEwYjA3YmFmMjc2MTIzMjg1ZDYyZTY5ODkwMTQwNDE1ZDhlN2QyMmI2YTEyZjdiOGY4ZQ%3D%3D%7CBDINfQdp84Fn05grKfHxoTKak%2Bc%3D; Version=1; Path=/idp; Secure|


### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate, br|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Content-Length: |35|
|Content-Type: |application/x-www-form-urlencoded|
|Cookie: |JSESSIONID=02E969898F2A3D8895F7BED8BE60CCC7; _idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9|
|Host: |idp.testshib.org|
|Origin: |https://idp.testshib.org|
|Referer: |https://idp.testshib.org/idp/Authn/UserPassword|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|


### Form Data

|||
|---|---|
|j_username: |myself|
|j_password: |myself|



## Get back to TestShib SSO endpoint URL after authenticated

### General

|||
|---|---|
|Request URL: |https://idp.testshib.org/idp/profile/SAML2/Redirect/SSO|
|Request Method: |GET|
|Status Code: |200 OK|
|Remote Address: |128.118.137.210:443|
|Referrer Policy: |no-referrer-when-downgrade|


### Response Headers

|||
|---|---|
|Cache-Control: |no-cache, no-store|
|Connection: |close|
|Content-Type: |text/html;charset=UTF-8|
|Date: |Fri, 21 Sep 2018 10:26:34 GMT|
|Expires: |0|
|Pragma: |no-cache|
|Set-Cookie: |_idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9; Version=1; Max-Age=0; Expires=Thu, 01-Jan-1970 00:00:10 GMT; Path=/idp|
|Transfer-Encoding: |chunked|


### Request Headers

|||
|---|---|
|Accept: |text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8|
|Accept-Encoding: |gzip, deflate, br|
|Accept-Language: |en-US,en;q=0.9,ja;q=0.8|
|Cache-Control: |max-age=0|
|Connection: |keep-alive|
|Cookie: |JSESSIONID=02E969898F2A3D8895F7BED8BE60CCC7; _idp_authn_lc_key=5fd1420e4346cdf54c7c860cd8519d09a8d11618c7b6023ba2d66ff1c5b7efe9; _idp_session=MTI1LjU0LjY3LjI0Mg%3D%3D%7CLQ%3D%3D%7CY2E1NWJiNGRlZWIxODEwYjA3YmFmMjc2MTIzMjg1ZDYyZTY5ODkwMTQwNDE1ZDhlN2QyMmI2YTEyZjdiOGY4ZQ%3D%3D%7CBDINfQdp84Fn05grKfHxoTKak%2Bc%3D|
|Host: |idp.testshib.org|
|Referer: |https://idp.testshib.org/idp/Authn/UserPassword|
|Upgrade-Insecure-Requests: |1|
|User-Agent: |Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36|


