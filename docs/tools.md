# Tools

## Vscode

### Common watcher ignore options

```json
{
    "files.watcherExclude": {
        "**/.git/objects/**": true,
        "**/.git/subtree-cache/**": true,
        "**/node_modules/**": true
    }
}
```

## Gitlab

### Basic runner config

```toml
concurrent = 6
check_interval = 0
[session_server]
  session_timeout = 1800
[[runners]]
  name = "<runner name"
  url = "<gitlab url>"
  token = "<runner token>"
  executor = "docker"
  environment = ["DOCKER_TLS_CERTDIR="]
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "centos"
    privileged = true 
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    shm_size = 0
    volumes = ["/cache”]
```

### Bind Docker socket in runner

```toml
  [runners.docker]
    tls_verify = false
    image = "docker:19.03.12"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    shm_size = 0
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache”]
```
### Docker login

```yaml
variables:
    REGISTRY: "<docker registry url>"
    CA_CERTIFICATE: "$CA_CERTIFICATE"
    DOCKER_HOST: tcp://docker:2375
    DOCKER_TLS_CERTDIR: ""
    DOCKER_DRIVER: overlay2

services:
    - name: docker:19.03-dind
command:
    - /bin/sh
    - -c
    - echo "$CA_CERTIFICATE" > /usr/local/share/ca-certificates/ca.crt && update-ca-certificates && dockerd-entrypoint.sh || exit

before_script:
    - echo $PASSWORD | docker login -u $USER --password-stdin $REGISTRY
```

### Publish NPM package

```yaml
publish-package:
    image: node:fermium
    stage: publish
    script:
    - npm i --cache .npm --prefer-offline
    - npm run build
    - echo "_auth=$(echo -n $USER:$PASSWORD | openssl base64)" >> .npmrc
    - npm config -g set strict-ssl false
    - npm publish
```

## Jenkins

### Project URLs

Jenkins web hook url

`/project/<project name>`

Jenkins internal URL

`/job/<project name>/build`

### Allow running docker on slave

```shell
usermod -aG docker jenkins
chmod 777 /var/run/docker.sock
```

### Sudoers nopass

```shell
echo "jenkins ALL=(ALL) NOPASSWD: ALL” >> /etc/sudoers
```

## Sonatype Nexus

### Upload file

```shell
curl --cacert <ca cert> -v --user '<user>:<password>' --upload-file <file> https://<nexus url>/repository/<repository>/<file>
```

### Download file

```shell
curl --cacert <ca cert>  https://<nexus url>/<repository>/megin-raw-repository/<file> --output <output file name>
```