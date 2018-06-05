# `eksctl` - a CLI for Amazon EKS

![Weaveworks](logo/weaveworks.svg)

[![Circle CI](https://circleci.com/gh/weaveworks/eksctl/tree/master.svg?style=shield)](https://circleci.com/gh/weaveworks/eksctl/tree/master)


`eksctl` is a simple CLI tool for creating clusters on EKS - Amazon's new managed Kubernetes service for EC2. It is written in Go, and based on Amazon's official CloudFormation templates.

You can create a cluster in minutes with just one command – **`eksctl create cluster`**!

## Usage

To download the latest release, run:

```console
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

You will need to have AWS API credentials configured. What works for AWS CLI or any other tools (kops, Terraform etc), should be sufficient. You can use [`~/.aws/credentials` file][awsconfig]
or [environment variables][awsenv]. For more information read [AWS documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html).

[awsenv]: https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html
[awsconfig]: https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html

To create a basic cluster, run:

```console
eksctl create cluster
```

A cluster will be created with default parameters
- exciting auto-generated name, e.g. "fabulous-mushroom-1527688624"
- 2x `m5.large` nodes (this instance type suits most common use-cases, and is good value for money)
- default EKS AMI
- `us-west-2` region
- dedicated VPC (check your quotas)

Once you have created a cluster, you will find `kubeconfig` in your current working directory. If you have `kubectl` v1.10.x as well as `heptio-authenticator-aws` commands in your PATH, you should be
able to use `kubectl`. You will need to make sure to use the same AWS API credentials for this also. Check [EKS docs][ekskubectl] for instructions.

[ekskubectl]: https://docs.aws.amazon.com/eks/latest/userguide/configure-kubectl.html

To list the details about a cluster or all of the clusters, use:

```console
eksctl get cluster [--cluster-name <name>] [--region <region>]
```

To create the same kind of basic cluster, but with a different name, run:

```console
eksctl create cluster --cluster-name cluster-1 --nodes 4
```

To write cluster credentials to a file other than default, run:

```console
eksctl create cluster --cluster-name cluster-2 --nodes 4 --kubeconfig ./kubeconfig.cluster-2.yaml
```

To prevent storing cluster credentials locally, run:

```console
eksctl create cluster --cluster-name cluster-3 --nodes 4 --write-kubeconfig=false
```

To use a 3-5 node Auto Scaling Group, run:

```console
eksctl create cluster --cluster-name cluster-4 --nodes-min 3 --nodes-max 5
```

To use 30 `c4.xlarge` nodes, run:

```console
eksctl create cluster --cluster-name cluster-5 --nodes 30 --node-type c4.xlarge
```

To delete a cluster, run:

```console
eksctl delete cluster --cluster-name <name> [--region <region>]
```

<!-- TODO for 0.3.0
To use more advanced configuration options, [Cluster API](https://github.com/kubernetes-sigs/cluster-api):

```console
eksctl apply --cluster-config advanced-cluster.yaml
```
-->

## Project Roadmap

### Developer use-case (0.2.0)

It should suffice to install a cluster for development with just a single command. Here are some examples:

To create a cluster with default configuration (2 `m4.large` nodes), run:

```console
eksctl create cluster
```

The developer may choose to pre-configure popular addons, e.g.:

- Weave Net: `eksctl create cluster --networking weave`
- Helm: `eksctl create cluster --addons helm`
- AWS CI tools (CodeCommit, CodeBuild, ECR): `eksctl create cluster --addons aws-ci`
- Jenkins X: `eksctl create cluster --addons jenkins-x`
- AWS CodeStar: `eksctl create cluster --addons aws-codestar`
- Weave Scope and Flux: `eksctl create cluster --addons weave-scope,weave-flux`


It should be possible to combine any or all of these addons.

It would also be possible to add any of the addons after cluster was created with `eksctl create addons`.

### Manage EKS the GitOps way (0.3.0)

Just like `kubectl`, `eksctl` aims to be compliant with GitOps model, and can be used as part of a GitOps toolkit!

For example, `eksctl apply --cluster-config prod-cluster.yaml` will manage cluster state declaratively.

And `eksctld` will be a controller inside of one cluster that can manage multiple other clusters based on Kubernetes Cluster API definitions (CRDs).

## Contributions
Code contributions are very welcome, however until a 0.1.0 release testing and bug reports are the contributions that authors will appreciate the most.

## Get in touch
[Create an issue](https://github.com/weaveworks/eksctl/issues/new), or login to [Weave Community Slack (#eksctl)](https://weave-community.slack.com/messages/CAYBZBWGL/) ([signup](https://slack.weave.works/)).
