h1. Create a self-signed SSL cert for nginx

https://gist.github.com/anhar/6d50c023f442fb2437e1

h1. Sign my apple-app-site-association

```
echo '{"webcredentials":{"apps":["D3KQX62K1A.com.example.DemoApp",
                           "D3KQX62K1A.com.example.DemoAdminApp"]}}' > json.txt
 
cat json.txt | openssl smime -sign \
                             -inkey myssl.key \
                             -signer myssl.crt \
                             -certfile myssl.csr \
                             -noattr \
                             -nodetach \
                             -outform DER > apple-app-site-association
```

https://developer.apple.com/documentation/security/shared_web_credentials/preparing_your_app_and_website_to_share

h1. Have nginx serve the generated file with a special MIME type via HTTPS. This way I can use a self-signed nginx cert.

```
   location /apple-app-site-association {
      default_type application/pkcs7-mime;
   }
```
