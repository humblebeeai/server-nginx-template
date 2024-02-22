# NGINX template

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/bybatkhuu/server.nginx-template/2.build-publish.yml?logo=GitHub)](https://github.com/bybatkhuu/server.nginx-template/actions/workflows/2.build-publish.yml)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/bybatkhuu/server.nginx-template?logo=GitHub)](https://github.com/bybatkhuu/server.nginx-template/releases)
[![Docker Image Version](https://img.shields.io/docker/v/bybatkhuu/nginx?sort=semver&logo=docker)](https://hub.docker.com/r/bybatkhuu/nginx/tags)
[![Docker Image Size](https://img.shields.io/docker/image-size/bybatkhuu/nginx?sort=semver&logo=docker)](https://hub.docker.com/r/bybatkhuu/nginx/tags)

This is a NGINX template docker image that can be used as a web server, reverse proxy, load balancer and HTTP cache.

## Features

- NGINX - <https://nginx.org>
- NGINX template configuration
- Web server
- Reverse proxy
- Load balancer
- Rate limiting
- HTTP cache
- HTTP header transformations
- HTTP/2 and HTTPS
- Basic authentication
- Websockets
- Docker and docker-compose

---

## Getting started

### 1. Prerequisites

- Prepare **server/PC** to run
- Install [**docker**](https://docs.docker.com/engine/install) and [**docker compose**](https://docs.docker.com/compose/install) in **server**

For **DEVELOPMENT**:

- Install [**git**](https://git-scm.com/downloads)
- Setup an [**SSH key**](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) ([video tutorial](https://www.youtube.com/watch?v=snCP3c7wXw0))

### 2. Download or clone the repository

**2.1.** Prepare projects directory (if not exists) in your **server**:

```sh
# Create projects directory:
mkdir -pv ~/workspaces/projects

# Enter into projects directory:
cd ~/workspaces/projects
```

**2.2.** Follow one of the below options **[A]** or **[B]**:

**A.** Clone the repository:

```sh
git clone https://github.com/bybatkhuu/server.nginx-template.git && \
    cd server.nginx-template
```

**B.** Download source code from [releases](https://github.com/bybatkhuu/server.nginx-template/releases) page.

### 3. Configure environment

> [!TIP]
> Skip this step, if you've already configured environment.

**3.1.** Configure **`.env`** file:

> [!NOTE]
> Please, check **[environment variables](#environment-variables)**!

```sh
# Copy .env.example file into .env file:
cp -v ./.env.example ./.env

# Edit environment variables to fit in your environment:
nano ./.env
```

**3.2.** Configure **`compose.override.yml`** file:

> [!IMPORTANT]
> Please, check **[arguments](#arguments)**!

```sh
# Copy 'compose.override.[ENV].yml' file to 'compose.override.yml' file:
cp -v ./templates/compose/compose.override.[ENV].yml ./compose.override.yml
# For example, DEVELOPMENT environment:
cp -v ./templates/compose/compose.override.dev.yml ./compose.override.yml
# For example, STATGING or PRODUCTION environment:
cp -v ./templates/compose/compose.override.prod.yml ./compose.override.yml

# Edit 'compose.override.yml' file to fit in your environment:
nano ./compose.override.yml
```

**3.3.** Validate docker compose configuration:

> [!WARNING]
> If you get an error or warning, check your configuration files (**`.env`** or **`compose.override.yml`**).

```sh
./compose.sh validate
# Or:
docker compose config
```

### 4. Configure NGINX

> [!TIP]
> Skip this step, if you've already configured NGINX.

> [!IMPORTANT]
> Please, check nginx configuration and best practices:

- <https://www.udemy.com/course/nginx-fundamentals>
- <https://www.baeldung.com/linux/nginx-config-environment-variables>
- <https://www.youtube.com/watch?v=pkHQCPXaimU>
- <https://www.nginx.com/blog/avoiding-top-10-nginx-configuration-mistakes>
- <https://www.nginx.com/nginx-wiki/build/dirhtml/start/topics/tutorials/config_pitfalls>
- <https://www.digitalocean.com/community/tools/nginx>
- <https://github.com/fcambus/nginx-resources>

Use template files to configure NGINX:

```sh
# Copy template file into storage directory:
cp -v ./templates/nginx.conf/[TEMPLATE_BASENAME].conf.template ./volumes/storage/nginx/configs/templates/[CUSTOM_BASENAME].conf.template
# For example, Let's Encrypt HTTPS configuration for example.com domain:
cp -v ./templates/nginx.conf/example.com_https.lets.conf.template ./volumes/storage/nginx/configs/templates/example.com.conf.template

# Edit template file to fit in your nginx configuration:
nano ./volumes/storage/nginx/configs/templates/[CUSTOM_BASENAME].conf.template
# For example:
nano ./volumes/storage/nginx/configs/templates/example.com.conf.template
```

### 5. Run docker compose

```sh
./compose.sh start -l
# Or:
docker compose up -d --remove-orphans --force-recreate && \
    docker compose logs -f --tail 100
```

### 6. Check service is running

Check the service is running:

```sh
./compose.sh list
# Or:
docker compose ps
```

### 7. Stop docker compose

```sh
./compose.sh stop
# Or:
docker compose down --remove-orphans
```

:thumbsup: :sparkles:

---

## Environment Variables

You can use the following environment variables to configure:

[**`.env.example`**](.env.example)

```sh
## --- NGINX configs --- ##
## NGINX basic auth username and password:
NGINX_BASIC_AUTH_USER=nginx_admin
NGINX_BASIC_AUTH_PASS="NGINX_ADMIN_PASSWORD123" # !!! CHANGE THIS TO RANDOM PASSWORD !!!
```

## Arguments

You can use the following arguments to configure:

```txt
-s=*, --https=[self | valid | lets]
    Enable HTTPS mode:
        self  - Self-signed certificate
        valid - Valid certificate
        lets  - Let's Encrypt certificate
-b, --bash, bash, /bin/bash
    Run only bash shell.
```

For example as in [**`compose.override.yml`**](templates/compose/compose.override.dev.yml) file:

```yml
    command: ["--https=self"]
    command: ["--https=valid"]
    command: ["--https=lets"]
    command: ["/bin/bash"]
```

## Documentation

- [Build docker image](docs/docker-build.md)

## Roadmap

- Add more documentation.

---

## References

- Download NGINX - <https://nginx.org/en/download.html>
- Building NGINX from sources - <https://nginx.org/en/docs/configure.html>
- NGINX documentation - <https://nginx.org/en/docs>
- NGINX directives - <https://nginx.org/en/docs/dirindex.html>
- NGINX variables - <https://nginx.org/en/docs/varindex.html>
- NGINX config generator (digitalocean) - <https://www.digitalocean.com/community/tools/nginx>
- NGINX 3rd party modules - <https://www.nginx.com/resources/wiki/modules>
- NGINX Avoid top 10 mistakes - <https://www.nginx.com/blog/avoiding-top-10-nginx-configuration-mistakes>
- NGINX Pitfalls and common mistakes - <https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls>
- Installing NGINX open source and NGINX Plus - <https://www.youtube.com/watch?v=pkHQCPXaimU>
- NGINX Proxy Manager - <https://nginxproxymanager.com>
- NGINX fundamental course - <https://www.udemy.com/course/nginx-fundamentals>
- NGINX resources - <https://github.com/fcambus/nginx-resources>
- NGINX config environment variables - <https://www.baeldung.com/linux/nginx-config-environment-variables>
