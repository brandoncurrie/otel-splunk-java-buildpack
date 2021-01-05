# Splunk distribution of OpenTelemetry Java Instrumentation Buildpack

A [CloudFoundry buildpack](https://docs.run.pivotal.io/buildpacks/) to install
and run the Splunk distribution of the OpenTelemetry Java Instrumentation agent in CloudFoundry apps.

This repository is a clone of the core [Splunk distribution of OpenTelemetry 
Java Instrumentation Buildpack](https://github.com/signalfx/splunk-otel-java/tree/main/deployments/cloudfoundry/buildpack). 

> Its recommended you use the core distribution if you're able to install a 
buildpack on your Cloud Foundry environment using 

```sh
$ cf create-buildpack ...
```

For some organizations, this privilege is restricted to platform owners and 
creating a new buildpack requires approval.  For the sake of POC, this repository
has been created to allow for a direct reference of the build pack in the 
manifest. 

## Installation

To use the Splunk OpenTelemetry Java Instrumentation Buildpack without installing
it at the platform level, modify your `manifest.yaml` to include the following
attributes:

> [REALM] needs to be replaced with your SignalFx realm 

> [ACCESS TOKEN] needs to be replaced with your SignalFx access token

```yaml
applications:
- name: my-java-app
  buildpacks: 
   - https://github.com/signalfx/splunk-otel-java/tree/main/deployments/cloudfoundry/buildpack
   - java_buildpack_offline
  ...
  env:
    ...
    OTEL_EXPORTER_JAEGER_SERVICE_NAME: my-java-app
    OTEL_EXPORTER_JAEGER_ENDPOINT: https://ingest.[REALM].signalfx.com/v1/trace
    OTEL_RESOURCE_ATTRIBUTES: environment=nonprod
    SIGNALFX_AUTH_TOKEN: [ACCESS TOKEN]
```

## Configuration

You can configure the Java instrumentation agent using environment variables 
listed in the [main README.md](../../../README.md).
All configuration options listed there are supported by this buildpack.

If you want to use a specific version of the Java agent in your application, you can set the `SPLUNK_OTEL_JAVA_VERSION`
environment variable before application deployment, either using `cf set-env` or the `manifest.yml` file:

```sh
$ cf set-env SPLUNK_OTEL_JAVA_VERSION "0.1.0"
```

By default the latest available agent version is used.

Note: the latest version won't use the buildpack cache, so if you want your deployments to be a bit quicker you may want to specify a concrete version.
