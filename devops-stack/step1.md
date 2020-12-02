# Start K3s stack


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
