# Supported tags and respective `Dockerfile` links

  * [`1.9.2`, `latest`](https://github.com/lauriku/docker-kubectl/blob/master/Dockerfile)

## What is `kubectl`

`kubectl` is a CLI tool to control a cluster [Kubernetes](http://kubernetes.io/).

## Usage

    $ docker run --rm lauriku/kubectl --help

Note: Entrypoint is set to kubectl so do **not** type `lauriku/kubectl kubectl`.

### Usage example 1

For example to access a local Kubernetes cluster you may run:

    $ docker run --rm --net=host --user $UID \
        -v ~/.kube:/config/.kube \
        lauriku/kubectl cluster-info

  * `-net=host`: (optional) allows to connect to a local Kubernetes cluster.
  * `--user $UID`: (optional) by default runs as random UID `2342`, this allows to access your existing `~/.kube` if you have one. As you can note, you can run `kubectl` as any UID/GID.
  * `-v XXX:/config`: (optional) allows to store its configuration and possibly access existing configuration. Note that `/config` will always be the directory containing `.kube` (it's the forced `HOME` directory). Can be read-only. Alternatively you can mount a Kubernetes service account for example: `-v /var/run/secrets/kubernetes.io/serviceaccount:/var/run/secrets/kubernetes.io/serviceaccount:ro`.

### Usage example 2

Here we use the service-account, so this should work from within a Pod on your cluster as long as you've docker installed (and may be `DOCKER_HOST` set up properly):

    $ docker run \
        -v /var/run/secrets/kubernetes.io/serviceaccount/:/var/run/secrets/kubernetes.io/serviceaccount/:ro \
        lauriku/kubectl \
        -s https://kubernetes \
        --token="$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" \
        --certificate-authority=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
        cluster-info

Note: Alternatively to using kube-dns, you can use environment variables set within Kubernetes containers: `https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_PORT_443_TCP_PORT`.

### Alias

You may setup an alias to run this is if you were running `kubectl` directly.
Here is a function POSIX-compatible that work for most shells:

    kubectl () {
      docker run --rm -it --user $UID:$GID \
        -v /var/run/secrets/kubernetes.io/serviceaccount:/var/run/secrets/kubernetes.io/serviceaccount:ro \
        -w /code -v "$PWD":/code:ro \
        lauriku/kubectl "$@"
    }

### Why use it

It's mostly meant to be used during continuous integration or as part of an automated build/deployment:

  * So that your machine (e.g. build server) doesn't need `kubectl` to be installed; only Docker.
  * To avoid `kubectl config use-context` and similar to affect your build and other projects' builds.

