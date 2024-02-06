# Securing Your Home Lab: Implementing Wazuh for Endpoint Detection and Response

## Introduction

Wazuh is an open-source, freely available and extensive EDR solution. Wazuh operates on a management and agent module.
Agent is the term that Wazuh calls a device that is being monitored for suspicious activity and potential security threats.
Manager is the term for a device that is responsible for managing these devices.
It can be used for:

- auditing device for vulnerabilities
- monitoring device for suspicious activity
- visualising complex data
- recording normal behavior to help detect anomalies

## Setting up Virtual Machines:

### 1. Creating Virtual machine instance for Wazuh-Server

So we already know that Wazuh operates on management and agent model, we need to setup a manager which will be our wazuh-server.

Go ahead and create a virtual machine instance of any linux distro of your choice and we will call it “Wazuh_Server” and this will be where we will setup up the wazuh-server.

![   Wazuh-Server](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/server.jpg)

   Wazuh-Server

### 2. Creating Virtual machine instance Wazuh-Agent

Now, we have our manager setup ready and we can monitor all of our endpoint machines through that manager. For the manager to be able monitor the end device we need to install Wazuh-Agents on all the endpoint machines.

Go ahead and create couple of virtual machine instances of any linux distro of your choice and we will call it “Wazuh_Agent 1” & “Wazuh_Agent 2” and it will be where we will setup up the wazuh-agent.

![   Wazuh-Agent](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agent1.jpg)

   Wazuh-Agent

By the end of it your virtualbox will look something like this:

![   Virtualbox](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/virtualbox_setup.png)

   Virtualbox

---

## Setting up Wazuh-Manager:

### 1. Installing Wazuh-Manager

Wazuh server is the manager in the “management and agent” model. Wazuh server is the place where the manager gets deployed and we can use the wazuh dashboard to keep track of agents and any detection on them.

To install run the following command:

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
#curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a -i  #ignore requirements
```

The installation might take some time depending on the system specification

Once its done we’ll be given a username and password which we have to use to login to the wazuh dashboard up and running on <ip-address>:443  

![ Wazuh installation](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/wazuh_installation.jpg)

 Wazuh installation

Check Status of Wazuh-Manager:

```bash
sudo systemctl status wazuh-manager
```

Now, the installation process is complete. Let’s take a look at the wazuh-dashboard.

Go to “https:<machine-ip>:443” and enter the login credentials when prompted for it. This should get you into the dashboard.

![Wazuh-Dashboard login](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/wazuh_login.jpg)

Wazuh-Dashboard login

![Wazuh-Dashboard Initialization](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/wazuh_dashboard_initialization.jpg)

Wazuh-Dashboard Initialization

![   Wazuh-Dashboard](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/wazuh_dashboard.jpg)

   Wazuh-Dashboard

### 2. Adding Agents to Dashboard:

Click on “Add agents”

You will be redirected to a page simply choose the right options and fill in the details and lastly copy the command that is provided and run it on the agent(endpoint) machine.

![agent_step1.jpg](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agent_step1.jpg)

![agent_step2.jpg](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agent_step2.jpg)

![                                 Wazuh-Agent command](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agent_step3.jpg)

                                 Wazuh-Agent command

Command:

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.2-1_amd64.deb && sudo WAZUH_MANAGER='192.168.0.108' WAZUH_AGENT_NAME='agent1' dpkg -i ./wazuh-agent_4.7.2-1_amd64.deb

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## Setting up a Wazuh-Agent:

### 1. Installing Wazuh-Agent

Wazuh server is the manager in the “management and agent” model. Wazuh Agent is installed on the endpoint machine for which we are setting up this EDR and we can use the wazuh dashboard of the server machine to keep track of agents and any detection on them.

Run the commands that we got from the dashboard

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.2-1_amd64.deb && sudo WAZUH_MANAGER='192.168.0.108' WAZUH_AGENT_NAME='agent1' dpkg -i ./wazuh-agent_4.7.2-1_amd64.deb
```

### 2. Enabling Wazuh-Agent

Once installation is complete, run the following command

This will enable and start  wazuh-agent service

```bash
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## Explore Wazuh:

Since in this project I am setting wazuh up mainly for event management in EDR, I won’t be exploring other aspects of it.

Open the dashboard in manager machine and you’ll see the below

![   Dashboard](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agent_added_dashboard.jpg)

   Dashboard

The ‘Total agents’ count indicates the number of agents currently up and running

Go ahead and click on the count and you’ll be redirected to ‘Agents’ page

![   Agents Dashboard](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/agents.jpg)

   Agents Dashboard

Again click on the agent that you wish to monitor the events of that particular agent, from there you can go to ‘Security Events’ to go to the following page

![security_dashboard.png](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/security_dashboard.png)

![Agents’ Dashboard](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/security_dashboard2.png)

Agents’ Dashboard

The Agents’ Dashboard will give an overview of the security alerts generated by the endpoint as it triggers various rules. These are represented in graphical view for quick overview and we can also get detailed overview of each alert.

Now goto the ‘Events’ tab

![   Agents’ Events](Securing%20Your%20Home%20Lab%20Implementing%20Wazuh%20for%20Thre%2003e71a5ca28045de8adf9253849e3ca9/security_events.png)

   Agents’ Events

This page will give detailed view of each event that took place on that endpoint/agent. From here we can do detailed analysis of events.

 

---

## Conclusion:

So in this project we saw how we can make use of Wazuh EDR to secure our endpoints and to monitor and analyze any events occuring on those end devices.

I would highly recommend for you all to spin your own wazuh manager-agent and play around with it and explore everything shown above in detail. 

It is really fun.

(P.S. For a more information go to official documentation of Wazuh)
