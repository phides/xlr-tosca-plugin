# XL Release TFS plugin

This plugin offers an interface from XL Release to Team Foundation Server to create, update and retrieve Work Items. 

Various APIs are supported:  Team Foundation Power Tools, TFS REST API, and the TFS SDK.

## CI status ##

[![Build Status][xlr-tfs-plugin-travis-image]][xlr-tfs-plugin-travis-url]
[![Codacy Badge][xlr-tfs-plugin-codacy-image] ][xlr-tfs-plugin-codacy-url]
[![Code Climate][xlr-tfs-plugin-code-climate-image] ][xlr-tfs-plugin-code-climate-url]
[![License: MIT][xlr-tfs-plugin-license-image]][xlr-tfs-plugin-license-url]
![Github All Releases][xlr-tfs-plugin-downloads-image]


[xlr-tfs-plugin-travis-image]: https://travis-ci.org/xebialabs-community/xlr-tfs-plugin.svg?branch=master
[xlr-tfs-plugin-travis-url]: https://travis-ci.org/xebialabs-community/xlr-tfs-plugin
[xlr-tfs-plugin-codacy-image]: https://api.codacy.com/project/badge/Grade/b11c699b6164409a93e9cfc8ee318016
[xlr-tfs-plugin-codacy-url]: https://www.codacy.com/app/joris-dewinne/xlr-tfs-plugin
[xlr-tfs-plugin-code-climate-image]: https://codeclimate.com/github/xebialabs-community/xlr-tfs-plugin/badges/gpa.svg
[xlr-tfs-plugin-code-climate-url]: https://codeclimate.com/github/xebialabs-community/xlr-tfs-plugin
[xlr-tfs-plugin-license-image]: https://img.shields.io/badge/License-MIT-yellow.svg
[xlr-tfs-plugin-license-url]: https://opensource.org/licenses/MIT
[xlr-tfs-plugin-downloads-image]: https://img.shields.io/github/downloads/xebialabs-community/xlr-tfs-plugin/total.svg


## Installation ##
+ The plugin https://github.com/xebialabs-community/xlr-tfs-plugin/releases should be placed under `plugins`.
+ Additional config (when using sdk or to run "Test Connection" for the REST API): https://www.microsoft.com/en-us/download/details.aspx?id=22616
  For configuration see this [section](https://github.com/xebialabs-community/xlr-tfs-plugin#tfs-sdk)


## Team Foundation Power Tools
The following actions are supported:

### CreateWorkItem
The CreateWorkItem.py script creates a new Work Item by executing a remote Windows batch wrapper script, CreateWorkItems.bat.  Input parameters are Project, Type, Collection, Title, AssignedTo, and Description; the script returns the number of the Work Item created.  

![screenshot of createWorkItem](images/xlr-tfs2013-plugin-2.png)

### GetWorkItem
The GetWorkItem.py script retrieves a Work Item given its number and collection using the GetWorkItem.bat wrapper script.  Input parameters are workItemNumber and collection.

![screenshot of getWorkItem](images/xlr-tfs2013-plugin-3.png)

### UpdateWorkItem
The UpdateWorkItem.py script updates a Work Item given its number, collection, and set of update fields and values in the format `fieldname1=value1;fieldname2=value2`.  See an example of setting `State=Done` at the end of this document.

![screenshot of updateWorkItem](images/xlr-tfs2013-plugin-4.png)

### Notes:  
The TFS machine must have Microsoft Visual Studio Team Foundation Server 2013 Update 2 Power Tools installed.  

The CreateWorkItem.bat, GetWorkItem.bat, and UpdateWorkItem.bat scripts must be placed in a location on the TFS machine.  The default location is C:\xlr-tfs2013-plugin.

A field is provided for the Windows CIFS port (default is 445) to allow overriding a blocked port.

The functionality will be enhanced as specific needs materialize.

## TFS REST API

This plugin offers an interface from XL Release to Team Foundation Server via the REST API valid for work items in TFS 2015.  It provides:

**CreateWorkItem.py** -- creates a Work Item given Collection, Project, Type and Title parameters

**AddWorkItemComment.py** -- adds a Work Item comment given Collection, Work Item Id and Comment parameters

**GetTfsRepoArtifacts.py** -- retrieves an item from a TFS Git repository given the collection and scopepath (assumes a single repository and a single match)

**QueueBuild.py** -- Queue a build using the REST api.

The functionality will be enriched with additional Work Item fields as specific needs materialize.

To test the connection on the Shared Configuration screen, you will need the SDK set up. 

## TFS Triggers

Using the REST API, the Plugin allows you to poll TFS repositories and trigger XL Release on changes. 

**GitCommitTrigger.py** -- Given a Collection, Project, Repo and Branch, will trigger a release if the latest commit ID changes. 

**TfvcChangesetTrigger.py** -- Given a Collection and Project, will trigger a release if the latest changeset ID changes. 

## TFS SDK

The TFS SDK depends on the following configuration changes in XL Release:

Script.policy file — confirm these lines:

```
permission  java.util.PropertyPermission "\*", "read, write";
permission java.lang.RuntimePermission "shutdownHooks";
permission java.io.FilePermission "conf/\*", "read";
permission java.io.FilePermission "lib/\*", "read";
permission java.io.FilePermission "plugins/\*", "read";
permission java.security.AllPermission;
```

Add the library com.microsoft.tfs.sdk-11.0.0.jar to /plugins.

Unzip native.zip in the <xl-release-server>/conf directory. 

### Example configuration, TFPT


**Shared Configuration**

You can specify the URL for your TFS Server, Username , password , domain. Then you can specify the authentication type as Basic or NTLM dependening on the type of account you are using.
The Depending on whether you are using TFSSDK( for TFS 2010-2013) or TFSREST API (2015), you can specify the value in dropdown. This will help to test the connection to target. Doesn't do test connection for CLI.

![screenshot of shared Configuration](images/xlr-tfs2013-plugin-010.png)

**Task Usage**

Here is a basic workflow of four items to create a Work Item, then retrieve it, update it, and retrieve it again to see the modification.

![screenshot of release template](images/xlr-tfs2013-plugin-1.png)

The variables appearing in the above screenshots are set in this manner:

![screenshot of release variables](images/xlr-tfs2013-plugin-5.png)

Note that workItemNumber is set as an output variable by the createWorkItem task.

Successful execution of the release results in the following output:

**Create Work Item**

![screenshot of createWorkItem output](images/xlr-tfs2013-plugin-6.png)

**Get Work Item, note State=New**

![screenshot of createWorkItem output](images/xlr-tfs2013-plugin-7.png)

**Update Work Item**

![screenshot of createWorkItem output](images/xlr-tfs2013-plugin-8.png)

**Get Work Item, note State=Done**

![screenshot of createWorkItem output](images/xlr-tfs2013-plugin-9.png)


## References ##
+ [TFS Rest api](https://www.visualstudio.com/en-us/docs/integrate/api/overview)
