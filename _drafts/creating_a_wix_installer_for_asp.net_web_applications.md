---
layout: post
title: Creating a WiX installer for ASP.NET Web Applications
category: blog
---

Manually maintaining a WiX installer for an ASP.NET web application is a very time consuming and error prone task.

In this tutorial I'm going to show you how to use WiX Toolset and MSDeploy to create a Windows Installer for ASP.NET web applications that automatically updates everytime you build your project.

### Step 1: Create a new setup project in Visual Studio

File &#10140; New &#10140; Project...

![Create New WiX Project](/images/wix_aspnet_tutorial/new_wix_project.png)

### Step 2: Add project reference to your web application

![Add Reference](/images/wix_aspnet_tutorial/add_reference.png)

![Add Project Reference](/images/wix_aspnet_tutorial/add_project_reference.png)

### Step 3: Add a new .wxs file

![Add Project Reference](/images/wix_aspnet_tutorial/add_new_wxs_file.png)

### Step 4: Edit .wixproj

{% gist 3ee098fb588da1985da2 %}
