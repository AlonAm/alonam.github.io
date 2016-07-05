---
layout: post
title: Creating a WiX installer for ASP.NET Web Applications
category: blog
---

Manually maintaining a WiX installer for an ASP.NET web application is a very time consuming and error prone task.

In this tutorial I'm going to show you how to use WiX Toolset and MSDeploy to automatically create a Windows Installer for ASP.NET web applications.

First create a new setup project in Visual Studio

File-> New-> Project...

![alt text](/images/wix_aspnet_tutorial/new_wix_project.png)

Then edit your .wixproj (I use an external editor)
