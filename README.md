# concourse-ci


## Set env
.env

```
CONCOURSE_BASIC_AUTH_USERNAME=concourse
CONCOURSE_BASIC_AUTH_PASSWORD=changeme
CONCOURSE_EXTERNAL_URL=http://192.168.33.10:8080
CONCOURSE_POSTGRES_USER=concourse
CONCOURSE_POSTGRES_PASSWORD=changeme
```

## Create ssh keys

```
$ mkdir -p keys/web keys/worker
$ ssh-keygen -t rsa -f ./keys/web/tsa_host_key -N ''
$ ssh-keygen -t rsa -f ./keys/web/session_signing_key -N ''
$ ssh-keygen -t rsa -f ./keys/worker/worker_key -N ''
$ cp ./keys/worker/worker_key.pub ./keys/web/authorized_worker_keys
$ cp ./keys/web/tsa_host_key.pub ./keys/worker
```

## boot

```
$ docker-compose up -d --build
```

## cli
Choose binary of fly from following site.

- https://concourse.ci/downloads.html

Install fly.

```
$ curl -L https://github.com/concourse/concourse/releases/download/v3.4.1/fly_linux_amd64 > /tmp/fly
$ sudo mv /tmp/fly /usr/local/bin/fly
$ sudo chmod 775 /usr/local/bin/fly
```

## Create a credential file
Set your git public key on credentials.yml to use key on git

~/.concourse/credentials.yml

```yaml
---
github-private-key: |
  -----BEGIN RSA PRIVATE KEY-----
  **snip**
  -----END RSA PRIVATE KEY-----
```

## Usage
Sign in to your concourse ci.

```
$ fly -t concourseci login -c http://192.168.33.10:8080
```

Check the targets.

```
$ fly targets
name         url                        team  expiry
concourseci  http://192.168.33.10:8080  main  Sat, 05 Aug 2017 03:46:25 UTC
```

set pipe-line

```
$ fly -t lite set-pipeline -p rails-sample -c ci/pipelines/pipeline.yml -l ~/.concourse/credentials.yml
```
