[![Build Status](https://apm-ci.elastic.co/buildStatus/icon?job=apm-server/apm-server-mbp/main)](https://apm-ci.elastic.co/job/apm-server/job/apm-server-mbp/view/change-requests/job/main/)
[![Smoke Tests on ESS](https://apm-ci.elastic.co/buildStatus/icon?job=apm-server/smoke-tests-ess-mbp/main&subject=smoke%20tests)](https://apm-ci.elastic.co/job/apm-server/job/smoke-tests-ess-mbp/job/main/)

# APM Server

The APM Server receives data from Elastic APM agents and transforms it into Elasticsearch documents.
Read more about Elastic APM at [elastic.co/apm](https://www.elastic.co/apm).

For questions and feature requests, visit the [discussion forum](https://discuss.elastic.co/c/apm).

## Getting Started

To get started with APM, see our [Quick start guide](https://www.elastic.co/guide/en/apm/get-started/current/install-and-run.html).

## APM Server Development

### Requirements

* [Go][golang-download] 1.19.x

[golang-download]: https://golang.org/dl/

### Install

* Fork the repo with the GitHub interface and clone it:

```
git clone git@github.com:[USER]/apm-server.git
```

Note that it should be cloned from the fork (replace [USER] with your GitHub user), not from origin.

* Add the upstream remote:

```
git remote add elastic git@github.com:elastic/apm-server.git
```

### Build

To build the binary for APM Server run the command below. This will generate a binary
in the same directory with the name apm-server.

```
make
```

If you make code changes, you may also need to update the project by running the additional command below:

```
make update
```

### Run

To run APM Server with debugging output enabled, run:

```
./apm-server -c apm-server.yml -e -d "*"
```

APM Server expects index templates, ILM policies, and ingest pipelines to be set up externally.
This should be done by [installing the APM integration](https://www.elastic.co/guide/en/fleet/current/fleet-quick-start-traces.html#add-apm-integration).
When running APM Server directly, it is only necessary to install the integration and not to run an Elastic Agent.

### Testing

For Testing check out the [testing guide](dev_docs/TESTING.md)

### Cleanup

To clean up the build directory and generated artifacts, run:

```
make clean
```

### Contributing

See [contributing](CONTRIBUTING.md) for details about reporting bugs, requesting features,
or contributing to APM Server.

### Releases

See [releases](dev_docs/RELEASES.md) for an APM Server release checklist.

## Updating dependencies

APM Server uses Go Modules for dependency management, without any vendoring.

In general, you should use standard `go get` commands to add and update modules. The one exception to this
is the dependency on `libbeat`, for which there exists a special Make target: `make update-beats`, described
below.

### Updating libbeat

By running `make update-beats` the `github.com/elastic/beats/vN` module will be updated to the most recent
commit from the main branch, and a minimal set of files will be copied into the apm-server tree.

You can specify an alternative branch or commit by specifying the `BEATS_VERSION` variable, such as:

```
make update-beats BEATS_VERSION=7.x
make update-beats BEATS_VERSION=f240148065af94d55c5149e444482b9635801f27
```

### Updating go-elasticsearch

It is important to keep the [go-elasticsearch client](https://github.com/elastic/go-elasticsearch) in sync
with the according major version. We also recommend to use the latest available client for minor versions.

You can use `go get -u -m github.com/elastic/go-elasticsearch/v7@7.x` to update to the latest commit on the
7.x branch.

## Packaging

To build all apm-server packages from source, run:

```
make package
```

This will fetch and create all images required for the build process. The whole process can take several minutes.
When complete, packages can be found in `build/distributions/`.

### Building docker packages

To customize image configuration, see [the docs](https://www.elastic.co/guide/en/apm/server/current/running-on-docker.html).

To build docker images from source, run:

```
make package-docker
```

When complete, Docker images can be found at `build/distributions/*.docker.tar.gz`,
and the local Docker image IDs are written at `build/docker/*.txt`.

Building pre-release images can be done by running `make package-docker-snapshot` instead.

## Documentation

[Documentation](https://www.elastic.co/guide/en/apm/server/current/index.html) for the APM Server can be found in the `docs` folder.
