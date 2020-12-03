Start by launching the Kubernetes cluster with `launch.sh`{{execute}}.

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
module "argocd" {
  source = "git::https://github.com/raphink/camptocamp-devops-stack.git//modules/argocd-helm?ref=app_of_apps_complex_vars"

  cluster_name = terraform.workspace

  repo_url = "https://github.com/raphink/camptocamp-devops-stack.git"
  target_revision = "app_of_apps_complex_vars"

  base_domain = "test"
  cluster_issuer = "ca-issuer"

  oidc                            = {
    issuer_url              = format("https://keycloak.apps.%s/auth/realms/kubernetes", local.base_domain)
    oauth_url               = format("https://keycloak.apps.%s/auth/realms/kubernetes/protocol/openid-connect/auth", local.base_domain)
    token_url               = format("https://keycloak.apps.%s/auth/realms/kubernetes/protocol/openid-connect/token", local.base_domain)
    api_url                 = format("https://keycloak.apps.%s/auth/realms/kubernetes/protocol/openid-connect/userinfo", local.base_domain)
    client_id               = "applications"
    client_secret           = random_password.clientsecret.result
    oauth2_proxy_extra_args = []
  }
}

resource "random_password" "clientsecret" {
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
wget -O- "https://raw.githubusercontent.com/camptocamp/camptocamp-devops-stack/v0.15.0/scripts/provision.sh" | sh
```{{execute}}
