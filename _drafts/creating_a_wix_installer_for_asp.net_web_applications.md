---
layout: post
title: Creating a WiX installer for ASP.NET Web Applications
category: blog
tags: wix
---

Manually maintaining a Windows Installer for a large ASP.NET web application using Wix Toolset can be a very time consuming and error prone task.

In this tutorial I'm going to show you how to use WiX Toolset and Web Deploy (msdeploy) to create a Windows Installer for a single ASP.NET web application.

This tutorial is based on [WiX Toolset](http://wixtoolset.org/) version 3.10.3

Source Code: https://github.com/AlonAmsalem/WebAppWixInstallerTemplate

### Step 1: Create the WiX setup project

Before adding a new setup project, Make sure you installed the latest version of [WiX Toolset](http://wixtoolset.org/).

To add a new setup project in Visual Studio go to File &#10140; New &#10140; Project...

![Add WiX Setup Project](/images/wix_aspnet_tutorial/new_wix_project.png)

Add a new Installer (.wxs) file . The file name should be the same as the web application name. For example: [WebApp.wxs](https://github.com/AlonAmsalem/WebAppWixInstallerTemplate/blob/master/WebAppInstaller/WebApp.wxs)

![Add Installer File](/images/wix_aspnet_tutorial/add_new_wxs_file.png)

### Step 2: Add project reference

Add project reference from the WiX setup project to the web application project.

![Add Reference](/images/wix_aspnet_tutorial/add_reference.png)

![Add Project Reference](/images/wix_aspnet_tutorial/add_project_reference.png)

### Step 4: Web Application Package

To automatically update the installer every time the setup project builds, Open the WiX setup project file (.wixproj) using any text editor:

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
  <MSBuild Projects="%(ProjectReference.FullPath)" Targets="Package" Properties="Configuration=$(Configuration);Platform=Any CPU" Condition="'%(ProjectReference.WebProject)'=='True'" />
  <ItemGroup>
    <LinkerBindInputPaths Include="%(ProjectReference.RootDir)%(ProjectReference.Directory)obj\$(Configuration)\Package\PackageTmp\" />
  </ItemGroup>
  
  <!-- Generate a WiX installer file using Heat Tool -->
  <HeatDirectory OutputFile="%(ProjectReference.Filename).wxs" Directory="%(ProjectReference.RootDir)%(ProjectReference.Directory)obj\$(Configuration)\Package\PackageTmp\" DirectoryRefId="INSTALLFOLDER" ComponentGroupName="%(ProjectReference.Filename)" AutogenerateGuids="True" SuppressCom="True" SuppressFragments="True" SuppressRegistry="True" ToolPath="$(WixToolPath)"  Condition="'%(ProjectReference.WebProject)'=='True'" />
</Target>
```

To exclude certain files or folders from the final .wxs file, Add a new .xslt file to the setup project and set the Transforms property of ```<HeatDirectory></HeatDirectory>

To disable Web.Config automatic connection string parameterization set MSBuild property AutoParameterizationWebConfigConnectionStrings to False.

Here's a .wixproj file for example  [WebAppInstaller.wixproj](https://github.com/AlonAmsalem/WebAppWixInstallerTemplate/blob/master/WebAppInstaller/WebAppInstaller.wixproj)

