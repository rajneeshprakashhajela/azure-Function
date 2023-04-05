
Design a Durable Function Work related to Employee Leave Approval Process
https://www.c-sharpcorner.com/article/azure-duration-functions-how-to-use-and-implement-it/

This workflow will contain the below steps,

The employee submits a Leave Application.
The approval task is assigned to the Reporting Manager so that they can review the Leave Application of the Employee.
The Leave Application is either approved or rejected.
An Escalation task is also allocated if the approval task is not completed within the pre-defined time limit.
![image](https://user-images.githubusercontent.com/43515480/229991664-3ba128a8-aebe-4b25-9206-92ad629c63b6.png)
<img width="591" alt="image" src="https://user-images.githubusercontent.com/43515480/229991742-d537746a-08a5-42b7-9bc8-3ee1da1ac17a.png">

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
