# "No job functions found" warning when upgrading Microsoft.Azure.Functions.Worker.Sdk to 1.16.0 or above

## Background

I am trying to migrate my HTTP-triggered Azure Functions Apps to isolated process. For myself to understand how an isolated-process Functions app work, I started from the template provided by the Azure Functions Core Tools (version 4.0.5348 - the latest NixOS provides at the time of writing) following official [Quick-Start Guide](https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-cli-csharp?tabs=linux%2Cazure-cli).

The template works without upgrading any of the libraries that come with it.
One can visit the local address `http://localhost:7071/api/HttpExample` and see
the HTTP response `Welcome to Azure Functions!`.

However, if after I upgraded the libraries to the latest version, the
HTTP-triggered API stopped working and gave me the following warning message.

```
No job functions found. Try making your job classes and methods public. If you're using binding extensions (e.g. Azure Storage, ServiceBus, Timers, etc.) make sure you've called the registration method for the extension(s) in your startup code (e.g. builder.AddAzureStorage(), builder.AddServiceBus(), builder.AddTimers(), etc.).
```

## The Question

How do I modify the codebase from the templates to fix this problem and let the
Azure Functions app
run under the latest `Microsoft.Azure.Functions.Worker.Sdk`?

## The cause

The library versions come with the framework, at the time of writing, are

```
   [net8.0]:
   Top-level Package                                       Requested   Resolved
   > Microsoft.Azure.Functions.Worker                      1.19.0      1.19.0
   > Microsoft.Azure.Functions.Worker.Extensions.Http      3.0.13      3.0.13
   > Microsoft.Azure.Functions.Worker.Sdk                  1.14.0      1.14.0

```

The latest library versions, at the time of writing, are

```
   [net8.0]:
   Top-level Package                                       Requested   Resolved
   > Microsoft.Azure.Functions.Worker                      1.21.0      1.21.0
   > Microsoft.Azure.Functions.Worker.Extensions.Http      3.1.0       3.1.0
   > Microsoft.Azure.Functions.Worker.Sdk                  1.17.2      1.17.2

```

After a few trial-and-errors, I found out the problem was upgrading `Microsoft.Azure.Functions.Worker.Sdk`
and its latest working version with the template is `1.15.1` - upgrading it to
`1.16.0`, the next version, will reproduce the error.

## Possible roots of the problem - my suspicions

1. A bug in `Microsoft.Azure.Functions.Worker.Sdk` since 1.16.0
1. Azure Functions Core Tools outdated
1. Project/Function template outdated - we need something

## Summary of steps to reproduce the problem

1. Install the [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) version 4.0.5348 on a Linux system.
1. Create project from template
   - `func init LocalFunctionProj --worker-runtime dotnet-isolated --target-framework net8.0`
1. Create function from template
   - `cd LocalFunctionProj`
   - `func new --name HttpExample --template "HTTP trigger" --authlevel "anonymous"`
1. Upgrade `Microsoft.Azure.Functions.Worker.Sdk`
   - `dotnet add package Microsoft.Azure.Functions.Worker.Sdk`
1. Run the Functions app locally
   - `func start --debug --verbose`

## Codebase

The codebase is listed in this [GitHub repository](https://github.com/kumkee/LocalFunctionProj).

## Similar questions

[This question](https://stackoverflow.com/questions/77540016/no-job-functions-found-migrating-to-net-8-isolated-worker-process) is the closest, but it is about upgrading from existing project but not about creating a brand new one.
