# End-to-end Solutions Package

This End-to-end solutions package uses novel applications to demonstrate Cloudify functionality. It is intended as a sales aid.

The solutions package tells the story of an organization with a private Openstack cloud and a public AWS cloud. You will reenact this story in the following demo script.

The following Cloudify features are used:

  * Multiple Management Networks (Multi-Cloud).
  * Traditional VMs, Containers, and Kubernetes Orchestration (Hybrid-Cloud).
  * Multi-tenancy.
  * Deployment Proxy (Deployments as a Service).
  * Secrets, both tenant-wide and global.
  * Scaling.


## Table of Contents

There are several steps:

* [Preparation](#preparation)
  * [Create Lab](#create-lab)
  * [Install Kubernetes](#install-kubernetes)
  * [Add Demo Tenant](#add-demo-tenant)
  * [Add Secrets](#add-secrets)
  * [Upload Plugins](#upload-plugins)
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


## Install Kubernetes

The first step of preparation for the demo is to install the Kubernetes Cluster. We run it first, because it needs to be in the default_tenant, and also because it takes a really long time to complete installation and you can run all other parts of the demo without it. It also demonstrates multi-tenancy and global secrets. This cluster is based on the [Cloudify Kubernetes Provider](http://docs.getcloudify.org/4.2.0/plugins/container-support/#infrastructure-orchestration). It has been modified specifically for this demo.

On the left navigation menu, select Local Blueprints, the click **Upload**.

![Upload Blueprints: Left Navigation Menu][blueprints-nav]

[Right-click and copy the Blueprint archive URL](https://github.com/EarthmanT/e2e/releases/download/k8s-e2e-download/k8s-e2e.zip) and paste in the _URL_ field, provide a _Blueprint Name_, such as `k8s`, select `openstack.yaml` as the _Blueprint filename_. Then click **Upload**.

_Help: If uploading from the UI takes a long time or fails, because of the size of the `zip` file, it may be a good idea to upload via the CLI._

![Upload Blueprints: Form][kubernetes-upload-blueprint]

Create the deployment:

![Create Deployments: Form][kubernetes-create-deployment]

Locate the deployment that you just created and execute the install workflow.


## Add Demo Tenant

Creating a demo tenant is necessary for demonstrating multi-tenancy. You will install the database, load balancer, and demo front end applications in this tenant.

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

  * Add your Openstack lab credentials:
    * `keystone_username`: it is `admin`.
    * `keystone_password`: it is `cloudify1234`.
    * `keystone_tenant_name`: it is `admin`.
    * `keystone_region`: it is `RegionOne`.
    * `keystone_url`: this varies by lab, but it will be something like `http://10.10.25.1:5000/v2.0`.
  * Add your AWS credentials, by creating the following secrets:
    * `aws_access_key_id`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `aws_secret_access_key`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `ec2_region_name`: See _Region_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `ec2_region_endpoint` See _Endpoint_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `availability_zone` See _AZ Names_ in [AWSRegionsAndAZs](https://gist.github.com/neilstuartcraig/0ccefcf0887f29b7f240).

The Kubernetes Deployment that you installed above will add secrets to the default_tenant with global visibility. These will be consumed, however, from the demo tenant.

**For each secret listed above, create a secret with the exact name, and the appropriate value, with the create secrets form as shown below.**

On the left navigation menu, select System Resources.

![Add Secrets: Left Navigation Menu][add-secrets-nav]

Locate the "Secret Store Management" panel. Click **Create**.

![Add Secrets: Panel][add-secrets-panel]

Provide a name for your demo tenant, and click **Create**.

![Add Secrets: Form][add-secrets-form]


## Upload Plugins

The blueprints that are installed in the demo require the following plugins.

  * [Openstack Plugin 2.6.0](https://github.com/cloudify-cosmo/cloudify-openstack-plugin/releases/download/2.6.0/cloudify_openstack_plugin-2.6.0-py27-none-linux_x86_64-centos-Core.wgn)
  * [AWS Plugin 1.5.1.2](https://github.com/cloudify-cosmo/cloudify-aws-plugin/releases/download/1.5.1.2/cloudify_aws_plugin-1.5.1.2-py27-none-linux_x86_64-centos-Core.wgn)
  * [AWSSDK Plugin 1.2.0.3](https://github.com/cloudify-incubator/cloudify-awssdk-plugin/releases/download/1.2.0.3/cloudify_awssdk_plugin-1.2.0.3-py27-none-linux_x86_64-centos-Core.wgn)
  * [Utilities Plugin 1.4.5](https://github.com/cloudify-incubator/cloudify-utilities-plugin/releases/download/1.4.5/cloudify_utilities_plugin-1.4.5-py27-none-linux_x86_64-centos-Core.wgn)
  * [Kubernetes Plugin 2.0.0](https://github.com/cloudify-incubator/cloudify-kubernetes-plugin/releases/download/2.0.0/cloudify_kubernetes_plugin-2.0.0-py27-none-linux_x86_64-centos-Core.wgn)

**For each link listed above, upload the plugin by copying the URL and pasting it in the upload plugins form as shown below.**

On the left navigation menu, select System Resources.

![Upload Plugins: Left Navigation Menu][add-secrets-nav]

Locate the "Plugins" panel. Click **Upload**.

![Upload Plugins: Panel][upload-plugins-panel]

Paste the URL in the _URL_ field and click **Upload**.

![Upload Plugins: Form][upload-plugins-form]


## Create Example Networks

Having created your tenant, created secrets, and uploaded plugins, you are now in a position to prepare the IaaS environments. In this demo, and all new examples, we handle network creating via network blueprints. These can be thought of a "Network as a Service" deployments. 

In this demo, you will need two "Network as a Service" deployments. One for AWS and one for Openstack:

  * [AWS Network Blueprint](https://github.com/cloudify-examples/aws-example-network/archive/master.zip)
  * [Openstack Network Blueprint](https://github.com/cloudify-examples/openstack-example-network/archive/master.zip)
  * [Agent Keys](https://github.com/cloudify-examples/helpful-blueprint/archive/master.zip): This is not a network blueprint, but creates the agents SSH key pairs that are used in the demo blueprints.

**For each link listed above, upload the blueprint, create the deployment, and execute install as shown below.**

On the left navigation menu, select Local Blueprints, the click **Upload**.

![Upload Blueprints: Left Navigation Menu][blueprints-nav]

Right-click on the name of the blueprint above to copy the link and paste it in the _URL_ field, provide a _Blueprint Name_, such as `aws` or `openstack`. Choose the _Blueprint filename_, for AWS this is `update-blueprint.yaml` and for Openstack this is `simple-blueprint.yaml`. Click **Upload**.

_In the AWS package, there is a `simple-blueprint.yaml` blueprint and an `update-blueprint.yaml`. If you want to show deployment update, you can first deploy the `simple-blueprint.yaml` and update the deployment with the `update-blueprint.yaml` blueprint._

![Upload Blueprints: Form][blueprints-form]

You should now see the blueprint that you just uploaded. Click **Deploy**.

![Upload Blueprints: Panel][blueprints-panel]

Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `aws`. For the Openstack network deployment, the input `external_network_name` should be set to `external_network`.

![Create Deployments: Panel][deployments-panel]

On the left navigation menu, select Deployments.

![Create Deployments: Left Navigation Menu][deployments-nav]

Locate the deployment that you just created and execute the install workflow.

![Create Deployments: Install][deployments-install]


# Demo


## Install Database

Our database is hosted in AWS. The blueprint that we use for the demo is a MariaDB/Galera database cluster.

To install the database, upload the database blueprint. [Right-click on this link](https://github.com/cloudify-examples/mariadb-blueprint/archive/e2e.zip) to copy the URL. Paste the URL in the _URL_ field. Provide a _Blueprint Name_, such as `db`, choose `aws.yaml` from the _Blueprint filename_ menu and click **Upload**.

![Upload Blueprints: Database][upload-blueprints-database]

Click **Deploy** to fill out the deployments create form.

![Create Deployments: Panel][database-create-panel]

Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `db`.
  * The input `network_deployment_name` should be set to your AWS network deployment name, which above we called `aws`.

![Create Deployments: Form][database-create-form]

On the left navigation menu, select Deployments.

Then execute the install workflow for the `db` deployment.

In the meantime, explore the deployment.

When the install workflow has finished, find the deployment outputs. There is one output called "cluster_addresses". It is a list of the IPs of members of this database cluster. Select the single IP from the list. You will need this value to seed our Load Balancer deployment with backends.

![Deployments Outputs: Database][database-deployments-outputs]


## Install Load Balancer

Our load balancer is hosted in AWS. It exposes the database deployment as a service to the external network. The blueprint that we use for the demo uses HAProxy.

To install the load balancer, upload the load balancer blueprint. [Right-click on this link](https://github.com/cloudify-examples/haproxy-blueprint/archive/e2e.zip) to copy the URL. Paste the URL in the _URL_ field. Provide a _Blueprint Name_, such as `lb`, choose `aws.yaml` from the _Blueprint filename_ menu and click **Upload**.

![Upload Blueprints: Load Balancer][upload-blueprints-loadbalancer]

Click **Deploy** to fill out the deployments create form.

Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `lb`.
  * The input `network_deployment_name` should be set to your AWS network deployment name, which above we called `aws`.
  * The input `application_ip` should be set to the single IP of the database deployment `cluster_addresses`.

![Create Deployments: Form][loadbalancer-create-form]

On the left navigation menu, select Deployments.

Then execute the install workflow for the `lb` deployment.

In the meantime, explore the deployment.

When the install workflow has finished, continue to install our front end application.


## Install Drupal

Our Drupal front end application is hosted in Openstack. It accesses and stores data in our database hosted in AWS via the load balancer in AWS.

To install the front end application, upload the drupal blueprint. [Right-click on this link](https://github.com/cloudify-examples/drupal-blueprint/archive/e2e.zip) to copy the URL. Paste the URL in the _URL_ field. Provide a _Blueprint Name_, such as `drupal`, choose `openstack.yaml` from the _Blueprint filename_ menu and click **Upload**.

![Upload Blueprints: Drupal][upload-blueprints-drupal]

Click **Deploy** to fill out the deployments create form.

Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `drupal`.
  * The input `db_deployment` should be set to the name of the database deployment.
  * The input `lb_deployment` should be set to the name of the load balancer deployment.
  * The input `image` should be set to your Openstack Ubuntu 14 image, which should be `05bb3a46-ca32-4032-bedd-8d7ebd5c8100`.
  * The input `network_deployment_name` should be set to your AWS network deployment name, which above we called `openstack`.

![Create Deployments: Form][drupal-create-form]

On the left navigation menu, select Deployments.

Then execute the install workflow for the `drupal` deployment.

In the meantime, explore the deployment. To access the Drupal front end, you will need to setup your lab's VPN.

_There are Drupal blueprints for AWS, Azure, and Openstack. Any of them can be used in this section of the demo._


## Install Wordpress

Our next front end application in this demo is a Wordpress application. It is served in Kubernetes. It accesses and stores data in our database hosted in AWS via the load balancer in AWS, same as the Drupal front end.

To install the front end application, upload the wordpress blueprint. [Right-click on this link](https://github.com/EarthmanT/db-lb-app/archive/e2e.zip) to copy the URL. Paste the URL in the _URL_ field. Provide a _Blueprint Name_, such as `wordpress`, choose `blueprint.yaml` from the _Blueprint filename_ menu and click **Upload**.

![Upload Blueprints: Wordpress][upload-blueprints-wordpress]

Click **Deploy** to fill out the deployments create form.

Fill out the deployments form. For `Deployment name`, use the same name as the blueprint, for example `wordpress`.
  * The input `db_deployment` should be set to the name of the database deployment if above you changed it from the default value `db`.
  * The input `lb_deployment` should be set to the name of the database deployment if above you changed it from the default value `lb`.

![Create Deployments: Form][wordpress-create-form]

On the left navigation menu, select Deployments.

Then execute the install workflow for the `wordpress` deployment.

In the meantime, explore the deployment. To access the Wordpress front end, you will need to setup your lab's VPN.


[blueprints-nav]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-nav.png "Left Navigation Menu"
[kubernetes-upload-blueprint]: https://github.com/EarthmanT/e2e/raw/final/images/kubernetes-upload-blueprint.png "Upload Blueprint"
[kubernetes-create-deployment]: https://github.com/EarthmanT/e2e/raw/final/images/kubernetes-create-deployment.png "Create Deployment"
[create-tenant-nav]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-nav.png "Left Navigation Menu"
[create-tenant-section]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-section.png "Create Tenant Panel"
[create-tenant-form]: https://github.com/EarthmanT/e2e/raw/final/images/create-tenant-form.png "Create Tenant Form"
[select-tenant]: https://github.com/EarthmanT/e2e/raw/final/images/select-tenant.png "Select Tenant"
[add-secrets-nav]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-nav.png "Left Navigation Menu"
[add-secrets-panel]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-panel.png "Add Secrets Panel"
[add-secrets-form]: https://github.com/EarthmanT/e2e/raw/final/images/add-secrets-form.png "Add Secrets Form"
[upload-plugins-panel]: https://github.com/EarthmanT/e2e/raw/final/images/upload-plugins-panel.png "Upload Plugins Panel"
[upload-plugins-form]: https://github.com/EarthmanT/e2e/raw/final/images/upload-plugins-form.png "Upload Plugins Form"
[blueprints-form]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-form.png "Upload Blueprints Form"
[blueprints-panel]: https://github.com/EarthmanT/e2e/raw/final/images/blueprints-panel.png "Upload Blueprints Panel"
[deployments-nav]: https://github.com/EarthmanT/e2e/raw/final/images/deployments-nav.png "Left Navigation menu"
[deployments-panel]: https://github.com/EarthmanT/e2e/raw/final/images/deployments-panel.png "Create Deployment"
[deployments-install]: https://github.com/EarthmanT/e2e/raw/final/images/deployments-install.png "Install Deployment"
[upload-blueprints-database]: https://github.com/EarthmanT/e2e/raw/final/images/upload-blueprints-database.png "Upload Database"
[database-create-panel]: https://github.com/EarthmanT/e2e/raw/final/images/database-create-panel.png "Create Database Panel"
[database-create-form]: https://github.com/EarthmanT/e2e/raw/final/images/database-create-form.png "Create Database"
[database-deployments-outputs]: https://github.com/EarthmanT/e2e/raw/final/images/database-deployments-outputs.png "Database Outputs"
[upload-blueprints-loadbalancer]: https://github.com/EarthmanT/e2e/raw/final/images/upload-blueprints-loadbalancer.png "Upload LB"
[loadbalancer-create-form]: https://github.com/EarthmanT/e2e/raw/final/images/loadbalancer-create-form.png "Create LB"
[upload-blueprints-drupal]: https://github.com/EarthmanT/e2e/raw/final/images/upload-blueprints-drupal.png "Upload Drupal"
[drupal-create-form]: https://github.com/EarthmanT/e2e/raw/final/images/drupal-create-form.png "Create Drupal"
[upload-blueprints-wordpress]: https://github.com/EarthmanT/e2e/raw/final/images/upload-blueprints-wordpress.png "Upload Wordpress"
[wordpress-create-form]: https://github.com/EarthmanT/e2e/raw/final/images/wordpress-create-form.png "Create Wordpress"


