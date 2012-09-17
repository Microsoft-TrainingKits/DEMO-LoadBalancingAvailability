<a name="title" />
# Load Balancing and Availability Sets #

---

<a name="Overview" />
## Overview ##
In this demonstration, you will see how to configure the load balancer and availability sets to have a highly scalable and available application.

> **Note:** In order to run through this complete demo, you must have network connectivity and a Microsoft account.

<a id="goals" />
### Goals ###
In this demo, you will see three things:

1.	How to configure the Windows Azure Load Balancer
2.	How to configure Windows Azure Availability Sets

<a name="technologies" />
### Key Technologies ###

This demo uses the following technologies:

- [Windows Azure Management Portal] [1]

[1]: https://manage.windowsazure.com/

<a name="setup" />
### Setup and Configuration ###

1. Create two **Windows Server 2012** Virtual Machines in the same cloud service (Connect to existing VM in the portal)
1. Configure the **Web Role** on both nodes.
1.	Modify the **IISStart.html** page to show the computer name in large text in the main body.

	````HTML
	<div><h3>servername</h3><br />
	````

---

<a name="Demo" />
## Demo ##

This demo is composed of the following segments:

1. [Configuring the Load Balancer](#segment1)
1. [Configuring Availability Sets](#segment2)

<a name="segment1" />
### Configuring the Load Balancer ###

1. Open the Windows Azure Management Portal.

1. Click on the first web server you provisioned in setup.

1. Click on the endpoints button at the top of your screen and click **NEW endpoint** at the bottom.

	Endpoints allow for traffic to the virtual machine. 

	![Add endpoint to VM](Images/add-endpoint-to-vm.png?raw=true "Add endpoint to VM")
	
	_Add endpoint to VM_

	> **Speaking Point**: Note how the traffic is directed to the public port and redirected internally to the private port. This is called port forwarding.

1. Name the endpoint web, protocol tcp, 80 and 80.

	![Endpoint details](Images/endpoint-details.png?raw=true "Endpoint details")

	_Endpoint details_

1. Wait… (45secs - over a minute)

	> **Speaking Point**: Endpoint configuration is serialized so once you add an endpoint to the first VM you must wait to add it to the 2nd until the first updated is completed.

	> The second endpoint is associated with the first to make the traffic load balanced for that specific port (80)

1. Add an endpoint to the 2nd VM. This time you will select load balance an existing endpoint


	![Adding the Second Endpoint](Images/adding-the-second-endpoint.png?raw=true "Adding the Second Endpoint")

	_Adding the Second Endpoint_

1. Fill in the same details

	![Second endpoint details](Images/second-endpoint-details.png?raw=true "Second endpoint details")

	_Second endpoint details_

1. Wait… (45secs - over a minute)

1.	Browse to the site <http://sitename.cloudapp.net/iisstart.htm>.

1. Use **CTRL-F5** to refresh and note the server name changes.

	![Browsing the server](Images/browsing-the-server.png?raw=true "Browsing the server")

	_Browsing the server_

<a name="segment2" />
### **Configuring Availability Sets** ###

> **Speaking Point**: Availability Sets tell the Windows Azure fabric controller to put different vms performing the same tasks on different racks in the data center.

> It also puts the VMs in separate upgrade domains so when the fabric controller performs host updates all of the VMs in the same set are never brought down at the same time.

1. Open the first web server in the portal.
1. Click configure and under availability set select **Create Availability Set**.

1. Enter "webavset".

	![Creating Availability Set](Images/creating-availability-set.png?raw=true "Creating Availability Set")

	_Creating Availability Set_

1. Click **Save**.

1. Wait 30-seconds to 1 minute for update.

1. Refresh browser to see changes faster.

1. Open the second web server in the portal.

1. Click configure and under availability set select **webavset** and click **Save**.

	![Configuring Availability Set](Images/configuring-availability-set.png?raw=true "Configuring Availability Set")

	_Configuring Availability Set_

---

<a name="summary" />
## Summary ##

In this demonstration, you learned how to configure two servers for load balancing and configured for availability sets so your virtual machines are highly in the event of a rack failure or a host update in the Windows Azure Data Centers.