This is a base nginx image with openssl.

Sample invocation: generate the image harvardlil/nginx:0.01
with the command line:

```
docker build -f Dockerfile -t harvardlil/nginx:0.01 .
```

CHANGELOG
---------
### 0.02
- Add error pages that don't mention "nginx" by name.
- Update from 1.15.8 to 1.15.12

### 0.01
- Initial commit.
