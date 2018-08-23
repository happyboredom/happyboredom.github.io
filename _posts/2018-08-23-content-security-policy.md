Browser Content Security Policy (CSP)

I've been having a problem developing a new mail for Gmail because of content security policy issue.

```
Refused to load the image 'https://cdn.......' because it violates the following Content Security Policy directive: "img-src data: https://*.googleusercontent.com".
```

I'd never heard of a CSP until today so I have some relevant links below. It appears that my problem is that my images are getting blocked by the browser because the `src` of my images are `https://cdn.....` when the CSP wants all the images to be from `*.googleusercontent.com`. I think that this is a Gmail-controlled issue and not something that I can override.

Normally in a Gmail email, all the `src` attributes for `img` tags get rewritten such that they proxy through Google. `https://my.image.com/myimage.png` ---> `https://c1.googleusercontent.com/foo/bar/<my img src>`. However since I am trying to inject the images through AMP the original URLs are not getting rewritten and are thusly blocked by the browser because my unwritten URLs do not adhere to the CSP.

I don't have a solution to this problem yet because I suspect it is a Google-induced issue but I am digging around to see if I'm mistaken in that belief.

Introduction to CSP

https://developers.google.com/web/fundamentals/security/csp/

For AWS users: create Lambda@Edge function to add HTTP security headers to CloudFront content

https://aws.amazon.com/blogs/networking-and-content-delivery/adding-http-security-headers-using-lambdaedge-and-amazon-cloudfront/

Other useful link

https://www.savjee.be/2018/05/Content-security-policy-and-aws-s3-cloudfront/
