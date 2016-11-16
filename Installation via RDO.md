RDO is a community of people using and deploying OpenStack on CentOS, Fedora, and Red Hat Enterprise Linux. 

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo.png)

RDO直接在CentOS上安装，无需虚拟机，可以直接进行All-in-One的安装。

##第一步 安装：RDO Quickstart

Deploying RDO is a quick and easy process. Setting up an OpenStack cloud takes approximately 15 minutes, and can be as short as 3 steps.

Below, we'll explain how to set up OpenStack on a single server. You'll be able to add more nodes to your OpenStack cloud later, if you choose.

If you just want to try it out without installing anything, check out TryStack. See also Installation for alternate deployment methods.

These instructions are to install the current ("Liberty") release.

###Summary for the Impatient

`sudo yum update -y`

`sudo yum install -y https://www.rdoproject.org/repos/rdo-release.rpm`

`sudo yum install -y openstack-packstack`

`packstack --allinone`

###Step 0: Prerequisites

**Software**: Red Hat Enterprise Linux (RHEL) 7 is the minimum recommended version, or the equivalent version of one of the RHEL-based Linux distributions such as CentOS, Scientific Linux, etc.x86_64 is currently the only supported architecture. See also RDO repository info for details on required repositories. Please name the host with a fully qualified domain name rather than a short-form name to avoid DNS issues with Packstack.

**Hardware**: Machine with at least 4GB RAM, processors with hardware virtualization extensions, and at least one network adapter.

###Step 1: Software repositories

Update your current packages:

`sudo yum update -y`

Setup the RDO repositories:

`sudo yum install -y https://rdoproject.org/repos/rdo-release.rpm`

Looking for an older version? See http://rdoproject.org/repos/ for the full listing.

###Step 2: Install Packstack Installer

`sudo yum install -y openstack-packstack`

###Step 3: Run Packstack to install OpenStack

Packstack takes the work out of manually setting up OpenStack. For a single node OpenStack deployment, run the following command.

`packstack --allinone`

If you encounter failures, see the Workarounds page for tips.

If you have run packstack previously, there will be a file in your home directory named something like packstack-answers-20130722-153728.txt You will probably want to use that file again, using the –answer-file option, so that any passwords you've already set (e.g.: mysql) will be reused.

The installer will ask you to enter the root password for each host node you are installing on the network, to enable remote configuration of the host so it can remotely configure each node using Puppet.

Once the process is complete, you can log in to the OpenStack web interface "Horizon" by going to http://$YOURIP/dashboard. The username is "admin". The password can be found in the filekeystonerc_admin in the /root/ directory of the control node.

###Next Steps

Now that your single node OpenStack instance is up and running, you can read on about running an instance, configuring a floating IP range, configuring RDO to work with your existing network, or about expanding your installation by adding a compute node.

From <https://www.rdoproject.org/install/quickstart/> 

##第二步 配置：安装一个Fedora虚拟机实例

Running an Instance

###Step 1: Visit the Dashboard

Log in to the Openstack dashboard at http://CONTROL_NODE/dashboard - the username is "demo". The password can be found in the file keystonerc_demo in the /root/ directory of the control node.

**Note: make sure you use the "demo" username here.**

###Step 2: Enable SSH on your default security group.

Once logged in to the OpenStack dashboard, click the "Project" tab in the left-side navigation menu, and then click "Access & Security" under the heading "Compute."

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step2_1.png)

Under the "Security Groups" heading, click the "Manage Rules" button for the "default" security group. Click the "Add Rule" button, and in the resulting dialog, enter "22" in the "Port" field, and then click the "Add" button.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step2_2.png)

###Step 3: Create or import a key pair.

In the left-side navigation menu, click "Access & Security" under the heading "Compute." In the main portion of the screen, click the tab labeled "Key Pairs," and choose either to "Create Key Pair" or "Import Key Pair." The "Create Key Pair" dialog will prompt you to supply a keypair name before downloading a private key to your client.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step3_1.png)

The "Import Key Pair" option will prompt you to provide a name and a public key to use with an existing private key on your client. For name, choose something to identify that key (like your username, for example) and for key, use the contents of your public key file, usually in ~/.ssh/id_rsa.pub or ~/.ssh/id_dsa.pub on the machine from which you will be ssh-ing in.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step3_2.png)

###Step 4: Add an image.

In the left-side navigation menu, click "Images" under the heading "Compute." Click the "Create Image" button, located in the upper-right portion of the screen. In the resulting dialog box, enter "Fedora22" in the "Name" field, "https://download.fedoraproject.org/pub/fedora/linux/releases/22/Cloud/x86_64/Images/Fedora-Cloud-Base-22-20150521.x86_64.qcow2" in the "Image Location" field, choose "QCOW2" from the "Format" drop-down menu, leave the "Minimum Disk" and "Minimum Ram" fields blank, leave the "Public" box unchecked, and click the "Create Image" button.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step4.png)

For a collection of links to alternative cloud-ready images, check out image resources.

###Step 5: Launch the instance.

In the main portion of the screen, under the "Images" heading, click the "Launch Instance" button for the "Fedora22" image. In the resulting dialog, provide a name in the "Instance Name" field and select "m1.small" in the "Flavor" field.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step5_1.png)

You have to assign a network, under "Networking" tab, either click on the "+" next to "private" or drag & drop the "private" box from "Available networks" to "Selected networks". Finally, click the "Launch" button.

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step5_2.png)

### Step 6: Associate Floating IP

In the main portion of the screen, under the "Instances" heading, click the down arrow button under "Actions" column for your instance you just launched, followed by the "Associate Floating IP". Click on "+" next to "IP Address" and select the "public" Pool in the "Allocate Floating IP" dialog, continue by clicking "Allocate IP". Being back in the "Manage Floating IP Associations" dialog you can select the allocated IP Address and click "Associate".

![](https://raw.githubusercontent.com/luckyyd/OpenStack-ToolsTeam/master/res/Rdo_step6.png)

The associated Floating IPs can be spotted in the "IP Address" for each instance.

For additional details, please read how to set a floating IP range.

###Step 7: SSH to Your Instance
Using the key pair file from step 3, ssh into the running vm using its floating ip address:

`$ ssh -i my_key_pair.pem fedora@floating_ip_address`

From <https://www.rdoproject.org/install/running-an-instance/> 








