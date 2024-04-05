# "No job functions found" warning when upgrading Microsoft.Azure.Functions.Worker.Sdk to 1.16.1 or above

I am trying to migrate my HTTP-triggered Azure Functions Apps to isolated process. For myself to understand how an isolated-process Functions app work, I started from the template provided by the Azure Functions Core Tools (version 4.0.5348 - the latest NixOS provides at the time of writing) following official [Quick-Start Guide](https://learn.microsoft.com/en-us/azure/azure-functions/dotnet-isolated-process-guide?tabs=linux).

The template works without upgrading any of the libraries that come with it.
One can visit the local address `http://localhost:7071/api/HttpExample` and see
the HTTP response `Welcome to Azure Functions!`.

However, if after I upgraded the libraries to the latest version, the
HTTP-triggered API stopped working and gave me the following warning message.

```
No job functions found. Try making your job classes and methods public. If you're using binding extensions (e.g. Azure Storage, ServiceBus, Timers, etc.) make sure you've called the registration method for the extension(s) in your startup code (e.g. builder.AddAzureStorage(), builder.AddServiceBus(), builder.AddTimers(), etc.).
```

The library versions come with the framework, at the time of writing, are

```
   [net8.0]:
   Top-level Package                                       Requested   Resolved
   > Microsoft.Azure.Functions.Worker                      1.19.0      1.19.0
   > Microsoft.Azure.Functions.Worker.Extensions.Http      3.0.13      3.0.13
   > Microsoft.Azure.Functions.Worker.Sdk                  1.14.0      1.14.0

```
