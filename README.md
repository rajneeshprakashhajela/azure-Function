
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
