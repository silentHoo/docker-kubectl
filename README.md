# Supported tags and respective `Dockerfile` links

  * [`1.9.4`, `latest`](https://hub.docker.com/r/silenthoo/docker-kubectl/tags/)

This repo is forked from https://github.com/lauriku/docker-kubectl

## What is `kubectl`

`kubectl` is a CLI tool to control a cluster [Kubernetes](http://kubernetes.io/).

## Usage

    $ docker run --rm silenthoo/docker-kubectl --help

Note: Entrypoint is set to kubectl so do **not** type `silenthoo/kubectl kubectl`.

### Usage example

For example to access a local Kubernetes cluster you may run:

    $ docker run --rm --net=host --user $UID \
        -v ~/.kube:/config/.kube \
        silenthoo/kubectl cluster-info

  * `--net=host`: (optional) allows to connect to a local Kubernetes cluster.
  * `--user $UID`: (optional) by default runs as random UID `2342`, this allows to access your existing `~/.kube` if you have one. As you can note, you can run `kubectl` as any UID/GID.
  * `-v XXX:/config`: (optional) allows to store its configuration and possibly access existing configuration. Note that `/config` will always be the directory containing `.kube` (it's the forced `HOME` directory). Can be read-only. Alternatively you can mount a Kubernetes service account for example: `-v /var/run/secrets/kubernetes.io/serviceaccount:/var/run/secrets/kubernetes.io/serviceaccount:ro`.

### Why use it

It's mostly meant to be used during continuous integration or as part of an automated build/deployment:

  * So that your machine (e.g. build server) doesn't need `kubectl` to be installed; only Docker.
  * To avoid `kubectl config use-context` and similar to affect your build and other projects' builds.

### Changes to this Dockerfile

* Change the Dockerfile
* Tag: `docker build -t silenthoo/docker-kubectl:1.9.4 -t silenthoo/docker-kubectl: latest .`
* Push: `docker push silenthoo/docker-kubectl`
