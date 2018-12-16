# SDN-Opendaylight-Shell-2G
[![Dependency Status](https://dependencyci.com/github/QualiSystems/SDN-Opendaylight-Shell-2G/badge)](https://dependencyci.com/github/QualiSystems/SDN-Opendaylight-Shell-2G)
[![Stories in Ready](https://badge.waffle.io/QualiSystems/cloudshell-sdn-odl.svg?label=ready&title=Ready)](http://waffle.io/QualiSystems/SDN-Opendaylight-Shell-2G)

![](https://github.com/QualiSystems/cloudshell-shells-documentaion-templates/blob/master/cloudshell_logo.png)

# **SDN OpenDaylight 2G Shell**  

Release date: October 2017

Shell version: 1.0.0

Document version: 1.0

# In This Guide

* [Overview](#overview)
* [Downloading the Shell](#downloading-the-shell)
* [Importing and Configuring the Shell](#importing-and-configuring-the-shell)
* [Updating Python Dependencies for Shells](#updating-python-dependencies-for-shells)
* [Typical Workflows](#typical-workflows)
* [References](#references)
* [Release Notes](#release-notes)


# Overview
A shell integrates a device model, application or other technology with CloudShell. A shell consists of a data model that defines how the device and its properties are modeled in CloudShell, along with automation that enables interaction with the device via CloudShell.

### Networking Shells
CloudShell's networking shells provide L2 or L3 connectivity between resources.

### **SDN OpenDaylight 2G Shell**
The **SDN OpenDaylight 2G** shell provides you with connectivity and management capabilities such as device structure discovery and power management for the **SDN OpenDaylight**. 

For more information on the **SDN OpenDaylight**, see the official **OpenDaylight** product documentation.

### Standard version
The **SDN OpenDaylight 2G** shell is based on the Networking Shell Standard version **1.0.0**.

For detailed information about the shell’s structure and attributes, see the [Networking Shell Standard](https://github.com/QualiSystems/cloudshell-standards/blob/master/Documentation/networking_standard.md) in GitHub.

### Requirements

Release: **SDN OpenDaylight 2G** shell

▪ CloudShell version: 8.0 and above

▪ OpenDaylight versions: Boron SR3

**Note:** If your CloudShell version does not support this shell, you should consider upgrading to a later version of CloudShell or contact customer support.

### Data Model

The shell's data model includes all shell metadata, families, and attributes.

#### **SDN OpenDaylight 2G Families and Models**

The SDN OpenDaylight families and models are listed in the following table:

|Family|Model|Description|
|:---|:---|:---|
|CS_SDNController|SDN Controller|Generic SDN Controller|
|CS_SDNSwitch|Generic SDN Switch|Generic SDN Switch|
|CS_Port|Generic SDN Port|Interface|

#### **SDN OpenDaylight Attributes**

The attribute names and types are listed in the following table:

|Attribute|Type|Description|
|:---|:---|:---|
|User|String|ODL controller username.|
|Password|Password|ODL controller password.|
|Controller TCP Port|Numeric|ODL controller port. Default value is 8181.|
|Scheme|String|ODL controller URL scheme (http or https).|
|Enable Full Trunk Ports|String|(Optional) Used when configuring a full trunk port, for example: ports that a private cloud provider are connected to. Ports should be listed in this format: *openflow:1::eth1;openflow:eth2*.|
|Disable Full Trunk Ports|String|(Optional) Used when removing a full trunk port configuration. Ports should be listed in this format: *openflow:1::eth1;openflow:eth2*.|
|Model Name|String|This attribute will be displayed in CloudShell instead of the CloudShell model.|
|Mac Address|String|MAC address of the network interface.|
|IPv4 Address|String|IPv4 address of the network interface.|
|IPv6 Address|String|IPv6 address of the network interface.|
|Port Description|String|Port description of the network interface.|
|Adjacent|String|(not used in this shell) Adjacent device (system name) and port, based on LLDP or CDP protocol.|

### Automation
This section describes the automation (drivers) associated with the data model. The shell’s driver is provided as part of the shell package. There are two types of automation processes, Autoload and Resource. Autoload is executed when creating the resource in the **Inventory** dashboard, while resource commands are run in the sandbox.

The following resource commands are available on the **SDN OpenDaylight 2G** Controller shell:

|Command|Description|
|:---|:---|
|Autoload|Discovers connections to the controller's vSwitches and their leaf ports.|
|Remove Openflow|Removes a specific openflow entry from the controller. Input values include: **Node ID (String)**, **Table ID (String)**, and **Flow ID (String)**.|

# Downloading the Shell
The **SDN OpenDaylight 2G** shell is available from the [Quali Community Integrations](https://community.quali.com/integrations) page. 

Download the files into a temporary location on your local machine. 

The shell comprises:

|File name|Description|
|:---|:---|
|SDNOpenDaylightShell2G.zip|SDN OpenDaylight shell package|
|cloudshell-sdn-odl-2-gendependencies-package-1.0.X.zip|Shell Python dependencies (for offline deployments only)|

The *ODL_CloudShell_plugin.zip* file, SDN OpenDaylight Controller plugin, can be found [here](https://github.com/QualiSystems/ODL_connectivity_plugin/releases).

# Importing and Configuring the Shell
This section describes how to import the **SDN OpenDaylight 2G** shell and configure and modify the shell’s devices.

### Importing the shell into CloudShell

**To import the shell into CloudShell:**
  1. Make sure you have the shell’s zip package. If not, download the shell from the [Quali Community's Integrations](https://community.quali.com/integrations) page.
  
  2. In CloudShell Portal, as Global administrator, open the **Manage – Shells** page.
  
  3. Click **Import**.
  
  4. In the dialog box, navigate to the shell's zip package, select it and click **Open**.

The shell is displayed in the **Shells** page and can be used by domain administrators in all CloudShell domains to create new inventory resources, as explained in [Adding Inventory Resources](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/INVN/Add-Rsrc-Tmplt.htm?Highlight=adding%20inventory%20resources). 

### Installing the CloudShell OpenDaylight plugin
This section describes how to install the *CloudShell SDN OpenDaylight* plugin. 

There are two available options (all these steps should be executed on the dedicated server, with the specific environment):

▪ **Install the controller and plugin from source:**

1. Download the *ODL_CloudShell_plugin_zip* file, see [Downloading the Shell](#downloading-the-shell).
	
2. Install **Java** and **Maven**: https://wiki.opendaylight.org/view/GettingStarted:Development_Environment_Setup
	
3. Compile the controller: **mvn clean install -DskipTests**.
	
4. Run the controller: **./karaf/target/assembly/bin/karaf**.
	
▪ **Add the plugin feature to the existing controller:**

1. Copy the CloudShell plugin files to the *system* folder. The system folder refers to the system folder for the karaf distribution located in the OpenDaylight controller. 
	
2. Run the controller: **./karaf/target/assembly/bin/karaf**.
	
3. Install the VTN feature: **feature:install odl-vtn-manager-rest**.
	
4. Add the repository for the CloudShell plugin: **repo-add mvn:quali/cloudshellfeatures/0.1.0-SNAPSHOT/xml/features**.
	
5. Install the plugin: **feature:install odl-cloudshell**. 


### Offline installation of a shell

**Note:** Offline installation instructions are relevant only if CloudShell Execution Server has no access to PyPi. You can skip this section if your execution server has access to PyPi. For additional information, see the online help topic on offline dependencies.

In offline mode, import the shell into CloudShell and place any dependencies in the appropriate dependencies folder. The dependencies folder may differ, depending on the CloudShell version you are using:

* For CloudShell version 8.3 and above, see [Adding Shell and script packages to the local PyPi Server repository](#adding-shell-and-script-packages-to-the-local-pypi-server-repository).

* For CloudShell version 8.2, perform the appropriate procedure: [Adding Shell and script packages to the local PyPi Server repository](#adding-shell-and-script-packages-to-the-local-pypi-server-repository) or [Setting the python pythonOfflineRepositoryPath configuration key](#setting-the-python-pythonofflinerepositorypath-configuration-key).

* For CloudShell versions prior to 8.2, see [Setting the python pythonOfflineRepositoryPath configuration key](#setting-the-python-pythonofflinerepositorypath-configuration-key).

### Adding shell and script packages to the local PyPi Server repository
If your Quali Server and/or execution servers work offline, you will need to copy all required Python packages, including the out-of-the-box ones, to the PyPi Server's repository on the Quali Server computer (by default *C:\Program Files (x86)\QualiSystems\CloudShell\Server\Config\Pypi Server Repository*).

For more information, see [Configuring CloudShell to Execute Python Commands in Offline Mode](http://help.quali.com/Online%20Help/9.0/Portal/Content/Admn/Cnfgr-Pyth-Env-Wrk-Offln.htm?Highlight=Configuring%20CloudShell%20to%20Execute%20Python%20Commands%20in%20Offline%20Mode).

**To add Python packages to the local PyPi Server repository:**
  1. If you haven't created and configured the local PyPi Server repository to work with the execution server, perform the steps in [Add Python packages to the local PyPi Server repository (offline mode)](http://help.quali.com/Online%20Help/9.0/Portal/Content/Admn/Cnfgr-Pyth-Env-Wrk-Offln.htm?Highlight=offline%20dependencies#Add). 
  
  2. For each shell or script you add into CloudShell, do one of the following (from an online computer):
      * Connect to the Internet and download each dependency specified in the *requirements.txt* file with the following command: 
`pip download -r requirements.txt`. 
     The shell or script's requirements are downloaded as zip files.

      * In the [Quali Community's Integrations](https://community.quali.com/integrations) page, locate the shell and click the shell's **Download** link. In the page that is displayed, from the Downloads area, extract the dependencies package zip file.

3. Place these zip files in the local PyPi Server repository.
 
### Setting the python PythonOfflineRepositoryPath configuration key
Before PyPi Server was introduced as CloudShell’s python package management mechanism, the `PythonOfflineRepositoryPath` key was used to set the default offline package repository on the Quali Server machine, and could be used on specific Execution Server machines to set a different folder. 

**To set the offline python repository:**
1. Download the *cloudshell-sdn-odl-2-gendependencies-package-1.0.X.zip* file, see [Downloading the Shell](#downloading-the-shell).

2. Unzip it to a local repository. Make sure the execution server has access to this folder. 

3.  On the Quali Server machine, in the *~\CloudShell\Server\customer.config* file, add the following key to specify the path to the default python package folder (for all Execution Servers):  
	`<add key="PythonOfflineRepositoryPath" value="repository 
full path"/>`

4. If you want to override the default folder for a specific Execution Server, on the Execution Server machine, in the *~TestShell\Execution Server\customer.config* file, add the following key:  
	`<add key="PythonOfflineRepositoryPath" value="repository 
full path"/>`

5. Restart the Execution Server.

### Configuring a new resource
This section explains how to create a new resource from the shell.

In CloudShell, the component that models the device is called a resource. It is based on the shell that models the device and allows the CloudShell user and API to remotely control the device from CloudShell.

You can also modify existing resources, see [Managing Resources in the Inventory](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/INVN/Mng-Rsrc-in-Invnt.htm?Highlight=managing%20resources).

**To create a resource for the device:**
  1. In the CloudShell Portal, in the **Inventory** dashboard, click **Add New**. 
     ![](https://github.com/QualiSystems/cloudshell-shells-documentaion-templates/blob/master/create_a_resource_device.png)
     
  2. From the list, select **SDN Opendaylight 2G** shell.
  
  3. Enter the **Name** and **IP address** of the **Switch**.
  
  4. Click **Create**.
  
  5. In the **Resource** dialog box, enter the device's settings. 
  
  6. Click **Continue**.

CloudShell validates the device’s settings and updates the new resource with the device’s structure.

# Updating Python Dependencies for Shells
This section explains how to update your Python dependencies folder. This is required when you upgrade a shell that uses new/updated dependencies. It applies to both online and offline dependencies.

### Updating offline Python dependencies
**To update offline Python dependencies:**
1. Download the latest Python dependencies package zip file locally.

2. Extract the zip file to the suitable offline package folder(s). 

3. Terminate the shell’s instance, as explained [here](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/MNG/Mng-Exctn-Srv-Exct.htm#Terminat). 

### Updating online Python dependencies
In online mode, the execution server automatically downloads and extracts the appropriate dependencies file to the online Python dependencies repository every time a new instance of the driver or script is created.

**To update online Python dependencies:**
* If there is a live instance of the shell's driver or script, terminate the shell’s instance, as explained [here](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/MNG/Mng-Exctn-Srv-Exct.htm#Terminat). If an instance does not exist, the execution server will download the Python dependencies the next time a command of the driver or script runs.

# Typical Workflows 

#### Workflow - *Remove Openflow* 

1. Login to CloudShell portal, reserve the **SDN OpenDaylight** resource.

2. Run the resource command **Remove Openflow**.

3. In the command input field, enter the following information (all are mandatory): 

	* **Node ID**: Enter the node ID, for example: openflow:1.
	* **Table ID**: Enter the table ID where openflow is located. 
	* **Flow ID**: Enter the openflow identifier.

# References
To download and share integrations, see [Quali Community's Integrations](https://community.quali.com/integrations). 

For instructional training and documentation, see [Quali University](https://www.quali.com/university/).

To suggest an idea for the product, see [Quali's Idea box](https://community.quali.com/ideabox). 

To connect with Quali users and experts from around the world, ask questions and discuss issues, see [Quali's Community forums](https://community.quali.com/forums). 

# Release Notes 

### What's New

For release updates, see the shell's [GitHub releases page](https://github.com/QualiSystems/SDN-Opendaylight-Shell-2G/releases).

