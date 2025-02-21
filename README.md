# cloud-platform

This is a GitOps approach to deploying the entire Living Open Source Lab Platform.

The motivation behind this deployment is to have an approach to deploy the entire Living Open Source Lab Platform with a single command. This setup heavily relies on using git as a single source of truth, while keeping track of changes and applying them in the cluster.

## Getting Started

The Cloud platform is meant to be run on any kubernetes cluster. Though we have a bias torwards Talos as the kubernetes operating system, these configurations should be able to work on any kubernetes cluster. Once you have a running kubernetes cluster, you can proceed to the next step.

## Deploy the Platform

To deploy the platform, you will need to have a running kubernetes cluster. Once you have a running kubernetes cluster, you'll need to install fluxcd on the machine where you'll be executing the commands from.

For instruction on how to install fluxcd, please refer to the [fluxcd installation documentation](https://fluxcd.io/flux/installation/).

Once you have fluxcd installed on your machine, you can test to see if your cluster version supports the fluxcd version you have installed.


```bash
flux check --pre
```

If you get a message saying that your cluster version does not support the fluxcd version you have installed, you will need to upgrade your cluster version to a version that supports the fluxcd version you have installed.

If all goes well, you can proceed to boostrap your cluster with this repository.

## Bootstrap Prerequisites

Before you can bootstrap your cluster, you will need to get access to this repository. To do so, you will need to create a personal access token (PAT) with the following permissions:
- `repo`

Or alternativeely, you can use the ssh keys to access this repository.

### Bootstrap Using Personal Token

Once you have a personal access token, you can bootstrap your cluster by running the following command:

```bash
export GITHUB_TOKEN=<my-token>

flux bootstrap github \
  --owner=<your-github-username> \
  --repository=<your-repository-name> \
  --path=clusters/your-cluster-name
```

### Bootstrap Using SSH Keys

```bash
flux bootstrap git \
  --url=ssh://git@github.com/livingopensource/lab-platform.git \
  --branch=main \
  --private-key-file=/path/to/your/private/key \
  --path=clusters/development
```

## Verify

To verify the bootstrap process, you can run the following command:
```bash
flux check
```

The you can check the deployment status of the platform by running the following command:

```bash
kubectl get hr -A
```

Then wait until all the helm releases are in a ready state.