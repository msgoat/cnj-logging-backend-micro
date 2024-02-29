# cnj-logging-backend-micro

Cloud native MicroProfile backend with support of cluster logging using an EFK stack:

* everything is logged to stdout (no file appenders)
* uses JSON logging format

The application is packaged as a multi-architecture docker image which supports the following platforms:
* linux/amd64
* linux/arm64/v8

## Synopsis

This showcase demonstrates
* how to switch logging format to JSON
* how to add important context information like currently authenticated user ID to log messages

### Enable JSON logging in Payara Micro

In order to switch Payara Micro to JSON logging output format, you will have to proceed through the following steps:

#### Add a custom logging.properties to the Docker image

Add a custom logging.properties file to your Docker image by copying it to the Payara home directory at __/home/payara__.
This custom logging.properties file should be a copy
of the original logging.properties file with the following modification:

```
java.util.logging.ConsoleHandler.formatter=fish.payara.enterprise.server.logging.JSONLogFormatter
```

This line attaches the `JSONLogFormatter`  to the `ConsoleHander` which renders each log entry as a JSON document.


#### Activate the custom logging.properties during application start

Add the command line argument `--logProperties` to the command line arguments passed to Payara Micro.
The easiest way to do that is to specify the following envvar `PAYARA_ARGUMENTS` on container start:

```
PAYARA_ARGUMENTS=${PAYARA_ARGUMENTS} --logProperties /home/payara/logging.properties
```

Command line argument `--logProperties` overrides the default logging.properties file location with the given one.

> The Docker image used in this showcase supports an additional envvar `PAYARA_LOGGING_FORMAT` which is set to __JSON__
> to add the `--logProperties` command line arguments to the default Payara command line arguments.

## Status

![Build status](https://codebuild.eu-west-1.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiSEJsMW1ZSTN1WXFPMGJhdllvN3pEWnJuK3dmQkdiWXFJM1FjUDlablNnRnRxQWwzdDFnUHUxQitsWDdSQ1oxeDQ3a0FIM1JlZzlmdlFTdHhlUy9mZEQ4PSIsIml2UGFyYW1ldGVyU3BlYyI6ImNMWnNEeS9GWFZYTHllWVMiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=main)

## Release information

Check [changelog](changelog.md) for latest version and release information.

## Docker Pull Command

`docker pull docker.cloudtrain.aws.msgoat.eu/cloudtrain/cnj-logging-backend-micro`

## Helm Pull Command

`helm pull oci://docker.cloudtrain.aws.msgoat.eu/cloudtrain-charts/cnj-logging-backend-micro`

## HOW-TO build this application locally

If all prerequisites are met, just run the following Maven command in the project folder:

```shell 
mvn clean verify -P pre-commit-stage
```

Build results: a Docker image containing the showcase application.


## HOW-TO start and stop this showcase locally

In order to run the whole showcase locally, just run the following docker commands in the project folder:

```shell 
docker compose up -d
docker compose logs -f 
```

Press `Ctlr+c` to stop tailing the container logs and run the following docker command to stop the showcase:

```shell 
docker compose down
```

## HOW-TO demo this showcase

The showcase application will be accessible:
* locally via `http://localhost:38080`
* remotely via `https://train2023-dev.k8s.cloudtrain.aws.msgoat.eu/cloudtrain/cnj-logging-backend-micro` (if the training cluster is up and running)
