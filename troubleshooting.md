# SAML messageの確認

see: https://bugzilla.redhat.com/show_bug.cgi?id=1434966

```python
 import pprint
 import webob
 import webob.dec


 @webob.dec.wsgify
 def application(req):
     return webob.Response(pprint.pformat(req.environ),
                           content_type='application/json')
```
