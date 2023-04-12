Azure Function  - Security, Monitoring, Logging, Statefull, steless, Serverless, mapping 1 Azure function to other Azure function.

<h1>Trigger in Azure Function </h1>

<h1>Security</h1> -Defender for Cloud
            Monitoring Azure Functions with Azure Monitor Logs.
             Require HTTPS - You should redirect HTTP to HTTPs because HTTPS uses the SSL/TLS protocol to provide a secure    
              connection, which is both encrypted and authenticated. To learn how, see Enforce HTTPS.
            
    <h1>Monitoring</h1> - App insight, 

some type of trigger
![image](https://user-images.githubusercontent.com/43515480/231435657-fb8b5aea-2cb5-43a8-9ea2-ec9c574b2ab7.png)

Now let’s see some of the most common types of triggers available in Azure:

Timer Trigger
This trigger is called on a predefined schedule. We can set the time for execution of the Azure Function using this trigger.

Blob Trigger
This trigger will get fired when a new or updated blob is detected. The blob contents are passed on as input to the function.

Event Hub Trigger
This trigger is used for the application instrumentation, the user experience, workflow processing, and the Internet of Things ( IoT). This trigger will get fired when any events are delivered to an Azure Event Hub.

HTTP Trigger
This trigger gets fired when the HTTP request comes.

Queue Trigger
This trigger gets fired when any new messages come in an Azure Storage Queue.

Generic Webhook
This trigger gets fired when the Webhook HTTP requests come from any service that supports Webhooks.

GitHub Webhook
This trigger is fired when an event occurs in your GitHub repositories. The GitHub repository supports events such as Branch created, delete branch, issue comment, and Commit comment.

Service Bus Trigger
This trigger is fired when a new message comes from a service bus queue or topic.

https://www.techgeeknext.com/azure-functions-interview-questions


Design a Durable Function Work related to Employee Leave Approval Process
https://www.c-sharpcorner.com/article/azure-duration-functions-how-to-use-and-implement-it/

This workflow will contain the below steps,

The employee submits a Leave Application.
The approval task is assigned to the Reporting Manager so that they can review the Leave Application of the Employee.
The Leave Application is either approved or rejected.
An Escalation task is also allocated if the approval task is not completed within the pre-defined time limit.
![image](https://user-images.githubusercontent.com/43515480/229991664-3ba128a8-aebe-4b25-9206-92ad629c63b6.png)
<img width="591" alt="image" src="https://user-images.githubusercontent.com/43515480/229991742-d537746a-08a5-42b7-9bc8-3ee1da1ac17a.png">

<b>Fan out and Fan In </b>

![image](https://user-images.githubusercontent.com/43515480/229993123-17b279c3-e565-4370-ba1f-c342687dc994.png)




https://i.stack.imgur.com/L1HtA.gif


using System; <BR/>
using System.IO;<BR/>
using Microsoft.Azure.WebJobs;<BR/>
using Microsoft.Azure.WebJobs.Host;<BR/>
using Microsoft.Extensions.Logging;<BR/>

namespace FunctionApp3<BR/>
{<BR/>
    public class Function1<BR/>
    {<BR/>
        [FunctionName("Function1")]<BR/>
        public void Run([BlobTrigger("samples-workitems/{name}", Connection = "")]Stream myBlob, string name, ILogger log)<BR/>
        {<BR/>
            log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");<BR/>
        }<BR/>
    }<BR/>
}



![image](https://user-images.githubusercontent.com/43515480/229988278-9ff12d3f-13de-4944-8582-8a215091961e.png)

File System Logs <- Log Stream <- Monitoring (Left Index Menu) of the Function App:
![image](https://user-images.githubusercontent.com/43515480/229989190-75973a30-538b-4f40-87fe-cbdf673c06da.png)
![image](https://user-images.githubusercontent.com/43515480/229989334-e43c55bb-6d6e-4f7e-874f-8ea34e3b7e41.png)


Logging in Azure Function:

<img width="467" alt="image" src="https://user-images.githubusercontent.com/43515480/229999438-93191aaa-0e0d-4407-a62a-656970de1ccf.png">



Billing Service in Azure Function
https://github.com/asc-lab/azure-functions-billing
<img width="467" alt="image" src="https://user-images.githubusercontent.com/43515480/230000958-9be48c90-5b4e-408c-aa49-40532b266570.png">

![image](https://user-images.githubusercontent.com/43515480/230001707-5778dfe9-1e10-4b7d-b121-f39f7b490a20.png)


<img width="580" alt="image" src="https://user-images.githubusercontent.com/43515480/230002209-efff9381-e4ea-43a8-9536-19e74b71d7cc.png">


User uploads CSV file (with name structure CLIENTCODE_YEAR_MONTH_activeList.txt.) with Beneficiaries (the sample file is located in the data-examples folder) to a specific data storage - active-lists Azure Blob Container.

The above action triggers a function (GenerateBillingItemsFunc) that is responsible for:

generating billing items (using prices from an external database - CosmosDB crm database, prices collection) and saving them in the table billingItems;
sending message about the need to create a new invoice to invoice-generation-request;
When a new message appears on the queue invoice-generation-request, next function is triggered (GenerateInvoiceFunc). This function creates domain object Invoice and save this object in database (CosmosDB crm database, invoices collection) and send message to queues: invoice-print-request and invoice-notification-request.

When a new message appears on the queue invoice-print-request, function PrintInvoiceFunc is triggered. This function uses external engine to PDF generation - JsReport and saves PDF file in BLOB storage.

When a new message appears on the queue invoice-notification-request, function NotifyInvoiceFunc is triggered. This function uses two external systems - SendGrid to Email sending and Twilio to SMS sending.

Tutorial from scratch to run locally
Install and run Microsoft Azure Storage Emulator.

Install and run CosmosDB Emulator. Check this on https://localhost:8081/_explorer/index.html.

Create in Emulator blob Container active-lists.

Upload ASC_2018_02_activeLists.txt file from data-examples folder to active-lists blob.

Create CosmosDB database crm and in this database create collections: prices, invoices.

Add CosmosDB properties PriceDbUrl and PriceDbAuthKey to local.appsettings.json in PriceDbInitializator and GenerateBillingIemsFunc. You can copy this properties from Azure CosmosDB Emulator - check point 2 (URI and Primary Key).

Run project PriceDbInitializator to init collection prices in crm database.

Add CosmosDB connection string as cosmosDb to local.settings.json in GenerateInvoiceFunc. You can copy this string from Azure CosmosDB Emulator - check point 2 (Primary Connection String).

Create an account in SendGrid and add property SendGridApiKey to local.settings.json in NotifyInvoiceFunc.

Create an account in Twilio and add properties TwilioAccountSid TwilioAuthToken to local.settings.json in NotifyInvoiceFunc.

Run JsReport with Docker: docker run -p 5488:5488 jsreport/jsreport. Check JsReport Studio on localhost:5488.

Add JsReport url as JsReportUrl to local.settings.json in PrintInvoiceFunc project.

Add JsReport template with name INVOICE and content:



<img width="664" alt="image" src="https://user-images.githubusercontent.com/43515480/230004618-a4dd7ccd-a5de-4b3e-a7e6-057f8c40e0ff.png">

<img width="635" alt="image" src="https://user-images.githubusercontent.com/43515480/230004699-fb6c8903-5a27-4396-ac00-ff8184ad4408.png">

<img width="550" alt="image" src="https://user-images.githubusercontent.com/43515480/230005115-48cf84ce-0774-458c-bbb7-aeb510988396.png">


Top Azure Functions Interview Questions (2023)
Azure Functions Interview Questions and Answers
What is Azure Functions?
What is Triggers in Azure Functions?
What is the maximum number of triggers that Azure Function can have?
What is Storage queue trigger in Azure function?
What is binding in azure function?
How to monitoring function apps in Azure Functions?

Powered By
VDO.AI
PlayUnmute
Fullscreen
Q: What is Azure Functions?
Ans:
Azure Functions is a serverless computing platform that allows you to write less code, manage less infrastructure, and save cost. Rather than worrying about establishing and managing servers, the cloud architecture delivers all of the current resources necessary to keep your apps running.

Q: What is Triggers in Azure Functions?
Ans:
Triggers are the events that cause a function to execute. A trigger specifies how a function is called, and each function must have only one. Triggers contain associated data, which is frequently provided as the function's payload.

Q: What is the maximum number of triggers that Azure Function can have?
Ans:
One trigger : There can only be one trigger per Azure function. There can't be more than one trigger in an Azure function.

Take a look at our suggested post :
Azure Active Directory Interview Questions
Azure Cosmos DB Interview Questions
Azure DevOps Interview Questions
Azure Logic Apps Interview Questions
Azure Fabric Interview Questions
Azure Data Factory Interview Questions
Jenkins Interview Questions
SonarQube Interview Questions
Q: What is Storage queue trigger in Azure function?
Ans:
Once messages are added to Azure Queue storage, the queue storage trigger performs a function. In function, a storage queue trigger is defined. When a new item is added to a queue, use the queue trigger to initiate a function. The function receives the queue message as input.

@FunctionName("queuetriggerfunction")
public void run(
    @QueueTrigger(name = "mytrigger",
                queueName = "triggerqueuename",
                connection = "triggerconnection") String message,
    final ExecutionContext context
) {
    .............
}
Q: What is binding in azure function?
Ans:
Binding to a function is a declarative manner of linking other resource to the function; bindings might be input or output bindings, or both. The function receives data from bindings as parameters. To meet our requirements, users can mix and combine different bindings.

Q: How to monitoring function apps in Azure Functions?
Ans:
Azure Functions uses Application Insights to keep track of Function Apps. Application Insights collects data produced by the function app, such as application traces and events we create.
