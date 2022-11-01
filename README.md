# Udacity_project3

## Ensuring Quality Releases using Azure CI/CD Pipeline

# Project objectives
The project illustrates building a CI/CD pipeline with Azure DevOps

# Status badge

# Contents

The project comprise of a CI/CD pipeline with four stages.

This involves creating the infrastructure in Azure necessary for the app to run including the Appservice, ehich is done using Terraform
The Build stage builds the fakerestapi src code and uploads it to Azure
The Deploy stage involves downloading the uploaded artifacts and deploys them to Azure Appservice
Finally the Testing stage which consists of three tests:
functional testing with Selenium
Integration tests with Postman
Performance tests with Jmeter



# Introduction

This project uses Azure DevOps to build a CI/CD pipeline that creates disposable test environments and runs a variety of automated tests to ensure quality releases. It uses Terraform to deploy the infrastructure, Azure App Services to host the web application and Azure Pipelines to provision, build, deploy and test the project. The automated tests run on a self-hosted virtual machine (Linux) and consist of: UI Tests with selenium, Integration Tests with postman, Stress Test and Endurance Test with jmeter. Additionally, it uses an Azure Log Analytics workspace to monitor and provide insight into the application's behavior.

Flowchart of the whole process

<img width="874" alt="Screenshot 2022-11-01 at 08 38 35" src="https://user-images.githubusercontent.com/110615576/199185225-f43283ac-21bf-4034-9e66-796262314a35.png">


# Prerequisite

Azure Account
Azure Command Line Interface
Azure DevOps Account

# Project Dependencies

Terraform
JMeter
Postman
Python
Selenium

# Steps taken

1. Download the Project Starter file provited.
2. Open it in your preferred code editor (Using PyCharm in my case)

Login to the Azure account

" az login " 

Create service principal or use the one your already have (In my case I used the provided service principal by Udacity Lab) 

az ad sp create-for-rbac --name ensuring-quality-releases-sp --role="Contributor" --scopes="/subscriptions/SUBSCRIPTION_ID"

Details below will be generated

''{
  "appId": "00000000-0000-0000-0000-000000000000",
  "displayName": "azure-cli-2017-06-05-10-41-15",
  "name": "http://azure-cli-2017-06-05-10-41-15",
  "password": "0000-0000-0000-0000-000000000000",
  "tenant": "00000000-0000-0000-0000-000000000000"
}''

Create a config.sh file inside terraform directory, Cd inside the terraform directory and run

bash config.sh

Create an azsecret.conf which will have variables to be uploaded and use the pipeline as group variable

Go to your local terminal and create SSH key that the VM will use to Login, A public key (id_rsa.pub) and A private key (id_rsa) will be created and save.

cd ~/.ssh/

ssh-keygen -t rsa -b 4096 -f az_eqr_id_rsa

# Azure Pipeline

# Setting up Azure Pipeline

You have to install terrafrom extention

<img width="725" alt="Screenshot 2022-11-01 at 09 18 23" src="https://user-images.githubusercontent.com/110615576/199189526-7a2e420e-81b3-4f04-a86e-255d09cbe893.png">

We will have to create a new Service Connection in the Project by going to Project Settings -> Service connections -> New service connection -> Azure Resource Manager -> Service Principal (automatics) -> Choose the subscription -> Fill the data from your azurecreds.conf file -> Name the new service connection to Azure Resource Manager

The next step is to upload our azsecret.conf to Azure Devops as a Secure File, to do this we have to navigate to Pipelines -> Library -> Secure Files -> + Secure File -> Upload File. Now the file should be uploaded.

Successful accessing of the VM that Terraform creates we will need us to also upload to Secure Files and a private key.

<img width="1440" alt="Screenshot 2022-11-01 at 01 08 57" src="https://user-images.githubusercontent.com/110615576/199210041-ad2f64b0-64cc-45b5-bb04-12e693e823f0.png">

We will then create a avariables group named azsecret.conf, and add the following data in a variable group.

<img width="1440" alt="VariablesData" src="https://user-images.githubusercontent.com/110615576/199210597-9277af8e-d284-4681-94da-6a4b9aa42fbd.png">


# Configuring Pipeline Environment

We need to manually register the Virtual Machine for self-test runner in Pipelines -> Environments -> TEST -> Add resource -> Virtual Machines -> Linux. Then copy the registration script and manually ssh into the virtual machine, paste it on the terminal and run it.

mkdir azagent;cd azagent;curl -fkSL -o vstsagent.tar.gz https://vstsagentpackage.azureedge.net/agent/2.210.1/vsts-agent-linux-x64-2.210.1.tar.gz;tar -zxvf vstsagent.tar.gz; if [ -x "$(command -v systemctl)" ]; then ./config.sh --environment --environmentname "TEST" --acceptteeeula --agent $HOSTNAME --url https://dev.azure.com/lawalshakirat66/ --work _work --projectname 'p3_demo' --auth PAT --token xlwqjycl6g5ab32rieorpuwa2ryxmvcp7dzgwri3mdjznz6b7p6a --runasservice; sudo ./svc.sh install; sudo ./svc.sh start; else ./config.sh --environment --environmentname "TEST" --acceptteeeula --agent $HOSTNAME --url https://dev.azure.com/lawalshakirat66/ --work _work --projectname 'p3_demo' --auth PAT --token xlwqjycl6g5ab32rieorpuwa2ryxmvcp7dzgwri3mdjznz6b7p6a; ./run.sh; fi

After a successful Deployment, it should look like in the following screenshot:

<img width="1440" alt="Screenshot 2022-11-01 at 01 09 54" src="https://user-images.githubusercontent.com/110615576/199211542-e7090722-5f3d-4b3c-aa4a-614aa8ba4ef7.png">

# Terrafrom apply

<img width="1440" alt="TerraformApply" src="https://user-images.githubusercontent.com/110615576/199212453-c070c978-1b54-4e36-8416-b41836eb82b3.png">

# Deployed Webapp

<img width="1440" alt="DeployedWebApp" src="https://user-images.githubusercontent.com/110615576/199212903-ce5178f5-5f5e-4368-a4a4-e5a41ebb9c41.png">

<img width="1440" alt="App" src="https://user-images.githubusercontent.com/110615576/199230035-f0d6bcf6-31da-419c-8478-78aba0a3dcf7.png">

<img width="1440" alt="WebApp" src="https://user-images.githubusercontent.com/110615576/199213128-c1178335-fdb9-41ee-8fc2-246aee8b56c0.png">

<img width="1440" alt="Swagger" src="https://user-images.githubusercontent.com/110615576/199213278-bf38a986-0750-49dc-bb27-28a70f96d4b3.png">


# Testing

# Regression test

<img width="1440" alt="RegressionTest" src="https://user-images.githubusercontent.com/110615576/199214088-52ab86bd-225f-4e2c-8cba-9896999ca46d.png">

#Validation test

<img width="1440" alt="ValidationTest" src="https://user-images.githubusercontent.com/110615576/199214285-39a1e589-bf73-499a-a9db-70bcf99013a1.png">

#Publish Test Results

<img width="1440" alt="PublishTestResults2" src="https://user-images.githubusercontent.com/110615576/199214950-6e13459b-c68e-4d11-aca6-7979553bfa34.png">

<img width="1440" alt="PublishTestResults1" src="https://user-images.githubusercontent.com/110615576/199215020-69d6b194-e03a-447e-bf56-228049bf1a44.png">

<img width="1440" alt="PublishTestResults3" src="https://user-images.githubusercontent.com/110615576/199215131-807b8a88-982f-433c-9c76-793789a089f7.png">

# Selenium Test Result

<img width="1440" alt="SeeleniumTestUItestResults" src="https://user-images.githubusercontent.com/110615576/199215585-82c3903a-be2b-4d3f-8105-0ff91653dbb0.png">

#JMeter Stress Test

<img width="1440" alt="JMeterStressTest" src="https://user-images.githubusercontent.com/110615576/199215983-bd209c1b-358d-4f7d-9f6b-c2da17eaa7e9.png">

# JMeter Endurance Test

<img width="1440" alt="JMeterEnduranceTest" src="https://user-images.githubusercontent.com/110615576/199216122-8ba2019f-6ce0-4a47-a5f3-29af21a0d879.png">

<img width="1440" alt="PublishJMeterTest" src="https://user-images.githubusercontent.com/110615576/199216203-5da183c9-4f3d-4ced-865a-c149156207c9.png">

<img width="1440" alt="Screenshot 2022-11-01 at 01 05 59" src="https://user-images.githubusercontent.com/110615576/199216319-96e64f13-6d46-43e0-8808-242540d4d783.png">

<img width="1440" alt="TestRuns" src="https://user-images.githubusercontent.com/110615576/199216423-828f8f77-2515-4a5b-9736-af8f0d1c9fe2.png">


# Stages in Azure Pipeline

<img width="1440" alt="StagesInAzurePipelines" src="https://user-images.githubusercontent.com/110615576/199216711-2ff75356-bba8-4e32-ae9b-5836354d963c.png">

# Creating Log Analytics workspace

You con create LAW from the portal, once the LAW is created, goto Agents management > Linux server > Log Analytics agent instructions > Download and onboard agent for Linux

SSH into the VM created above (Under test) and install the OSMAgent using the following command.

wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 0ca00083-6708-4862-871d-6ba01ed6f0d8 -s p6HsJnyMJOY3sZRGCyNIkwcQo+UkXL9i8GOrf5wDgzM5JSikM5oY+8k2bfI2uejLsyMu3ra5Y7SlrtiUFX/B2Q== -d opinsights.azure.com

# Create a new alter for the App Service
a) From the Azure Portal go to:
Home > Resource groups > "RESOURCE_GROUP_NAME" > "App Service Name" > Monitoring > Alerts
b) Click on New alert rule
c) Double-check that you have the correct resource to make the alert for.
d) Under Condition click Add condition
d) Choose a condition e.g. Http 404
e) Set the Threshold value to e.g. 1. (You will get altered after two consecutive HTTP 404 errors)
f) Click Done

# Create a new action group for the App Service
a) In the same page, go to the Actions section, click Add action groups and then Create action group
b) Give the action group a name e.g. http404
c) Add an Action name e.g. HTTP 404 and choose Email/SMS message/Push/Voice in Action Type.
d) Provide your email and then click OK

<img width="1440" alt="Alert" src="https://user-images.githubusercontent.com/110615576/199217558-2c35e714-639a-4308-aefd-aa216e050b2b.png">

# Create AppServiceHTTPLogs
Go to the App service > Diagnostic Settings > + Add Diagnostic Setting. Tick AppServiceHTTPLogs and Send to Log Analytics Workspace created on step above and Save.

Go back to the App service > App Service Logs . Turn on Detailed Error Messages and Failed Request Tracing > Save. Restart the app service.

# Setting up Log Analytics
Set up custom logging, in the log analytics workspace go to Settings > Custom Logs > Add + > Choose File. Select the file selenium.log > Next > Next. Put in the following paths as type Linux:

/var/log/selenium/selenium.log

I named it selenium_ommy_CL, Tick the box Apply below configuration to my Linux machines.

<img width="1440" alt="LogAnalyticSelenium" src="https://user-images.githubusercontent.com/110615576/199218089-68b3ab77-5fe1-4a11-bb00-e25be249de84.png">

I then went back to Log Analytics workspace and ran below query to see Logs

AppServiceHTTPLogs 
|where _SubscriptionId contains "d49264d6-43a4-4c49-a3fd-4b74ad940d69"
| where ScStatus == '404'

<img width="1440" alt="LogQuery1" src="https://user-images.githubusercontent.com/110615576/199218746-bf8d4b1a-aa98-48fe-9aca-c743fd6ad598.png">

<img width="1440" alt="Query2" src="https://user-images.githubusercontent.com/110615576/199218848-4d622585-0b6d-4a85-a3ea-b83f14f6c779.png">

<img width="1440" alt="Query3" src="https://user-images.githubusercontent.com/110615576/199218886-5de3d8f8-d40a-46b1-b777-87e7b426d9e7.png">

Go back to the App Service web page and navigate on the links and also generate 404 not found , 
example:
https://3rdproject-appservice.azurewebsites.net/heeeu

After the trigger, check the email configured since an alert message will be received.

<img width="1440" alt="AlertTriggered" src="https://user-images.githubusercontent.com/110615576/199219375-8c4ab663-21e7-4f29-9392-401fe1e565b0.png">

<img width="1440" alt="AlertResolved" src="https://user-images.githubusercontent.com/110615576/199219470-923d2a37-7281-42d4-8c3a-739947a54ab5.png">


# The final repo after updating the repo in GitHub

<img width="1440" alt="UpdatedGitHubRepo" src="https://user-images.githubusercontent.com/110615576/199219969-031602ae-6c14-42d2-aff0-df0ee1edef8a.png">


# URL Used for the project
Postman: https://dummy.restapiexample.com/api/v1/create Selemium: https://www.saucedemo.com/ Jmeter: Delpoyed webapp - http://project3demo-appservice.azurewebsites.net/ (Bringing down soon)

# Helpful resources from Microsoft
Example setup using GitHub
Environment - virtual machine resource
Set secret variables
Design a CI/CD pipeline using Azure DevOps
Create a CI/CD pipeline for GitHub repo using Azure DevOps Starter
Create a CI/CD pipeline for Python with Azure DevOps Starter




