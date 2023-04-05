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
