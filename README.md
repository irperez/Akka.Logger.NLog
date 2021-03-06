# Akka.Logger.NLog
[![Build status](http://petabridge-ci.cloudapp.net/app/rest/builds/buildType:AkkaDotNetContrib_AkkaLoggerNLo_NightlyBuilds/statusIcon)](http://petabridge-ci.cloudapp.net/viewType.html?buildTypeId=AkkaDotNetContrib_AkkaLoggerNLo_NightlyBuilds&guest=1) [![NuGet Version](http://img.shields.io/nuget/v/Akka.Logger.NLog.svg?style=flat)](https://www.nuget.org/packages/Akka.Logger.NLog/)

This is the NLog integration plugin for Akka.NET.

## Configuration

### Configuration via code
```C#
// Step 1. Create configuration object 
var config = new LoggingConfiguration();

// Step 2. Create targets and add them to the configuration 
var consoleTarget = new ColoredConsoleTarget();
config.AddTarget("console", consoleTarget);

// Step 3. Set target properties 
consoleTarget.Layout = @"${date:format=HH\:mm\:ss} ${logger} ${message}";

// Step 4. Define rules
var rule1 = new LoggingRule("*", LogLevel.Debug, consoleTarget);
config.LoggingRules.Add(rule1);

// Step 5. Activate the configuration
LogManager.Configuration = config;

Config myConfig = @"akka.loglevel = DEBUG
                    akka.loggers=[""Akka.Logger.NLog.NLogLogger, Akka.Logger.NLog""]";

var system = ActorSystem.Create("my-test-system", myConfig);
```

## Configuration via NLog.config file
Add NLog.config file to your project
```xml
﻿<?xml version="1.0" encoding="utf-8" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <targets>
    <target name="console" xsi:type="Console" layout="[${logger}] [${level:uppercase=true}] : ${message}"/>
  </targets>
  <rules>
    <logger name="*" minlevel="Debug" writeTo="console"/>
  </rules>
</nlog>
```

Change your *.csproj file with this content
```xml
<ItemGroup>
  <None Include="NLog.config">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </None>
</ItemGroup>
```

Change your Akka.NET configuration
```C#
Config myConfig = @"akka.loglevel = DEBUG
                    akka.loggers=[""Akka.Logger.NLog.NLogLogger, Akka.Logger.NLog""]";

var system = ActorSystem.Create("my-test-system", myConfig);
```

## Maintainer
- Akka.NET Team
