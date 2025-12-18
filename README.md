![Banner](./docs/img/project_banner.png)

# mlflow-setup

This repository contains the code for setting up MLFlow Tracking Server with PostgreSQL as backend and MinIO as artifact store, using docker-compose.

## Prerequisites

Docker and docker-compose should be installed on your machine, either through [Docker Desktop](https://www.docker.com/products/docker-desktop/), or its alternatives such as [Orbstack](https://orbstack.dev/)

## Configure environment variables

Make a copy of the `docker/.env.example` file and rename it to `docker/.env`. Then, update the environment variables in the `.env` file as per your requirements.

## Build and start the services

```bash
docker compose up -d --build
```

If everything is setup properly, you should be able to access the services at the following URLs:

- MLFlow Tracking Server: [http://localhost:5001](http://localhost:5001)
- MinIO Console UI: [http://localhost:9001](http://localhost:9001)

![Screenshots](./docs/img/minio_mlflow_screenshot.png)

## Provision Server as DigitalOcean Droplet

Run the following command to get the public IP of the droplet:
```fish
doctl compute droplet create mlflow --region sgp1 --image docker-20-04 --size s-2vcpu-2gb --ssh-keys 52609419 --user-data-file dod-mlflow-setup.yaml --wait
```

Add a SSH alias *dod* in SSH config file with above public IP.
Monitor the progress of the execution of dod-mlflow-setup.yaml with:
`ssh dod tail -f /var/log/cloud-init-output.log`.

After the provision finishes successfully, access the Mlflow server at: `http://<public-ip>:5001`.

Note: do NOT use `~` or `$HOME` in provision script (here it is dod-mlflow-setup.yaml).
I found they can be parsed correctly.
