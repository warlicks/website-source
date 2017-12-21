---
title: A Confession
author: Sean Warlick
date: '2017-12-21'
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
draft: True
---

I have a confession to make.  I hated RStudio.  I want to emphasize the past 
tense of the previous sentence, given the fact that RStudio has become the 
default way for people to work with R.  Over the past year I have really come to 
appreciate the power of RStudio and rebuilt my workflow around it.  
  
The rebuilding of my work flow around RStudio has been driven by three things.
First improvements to RStudio allowed it to fit into my workflow.  Second, the 
type of tasks I've been trying to acomplish with R have changed.  Finally, I
have accrued a lot more time using the program.  
  
Prior to incorporating RStudio into my workflow I used Sublime Text with the and
the R-Box extension to pass code to the R console.  I utilized this setup
because it allowed me to have my text editor (in darkmode0 on one monitor and 
the console output and graphics window on a different monitor. With older 
versions of RStudio I couldn't replicate this workflow. The four pane layout 
felt constricting by comparision.  

With the advent of RStudio X.X. I was able to replicate this work flow.  
I can now pop out the source editor on one monitor and the console pane of RStudio
on my other screen.  This also allowed me to have other tools like a file 
browser and details about my enviroment right at my figner tips.  The most recent
release of RStuido (Version 1.1.383) included a true darkmode, ending one of my
remaining gripes.  
  
In addition to fitting into my perfered work set up RStudio has features that
make building packages, working with Rmarkdown and developing shiny applications
much easier.  While I could carry out these taks in Sublime Text, it wasn't as 
convient.  While working on any of these types of projects I needed to keep 
swithcing to an additional R script with helper funcitons like `devtools::test()`
or `rmarkdown::render()` to check on things as I progressed.  RStudio provides 
excellent keyboard short cuts like `Shift` + `Cmd` + `T` and `Shift` + `Cmd` + `K`
for these functions.  The inclusion of a viewer for Rmarkdown documents and Shiny
applications is also a huge bonus.  I don't need to switch over to my web browswer
to preview these products.  