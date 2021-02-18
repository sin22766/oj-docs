# HTTPS related issues

OnlineJudge strongly recommends using HTTPS protocol

  -Data transmission encryption to improve security and prevent hijacking
  -HTTP2 can be used to speed up access (default configuration)

The deployment script of OnlineJudge will generate a self-signed certificate by default, and the browser will prompt you do not trust. You can apply for a trusted certificate corresponding to the domain name yourself. OnlineJudge also provides the following two features to facilitate the application and use of HTTPS certificates.

## Apply for HTTPS certificate

The url prefix of `/.well-known` will automatically use the files in the `data/backend/ssl/.well-known` directory. By default, `data/backend/ssl/` already exists, so you can create it manually `.well-known` and its subfolders, for example, if you need a verification file whose url is `/.well-known/pki-validation/fileauth.txt`, you can create `data/backend/ssl/.well-known/ pki-validation/fileauth.txt` file, the content is the specified content.

Then replace the certificate and private key files under `data/backend/ssl/`, and then `docker exec -it oj-backend sh -c "cd /app/deploy; supervisorctl restart nginx"`.

## FORCE_HTTPS

If the HTTPS configuration is successful, in order to enhance security, it is recommended to redirect HTTP traffic to HTTPS traffic. At this time, you can uncomment the line `FORCE_HTTPS=1` in the `docker-compose.yml` file, and then `docker-compose up -d `Just restart.
