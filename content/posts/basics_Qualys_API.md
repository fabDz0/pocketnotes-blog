---
title: "Qualys API basics"
date: 2019-05-27T08:08:19+05:30
draft: false
tags: ["Qualys","API","PowerShell","how-to"]
---
# Basics of Qualys API

### Introduction
Qualys has published a detailed guide on using [API](https://www.qualys.com/docs/qualys-api-vmpc-user-guide.pdf). Primarily this blog will concentrate on "cURL" and "PowerShell" as the scripting language to query the API. 

> **Note on API limits:**
Qualys has applied rate limit on API calls based on subscription.
Reading API limit is easy, look for *X-Powered-By* header in http-reponse headers,\
**X-RateLimit-Limit:** API calls limit\
**X-RateLimit-Window-Sec:/60*60**" hrs.\
Ex:  if "X-RateLimit-Limit: 100"\
          "X-RateLimit-Window-Sec: 86400"\

Then,<!--more-->
**100** API calls every (86400/3600) 24 hrs.

To check API call limit,

```curl
c:\ curl -s -I -D header.txt -u username:password --insecure "https://qualysapi.qualys.com/msp/about.php"
```
More about API limit is available in this [thread](https://discussions.qualys.com/thread/2064).

### Getting Started
Qualys support REST API calls to query data, these calls has to be an authenticated call. Currently as per the documentation both basic all well as session based authentication is supported. Qualys URL's used here are based on a certain [platform](https://community.qualys.com/docs/DOC-4172-identify-your-qualys-platform). Change this based on your subscription.

#### Establish connection using cURL
Basic Authentication no session is established. "*cURL* connects and collects the data requested for.

*  -H "X-Requested-With: Curl"   --> "X-requested-with" is mandatory\
*  -u "username:password"        --> Qualys username and password\
*  "- -insecure"                    --> for bypassing certificate errors while connecting

```curl
curl -H "X-Requested-With: Curl " -X "POST" -F truncation_limit=10 -u "username:password" --insecure "https://qualysapi.qg2.apps.qualys.com/api/2.0/fo/asset/host/?action=list"
```
*truncation_limit*  defines the count of records to fetch. By default 1000 records are fetched.\
The basic URL - `https://qualysapi.qg2.apps.qualys.com/api/2.0/fo/` remains same. Based on resources queried the URL changes after this basic URL.

`https://qualysapi.qg2.apps.qualys.com/api/2.0/fo/scan/?action=list`
This gives list of VM scans performed in last 30 days.

<hr>

#### Establish Connection with PowerShell

This script will prompt user for username and password and set headers. 
```powershell
$username = Read-Host "Enter username"
$password = Read-Host "Enter password"
$headerValue = @{"X-Requested-With"="PowerShell"}
```
Setting basic URL to a variable, and setting content of body for powershell request
```powershell
$basicURL = "https://qualysapi.qg2.apps.qualys.com/api/2.0/fo/"
$body = "action=login&username=$username&password=$password"
```
**Make requests**
Will use *Invoke-RestMethod* cmdlet to query the API. 


```powershell
Invoke-RestMethod -Headers $headerValue -Uri "$basicURL/session" -Method Post -Body $body -SessionVariable sessionValue
```
-Uri --> URL to send the query
-Method --> method to be used to send the query (POST here)
- SessionVariable --> Store contents of session to a variable (sessionValue is the variable here)
-Body --> content of body to be send in query

This will establish connection and `$sessionValue` can be reused when new requests are made.

#### Request to get Asset records
Putting it together, this script will get list of assets from Qualys using API. Count of assets depends on *"truncation limit"* header

```powershell
#Set initial parameters
#Additional parameter here is *truncation_limit*, this sets the limit of records to be fetched. Used especially while querying for asset records.

$username = Read-Host "Enter username"
$password = Read-Host "Enter password"
$headerValue = @{"X-Requested-With"="PowerShell","truncation_limit=10"}

#Set URL and content of body
$basicURL = "https://qualysapi.qg2.apps.qualys.com/api/2.0/fo/"
$body = "action=login&username=$username&password=$password"

#make request to get asset records
Invoke-RestMethod -Headers $headerValue -Method Post -Body $body 
-WebSession $sessionValue -Uri "$basicURL/asset/host/?action=list"

# logout of session
Invoke-RestMethod -Headers $headerValue -Method Post -WebSession $sessionValue -Uri "$basicURL/session/?action=logout"

```

<hr>









