# End-to-end Solutions Package

This End-to-end solutions package uses novel applications to demonstrate Cloudify functionality. It is intended as a sales aid.

## About

The solutions package tells the story of an organization with a private Openstack cloud and a public AWS cloud. You will reenact this story in the following demo script.

There are several steps:

* [Preparation](#preparation)
  * [Create Lab](#create-lab)
  * [Add Demo Tenant](#add-demo-tenant)
  * [Add Secrets](#add-secrets)
  * [Upload Plugins](#upload-plugins)
  * [Install Kubernetes](#install-kubernetes)
  * [Create Example Networks](#create-example-network)
* [Demo](#demo)
  * [Install Database](#install-database)
  * [Install Load Balancer](#install-load-balancer)
  * [Install Drupal CMS](#install-drupal)
  * [Install Wordpress CMS](#install-wordpress)


# Preparation

These are steps that you need to follow in order to run the demo.


## Create Lab

See [Create New Lab](http://labs.cloudify.co/).


## Add Demo Tenant

To create a tenant, select "Tenant Management" from the left navigation menu.

![Create Tenant: Left Navigation Menu][create-tenant-nav]


Locate the "Tenants Management" panel. Click **Add**.

![Create Tenant: Pane][create-tenant-section]


Provide a name for your demo tenant, and click **Add**.

![Create Tenant: Form][create-tenant-form]


Activate your demo tenant by moving your mouse by selecting the demo tenant from the tenant drop-down menu in the top menu.

![Select Tenant][select-tenant]


Now that you have created the demo tenant and made it your active tenant, you can proceed to the next steps.


## Add Secrets

Cloudify stores your Openstack and AWS credentials as secrets.

  * In the lab environment, the Openstack credentials are already stored as secrets.
  * The user should manually add their AWS credentials, by creating the following secrets:
    * `aws_access_key_id`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `aws_secret_access_key`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `ec2_region_name`: See _Region_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `ec2_region_endpoint` See _Endpoint_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `availability_zone` See _AZ Names_ in [AWSRegionsAndAZs](https://gist.github.com/neilstuartcraig/0ccefcf0887f29b7f240).

**For each secret listed above, create a secret with the exact name, and the appropriate value, with the create secrets form as shown below.**

On the left navigation menu, select System Resources.

![Add Secrets: Left Navigation Menu][add-secrets-nav]


Locate the "Secret Store Management" panel. Click **Create**.

![Add Secrets: Panel][add-secrets-panel]


Provide a name for your demo tenant, and click **Create**.

![Add Secrets: Form][add-secrets-form]


## Upload Plugins

The blueprints that are installed in the following steps require these plugins.

  * [Openstack Plugin 2.6.0](https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.6.0/cloudify_openstack_plugin-2.6.0-py27-none-linux_x86_64-centos-Core.wgn)
  * [AWS Plugin 1.5.1.2](https://github.com/cloudify-cosmo/cloudify-aws-plugin/releases/download/1.5.1.2/cloudify_aws_plugin-1.5.1.2-py27-none-linux_x86_64-centos-Core.wgn)
  * [AWSSDK Plugin 1.2.0.3](https://github.com/cloudify-incubator/cloudify-awssdk-plugin/releases/download/1.2.0.3/cloudify_awssdk_plugin-1.2.0.3-py27-none-linux_x86_64-centos-Core.wgn)
  * [Utilities Plugin 1.4.5](https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.4.5/cloudify_utilities_plugin-1.4.5-py27-none-linux_x86_64-centos-Core.wgn)

**For each link listed above, upload the plugin by copying the URL and pasting it in the upload plugins form as shown below.**

On the left navigation menu, select System Resources.

![Upload Plugins: Left Navigation Menu][add-secrets-nav]


Locate the "Plugins" panel. Click **Upload**.

![Upload Plugins: Panel][upload-plugins-panel]


Paste the URL in the _URL_ field and click **Upload**.

![Upload Plugins: Form][upload-plugins-form]


## Create Example Networks

Having created your tenant, created secrets, and uploaded plugins, you are now in a position to begin creating the environements and creating deployments. Below are the links to an Openstack network blueprint and an AWS network blueprint.

  * [AWS Network Blueprint](https://github.com/cloudify-examples/aws-example-network/archive/master.zip)
  * [Openstack Network Blueprint](https://github.com/cloudify-examples/openstack-example-network/archive/master.zip)

**For each link listed below, upload the blueprint, create the deployment, and execute install as shown below.**

On the left navigation menu, select Local Blueprints, the click **Upload**.

![Upload Blueprints: Left Navigation Menu][blueprints-nav]


Paste the URL in the _URL_ field, provide a _Blueprint Name_, such as `aws`, choose the _Blueprint filename_, such as `update-blueprint.yaml` and click **Upload**.

![Upload Blueprints: Form][blueprints-form]


You should now see the blueprint that you just uploaded. Click **Deploy**.

![Upload Blueprints: Panel][blueprints-panel]


Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `aws`. For Openstack, the input `external_network_name` should be set to `external_network`.

![Create Deployments: Panel][deployment-panel]


On the left navigation menu, select Deployments.

![Create Deployments: Left Navigation Menu][deployments-nav]


Locate the deployment that you just created and execute the install workflow.

![Create Deployments: Install][deployments-install]


## Install Kubernetes


# Demo


## Install Database


## Install Load Balancer


## Install Drupal


## Install Wordpress


[create-tenant-nav]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-nav.png "Left Navigation Menu"
[create-tenant-section]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-section.png "Create Tenant Panel"
[create-tenant-form]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-form.png "Create Tenant Form"
[select-tenant]: https://github.com/EarthmanT/e2e/raw/final/images/select-tenant.png "Select Tenant"
[add-secrets-nav]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-nav.png "Left Navigation Menu"
[add-secrets-panel]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-panel.png "Add Secrets Panel"
[add-secrets-form]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-form.png "Add Secrets Form"
[upload-plugins-panel]: https://github.com/EarthmanT/e2e/raw/final/images/upload-plugins-panel.png "Upload Plugins Panel"
[upload-plugins-form]: https://github.com/EarthmanT/e2e/raw/final/images/upload-plugins-form.png "Upload Plugins Form"
[blueprints-nav]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-nav.png "Left Navigation Menu"
[blueprints-form]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-form.png "Upload Blueprints Form"
[blueprints-panel]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-panel.png "Upload Blueprints Panel"
[deployments-nav]: https://github.com/EarthmanT/e2e/raw/final/images/deployments-nav.png "Left Navigation menu"
[deployments-install]: https://github.com/EarthmanT/e2e/raw/final/images/deployments-install.png "Install Deployment"
