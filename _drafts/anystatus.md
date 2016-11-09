---
layout: post
title: Introducing AnyStatus Visual Studio Extension
category: blog
tags: vsix, anystatus
---

I've recently published a new Visual Studio extension called AnyStatus. Progress is going well, so I'd like to share some details, and get your feedback.

## What is it?

AnyStatus extension adds basic monitoring capabilities to Visual Studio.

With AnyStatus you can monitor HTTP servers, send Ping requests, check TCP ports, monitor GitHub Issues, monitor CI builds on Team Foundation Server, Jenkins, TeamCity, AppVeyor and much more. All from within Visual Studio.

AnyStatus runs in the background and does not have a significant impact on the performance or startup of Visual Studio. Moreover, it loads only when the tool window is activated.

## Why?

AnyStatus provides a quick view of the status of services and resources.

## How?

AnyStatus extensions adds a Tool Window in Visual Studio that shows status items in a folder hierarchy.

![](https://github.com/AlonAm/AnyStatus/blob/master/art/Screenshot_blue.png)
![](https://github.com/AlonAm/AnyStatus/blob/master/art/Screenshot_dark.png)

- [x] Basic
  - [x] HTTP
  - [x] Ping
  - [x] Windows Service
  
  ## Download
