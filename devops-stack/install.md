Start by launching the Kubernetes cluster with `launch.sh`{{execute}}.

Note that the DevOps Stack allows to provision Kubernetes clusters (on EKS,
AKS & K3s at the moment), but Katacoda is not a proper environment for these
use cases, as they require either credentials (EKS/AKS) or lots of resources
(for a local K3s cluster on a single node).

Verify that the cluster is up by listing the nodes with `kubectl get nodes`{{execute}}


## Install dependencies


Install Terraform:


```
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
export PATH="$HOME/.tfenv/bin:$PATH"
tfenv install latest
tfenv use latest
```{{execute}}


Install Helm:

```
snap install helm --classic
```{{execute}}


## Create project

```
mkdir my-project
cd my-project
mkdir terraform
```{{execute}}


<pre class="file" data-filename="my-project/terraform/main.tf" data-target="replace">
locals {
  base_domain = "[[HOST_SUBDOMAIN]]-443-[[KATACODA_HOST]].environments.katacoda.com"
}

module "cluster" {
  source = "git::https://github.com/raphink/camptocamp-devops-stack.git//modules/argocd-helm?ref=app_domain"

  cluster_name = terraform.workspace

  repo_url = "https://github.com/raphink/camptocamp-devops-stack.git"
  target_revision = "app_domain"

  base_domain = local.base_domain
  cluster_issuer = "ca-issuer"

  oidc       = {
    issuer_url              = format("https://keycloak.%s/auth/realms/kubernetes", local.base_domain)
    oauth_url               = format("https://keycloak.%s/auth/realms/kubernetes/protocol/openid-connect/auth", local.base_domain)
    token_url               = format("https://keycloak.%s/auth/realms/kubernetes/protocol/openid-connect/token", local.base_domain)
    api_url                 = format("https://keycloak.%s/auth/realms/kubernetes/protocol/openid-connect/userinfo", local.base_domain)
    client_id               = "applications"
    client_secret           = random_password.clientsecret.result
    oauth2_proxy_extra_args = []
  }
  olm        = {
    enable = true
  }
  keycloak   = {                                               
    enable         = true                                                           
    admin_password = random_password.admin_password.result                          
    domain         = "keycloak.${local.base_domain}"
  }
  argocd     = {
    domain = "argocd.${local.base_domain}"
  }
  grafana    = {
    generic_oauth_extra_args = {}
    domain                   = "grafana.${local.base_domain}"
  }
  prometheus = {
    domain = "prometheus.${local.base_domain}"
  }
  alertmanager = {
    domain = "alertmanager.${local.base_domain}"
  }

  app_of_apps_values_overrides = [
<&lt;EOF
traefik:
  tolerations:
    - key: node-role.kubernetes.io/master
      operator: Exists
      effect: NoSchedule
  ports:
    web:
      hostPort: 80
    websecure:
      hostPort: 443
  service:
    type: NodePort
EOF
,
  ]
}

resource "random_password" "clientsecret" {
  length  = 16
  special = false
}

resource "random_password" "admin_password" {
  length  = 16
  special = false
}
</pre>


Set your cluster name:

```
export CLUSTER_NAME=default
```{{execute}}


## Launch the stack

```
wget -O- "https://raw.githubusercontent.com/camptocamp/camptocamp-devops-stack/v0.17.0/scripts/provision.sh" | sh
```{{execute}}


## Inspect the cluster with K9s

```
docker run -ti --net host \
  -v ~/.kube/config:/root/.kube/config \
  quay.io/derailed/k9s
```{{execute T2}}

By watching all the pods (with `:pods`{{execute T2}} then `0`{{execute T2}})
and/or the ArgoCD applications (with `:applications`{{execute T2}}),
you will observe the following:
 - Deployment of ArgoCD as a Helm chart
 - Deployment of [OLM](https://github.com/operator-framework/operator-lifecycle-manager) as a pre-requisite to operators
 - Deployment of the Kube Prometheus Stack for monitoring and observability
 - Deployment of [Traefik](https://traefik.io/) as Ingress Controller
 - Deployment of [Keycloak](https://www.keycloak.org/)
 - Deployment of the App of Apps itself
 - Deployment of [Cert Manager](https://cert-manager.io/)
 - Re-deployment of ArgoCD as an Argo CD application
 - Deployment of [Loki](https://grafana.com/oss/loki/) for log management
 - Deployment of the Metrics Server


You can also follow these steps by setting up a port forward on the ArgoCD
server (in the K9s Pod view, select the ArgoCD server pod and hit Shift+F)
forwarding port `8080` to `8080` on `0.0.0.0`, then
[accessing it in your browser](https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com),
with the login `admin`/`argocd`.



Current caveats:
  - there is no Storage Class on Katacoda, which prevents Keycloak, Prometheus,
    & Alertmanager from starting.
