---
layout: post
title: Creating a WiX installer for ASP.NET Web Applications
category: blog
---

Manually maintaining a WiX installer for an ASP.NET web application is a very time consuming and error prone task.

In this post, I will describe how to use WiX installer harvesting utility with Visual Studio built-in packaging to generate a setup for ASP.NET web applications during build.

First create a new setup project in Visual Studio the usuall way.
File->New Project...

Then edit your .wixproj (I use an external editor)

