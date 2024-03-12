# kubeflow-kluctl

This repository contains a [Kluctl](https://kluctl.io) based deployment project for [Kubeflow](https://www.kubeflow.org/).

## Table of Content

## Motivation

This project has started as a PoC to demonstrate the capabilities of Kluctl as a deployment tool for complex Kubernetes
deployment projects. Kubeflow turns out to be a much more complex deployment than usual Kubernetes projects, leading to a lot
of complicated and partially manual steps required to install and maintain a Kubeflow instance.

I believe that Kluctl is able to simplify this process a lot, so I started building this PoC. My motivation is to
get attention on the Kluctl project and in best case find users that feel that their needs are fulfilled, ultimately
bringing adoption and maintainers to the project.

At the same time, I believe Kubeflow could potentially benefit from this effort. This project might even turn out to be
a viable distribution of Kubeflow as it makes installing and long-term maintaining it so much easier.

I have read into a lot of issues in the [manifests](https://github.com/kubeflow/manifests) and also looked into other
distributions (platform/cloud specific and more generic solutions like deployKF). What I found so far confirms my
assumptions.

## Why Kluctl?

If you're already familiar with the [manifests](https://github.com/kubeflow/manifests) repo, you can think of Kluctl as a
replacement for the top-level `kustomization.yaml` and the top-level `kustomize build | kubectly apply -f-` invocation.

The difference is that it allows much better control over the deployment process. For example, the
[`deployment.yaml`](https://kluctl.io/docs/kluctl/deployments/deployment-yml/) files found on every folder level
(starting at the root), allow to control deployment order by introducing [`barriers`](https://kluctl.io/docs/kluctl/deployments/deployment-yml/#barriers)
between individual deployment items. The deployment items itself are either
[includes of sub-deployments](https://kluctl.io/docs/kluctl/deployments/deployment-yml/#includes), simple
[Kustomize](https://kluctl.io/docs/kluctl/deployments/deployment-yml/#kustomize-deployments) deployments or
[Helm Charts](https://kluctl.io/docs/kluctl/deployments/helm/).

Also, whole sub-deployments and individual items can be disabled conditionally, as seen for example in `common/cert-manager/deployment.yaml`.

Ultimately, these features allow to remove most (if not all?) manual interventions required by the user while installing
or upgrading Kubeflow. For example, there is no need to choose different sets of Kustomize overlays based on which type
of auth setup you want to install. Instead, this can be configured via configuration files and will automatically lead
to all required modifications to the deployment process.

This also allows to easily implement features like "bring your own cert-manager or istio", simply by changing
configuration and using appropriate conditionals and templating in the deployment project.

Another advantage that comes for free is the integration Helm Charts with proper support of templated values. This can
for example be seen in `common/dex/helm-values.yaml`. 

## Future

- More configuration
- Integration of manifests repo
- SOPS
- GitOps