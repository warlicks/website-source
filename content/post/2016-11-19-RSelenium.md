---
title: "RSelenium"
date: 2016-11-19
author: "Sean Warlick"
--- 

Introduction {#introduction}
------------

In my recent post *Learning Python*, I promised an article about using
python to gather data from web APIs. I was in the midst of building
several functions to gather and clean real estate data from
[Trulia](https://www.trulia.com), when I received an email announcing
that they were shutting down the API. While it did bring that project to
an end, it did inspire me to resume a project to analyze college
swimming meet results that had been long abandoned.

The first step in the project was to gather the meet results in an
automated way from a unified source. While there are many sources that
have college swimming results, [Swimming World](www.swimmingworld.com),
[SwimSwam](www.swimswam.com) and [CollegeSwimming.com](www.collegeswimming.com), to name a few, none of them have
a complete list of all results. In addition, the results are often
stored as a PDF, making the data difficult to extract. The only complete
source of college swimming results is available at [USA Swimming](www.usaswimming.org).

While USA Swimming has all the college swimming results, they are not
easy to access. There is no API available, so I needed to rely on their
[Top Times Report](http://www.usaswimming.org/DesktopDefault.aspx?TabId=1971&Alias=Rainbow&Lang=en).
Unfortunately this form makes it hard to automate the data collection.
You have to fill out a search form each time you want to get results and
the results are limited to the top 500 swimming times in each meet
event. The default format of the results is a PDF, though you can change
it to a CSV after you get the results back.

Selenium {#selenium}
--------

The solution to my problems is a tool called Selenium and it's R
implementation **RSelenium**. Though designed to do automated web
testing, **RSelenium** can facilitate gathering data from the web. The
package allows the programmer to drive a web browser from an R script.
It allows the user to send mouse clicks and keyboard input to the
website. While it does require the user to look at the underlying html
to identify objects on the webpage, **RSelenium** is fairly
straightforward to use.

### Starting Selenium Server {#starting-selenium-server}

For this example, we will be using a Selenium Standalone Server, so we
can drive our browser locally. While running a Docker container is the
suggested method of running Selenium server, we need to run a local
server so that we download and access the data. In this example, I am
running Selenium Standalone Server version 2.53.1 [(Click Here to
Download)](http://selenium-release.storage.googleapis.com/index.html?path=2.53/)
and using Firefox version 46.0.1. Not all combinations of Firefox and
Selenium Server play nicely together.

To start the Selenium server, we need to utilize the command line. After
navigating to the folder where you have saved the Selenium server, to
start the server, type the command below. The -port 4400 is optional; by
default Selenium listens for connections on port 4444. To keep the
server running, we need to leave the console open.

``` bash
java -jar selenium-standalone-server.2.53.1.jar -port 4440
```

### Firefox Profile {#firefox-profile}

Since Selenium is primarily designed to automate website testing, it
does not handle the downloading of files in Firefox automatically. To
handle the downloading of the CSV file we will generate from the top
times report we need to set up a Firefox profile.

```r
# Library Load
library(RSelenium)

# Set Filepath for download location
downloadPath <- "~/Downloads"

# Build Firefox Profile. 
fprof <- makeFirefoxProfile(list(
        browser.download.folderList = 2L,  
        browser.download.manager.showWhenStarting = FALSE,
        browser.download.dir = downloadPath,
        browser.helperApps.neverAsk.openFile = "application/excel",
        browser.helperApps.neverAsk.saveToDisk = "application/excel",
        browser.helperApps.alwaysAsk.force = FALSE,
        browser.download.manager.showAlertOnComplete = FALSE,
        browser.download.manager.closeWhenDone = TRUE )
    )
```

### Starting A Broswer {#starting-a-broswer}

Now we are almost ready to start navigating the web from our R script.
First we are going to create a Remote Driver. The remote driver will be
the key component that we come back to time and time again to navigate
the Top Times report form.

```r
# Set Website URL
url <- "http://usaswimming.org/DesktopDefault.aspx?TabId=1971&Alias=Rainbow&Lang=en"

# Create Remote Driver Object
remDr <- remoteDriver(remoteServerAddr = 'localhost', 
                      port = 4440, 
                      browser = "firefox", 
                      extraCapabilities = fprof)

# Open Browser & Navigate to Webpage.
remDr$open(silent = TRUE)
remDr$navigate(url)  
```

### Interacting With the Report Form {#interacting-with-the-report-form}

Now that we are on the desired website, we can start sending information
to various parts of the search form. To do this, we need to tell the
remote driver what elements of the form we want to interact with. We do
this by providing the html tag IDs to `remDr$findElement()`. You can
find the correct IDs by using Firefox's inspector.

```r
# Make Sure Indivual Times are Selected  
time_type <- remDr$findElement(using = "id", value = "ctl82_rbIndividual")
time_type$clickElement() 

# Turn Off Long Course and Short Course Meter times
lcm <- remDr$findElement(using = "id", value = "ctl82_cblCourses_0")
lcm$clickElement()

scm <- remDr$findElement(using = "id", value = "ctl82_cblCourses_1")
scm$clickElement()

# Trun off altitude adjustment
altitude <- remDr$findElement(using = "id", value = "ctl82_cbUseAltitudeAdjTime")
altitude$clickElement()

# Submit report search
search <- remDr$findElement(using = "id", value = "ctl82_btnCreateReport")
search$clickElement()
```

### Downloading the Data {#downloading-the-data}

Now the above code made all of our desired selections in the Top Times
report and submitted our criteria. By default, the page returns a PDF,
but once the report is returned we can select a CSV file. After
selecting the CSV, a download dialog will open, but Selenium cannot
interact with it, which is why we set up the Firefox profile above.
After downloading the CSV, the browser can be closed and the server
shutdown.

```r
Sys.sleep(5)

# Change The Output To CSV & Save!
output_select <- remDr$findElement(using = "id", 
                                value = "ctl82_ucReportViewer_ddViewerType")

output_select$sendKeysToElement(list("E", "E", "E"))
    
change_output <- remDr$findElement(using = "id", 
                            value = "ctl82_ucReportViewer_lbChangeOutputType" )

Sys.sleep(5) # Need to pause to get the export click to work
change_output$clickElement()

Sys.sleep(10) # Give Time for Down Load
remDr$close()
remDr$serverClose()
```

Conclusion {#conclusion}
----------

**RSelenium** is a capable and relatively easy to use tool to facilitate
grabbing data from the web. If you would like to watch the code from
this article execute in real time you can click the image below. In
addition, you can find the example code from this article in a
standalone R script at [Github](https://github.com/warlicks/RSelenium-Data-Collection). The
repository also has the R markdown document used to create this post.

![](/video_thumbnail.jpg)