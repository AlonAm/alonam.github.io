---
layout: post
title: ASP.NET Web Application WiX Installer
category: blog
published: true
tags: wix
---

Manually maintaining a Windows Installer for a large ASP.NET web application using Wix Toolset can be a very time consuming and error prone task.

In this tutorial I'm going to show you how to use WiX Toolset and Web Deploy (msdeploy) to create a Windows Installer for ASP.NET web applications.

Source Code: https://github.com/AlonAmsalem/WebAppWixInstallerTemplate

### Overview

**Build**

The installer is created during the build process on the developer's workstation or a build server.
During the build process, Visual Studio compiles the web application and all of its dependencies and creates a Web Deploy package of all necessary files and folders.

Compile &#10140; Package &#10140; Harvest &#10140; Create MSI

**Installation**

Install &#10140; Create Application Pool &#10140; Create Website &#10140; Start

### Step 1: Add a WiX setup project

Before adding a new setup project, Make sure you installed the latest version of [WiX Toolset](http://wixtoolset.org/).

This tutorial is based on [WiX Toolset](http://wixtoolset.org/) version 3.10.3

To add a new setup project in Visual Studio go to File &#10140; New &#10140; Project...

![Add WiX Setup Project](/images/wix_aspnet_tutorial/new_wix_project.png)

Add a new Installer (.wxs) file. The file name should be the same as the web application name.

![Add Installer File](/images/wix_aspnet_tutorial/add_new_wxs_file.png)

### Step 2: Add project reference

Add project reference from the WiX setup project to the web application project.

![Add Reference](/images/wix_aspnet_tutorial/add_reference.png)

![Add Project Reference](/images/wix_aspnet_tutorial/add_project_reference.png)

### Step 4: Pack and harvest web application

To automatically update the installer every time the setup project builds, Open WiX setup project file (.wixproj) using any text editor (To edit the file using Visual Studio, Unload the WiX setup project, right-click the project name and select Edit WebApp.wxs).

Add ```<WebProject>True</WebProject>``` to the web application project reference.

```xml
<ProjectReference Include="..\WebApp\WebApp.csproj">
  <Name>WebApp</Name>
  <Project>{55b02cdb-fdd6-411d-8524-2429bab38d3b}</Project>
  <Private>True</Private>
  <DoNotHarvest>True</DoNotHarvest>
  <RefProjectOutputGroups>
  </RefProjectOutputGroups>
  <RefTargetDir>INSTALLFOLDER</RefTargetDir>
  <WebProject>True</WebProject>
</ProjectReference>
```

Paste the following XML snippet inside ```<Project></Project>``` tag.

```xml
<Target Name="BeforeBuild">
  <!-- Remove read-only attribute -->
  <Exec Command="attrib -R %(ProjectReference.Filename).wxs" Condition="'%(ProjectReference.WebProject)'=='True'" />
  
  <!-- Package web application using Web Deploy (msdeploy) -->
  <MSBuild Projects="%(ProjectReference.FullPath)" Targets="Package" Properties="Configuration=$(Configuration);Platform=AnyCPU" Condition="'%(ProjectReference.WebProject)'=='True'" />
  <ItemGroup>
    <LinkerBindInputPaths Include="%(ProjectReference.RootDir)%(ProjectReference.Directory)obj\$(Configuration)\Package\PackageTmp\" />
  </ItemGroup>
  
  <!-- Generate a WiX installer file using Heat Tool -->
  <HeatDirectory OutputFile="%(ProjectReference.Filename).wxs" Directory="%(ProjectReference.RootDir)%(ProjectReference.Directory)obj\$(Configuration)\Package\PackageTmp\" DirectoryRefId="INSTALLFOLDER" ComponentGroupName="%(ProjectReference.Filename)" AutogenerateGuids="True" SuppressCom="True" SuppressFragments="True" SuppressRegistry="True" ToolPath="$(WixToolPath)"  Condition="'%(ProjectReference.WebProject)'=='True'" />
</Target>
```

To disable Web.Config automatic connection string parameterization set MSBuild property AutoParameterizationWebConfigConnectionStrings to False.

Here's a .wixproj file for example  [WebAppInstaller.wixproj](https://github.com/AlonAmsalem/WebAppWixInstallerTemplate/blob/master/WebAppInstaller/WebAppInstaller.wixproj)

