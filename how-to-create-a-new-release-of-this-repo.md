##  Instructions

Sometimes, you will want to update this repo. For example, when we release a new version of one of the blueprints or update a plugin. 

This is how I have built it:

**Prepare a temporary work environment:**

```
mktmpenv
pip install cloudify
```

**Download each of the blueprints:**

```
git clone https://github.com/cloudify-examples/mariadb-blueprint.git
pushd mariadb-blueprint
git checkout e2e
popd
git clone https://github.com/cloudify-examples/haproxy-blueprint.git
pushd haproxy-blueprint
git checkout e2e
popd
git clone https://github.com/cloudify-examples/drupal-blueprint.git
pushd drupal-blueprint
git checkout e2e
popd
git clone https://github.com/earthmant/db-lb-app.git
pushd db-lb-app
git checkout e2e
popd
git clone https://github.com/cloudify-incubator/cloudify-kubernetes-provider.git
pushd cloudify-kubernetes-provider
git checkout tags/0.0.0+13
wget https://github.com/cloudify-incubator/cloudify-kubernetes-provider/releases/download/0.0.0+13/cfy-autoscale -O examples/cluster_blueprint/resources/cfy-autoscale
wget https://github.com/cloudify-incubator/cloudify-kubernetes-provider/releases/download/0.0.0+13/cfy-kubernetes -O examples/cluster_blueprint/resources/cfy-kubernetes
popd
wget https://github.com/EarthmanT/cloudify-dblb/releases/download/0.2/cloudify_dblb-0.2-py27-none-linux_x86_64-centos-Core.wgn -O ~/Desktop/cloudify_dblb-0.2-py27-none-linux_x86_64-centos-Core.wgn
```

**Package them for cloudify:**

```
cfy blueprints package mariadb-blueprint --output-path ~/Desktop/db
cfy blueprints package haproxy-blueprint --output-path ~/Desktop/lb
cfy blueprints package drupal-blueprint --output-path ~/Desktop/drupal
cfy blueprints package db-lb-app --output-path ~/Desktop/wordpress
cfy blueprints package cloudify-kubernetes-provider --output-path ~/Desktop/k8s
```
