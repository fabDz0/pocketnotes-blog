---
title: " FolderPaths and FilesNames - PowerShell"
date: 2019-05-27T08:25:11+05:30
draft: false
tags: ["how-to","PowerShell"]
---
# FolderPaths and FilesNames - PowerShell

Loading folder path and filenames manually gives many errors. Spaces, spelling, different file paths and many others add to these. One way to avoid this is to allow the user to choose the path and filenames. Unless these are some predefined file/folderpaths which are static everywhere.

In this blog post two PowerShell functions will be defined for this purposes.

#### Folder Path
This script will create a pop-up window to select folder path.
<!--more-->
```powershell
function Select-Folder{
Add-Type -AssemblyName System.Windows.Forms
$Global:FolderBrowser=New-Object System.Windows.Forms.FolderBrowserDialog
#Newfolder option is removed with below code
$FolderBrowser.ShowNewFolderButton = $false
#Open folder browser dialog
$null = $FolderBrowser.Dialog()
$FileBrowser.Dispose()
}
Select-Folder
```
To get the 📁 folder path,

$FolderBrowser.SelectedPath

<hr>

#### FileNames
```powershell
function select-filename{
Add-Type -AssemblyName System.Windows.forms
#Define a new object
$Global:FileBrowser = New-Object System.Windows.Forms.OpenFileDialog
#Define file dialog parameters
#Set initial directory 
$FileBrowser.InitialDirectory="$env:USERPROFILE"
#Ensure only one file is selected
$FileBrowser.MultiSelect = $false
#Defining Title, this is title of the pop-up which opens
$FileBrowser.Title = " Locate the file..."

#open selection window
$null = $FileBrowser.ShowDialog()
}
Select-Filename
```
To get filename, 
$FileBrowser.Filename




