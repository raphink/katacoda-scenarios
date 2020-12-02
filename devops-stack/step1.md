# Start K3s stack


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
module "cluster" {
  source = "git::https://github.com/camptocamp/camptocamp-devops-stack.git//modules/k3s-docker?ref=v0.15.0"

  cluster_name = terraform.workspace
  node_count   = 1

  repo_url        = "https://github.com/camptocamp/camptocamp-devops-stack.git"
  target_revision = "v0.15.0"
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
