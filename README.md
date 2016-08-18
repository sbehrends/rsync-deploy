# rsync-deploy

rsync-deploy is a docker container serves static files which can be uploaded via rsync.

## Usage

Use the following `docker-compose.yml`:

```yaml
rsync-deploy:
  build: ./
  ports:
    - "2222:22"
    - "80:80"
  environment:
    DEPLOY_KEY_URL: https://github.com/yourname.keys
```

Or using `docker run`

```
docker build -t sbehrends/rsync-deploy .
docker run -e DEPLOY_KEY_URL=https://github.com/yourname.keys -p 2222:22 -p 8080:80 sbehrends/rsync-deploy
```

GitHub exposes your public keys at `https://github.com/<username>.keys`, which is perfect for the `DEPLOY_KEY_URL` here.

Then upload the files via rsync:

```
rsync -a -e "ssh -p 2222" local_dir rsync@remote_domain:/rsync
```

The files will be available at `http://remote_domain:8080`.