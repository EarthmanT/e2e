# How to use this demo:

1. Create a Lab.
2. Download the latest version of this demo from the releases page. ![download zip files][downloadzips]
3. Upload the plugin for scaling the database cluster. ![upload dblb plugin][uploadplugin]
4. Create dummy secrets for the k8s cluster: `for i in kubernetes_master_ip kubernetes_certificate_authority_data kubernetes_master_port kubernetes-admin_client_key_data kubernetes-admin_client_certificate_data; do cfy secrets create -s null $i; done`
5. Upload the `db` blueprint. ![upload db blueprint][uploaddb]
6. Upload the `lb` blueprint. ![upload lb blueprint][uploadlb]
7. Upload the `drupal` blueprint. ![upload drupal blueprint][uploaddp]
8. Upload the `wordpress` blueprint. ![upload wordpress blueprint][uploadwp]
9. Upload the `k8s` blueprint. ![upload k8s blueprint][uploadk8s]
10. Create a `k8s` deployment. Just provide a name, you do not need to change inputs. ![create k8s deployment][createk8s]
11. Execute `install` on `k8s` deployment. ![install k8s deployment][installk8s]
12. Create on `drupal` deployment. Change these inputs: `new_database_user` should be `appuser`, and `proxy_manager_network` should be `external`. ![install drupal deployment][installdpa] ![install drupal deployment][installdpb]
11. Execute `install` on `drupal` deployment.
12. When the `drupal` deployment is finished. Scale the database cluster.
13. When the `k8s` deployment is finished, execute the `wordpress` deployment and execute `install`.


##  build instructions

The following instructions are to build this demo - meaning you want to create the archive used above.

1. Prepare a temporary work environment.
```
mktmpenv
pip install cloudify
```

Download each of the blueprints:
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

Package them for cloudify:
```
cfy blueprints package mariadb-blueprint --output-path ~/Desktop/db
cfy blueprints package haproxy-blueprint --output-path ~/Desktop/lb
cfy blueprints package drupal-blueprint --output-path ~/Desktop/drupal
cfy blueprints package db-lb-app --output-path ~/Desktop/wordpress
cfy blueprints package cloudify-kubernetes-provider --output-path ~/Desktop/k8s
```

[downloadzips]: https://github.com/EarthmanT/e2e/raw/master/images/step3.png "Download Zips"
[uploadplugin]: https://github.com/EarthmanT/e2e/raw/master/images/step3.png "Upload dblb Plugin"
[uploaddb]: https://github.com/EarthmanT/e2e/raw/master/images/step5.png "Upload db Blueprint"
[uploadlb]: https://github.com/EarthmanT/e2e/raw/master/images/step6.png "Upload lb Blueprint"
[uploaddp]: https://github.com/EarthmanT/e2e/raw/master/images/step7.png "Upload drupal Blueprint"
[uploadwp]: https://github.com/EarthmanT/e2e/raw/master/images/step8.png "Upload wordpress Blueprint"
[uploadk8s]: https://github.com/EarthmanT/e2e/raw/master/images/step9.png "Upload k8s Blueprint"
[createk8s]: https://github.com/EarthmanT/e2e/raw/master/images/step10.png "Create k8s Deployment"
[installk8s]: https://github.com/EarthmanT/e2e/raw/master/images/step11.png "Install k8s Deployment"
[installdpa]: https://github.com/EarthmanT/e2e/raw/master/images/step12a.png "Install Drupal Deployment A"
[installdpb]: https://github.com/EarthmanT/e2e/raw/master/images/step12b.png "Install Drupal Deployment B"