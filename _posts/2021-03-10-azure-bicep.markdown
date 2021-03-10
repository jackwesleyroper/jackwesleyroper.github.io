---
layout: post
title:  Azure Bicep — Pros & Cons
date:   2021-03-10 15:05:55 +0300
image:  /assets/images/blog/bicep.png
author: Jack Roper
tags:   Azure, Azure Bicep, IaC
---

**Azure Bicep is an declaritive abstraction language used to simplfy .json ARM templates that are used to provision infrastructure in Azure. But I was curious as to why I would use it over ARM, or a 3rd party tool like Terraform?**

Positives
It simplfies ARM templates — Bicep aims to take away some of the unecessery .json boilerplating, leaving simpler, cleaner code, similar to Hashicorps HCL.
For example, here is an ARM template to deploy an app service plan:
Image for post
ARM Template to deploy App Service
And here is the Bicep equivalent:
Image for post
Bicep code to deploy App service
2. You can use modules — similar to Terraform or any programmming language you can modularise the deployment of VMs for example. This wasn’t really possible in ARM templates. This should lead to a repository of modules being avalible on Github, similar to Terraform Registry. An example of a module in bicep to deploy our app service plan and resource group is shown below:
Image for post
The appPlan.bicep would be updated to include a few parameters:
Image for post
Then you would run using Azure CLI:
az deployment sub create -f ./main.bicep -l uksouth -c
The equivalent ARM template for reference (much more complicated!):
Image for post
3. There is no state management — Bicep can query the state directly from Azure. This removes problems with collaboration when multiple developers are accessing the state file ala Terraform. There are therefore no issues with keeping the state file secure, or refreshing the state either!
4. ‘Day zero’ support for Azure resources — As soon as an Azure resource becomes avaliable for consumption, you can use Azure Bicep to deploy it. Everything you can do with ARM templates, you can do with Bicep. The Azure Terraform provider typically is not updated on ‘day zero’, and normally has significant delay from when new resources or features are released, which can be very frustrating, and can lead to injecting ARM templates into Terraform code!
Negatives
The big one — it is limited to Azure. Bicep is a Domain specific language (DSL). If you’re using multi-cloud, want to use the same language across multiple providers, or there is a possibility of expansion to multi-cloud, this probably isn’t going to fly.
You need to learn ‘another language’ — although it is a fairly simple language to learn and understand, similar to HCL for Terraform, there is a learning curve. If you already understand ARM templates you might just opt to stick with that for now.
Its early days — at the time of writing it’s version 0.3 and it not widely used as yet, support for loops and conditions has only just been introuduced. More mature products like Terraform have many more features avaliable. However it is now deemed ‘production ready’ and does seem to be somthing Microsoft are pursuing so more features will be added in future. It is supported by support staff for production scenarios, and now has integration with Azure CLI, PowerShell, and VSCODE. Lastly, Terraform and ARM templates have many more examples and modules avaliable out there on the web for reference at the moment.
Other notes
You can convert existing ARM templates into bicep files — running the command below in cloud shell will convert your .json ARM template into a .bicep file:
az bicep decompile -f .\arm-template\arm-example.json
I don’t know of anything that can do this to convert ARM templates into Terraform (if you do let me know!)
There was a great overiew on Bicep at the Ignite conference recently which is what this articale is based upon, I copied the slide below which I thought highlighted a few points relevant for this article.
MyIgnite — Learn everything about the next generation of ARM Templates (microsoft.com)
Image for post
Check it out on Github:
GitHub — Azure/bicep: Bicep is a declarative language for describing and deploying Azure resources
Conclusions
As it stands, Azure Bicep is an exciting but immature technology in the IaC space. If you are using only Azure, it should definitely be considered.
Hope you enjoy delving into Azure Bicep!