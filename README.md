# ansible-dcos-aws-playbook
Ansible Playbooks for setting up DC/OS on AWS. Default configuration of three masters, three slaves and two public slaves behing an AWS elastic load-balancer on a three subnet VPC, two public subnets on different availiability zones and one private subnet.

To try it out do:

1. Configure `~/.boto` as described in [Boto Config](http://boto.readthedocs.org/en/latest/boto_config_tut.html):

   ```
   [Credentials]
   aws_access_key_id = <your_access_key_here>
   aws_secret_access_key = <your_secret_key_here>
   ```
1. Add your `*.pem` file to the `ssh-agent`:

   ```
   $ ssh-agent bash 
   $ ssh-add ~/.ssh/<amazon key file> 
   ```
1. Define the environment variables (see below). The AWS variables are mandatory.
1. Run the playbook as `ansible-playbook -i ec2.py main.yml` to set up the EC2 instances, VPC, subnets, ELB etc.
  1. [boto3](https://pypi.python.org/pypi/boto3) must be installed.
1. Run the playbook as `ansible-playbook -i ec2.py provision_cluster.yml` to provision the instances with DC/OS nodes.

## Environment variables
The following environment varibles can be configured for Ansible lookup

### AWS
* AWS_EC2_KEY_PAIR: the name of the key pair to use when provisioning the cluster (EC2 instances etc.).
* AWS_EC2_LB_CERT: the `arn` resource for the AWS ELB certificate.
* AWS_S3_ELB_LOGS_BUCKET_NAME: the name of the S3 bucket where to store the ELB logs.

### DC/OS
* DCOS_CONTROLLER_INSTALL_DEMOS: `true` - install demo(s) (default), `false` - skip demo installation.

#### Demos
* DCOS_CONTROLLER_INSTALL_DEMO_SD_AND_LB_NGINX: `true` - install demo setting up Nginx as described in [Service discovery and load balancing with DCOS and marathon-lb: Part 1](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/).
* DCOS_CONTROLLER_INSTALL_TUTUM_HELLO_WORLD: `true` - install [Tutum hello-world](https://github.com/tutumcloud/hello-world) demo.

