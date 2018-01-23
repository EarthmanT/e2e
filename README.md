# How to use this demo:

Create a Lab.

Download the latest version of this demo from the releases page. ![download zip files][downloadzips]

Upload the plugin for scaling the database cluster.

![upload dblb plugin][uploadplugin]

Create dummy secrets for the k8s cluster: `for i in kubernetes_master_ip kubernetes_certificate_authority_data kubernetes_master_port kubernetes-admin_client_key_data kubernetes-admin_client_certificate_data; do cfy secrets create -s null $i; done`

```
for i in kubernetes_master_ip \
         kubernetes_certificate_authority_data \
         kubernetes_master_port \
         kubernetes-admin_client_key_data \
         kubernetes-admin_client_certificate_data;
do cfy secrets create -s null $i;
done
```

Upload the `db` blueprint.

![upload db blueprint][uploaddb]

Upload the `lb` blueprint.

![upload lb blueprint][uploadlb]

Upload the `drupal` blueprint.

![upload drupal blueprint][uploaddp]

Upload the `wordpress` blueprint.

![upload wordpress blueprint][uploadwp]

Upload the `k8s` blueprint. ![upload k8s blueprint][uploadk8s]

Create a `k8s` deployment. Just provide a name, you do not need to change inputs.

![create k8s deployment][createk8s]

Execute `install` on `k8s` deployment.

![install k8s deployment][installk8s]

Create on `drupal` deployment.

Change these inputs: `new_database_user` should be `appuser`, and `proxy_manager_network` should be `external`.

![install drupal deployment][uploaddpa]

![install drupal deployment][uploaddpb]

Notice that the drupal deployment will create some resources in AWS and Openstack. The database cluster (`db` deployment) and loadbalancer (`lb` deployment) are both in AWS. The Drupal application itself will be in Openstack.

![Multicloud Notes][multicloud]

Execute `install` on `drupal` deployment.

![install drupal deployment][installdp]

You will notice that the drupal deployment install creates and installs the `db` deployment. Shortly thereafter the `lb` deployment will be created and installed.

![install db deployment][installdb]

When the `drupal` deployment is finished. Scale the database cluster by executing the `scale and update` workflow. You'll notice that the `scale`  workflow is executed on the `db` deployment and the `execute_operation` workflow is executed on the `lb` deployment.

![scale db/lb stack][scaledblb] ![scale db/lb stack][scaledblba] ![scale db/lb stack][scaledblbb]

When the `k8s` deployment is finished, create the `wordpress` deployment.

Change these inputs: `new_database_user` should be `appuser`, and `environment_blueprint_filename` should be `aws-blueprint.yaml`.

![create wp deployment][deploywp] ![create wp deployment][deploywpb]

Execute `install` workflow.


[downloadzips]: https://github.com/EarthmanT/e2e/raw/master/images/downloadzips.png "Download Zips"
[uploadplugin]: https://github.com/EarthmanT/e2e/raw/master/images/uploadplugin.png "Upload dblb Plugin"
[uploaddb]: https://github.com/EarthmanT/e2e/raw/master/images/uploaddb.png "Upload db Blueprint"
[uploadlb]: https://github.com/EarthmanT/e2e/raw/master/images/uploadlb.png "Upload lb Blueprint"
[uploaddp]: https://github.com/EarthmanT/e2e/raw/master/images/uploaddp.png "Upload drupal Blueprint"
[uploadwp]: https://github.com/EarthmanT/e2e/raw/master/images/uploadwp.png "Upload wordpress Blueprint"
[uploadk8s]: https://github.com/EarthmanT/e2e/raw/master/images/uploadk8s.png "Upload k8s Blueprint"
[createk8s]: https://github.com/EarthmanT/e2e/raw/master/images/createk8s.png "Create k8s Deployment"
[installk8s]: https://github.com/EarthmanT/e2e/raw/master/images/installk8s.png "Install k8s Deployment"
[uploaddpa]: https://github.com/EarthmanT/e2e/raw/master/images/uploaddpa.png "Create Drupal Deployment A"
[uploaddpb]: https://github.com/EarthmanT/e2e/raw/master/images/uploaddpb.png "Create Drupal Deployment B"
[installdp]: https://github.com/EarthmanT/e2e/raw/master/images/installdp.png "Install Drupal Deployment"
[installdb]: https://github.com/EarthmanT/e2e/raw/master/images/installdb.png "Install DB"
[scaledblb]: https://github.com/EarthmanT/e2e/raw/master/images/scaledblb.png "Scale DB/LB"
[scaledblba]: https://github.com/EarthmanT/e2e/raw/master/images/scaledblba.png "Scale DB/LB A"
[scaledblbb]: https://github.com/EarthmanT/e2e/raw/master/images/scaledblbb.png "Scale DB/LB B"
[deploywp]: https://github.com/EarthmanT/e2e/raw/master/images/deploywp.png "Deploy wordpress Deployment"
[deploywpb]: https://github.com/EarthmanT/e2e/raw/master/images/deploywp.png "Deploy wordpress Deployment"
[installwp]: https://github.com/EarthmanT/e2e/raw/master/images/installwp.png "Install wordpress Deployment"
[multicloud]: https://github.com/EarthmanT/e2e/raw/master/images/multicloud.png "Multicloud Deployment"