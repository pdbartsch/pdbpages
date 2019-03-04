---
title: "Using Python to Update ArcGIS Online Services"
date: 2019-03-04T10:56:42-08:00
publishdate: 2019-03-04
lastmod: 2019-03-04
draft: 
tags: ['ArcPy','ArcGIS-Desktop', 'ArcGIS-Pro', 'ArcGIS-Online', 'Python']
---

![Python to ArcGIS Online image](/img/agol.png) 

### The problem:
It's tough to keep an ever changing underground utility model up to date.  It is especially difficult when your audience uses a variety of methods to access that data.  The majority of my audience accesses this data via hosted PDFs but an ever growing group is now using ArcGIS Explorer and ArcGIS Collector on smartphones and tablets to access the same set of data.  

Since I manage the GIS data model of over ten types of utilities across multiple formats it was becoming quite common for a change to be made on my desktop but then either the PDF print or ArcGIS Online hosted data to be out of sync.  I needed to find an automated solution to keep all of this up to date.  [I've previously posted](https://geopy.dev/automate-export-to-pdf-of-arcgis-desktop-data-driven-pages/) about my method for keeping the PDF version of UC Santa Barbara atlas grids up to date and below you'll find my method for keeping the ArcGIS Online services in sync.

### The Solution:
As with most problems of this type, my first stop was Github to see if someone had already shared a solution that I could modify to fit my use case.  Luckily I found that [Kevin Hibma](https://github.com/khibma) had already done the heavy lifting on this.  His script did what I wanted for one feature service at a time.  I wanted to update an entire directory of map files at once.  One great thing about open source is that if something doesn't do exactly what you need you can modify it and submit a pull request.  So, I made a minor contribution to update multiple services at one time.  

I'd recommend checking out [this repository](https://github.com/khibma/update-hosted-feature-service) if you have a similar task to perform.  The instructions there are fairly detailed.  Here's the gist of the setup for updating multiple services:

1. Download the update_directory.py and settings.ini files.
2. Save these files to your local working directory
3. Update a settings.ini file for each of your services.  Create one .ini file for each service.  Place these multiple .ini files in a single directory.  The file names do not matter as long as you keep the .ini extension.  
4. Update the directory variable of update_directory.py to point to the directory holding your multiple .ini files.
5. Run the update_directory.py python script.
