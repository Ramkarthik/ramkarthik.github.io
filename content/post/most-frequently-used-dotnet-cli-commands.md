---
title: "My Most Frequently Used Dotnet Command-Line Interface (CLI) Commands"
date: 2019-03-11T01:00:00-01:00
draft: false
---
As a .NET developer, I spend a huge amount of time working on .NET projects both at work and at home. At work, I have a Windows machine and hence use Visual Studio to do every step in the development process (like creating project, running tests etc). But when I work on anything at home, I use a MacBook Air. While there is Visual Studio Community edition for Mac, I find it extremely slow and that's where Dotnet Command-Line Interface (CLI) comes to the rescue.

Wait a minute, did I just say macOS and .NET in the same sentence? Yes, I did.

ASP dotnet core is the best thing to happen for a C# developer because it lets you build applications on macOS or Linux that can be run pretty much anywhere (even Raspberry Pi).

Now if you don't know what dotnet CLI is, you can read more about it <a href="https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x">here</a>.

Dotnet CLI and VS Code together make it extremely easy to build apps on macOS and Linux. You don't have to remember many commands. Over the past two years of using dotnet CLI, I found that there are few commands that I constantly use. I Google the rest.

<h2>Here are my most frequently used dotnet CLI commands...</h2>
<br />
<b>Creating a solution:</b>
<br />
<code>dotnet new sln -n MySolution -o MySolution</code>
<br />
'-n' flag is used to specify the name for your solution and '-o' flag will place the output in this new folder.
<br />
<br />

<b>Creating a project:</b>
<br />
<code>dotnet new console -o MyConsoleApp</code>
<br />
Console creates a console application. You can use 'webapp' for Web Application, 'webapi' for Web API, 'classlib' for Class Library and so on. You can run 'dotnet new --help' to know about other project types that you can create.
<br />
<br />

<b>Adding project to a solution:</b>
<br />
<code>dotnet sln MySolution.sln add MyConsoleApp/MyConsoleApp.csproj</code>
<br />
The above command should be run from inside your solution folder.
<br />
<br />

<b>Adding project reference to another project:</b>
<br />
<code>dotnet add MyConsoleApp/MyConsoleApp.csproj reference MyClassLibrary/MyClassLibrary.csproj</code>
<br />
Again, this command should be run from inside your solution folder. If you are running it from your ConsoleApp folder, you will have to make necessary changes to the relative location of both .csproj files.
<br />
<br />

<b>Build a project:</b>
<br />
<code>dotnet build</code>
<br />
<br />

<b>Run a project:</b>
<br />
<code>dotnet run</code>
<br />
<br />

<b>Restore a project:</b>
<br />
<code>dotnet restore</code>
<br />
<br />

<h3>And now for testing (MS Unit/NUnit/XUnit etc)...</h3>
<br />
<b>Run all test:</b>
<br />
<code>dotnet test</code>
<br />
You will be running this from the test project folder.
<br />
<br />
<b>Run a specific test:</b>
<br />
<code>dotnet test --filter FullyQualifiedName=MyNamespace.MyClass.MyMethod</code>
<br />
<br />
<b>Run tests from a specific class:</b>
<br />
<code>dotnet test --filter FullyQualifiedName~MyNamespace.MyClass</code>
<br />
<br />
<b>Run specific category of tests:</b>
<br />
MSTest: <code>dotnet test --filter TestCategory=CategoryName</code>
<br />
XUnit: <code>dotnet test --filter Category=CategoryName</code>
<br />
The difference here is because of the difference in how you setup categories for different frameworks. MSTest uses "TestCategory" whereas XUnit uses "Trait".
<br />
<br />

Most of the above commands have many different flags that you can set but I rarely use them. This makes it easy to remember all the commands that I do use frequently.

Your productivity will improve significantly once you learn these dotnet CLI commands. Especially if you use Mac or Linux.