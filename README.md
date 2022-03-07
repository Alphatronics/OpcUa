# Introduction 

The OpcUa library facilitates the interaction with OpcUA enabled devices manufactured by Alphatronics.

The library currently supports the following devices:

* SmartOne barrier

It is advisable to work with the provided OpcUaClient interfaces in the library, thus allowing to create fake or simulated devices.

# Getting Started

## Create a connection

```csharp

//create a logger from Microsoft.Extensions.Logging library
var loggerFactory = LoggerFactory.Create(
                builder =>
                {
                    builder.AddConfiguration(config.GetSection("Logging"))
                   // builder.SetMinimumLevel(LogLevel.Debug)
                    //builder.AddFilter("", LogLevel.Debug)
                    .AddConsole();
                });

var logger = loggerFactory.CreateLogger<Program>();
            
//create instance
ISmartOneOpcUaClient smartOne = new SmartOneOpcUaClient(logger, "smartOne1");

//register for events
smartOne.SmartOneStateChanged += SmartOne_SmartOneStateChanged;
smartOne.InputStateChanged += SmartOne_InputStateChanged;
smartOne.ConnectionStateChanged += SmartOne_ConnectionStateChanged;

//establish a connection, an Exception is raised when the connection could not be established.
smartOne.Connect("10.0.1.35", 4840);

```

## Connection recovery

To reassure a permanent tcp/ip connection between client application and opc-ua server, it is advisable to perform a **connection** check on a regular time period (eg: every second) and try re-connecting in case of a connection problem.

The following code can be executed on a regular timer interval:

```csharp

if (!smartOne.Connected)
    smartOne.Connect("10.0.1.35", 4840);

```

## Cleanup

```csharp
smartOne.Dispose()
```
