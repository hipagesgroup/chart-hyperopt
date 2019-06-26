# chart-hyperopt
© Hipages Group Pty Ltd 2019

Helm Chart to deploy [hyperopt](https://github.com/hyperopt/hyperopt): a Distributed Asynchronous Hyperparameter 
Optimization in Python.
This repository uses the docker images published from [hyperkops](https://github.com/hipagesgroup/hyperkops) repository 
and provides a helm chart for deployment on Kubernetes. 

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
git clone https://github.com/hipagesgroup/chart-hyperopt.git
```

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

The command deploys Hyperopt on the Kubernetes cluster in the default configuration. The [configuration](#configuration) 
section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

The helm chart outputs some useful information at the time of installation, like the portforward command required to 
connect to mongo db. This can be seen in the NOTES section of the output, eg.:
```
$ helm install .   

NAME:   silly-newt
LAST DEPLOYED: Tue Jun 25 10:50:15 2019
NAMESPACE: default
STATUS: DEPLOYED
...
...
...

NOTES:
1. To connect mongo db using your local to deploy hyperopt job

export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=chart-hyperopt-mongo,app.kubernetes.io/instance=silly-newt" -o jsonpath="{.items[0].metadata.name}")

kubectl port-forward $POD_NAME 27017:27017

You can then connect to mongo db at:
localhost:27017

2. you can connect to the mongo db on other deployments within the kubernetes environment using this service url:
"silly-newt-chart-hyperopt-mongo.default.svc.cluster.local"
```

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

> **Tip**: You can use the default [values.yaml](values.yaml) but it is suggested you customise these values to suit your work load.


The following table lists the configurable parameters of the MongoDB chart and their default values.


| Parameter                                         | Description                                                                                   | Default                                   |
| ------------------------------------------------- | --------------------------------------------------------------------------------------------  | ----------------------------------------- |
| `nameOverride`    								| the name override for the cahr                                                                | `""`                                      |
| `fullnameOverride`		    					| the full name override for the chart                                                          | `""`                                      |
| `mongo.image.repository`							| The image repository to pull mongodb image from												| `mongo`	    							|
| `mongo.image.tag`									| The tag for the mongo image to be pulled														| `latest`									|
| `mongo.image.pullPolicy`							| The pull policy for the chart to use while pulling mongodb containers							| `IfNotPresent`							|
| `mongo.port`										| The port to expose for mongodb container					                               		| `27017`                                   |
| `mongo.trials_db`									| The db in mongo to be used for trials storage				                        			| `model_db`                                |
| `mongo.trials_collection`							| The collection in mongo to be used for trials				                        			| `jobs`                                    |
| `mongo.service.type`								| Mongodb's service type (choose from ClusterIP / NodePort / LoadBalancer / ExternalName)		| `ClusterIP`                               |
| `mongo.resources`									| Override to specify the resource requests and limits for the mongodb pods						| `{}`                                      |
| `mongo.nodeSelector`								| Override to apply a node selector for mongodb deployment						            	| `{}`                                      |
| `mongo.affinity`									| Override to define affinity for mongodb deployment						                	| `{}`                                      |
| `mongo.tolerations`								| Override to define tolerations for mongodb deployment						                	| `{}`                                      |
| `mongo.env`										| Env vars to provide to mongodb						                                    	| `{}`                                      |
| `monitor.image.repository`						| The image repository to pull monitor image from					                			| `hipages/hyperkops-monitor`               |
| `monitor.image.tag`								| The tag for the monitor image to be pulled							                       	| `latest`                                  |
| `monitor.image.pullPolicy`						| The pull policy for the chart to use while pulling monitor containers							| `IfNotPresent`                            |
| `monitor.resources`								| Override to specify the resource requests and limits for the monitor pods						| `{}`                                      |
| `monitor.nodeSelector`							| Override to apply a node selector for monitor deployment						            	| `{}`                                      |
| `monitor.affinity`								| Override to define affinity for monitor deployment						                	| `{}`                                      |
| `monitor.tolerations`								| Override to define tolerations for monitor deployment						                	| `{}`                                      |
| `monitor.env.TRIALS_TIMEOUT_INTERVAL`				| Env variable to set time out interval for the workload application							| `15`                                      |
| `monitor.env.UPDATE_INTERVAL`						| Env variable to set update interval for the workload application			       				| `1`                                       |
| `monitor.env.LOGLEVEL`							| Env variable to set log level for the monitor application				            			| `INFO`                                    |
| `monitor.secrets`				    				| Settings to specify which secrets monitor will have access to		        					| `{}`                                      |
| `worker.image.repository`							| The image repository to pull worker image from						                		| `hipages/hyperkops-worker`                |
| `worker.image.tag`								| The tag for the worker image to be pulled							                        	| `latest`                                  |
| `worker.image.pullPolicy`							| The pull policy for the chart to use while pulling worker containers							| `IfNotPresent`                            |
| `worker.resources`								| Override to specify the resource requests and limits for the workload pods					| `{}`                                      |
| `worker.nodeSelector`								| Override to apply a node selector for worker deployment						            	| `{}`                                      |
| `worker.affinity`									| Override to define affinity for worker deployment					                    		| `{}`                                      |
| `worker.tolerations`								| Override to define tolerations for worker deployment						                	| `{}`                                      |
| `worker.env`										| Env vars to provide to the worker						                                    	| `{}`                                      |
| `worker.hpa.enabled`								| Setting to enable Horizontal Pod autoscaler for worker deployment                           	| `false`                                   |
| `worker.hpa.minReplicas`							| If the `worker.hpa.enabled` is true then this setting speciifies the minimum number of pods   | `1`                                       |
| `worker.hpa.maxReplicas`						    | If the `worker.hpa.enabled` is true then this setting speciifies the maximum number of pods   | `50`                                      |
| `worker.hpa.targetCPUUtilizationPercentage`	    | If the `worker.hpa.enabled` is true then this setting speciifies the taget CPU% for pods	    | `75`                                      |
| `worker.hpa.targetMemoryUtilizationPercentage`    | If the `worker.hpa.enabled` is true then this setting speciifies the taget Memory% for pods   | `75`                                      |
| `workload.image.repository`						| The image repository to pull workload image from		                						| `hipages/hyperkops-example`               |
| `workload.image.tag`								| The tag for the workload image to be pulled				                       				| `latest`                                  |
| `workload.image.pullPolicy`						| The pull policy for the chart to use while pulling workload containers						| `IfNotPresent`                            |
| `workload.resources`								| Override to specify the resource requests and limits for the workload pods					| `{}`                                      |
| `workload.nodeSelector`							| Override to apply a node selector for workload deployment					            		| `{}`                                      |
| `workload.affinity`								| Override to define affinity for workload deployment							                | `{}`                                      |
| `workload.tolerations`							| Override to define tolerations for workload deployment						               	| `{}`                                      |
| `workload.env.LOGLEVEL`							| Env variable to set log level for the workload application							        | `INFO`                                    |
| `workload.env.NUM_EVAL_STEPS`						| Env variable to set number of evaluation steps for the workload application					| `1000`                                    |
| `workload.env.MULTIPLIER`							| Env variable to set multiplier for the workload application					        		| `2`                                       |
| `workload.secrets.enabled`						| Specifies if the secrets are enabled for the chart			                   				| `false`                                   |
| `workload.secrets.data.aws_access_key_id`	    	| Secrets to be specified as key value pair					                               		| `example`                                 |
| `workload.secrets.data.aws_secret_access_key`	    | Secrets to be specified as key value pair				                               			| `example`                                 |

