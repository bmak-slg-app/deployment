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
