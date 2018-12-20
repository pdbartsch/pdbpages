---
title: "Export ArcGIS Data Driven Pages & Sync Using Python and Google Drive"
date: 2018-12-20T13:00:23-08:00
publishdate: 2018-12-20
lastmod: 2018-12-20
draft: false
tags: ['ArcPy','ArcGIS-Desktop', 'Google-Drive', 'Python']
---
### Quick Background:
I work at UC Santa Barbara and one of my many jobs is to keep our underground utility atlas up to date.  This atlas is accessed in multiple formats including PDF, desktop, and mobile applications.  The campus atlas sheets are broken up to 500x500 foot grids which each represent a single sheet when printed or viewed as a PDF.  The entire Santa Barbara campus makes up just over 300 of these grids. The data and sheet layout for these is currently broken up into two ArcMap documents, one representing main campus and one for west campus.

### PDF Based Index Map:
One method of making these accessible to the people that need the data is a simple web map with the campus broken up into grids.  Each grid has a hyperlink to the corresponding PDF atlas sheet. The [site is live here](www.arcgis.com/apps/webappviewer/index.html?id=f378ea1bbbe34a2eae12793d24f08c66&extent=-13344039.8359,4083428.4503,-13339453.6142,4085709.6178,102100) (you won't be able to download atlas sheets unless you're an also an employee of UCSB).

### The Problem:
Keeping over 300 of these PDFs up to date in addition to keeping multiple other formats of the same data in sync became cumbersome.

### My Solution:
1. Host PDFs on Google Drive.  Sync hosted with desktop using Google File Stream App.
2. Use ArcGIS Desktop data driven pages
3. Export updated PDFs to local Google File Stream Folder which then stays in sync with the hosted file.  The hyperlink to these sheets remains the same because the file versions are changed but the files are not replaced with new files as long as the naming is consistent.
4. Creat a Python script to automate the export of these PDFs

### The Python piece:
- Python 2 because it still has to be to interact with ArcGIS Desktop.  I'll move this over to Pro at some point.
- Was initially multiple scripts. One for each of the MXD documents.  One set for printing all pages and another for printing a selection of pages.
- I recently simplified it to be one script which replaces all the functionality of the previous multiple by asking for more user input.
- The [code is available here](https://github.com/pdbartsch/geospatial-code-snippets/blob/master/dd_pages_export.py) and also reproduced below. I commented it heavily and removed anything that needed to remain private to UCSB, so please go ahead and clone yourself a copy. Hopefully it can help you out with a similar task.


{{< highlight python >}}
# Name: dd_pages_export.py  - Data Driven Page(s) to PDF
# Description: exports either a single data Driven Page or all pages from a .mxd document to pdf(s). Replaces multiple previous scripts in favor of user input.
# Author: Paul Bartsch  - updated December 2018
# Intent: for use at UCSB to keep hosted PDF atlas sheets up to date with GIS data and ArcGIS Online Services (see other documents for help keeping online services in sync.)
# Python 2 because this one is intended for use with ArcGIS Desktop maps that have not yet been imported to ArcGIS Pro

## IMPORTS
import arcpy

## CONSTANTS
# out_path
out_path = r"I:\\My Drive\\ATLAS\\Atlas Export\\" # My Google File Stream Location
# quality of the output pdf
quality = "BEST" #Options include: BEST BETTER NORMAL FASTER FASTEST
res = 150
# two possible mxd documents. Each setup for data driven pages.
main = r"G:\GIS_Projects\Util_Atlas_Updates\Util_Atlas_Print_DO_NOT_EDIT.mxd"
west = r"G:\GIS_Projects\Util_Atlas_Updates\Util_Atlas_West_Print_DO_NOT_EDIT.mxd"

## DEFAULTS
in_mxd = main
out_prefix = "ucsbatlas"
# selects mxd of either default choice or west campus choice based on user input

## USER INPUTS
# py2 uses raw_input() and py3 uses input()
# because of above this works for ArcGIS Desktop but would need adapting for ArcGIS Pro
mode = raw_input("Please select Mode: 1)Export All Mode or 2)Single Page Mode [1/2]? : ")

## DEFINE FUNCTIONS
# export all data driven pages in chosen mxd document to PDF
def export_multi():
    mxd = arcpy.mapping.MapDocument(in_mxd)
    # pc = mxd.dataDrivenPages.pageCount
    for pageNum in range(first, last):
        mxd.dataDrivenPages.currentPageID = pageNum
        print("Exporting " +descrip+ " page {0} of {1}".format(str(mxd.dataDrivenPages.currentPageID), str(last - first)))
        arcpy.mapping.ExportToPDF(mxd, out_path + out_prefix + str("%03d" % (pageNum)) + ".pdf", resolution = res, image_quality = quality)
    # cleanup
    del mxd

# export a single page to PDF based on user input values
def export_single():
    mxd = arcpy.mapping.MapDocument(in_mxd)
    # correct the number for use in file naming and messaging based on UCSB grid naming convention
    # (e.g. we have a grid 001 and a grid W001)
    pg = int(grid)
    # export to pdf
    # my workflow exports to a Google Drive File Stream location for automatic upload and
    # update of an online atlas lookup that our employees use to access underground utility grids in PDF form.
    mxd.dataDrivenPages.currentPageID = page
    print("Exporting " + out_prefix + str("%03d" % pg) + ".pdf to: " + out_path)
    arcpy.mapping.ExportToPDF(mxd, out_path + out_prefix + str("%03d" % pg) + ".pdf", resolution = res, image_quality = quality)
    # cleanup
    del mxd

# logic based on user input choices
if mode == '1':
    print('All')
    area = raw_input("Please select either: 1)Main Campus or 2)West Campus. [1/2]? : ")
    single = False
elif mode == '2':
    print('single page mode')
    area = raw_input("Please select either: 1)Main Campus or 2)West Campus. [1/2]? : ")
    grid = raw_input("Please enter a single grid # to export : ")
    single = True
else:
    print("That's not an option.  Exiting script now.  Simply run script again to try again.")
    exit()

# set variables and run appropriate function based on user choices
if single and area == '2': # west campus single mode
    in_mxd = west
    out_prefix = "ucsbatlasW"
    page = int(grid) + 118 #correct the number based on UCSB grid naming convention
    export_single()
elif single: # main campus single mode
    # because Python 2 raw_input is a string
    # go with defaults other than that
    page = int(grid)
    export_single()
elif not single and area =='2': # west campus print all
    in_mxd = west
    out_prefix = "ucsbatlasW"
    first = 119
    last = 328
    descrip = 'West Campus Atlas '
    export_multi()
else: # main campus print all
    first = 1
    last = 119
    descrip = 'Main Campus Atlas '
    export_multi()
{{< /highlight >}}
