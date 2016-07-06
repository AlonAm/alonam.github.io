---
layout: post
title: Creating a WiX installer for ASP.NET Web Applications
category: blog
---

Manually maintaining a Windows Installer for a large ASP.NET web application using Wix Toolset can be a very time consuming and error prone task.

In this tutorial I'm going to show you how to use WiX Toolset and Web Deploy (msdeploy) to create a Windows Installer for a single ASP.NET web application.

Source Code: https://github.com/AlonAmsalem/WebAppWixInstallerTemplate

### Step 1: Add a new WiX setup project

Before adding a new setup project, Make sure you installed the latest version of [WiX Toolset](http://wixtoolset.org/). This tutorial is based on version 3.10.3

To add a new setup project in Visual Studio go to File &#10140; New &#10140; Project...

![Create New WiX Project](/images/wix_aspnet_tutorial/new_wix_project.png)

### Step 2: Add a project reference to the web application

![Add Reference](/images/wix_aspnet_tutorial/add_reference.png)

![Add Project Reference](/images/wix_aspnet_tutorial/add_project_reference.png)

### Step 3: Add a new Installer (.wxs) file

![Add Project Reference](/images/wix_aspnet_tutorial/add_new_wxs_file.png)

### Step 4: Add a Before Build event to the WiX setup project

Open the WiX setup project .wixproj file using a text editor and paste the following XML inside the <Project> XML tag.
Here's a full Example: https://github.com/AlonAmsalem/WebAppWixInstallerTemplate/blob/master/WebAppInstaller/WebAppInstaller.wixproj

{% gist AlonAmsalem/3ee098fb588da1985da2 %}

<script src="https://gist.github.com/AlonAmsalem/3ee098fb588da1985da2.js"></script>
