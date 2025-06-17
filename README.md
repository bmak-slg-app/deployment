# Signify Deployment

This repository contains the code and instructions for deploying the API
and worker services for the Signify mobile application.

## Overview

The stack is composed of mulitiple services and is managed by supervisord. This
is due to root access and systemd user units not being available on the server
provided by HKUST.

The services are:

- caddy: a reverse proxy for the API services, serve as a gateway to downstream
  APIs and hosting static files.
- redis: a multi-purpose in-memory data store, for providing a queue for worker
  jobs and storing results temporarily.
- sign2text: Sign2Text API. This is the API for the SingleStreamNetwork model
  for sign language translation.
- text2gloss: Text2Gloss API for sign language generation. This is an auxiliary
  API for text-to-gloss translation process in sign language generation.
- worker: a worker service for rendering sign language generation outputs to
  videos.
- spoken2sign: Spoken2Sign API for sign language generation. This is an API for
  sign language generation model to process sign language generation requests.

## Prerequisites

### Package Manager

`pixi` is used to install required system programs and manage python libraries.
It is recommended to install `pixi` binary under `<path to repository>/.pixi/bin`
for easy referencing in the following steps with absolute paths. You are also
recommended to add the bin directory to your `PATH` environment variable, which
you may achieve by using direnv.

For instruction installing pixi, see [pixi](https://github.com/prefix-dev/pixi).

### Background Services

To run the services as background processes, a process manager is required. For
running in the HKUST server, we may use a cron job for keeping supervisord
running in the background.

For this, add the following line to the crontab by running `crontab -e`:

```
@reboot <path to repository>/.pixi/bin/pixi run start supervisord
```

You should also modify `supervisord.conf` to set the pixi binary path to
correct location.

### Model Files

Please refer to the below google sheet link for a list of resources required
for running the services. The list contains the download link and the location
of the files to be put locally.

## Running the Services

### Starting the Services

By default, supervisord will automatically start all services. To start the
services manually, run the following command:

```
pixi run start
```

This will start all services in the background. Python libraries will be
installed automatically by `pixi` on first invocation.

### Stopping the Services

To stop the services, run the following command:

```
pixi run stop
```

### Managing the Services

To manage the services, run the following command:

```
pixi run supervisorctl
```

This will start supervisorctl in interactive mode. You can then manage the
status of the services by issuing commands to supervisorctl, for example:
viewing the logs and status information.
