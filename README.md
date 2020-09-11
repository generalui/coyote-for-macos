
# MacOSX Coyote Install Instructions

Based on: https://microsoft.github.io/coyote/learn/get-started/install

Install:
* [dotnet-core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [homebrew](https://brew.sh/) (in order to install powershell, below)

Then run these commands in a shell:
```bash
# Install Powershell
brew cask install powershell

# Install Coyote
dotnet tool install --global Microsoft.Coyote.CLI
```

# Install and Run Samples

After installing (above), run these commands to run your first Coyote session:

```bash
# Clone and Build samples
git clone https://github.com/microsoft/coyote-samples
cd coyote-samples
pwsh -f build.ps1

# Run HelloWorldTasks Example:
# NOTE:
#   This may or may not fail.
#   This example intentionally has a bug to be found...
cd HelloWorldTasks
dotnet run --project HelloWorldTasks --framework netcoreapp3.1
```

# Test Examples with Coyote

Command to run Coyote on `HelloWorldTasks`:

```bash
~/.dotnet/tools/coyote test ./bin/netcoreapp3.1/HelloWorldTasks.dll --iterations 100
```

Coyote output for `HelloWorldTasks`:
```
Microsoft (R) Coyote version 1.0.17.0 for .NET Core 3.1.7
Copyright (C) Microsoft Corporation. All rights reserved.

. Testing ./bin/netcoreapp3.1/HelloWorldTasks.dll
... Started the testing task scheduler (process:48850).
... Created '1' testing task (process:48850).
... Task 0 is using 'random' strategy (seed:4245419624).
... Telemetry is enabled, see http://aka.ms/coyote-telemetry.
..... Iteration #1
..... Iteration #2
..... Iteration #3
... Task 0 found a bug.
... Emitting task 0 traces:
..... Writing ./bin/netcoreapp3.1/Output/HelloWorldTasks.dll/CoyoteOutput/HelloWorldTasks_0_0.txt
..... Writing ./bin/netcoreapp3.1/Output/HelloWorldTasks.dll/CoyoteOutput/HelloWorldTasks_0_0.schedule
... Elapsed 0.0624267 sec.
Microsoft (R) Coyote version 1.0.17.0 for .NET Core 3.1.7
Copyright (C) Microsoft Corporation. All rights reserved.

... Testing statistics:
..... Found 1 bug.
... Scheduling statistics:
..... Explored 3 schedules: 3 fair and 0 unfair.
..... Found 33.33% buggy schedules.
..... Number of scheduling points in fair terminating schedules: 17 (min), 19 (avg), 21 (max).
... Elapsed 1.1921272 sec.
```

CoyoteOutput/HelloWorldTasks_0_0.txt:
```
<TelemetryLog> Telemetry is enabled, see http://aka.ms/coyote-telemetry.
<TestLog> Running test 'Microsoft.Coyote.Samples.HelloWorldTasks.Program.Execute'.
<ErrorLog> Value is 'Good Morning' instead of 'Hello World!'.
<StackTrace>    at Microsoft.Coyote.SystematicTesting.OperationScheduler.NotifyAssertionFailure(String text, Boolean killTasks, Boolean cancelExecution)
   at Microsoft.Coyote.SystematicTesting.ControlledRuntime.Assert(Boolean predicate, String s, Object[] args)
   at Microsoft.Coyote.Samples.HelloWorld.Greeter.RunAsync()
   at System.Runtime.CompilerServices.AsyncMethodBuilderCore.Start[TStateMachine](TStateMachine& stateMachine)
   at Microsoft.Coyote.Samples.HelloWorld.Greeter.RunAsync()
   at Microsoft.Coyote.Samples.HelloWorldTasks.Program.Execute()
   at System.Runtime.CompilerServices.AsyncMethodBuilderCore.Start[TStateMachine](TStateMachine& stateMachine)
   at Microsoft.Coyote.Samples.HelloWorldTasks.Program.Execute()
   at Microsoft.Coyote.SystematicTesting.ControlledRuntime.<>c__DisplayClass21_0.<RunTest>b__0()
   at System.Threading.Tasks.Task.InnerInvoke()
   at System.Threading.Tasks.Task.<>c.<.cctor>b__274_0(Object obj)
   at System.Threading.ExecutionContext.RunFromThreadPoolDispatchLoop(Thread threadPoolThread, ExecutionContext executionContext, ContextCallback callback, Object state)
   at System.Threading.Tasks.Task.ExecuteWithThreadLocal(Task& currentTaskSlot, Thread threadPoolThread)
   at System.Threading.Tasks.Task.ExecuteEntryUnsafe(Thread threadPoolThread)
   at System.Threading.Tasks.Task.ExecuteFromThreadPool(Thread threadPoolThread)
   at System.Threading.ThreadPoolWorkQueue.Dispatch()
   at System.Threading._ThreadPoolWaitCallback.PerformWaitCallback()

<StrategyLog> Found bug using 'random' strategy.
<StrategyLog> Testing statistics:
<StrategyLog> Found 1 bug.
<StrategyLog> Scheduling statistics:
<StrategyLog> Explored 3 schedules: 3 fair and 0 unfair.
<StrategyLog> Found 33.33% buggy schedules.
<StrategyLog> Number of scheduling points in fair terminating schedules: 17 (min), 19 (avg), 21 (max).
```

CoyoteOutput/HelloWorldTasks_0_0.schedule:
```
--fair-scheduling
--liveness-temperature-threshold:50000
(1)
(0)
(0)
(0)
(0)
(0)
(2)
(4)
(3)
(0)
(0)
(8)
(6)
(7)
(0)
(9)
(5)
(9)
(0)
```