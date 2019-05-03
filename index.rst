.. title:: Nutanix .Next Prism Pro HOL


.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  tools_vms/linux_tools_vm

.. _xplay:

------------------------
Prism Pro
------------------------

*The estimated time to complete this lab is 60 minutes.*

Overview
++++++++

Prism Pro is a product designed to make our customer IT operations smarter and automated. Today, there is no solution that is specifically designed for IT operations for the data center built around HCI. The infrastructure in this type of data center is dynamic, scalable, and highly performed. The traditional performance monitoring and IT OPS tools are built for the static infrastructure. When the IT admins use the traditional tool to manage HCI environment, they are overwhelmed by the complexity and noisy signal the tool brings. This decreases the productivity of the operations and reduces the ROI from adopting the HCI.

Prism Pro takes a unique approach that maximizes the operation efficiency of an HCI based data center. First, Prism Pro uses purpose-built machine learning (X-FIT) to extract the insights from the mass amount of operations data the HCI produces. The first three use cases Prism Pro shipped are capacity forecast and planning, VM right sizing, and anomaly detection. These use cases help our customer detect problems and waste with the actionable signal. Second, Prism Pro delivers an automation mechanism (X-Play) that enables customers to automate their operations tasks confidently to respond to the signal X-FIT detects.

X-Play is designed to address the number 1 pain point when customers deal with automation - the fear of amplified impact because of the complexity of the automation. Not like the solution, such as Calm,  for the application lifecycle automation, X-Play’s goal is to automate the tasks that admins face daily. To eliminate the fear and give the control back to the admin, X-Play takes the codeless approach which has been proven in the companies such as IFTTT and Zapier that it is easy to adopt and extremely versatile.

There are no other tools in the market taking this approach and has the power to combine intelligence and codeless automation. The power of X-FIT and X-Play allows the customer to truly leverage the machine data the HCI infrastructure produces and operate it efficiently, confidently, and intelligently.

Takeaways
.........

#. Prism Pro is our solution to make IT OPS smarter and automated. It covers the IT OPS process ranging from intelligent detection to automated remediation.
#. X-FIT is our machine learning engine to support smart IT OPS, including forecast, anomaly detection, and inefficiency detection.
#. X-Play, the IFTTT for the enterprise, is our engine to enable the automation of daily operations tasks.
#. X-Play enables admins to confidently automate their daily tasks within minutes.
#. X-Play is extensive that can use customer’s existing APIs and scripts as part of its playbooks.


Lab Setup
+++++++++

This lab requires a VM to be provisioned and will be stressed latter in the lab to produce CPU and memory metrics.

#. Open Google Chrome and log in to the Prism Central UI, if not done so already.
#. Navigate using the Hamburger menu to Virtual Infrastructure > VMs and click on the List view.
#. Click the Create VM button.

   .. figure:: images/ppro_01.png

#. Name it ‘PrismProVM’ this is important for some of our setup scripts to run properly, so make sure it matches exactly.

   .. figure:: images/ppro_02.png

#. Add two 2 VCPUs, 1 core per VCPU, 2GB memory.

   .. figure:: images/ppro_03.png

#. Click the ‘Add New Disk’ button. Choose to Clone from the Image service and select the Linux_ToolsVM.qcow2 image, then click ‘Add’

   .. figure:: images/ppro_62.png

#. Select the ‘Add a NIC’ button. Use the 'Primary' VLAN and click Add.

   .. figure:: images/ppro_05.png

#. Now Save the VM and Power it on once it is created.

   .. figure:: images/ppro_06.png


#. Right click the following URL to open a new tab and navigate to the webpage at http://10.42.247.70:3000/ and enter your Prism Central IP Address. This will get your environment ready for this lab. **Keep this tab open during entire Prism Pro lab to return to as directed in later portions**

   .. figure:: images/ppro_63.png

#. After hitting continue, it will take a bit of time for the setup to complete. In the meantime, switch back to Prism Central and go through Story 1 through 5.

#. With the Prism Central page open in the current Google Chrome window, click on the ‘Enable X-Play Tech Preview’ bookmark to enable the X-Play Tech Preview. It will refresh the UI when complete.

   .. figure:: images/ppro_07.png

Lab Story 1 - VM Efficiency
+++++++++++++++++++++++++++

Prism Pro uses X-Fit machine learning to detect the behaviors of VMs running within the managed clusters. Then applies a classification to VMs that are learned to be inefficient. The following are short descriptions of the different classifications:

* **Overprovisioned:** VMs identified as using minimal amounts of assigned resources.
* **Inactive:** VMs that have been powered off for a period of time or that are running VMs that do not consume any CPU, memory, or I/O resources.
* **Constrained:** VMs that could see improved performance with additional resources.
* **Bully:** VMs identified as using an abundance of resources and affecting other VMs.


#. If not already on the Dashboard, open the Hamburger menu and select the Dashboard. From the Dashboard, take a look at the VM Efficiency widget. This widget gives a summary of inefficient VMs that Prism Pro’s X-FIT machine learning has detected in your environment. Click on the ‘View All Inefficeint VMs’ link at the bottom of the widget to take a closer look.

   .. figure:: images/ppro_58.png

#. You are now viewing the Efficiency focus in the VMs list view with more details about why Prism Pro flagged these VMs. You can hover the text in the Efficiency detail column to view the full description.

   .. figure:: images/ppro_59.png

#. Once an admin has examined the list of VM on the efficiency list they can determine any that they wish to take action against. From VMs that have too many or too little resources they will require the individual VMs to be resized. This can be done in a number of ways with a few examples listed below:

* **Manually:** An admin edits the VM configuration via Prism or vCenter for ESXi VMs and changes the assigned resources.
* **X-Play:** Use X-Plays automated play books to resize VM(s) automatically via a trigger or admins direction. There will be a lab story example of this later in this lab.
* **Automation:** Use some other method of automation such as powershell or REST-API to resize a VM.


Lab Story 2 - Anomaly Detection
+++++++++++++++++++++++++++++++

In this lab story you will take a look at VMs with an anomaly. An anomaly is a deviation from the normal learned behavior of a VM. The X-FIT alogrythms learn the normal behavior of VMs and represent that as a baseline range on the different charts for each VM.

#. Now let's take a take a look at a VM by searching for ‘bootcamp_good’ and selecting ‘bootcamp_good_1’.

   .. figure:: images/ppro_61.png


#. Go to Metrics > CPU Usage. Notice the expected range for this item has learned some patterns for the CPU Usage for this VM. Notice a dark blue line, and a lighter blue area around it. The line is the Memory Usage. The light blue area is the expected Memory Usage range for this VM. This range is calculated using Prism Pro’s X-FIT machine learning engine. In this case, an anomaly has been raised for this VM, because the Usage is far below the expected range. You can also reduce the time range “Last 24 hours” to examine the chart more closely.

   .. figure:: images/ppro_60.png

#. Click **“Alert Setting”** to set an alert policy for this kind of situation.

#. In the left hand side, you can change some of the configurations however you would like. In this example I have changed the Behavioral Anomaly threshold to ignore anomalies between 10% and 70%. All other anomalies will generate a Warning alert. I have also adjusted the Static threshold to Alert Critical if the CPU Usage on this VM exceeds 95%.

   .. figure:: images/ppro_25.png

#. Save the policy.




Lab Story 3 - Capacity Planning Runway
++++++++++++++++++++++++++++++++++++++

Capacity runway is a measure of the remaining capacity left within a given cluster or node. There is an overall cluster runway as well as individual runway measurements for CPU, Memory and storage capacity. Lets view the Capacity Runway of your lab cluster.

#. Navigate to ‘Planning’ using the search bar.

   .. figure:: images/ppro_09.png

#. Click on the **‘Capacity Runway’** tab and select the cluster ‘Prism-Pro-Cluster’.

   .. figure:: images/ppro_10.png

   .. figure:: images/ppro_11.png

#. You can now take a look at the Runway for Storage, CPU, and Memory.

   .. figure:: images/ppro_12.png

#. When selecting the Memory tab, you can see a Red Exclamation mark, indicating where this cluster will run out of Memory. You can hover the chart at this point to see on which day this will occur.

   .. figure:: images/ppro_13.png

#. Click on the **‘Optimize Resources’** button on left. This is where you can see the inefficient VMs in the environment with suggestions on how you can optimize these resources to be as efficient as possible.

   .. figure:: images/ppro_14.png

#. Close the optimize resources popup.

#. Under the **‘Adjust Resources’** section in the left side of this page, click the **‘Get Started’** button. We can now use this to start planning for new workloads and see how runway will need to be extended in the future.

#. Click the **add/adjust** button in the left side underneath the ‘Workloads’ item.

   .. figure:: images/ppro_15.png

#. Add one for VDI and select 1000 Users. You can also set a date for when this workload should be added to the system. Save this workload when you are done.

   .. figure:: images/ppro_16.png

   .. figure:: images/ppro_17.png

#. Add another workload of your choice.

#. Now click the **‘Recommend’** button on the right side of the page.

   .. figure:: images/ppro_18.png

#. Once the Recommendation is available, toggle between list and chart view to get a better overview of your Scenario.

   .. figure:: images/ppro_19.png

#. Click the **Generate PDF** button in the upper right hand corner. This will open a new tab with a PDF report for the scenario/workloads you have created.

   .. figure:: images/ppro_19b.png

#. View your report.

   .. figure:: images/ppro_20.png




Lab Story 4 - Increase Constrained VM Memory with X-Play
++++++++++++++++++++++++++++++++++++++++++++++++++++++++

In this lab story we will now create an X-Play to automatically add memory to the lab VM that was created ealier when a memory constraint is detected.

#. Click on the Hamburger menu and choose Operations > Playbooks.

   .. figure:: images/ppro_26.png

#. We will start by creating a Playbook. Click **Create Playbook** at the top of the table view

   .. figure:: images/ppro_27.png

#. Select Alert as a trigger

   .. figure:: images/ppro_28.png

#. Search and select **VM Memory Constrained** as the alert policy, since this is the issue we are looking to take automated steps to remediate.

   .. figure:: images/ppro_29.png

#. We will first need to snapshot the VM. Click **Add Action** on the left side and select the **VM snapshot** action.

   .. figure:: images/ppro_30.png


#. Select **Source Entity** from the parameters link below field. Source entity means the entity triggers the alert.

   .. figure:: images/ppro_31.png

#. Enter a **1** in the Time to Live field.

   .. figure:: images/ppro_32.png

#. Next we would like to remediate the constrained memory by adding more memory to the VM. Click **Add Action** to add the **Hot add memory** action

   .. figure:: images/ppro_33.png

#. Select **source entity** from parameters link for the target VM field, and set rest of field according to the screen below.

   .. figure:: images/ppro_34.png


#. Next we would like to notify someone that an automated action was taken. Click **Add Action** to add the email action

   .. figure:: images/ppro_35.png

#. Fill in the field in the email action. Here are the examples

**Recipient:** use your email email address

**Subject :**
Playbook {{playbook.playbook_name}} addressed alert {{trigger[0].alert_entity_info.name}}

**Message:**
Prism Pro X-FIT detected  {{trigger[0].alert_entity_info.name}} in {{trigger[0].source_entity_info.name}}.  Prism Pro X-Play has run the playbook of "{{playbook.playbook_name}}". As a result, Prism Pro increased 1GB memory in {{trigger[0].source_entity_info.name}}.

The attendee is encouraged to compose their own subject tor message. The above is just an example. They are encouraged to use “parameter” to enrich the message.

Note: there is a bug right now that when you click a parameter in the “parameter” popup, the parameter string will be appended at the end of the text string, not at the place of the cursor. You have to cut and paste it into the correct place if that is the case.

   .. figure:: images/ppro_36.png

#. Click **Add Action** to add the **Acknowledge Alert** action

   .. figure:: images/ppro_37.png

#. Select **Alert** in the parameter link pop up.

   .. figure:: images/ppro_38.png

#. Click **Save & Close** button and save it with a name “Auto Increase Constrained VM Memory” and make sure toggle the **enable button**.

   .. figure:: images/ppro_39.png

#. You should see a new playbook in the “Playbooks” list page.

   .. figure:: images/ppro_40.png

#. Search VM “PrismProVM” and record the current memory capacity. You can scroll down in the properties widget to see the configured memory.

   .. figure:: images/ppro_41.png

#. **Switch tabs back to** the http://10.42.247.70:3000/ page and continue to the Story 1-3 Step.

   .. figure:: images/ppro_66.png

#. Now we will simulate an alert for ‘VM Memory Constrained’ which will trigger the Playbook we just created. Click the ‘Simulate Alert’ button to create the alert.

   .. figure:: images/ppro_64.png

#. Go back to Prism page and check the “PrismProVM” page again, you should now see the memory capacity is increased by 1GB. If the memory does not show updated you can refresh the browser page to speedup the process.

#. You should also receive an email. Check the email to see that its subject and email body have filled the real value for the parameters you set up.

#. Go to the **Playbook** page, click the playbook you just created and click the **disable** button in the upper right corner to disable this playbook

   .. figure:: images/ppro_44.png

#. Click the **Plays** tab, you should see that a play has just completed.

   .. figure:: images/ppro_45.png

#. Click the “Play” to examine the details

   .. figure:: images/ppro_46.png


Lab Story 5 - Using X-Play with 3rd Party API
+++++++++++++++++++++++++++++++++++++++++++++

For this story we will be using Habitica to show how we can use 3rd Party APIs with X-Play. Habitica is a free habit and productivity app that treats your real life like a game. We will be creating a task with Habitica.


#. Use the hamburger menu to navigate to Operations > Action Gallery.

   .. figure:: images/ppro_47.png

#. Click the checkbox next to the item for ‘Rest API’ and then from the actions menu select the ‘Clone’ option.

   .. figure:: images/ppro_48.png

#. We are creating a template that we can later use in our playbook to create a Task in Habitica. Fill in the following values replacing your name in the <YOUR NAME HERE> part.

**Name:** Create Habitica Task

**Method:** POST

**URL:** https://habitica.com/api/v3/tasks/user

**Request Body:** {"text":"<YOUR NAME HERE> Check {{trigger[0].source_entity_info.name}}","type":"todo","notes":"VM has been detected as a bully VM and has been temporarily powered off.","priority":2}

**Request Header:**
x-api-user:fbc6077f-89a7-46e1-adf0-470ddafc43cf
x-api-key:c5343abe-707a-4f7c-8f48-63b57f52257b
Content-Type:application/json;charset=utf-8

   .. figure:: images/ppro_49.png

#. Click the **copy** button to save the template.

#. Navigate using the Hamburger menu back to Operations > Playbooks and click the Create Playbook button.

#. Select the **Alert trigger** and search for and select the alert policy **VM Bully Detected**. This is the alert that we would like to act on to handle when the system detects a Bully VM.

   .. figure:: images/ppro_50.png

#. The first thing we would like to do is Power off the VM, so we can make sure it is not starving other VMs of resources. Click the **Add Action** button and select **Power Off VM**. Select the Parameter for **Source Entity** as you did in Story 3.

   .. figure:: images/ppro_51.png

#. Also be sure to select **Power off** for Type of Power Off Action.

   .. figure:: images/ppro_52.png

#. Next we would like to create a task so that we can look into what is causing this VM to be a Bully. Add another Action. This time select the template you created called, Create Habitica Task.

   .. figure:: images/ppro_53.png

#. Add one more action, select the Acknowledge Alert action. Use the parameters for this action to fill in the ‘Alert’ parameter.

   .. figure:: images/ppro_54.png

#. Save & Enable the playbook. You can name it  ‘Power Off Bully VM for Investigation’. Be sure to enable the ‘Enabled’ toggle.

   .. figure:: images/ppro_55.png

#. **Switch back to the other tab** running http://10.42.247.70:3000/ and Simulate the ‘VM Bully Detected’ alert for Story 5.

   .. figure:: images/ppro_65.png

#. Once the alert is successfully simulated, you can log in to Habitica in another tab at https://habitica.com using the credentials:

Username : next19LabUser
Password: Nutanix.123

And verify your task is created.

   .. figure:: images/ppro_57.png
