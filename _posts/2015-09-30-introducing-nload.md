---
layout: post
title: Introducing NLoad
category: Multithreading
---

NLoad is a simple and easy to use open-source .NET library for running small bits of code on multiple threads to measure throughput and iteration count.

To start using the library run the following command in the Package Manager Console in Visual Studio or download the package from [NuGet](https://www.nuget.org/packages/NLoad/).

```
Install-Package NLoad
```

### Usage

Implement the ITest interface to create a test class with your custom code. For example:


```csharp
public class MyTest : ITest
{
  public void Initialize()
  {
    // Initialize the test, e.g., create a WCF client, load files into memory, etc.
  }

  public TestResult Execute()
  {
    // Send an http request, invoke a WCF service or whatever you want to load test.
    return TestResult.Success; // or TestResult.Failure
  }
}
```

This test class is created and initialized multiple times during the load test.

Create a new test and run it

```csharp
var loadTest = NLoad.Test<MyTest>()
		  .WithNumberOfThreads(5)
		  .WithDurationOf(TimeSpan.FromMinutes(5))
		  .OnHeartbeat((s, e) => Console.WriteLine(e.Throughput))
		.Build();

var result = loadTest.Run();
```

The test result includes the following information

- Total Errors
- Total Runtime
- Total Iterations
- Min/Max/Average Throughput
- Min/Max/Average Response Time
