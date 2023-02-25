# cnj-logging-backend-micro

Cloud native MicroProfile backend with support of cluster logging using an EFK stack:

* everything is logged to stdout (no file appenders)
* uses JSON logging format

## Status

![Build status](https://codebuild.eu-west-1.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiSEJsMW1ZSTN1WXFPMGJhdllvN3pEWnJuK3dmQkdiWXFJM1FjUDlablNnRnRxQWwzdDFnUHUxQitsWDdSQ1oxeDQ3a0FIM1JlZzlmdlFTdHhlUy9mZEQ4PSIsIml2UGFyYW1ldGVyU3BlYyI6ImNMWnNEeS9GWFZYTHllWVMiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=main)

## Release information

Check [changelog](changelog.md) for latest version and release information.

## Enable JSON logging in Payara Micro

In order to switch Payara Micro to JSON logging output format, you will have to proceed through the following steps:

### Add a custom logging.properties to the Docker image

Add a custom logging.properties file to your Docker image by copying it to the Payara home directory at __/home/payara__.
This custom logging.properties file should be a copy
of the original logging.properties file with the following modification:

```
java.util.logging.ConsoleHandler.formatter=fish.payara.enterprise.server.logging.JSONLogFormatter
```

This line attaches the `JSONLogFormatter`  to the `ConsoleHander` which renders each log entry as a JSON document.


### Activate the custom logging.properties during application start

Add the command line argument `--logProperties` to the command line arguments passed to Payara Micro.
The easiest way to do that is to specify the following envvar `PAYARA_ARGUMENTS` on container start:

```
PAYARA_ARGUMENTS=${PAYARA_ARGUMENTS} --logProperties /home/payara/logging.properties
```

Command line argument `--logProperties` overrides the default logging.properties file location with the given one. 

> The Docker image used in this showcase supports an additional envvar `PAYARA_LOGGING_FORMAT` which is set to __JSON__
> to add the `--logProperties` command line arguments to the default Payara command line arguments.

## Build this application 

``` 
mvn clean verify -P pre-commit-stage
```

Build results: a Docker image containing an Payara MicroProfile application.
