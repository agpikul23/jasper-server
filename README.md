# Introduction

Docker image for JasperServer Community Edition.  Please see [Jaspersoft Community](https://community.jaspersoft.com) for more information.

This repository is based on Jaspersoft's sample Dockerfile and related files for their PRO Edition, which can be found at [TIBCOSoftware/js-docker](https://github.com/TIBCOSoftware/js-docker/).  

# Support
No warranty, explicit or implied, with usage of this repository.  As such, we don't offer any support for this project.

Please refer to the [Jaspersoft Forums](https://community.jaspersoft.com/questions) for support.

Related [Jaspersoft Forum](https://community.jaspersoft.com/questions) Posts:

* [[SOLVED] Getting Atk/Swing errors running a report](https://community.jaspersoft.com/questions/1064816/solved-getting-atkswing-errors-running-report)

# Pre-built Docker image
A prebuilt docker image is available at [agpikul23/jasperserver-ce](https://hub.docker.com/r/agpikul23/jasper-server).

# Building your own image
Download the latest JasperServer and put in `./resources`: [JasperServer CE Releases](http://community.jaspersoft.com/project/jasperreports-server/releases)

Build a new Docker image and store it in local Docker
```bash
docker build -t jasper-server .
```

Tag the local Docker image with the Docker Hub username/repo:version
```bash
docker tag jasper-server agpikul23/jasper-server:1.0
```

Push new Docker image to Docker Hub
```bash
docker push agpikul23/jasper-server:1.0
```

# Using with Docker Compose
You can use our [agpikul23/jasper-server](https://hub.docker.com/r/agpikul23/jasper-server) or your custom image.

Deploy local services using your `docker-compose.yml`.
```bash
docker stack deploy -c docker-compose.yml jasper-server
```

Here is a sample `docker-compose.yml`:
```
version: '3'

services:
  jasperserver:
    image: agpikul23/jasper-server:1.0
    ports:
      - "8080:8080"
      - "8443:8443"
    depends_on:
      - jasperdatabase
    env_file: .env
    environment:
      - DB_HOST=jasperdatabase
    volumes:
      - jasper_webapp:/usr/local/tomcat/webapps/jasperserver
      - jasper_license:/usr/local/share/jasperreports-ce/license 
      - jasper_customization:/usr/local/share/jasperreports-ce/customization
    networks:
      - reportsnet

  jasperdatabase:
    image: postgres:9.5
    env_file: .env
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - reportsnet

networks:
  reportsnet:

volumes:
  pgdata:
  jasper_webapp:
  jasper_license:
  jasper_customization:
```

*.env*
```
DB_USER=postgres
DB_PASSWORD=postgres
DB_PORT=5432
DB_NAME=jasperserver
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
```
