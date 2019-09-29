# Project HACK - Hybrid Application Control on Kubernetes   
PyRK8 Dubai - Team Cloud Platfrom G&amp;L - MVP

# Project Overview & Summary
Project HACK is aimed at delivering a Minimum Viable Product (MVP) that aims to create a customer facing application within a Hybrid Environment that is split across IBM cloud and on-premise. It uses Redhat OpenShift to host and manage the application development that is based on of Node.JS. Managed Redhat OpenShift will be used on cloud and OKD hosted on a VMware node will simulate the on-premise environment. An external API integration will connect the application to Watson NLP for the interface (chatbot). 

![Project Overview](/images/overview.jpg)

The project aims are to acheive the following:
- Use Managed OpenShift on IBM Cloud as the key platform
- Build a CI/CD pipeline for the Node.JS application
- Interface with Watson Assistant via APIs
- Provision the test/dev on cloud production instance on OKD (on-premise)

The success criteria have been identified as:
* The Ability to create/update node.JS code on IBM Cloud, which then pushes pre-prod containers on cloud and prod to OKD.
* The Ability to integrate OCP and WA services over API

# Solution Architecture
The solution architecture consists of the following key components:
- Managed Openshift on IBM Cloud: this provide the test/dev/QA environment
- Openshift on VMware Bare metal servers (OKD based): this provides the 'on-premise' production level environment
- Watson Assistant service running on IBM Cloud for the NLP capabilities
- DevOps: pipeline integrated with Github by leveraging RAZEE on IBM Cloud

![Solution Architecture](/images/architecture1.png)

## High level cloud services overview

The diagram above illustrates the key services that will be provided at IBM Cloud:
-	Watson Assistant – for the AI NLP capabilities and conversation logic
-	Managed Redhat Openshift – to host Node.JS (and other) application logic required as part of the application implementation

## Managed Openshift 
Red Hat OpenShift on IBM Cloud™ is an extension of IBM® Cloud Kubernetes Service, where IBM manages OpenShift Container Platform for you. You manage both your community Kubernetes and OpenShift clusters with the same IBM Cloud Kubernetes Service API, CLI, and console for a consistent user experience.

With Red Hat OpenShift on IBM Cloud, it provides a fast and secure way to containerize and deploy the Node.JS application in Kubernetes clusters. OpenShift clusters build on Kubernetes container orchestration to offer consistency and flexibility for the development lifecycle operations.

The following diagram and table describe the default components that are set up in a Red Hat OpenShift on IBM Cloud™ architecture.
<img src="https://cloud.ibm.com/docs-content/v1/content/29639038d7721da887bfc13ac86b2ee074f214e2/openshift//images/cs_org_ov_both_ses_roks.png">

The cloud environment will be hosted on 3 OpenShift worker nodes, each one placed within a separate zone inside a Multizone cluster. 
The Master Nodes are IBM managed within a parallel Highly Available architecture.
A MultiZone Load Balancer will handle the traffic to the cluster to ensure full resiliency.
An Engress Gateway will allow traffic flow to-from the Watson API services

### OKD (on VMware)
For the on-premise environment, it is simulated by having a VMware vSphere compute node and OKD is installed on the VMware hypervisor. This will act as the 'production' instance host that is pushed from the cloud 'test/dev/pre-prod' environment.
For guidance on the installation and setup on VMware, reference the Redhat reference architecture link:
https://access.redhat.com/documentation/en-us/reference_architectures/2018/html-single/deploying_and_managing_openshift_3.9_on_vmware_vsphere/index

The Red Hat OpenShift Container Platform Architecture diagram shows the different components in the reference architecture. 
![OKD](/images/okd1.png)

The on-premise environment will be hosted on 1 OpenShift worker node running on VMware vSphere ESXi.
An Engress Gateway will allow traffic flow to-from the Watson API services


## Razee for DevOps CI/CD
Razee is an open-source project that was developed by IBM to automate and manage the deployment of Kubernetes resources across clusters, environments, and cloud providers, and to visualize deployment information for your resources so that you can monitor the rollout process and find deployment issues more quickly.

In this project, Razee will drive the automated pipeline to build and deploy the application on both the cloud and on-premise environments using a CI/CD pipleline
<img src="https://raw.githubusercontent.com/razee-io/Razee/master/images/razee_ov.png">

For full details on how to setup and install Razee, refer to the following link:
https://github.com/razee-io/Razee#step-1-install-razee
 
## Watson Assistant

Watson Assistant will be used to provide the Natural Language Processing (NLP) and will act as a chatbot service triggered by APIs from the Node.JS application hosted on OpenShift.
<p align="center">
 <img src="/images/watson1.png">
</p>

For setup and configuration guidance, refer to this link:
https://github.com/watson-developer-cloud/assistant-simple
 
## Key Architecture decisions
Design Creteria | Decision | description
--------------- | -------- | -----------
Architecture | Microservices | Node.JS , Watson APIs
Delivery | DevOps / SRE | Razee (test / pre-prod / prod)
Infrastructure | HA Multi-Master | Managed OCP multi-zone
Multi-cloud | Multiple Clouds (on and off premises solution) | IBM Cloud OCP + On-premise (OKD)
Open | Flexible Architecture with Open Components | OCP, Node.JS, Razee
Security | Protected LDAP with Certs | **KeyProtect service** is a roadmap item for next release
Networking | Using Calico network policies to block traffic | OCP service mesh (based on Calico)

### links to access the application GUI and the chatbot service
* Chatbot application link on Managed Openshift On Cloud: http://assistant-bot-assistant-bot.rhosmn-ce16e8809c82e1ef7eea8f973108a42d-0001.eu-de.containers.appdomain.cloud/
* Chatbot application link on Managed Openshift On Cloud: 161.156.72.178:32306/
* Openshift on OKD dashboard: https://master.openshift.local:8443/ (note must add the IP 161.156.72.178 to DNS/hosts file)
 
