# End-to-end Solutions Package

This End-to-end solutions package uses novel applications to demonstrate Cloudify functionality. It is intended as a sales aid.

## About

The solutions package tells the story of an organization with a private Openstack cloud and a public AWS cloud. You will reenact this story in the following demo script.


There are several steps:

* [Preparation](#preparation)
  * [Create Lab](#create-lab)
  * [Add Demo Tenant](#add-demo-tenant)
  * [Add Secrets](#add-secrets)
  * [Install Plugins](#install-plugins)
  * [Install Kubernetes](#install-kubernetes)
  * [Create Example Networks](#create-example-network)
* [Demo](#demo)
  * [Install Database](#install-database)
  * [Install Load Balancer](#install-load-balancer)
  * [Install Drupal CMS](#install-drupal)
  * [Install Wordpress CMS](#install-wordpress)
  *

# Preparation

These are steps that you need to follow in order to run the demo.


## Create Lab

See [Create New Lab](http://labs.cloudify.co/).


## Add Demo Tenant

To create a tenant, select "Tenant Management" from the left navigation menu.

![Create Tenant: Left Navigation Menu][create-tenant-nav]


Locate the "Tenants Management" pane. Click **Add**.

![Create Tenant: Pane][create-tenant-section]


Provide a name for your demo tenant, and click **Add**.

![Create Tenant: Form][create-tenant-form]


## Add Secrets

Cloudify stores your Openstack and AWS credentials as secrets.

  * In the lab environment, the Openstack credentials are already stored as secrets.
  * The user should manually add their AWS credentials, by creating the following secrets:
    * `aws_access_key_id`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `aws_secret_access_key`: See [answer](https://stackoverflow.com/questions/21440709/how-do-i-get-aws-access-key-id-for-amazon).
    * `ec2_region_name`: See _Region_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `ec2_region_endpoint` See _Endpoint_ column in [Amazon EC2 Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region).
    * `availability_zone` See _AZ Names_ in [AWSRegionsAndAZs](https://gist.github.com/neilstuartcraig/0ccefcf0887f29b7f240).


## Install Plugins

The blueprints that are installed in the following steps require these plugins. For each link listed below, copy the URL and paste in the upload plugins.


## Create Example Networks


## Install Kubernetes


# Demo


## Install Database


## Install Load Balancer


## Install Drupal


## Install Wordpress


[create-tenant-nav]: https://github.com/EarthmanT/e2e/raw/master/images/create-tenant-nav.png "Left Navigation Menu"
[create-tenant-section]: https://github.com/EarthmanT/e2e/raw/master/images/create-tenant-section.png "Create Tenant Section"
[create-tenant-form]: https://github.com/EarthmanT/e2e/raw/master/images/create-tenant-form.png "Create Tenant Form"
