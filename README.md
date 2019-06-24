# chart-hyperopt
© Hipages Group Pty Ltd 2019

Helm Chart to deploy [hyperopt](https://github.com/hyperopt/hyperopt): a Distributed Asynchronous Hyperparameter Optimization in Python
This complements the [hyperkops](https://github.com/hipagesgroup/hyperkops) repository reared towards running hyperopt in Kubernetes. 

## TL;DR;

```bash
$ helm install .
```

## Introduction

This chart bootstraps a [Hyperopt](https://github.com/hyperopt/hyperopt) deployment on a 
[Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.4+ with Beta APIs enabled

## Installing the Chart
First clone the repository using git clone:

```bash
git clone https://github.com/hyperopt/hyperopt.git
```

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

The command deploys Hyperopt on the Kubernetes cluster in the default configuration. The [configuration](#configuration) 
section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

The helm chart outputs some useful information at the time of installation, like the portforward command required to 
connect to mongo db.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration
A YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml .
```

> **Tip**: You can use the default [values.yaml](values.yaml)

