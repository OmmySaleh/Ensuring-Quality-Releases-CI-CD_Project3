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

<img width="874" alt="Screenshot 2022-11-01 at 08 38 35" src="https://user-images.githubusercontent.com/110615576/199185225-f43283ac-21bf-4034-9e66-796262314a35.png">


#Prerequisite

Azure Account
Azure Command Line Interface
Azure DevOps Account

#Project Dependencies

Terraform
JMeter
Postman
Python
Selenium

# Steps taken

1. Download the Project Starter file provited.
2. Open it in your preferred code editor (Using PyCharm in my case)

Login to the Azure account

az login 

Create service principal or use the one your already have (In my case I used the provided service principal by Udacity Lab) 

az ad sp create-for-rbac --name ensuring-quality-releases-sp --role="Contributor" --scopes="/subscriptions/SUBSCRIPTION_ID"

Details below will be generated

{
  "appId": "00000000-0000-0000-0000-000000000000",
  "displayName": "azure-cli-2017-06-05-10-41-15",
  "name": "http://azure-cli-2017-06-05-10-41-15",
  "password": "0000-0000-0000-0000-000000000000",
  "tenant": "00000000-0000-0000-0000-000000000000"
}

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




