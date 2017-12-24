---
title: A Confession
author: Sean Warlick
date: '2017-12-23'
slug: a-confession
categories:
  - R
tags:
  - RStudio
description: ''
featured: ''
featuredalt: ''
featuredpath: ''
linktitle: ''
draft: False
---

I have a confession to make.  When I first started using R, I hated RStudio. 
I want to emphasize the past tense of the previous sentence, given that it is 
a rather adversarial statement.  However, over the past year I have really come 
to appreciate the power of RStudio and have since rebuilt my workflow around it.  
  
The restructuring of my work flow around RStudio has been driven by three things.
First, improvements to RStudio allowed it to fit into my workflow.  Second, the 
type of tasks I've been trying to accomplish with R have changed.  Finally, I
have accrued a lot more time using the program.  
  
Prior to incorporating RStudio into my workflow, I used Sublime Text with the 
R-Box extension to pass code to the R console.  I utilized this setup
because it allowed me to have my text editor (in dark mode) on one monitor and 
the console output and graphics window on a different monitor. With older 
versions of RStudio, I couldn't replicate this workflow. The four-pane layout 
felt constricting by comparison. The constant re-sizing of the console 
and source panes to make one or the other readable was a huge annoyance.      

With the release of RStudio 0.99.878, I was able to replicate my preferred workflow.
I can now pop out the source editor to one monitor and put the console pane 
of RStudio on my other screen. I no longer need to re-size the windows to make 
the code or the output readable. This new set-up also allows me to have other 
tools like a file browser and details about my environment right at my finger 
tips. The most recent release of RStudio (Version 1.1.383) included an improved 
dark mode, ending one of my remaining gripes with the UI.
  
In addition to fitting into my preferred work set-up, RStudio has features that
make building packages, working with Rmarkdown and developing shiny applications
much easier. While I could carry out these tasks in Sublime Text, it wasn't as 
convenient. While working on any of these types of projects, I needed to keep 
switching to an additional R script with helper functions like `devtools::test()`
or `rmarkdown::render()` to check on things as I progressed.  RStudio provides 
excellent keyboard shortcuts like `Shift` + `Cmd` + `T` and `Shift` + `Cmd` + `K`
for these functions.
  
In addition to nice shortcuts, RStudio includes bigger features that make
development easier. I particularly like the inclusion of a viewer to view for 
Rmarkdown documents output to html and Shiny applications. There is no need to 
switch over to a web browser to preview these products. The most recent 
version of RStudio also includes a terminal window so I can run UNIX/Linux system
commands without needing to open a separate program. Given that I prefer to 
take advantage of git's power from the command line rather than the included GUI,
this is a nice feature.  
  
Perhaps the biggest reason for my incorporation of RStudio into my workflow is 
the fact that I've spent more time with it over the past year. Initially some of
the time felt forced. At work I don't get as much say about my workflow; I could
either use the default R set up for Windows or RStudio. As a result I RStudio 
became my default at work. This exposure led me to encounter many of the
features I mentioned above. As I found more and more advantages, I slowly moved 
on from Sublime Text at home too.  
  
For the longest time I resisted RStudio. It didn't fit well into my preferred 
work set-up and I didn't see the advantages over my old set-up. However, 
greater practice with the IDE, the powerful development features and improvements
to the GUI led me to adopt RStudio as my default programming environment for R.  