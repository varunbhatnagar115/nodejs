---

copyright:
  years: 2017
lastupdated: "2017-09-18"

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# SDK for Node.js troubleshooting
{: #ts}


Here are the answers to common troubleshooting questions about using SDK for Node.js on {{site.data.keyword.Bluemix}}.
{:shortdesc}

## Application fails to start with a “No space left on device” error
{: #no_space_left_on_device}


A Node.js application fails to start with a “No space left on device” error. For example, the error in the logs might look like the following:
{: tsSymptoms}

```
   2017-01-16T14:25:14.61-0500 [CELL/0]     ERR tar: ./app/node_modules/pm2/node_modules/cron/node_modules/moment-timezone/LICENSE: Cannot write: No space left on device

```
{: #codeblock}

Node.js applications using NPM versions prior to version 3 consume more space downloading dependencies.
{: tsCauses}

Modify the package.json file for your application to use an NPM version 3 or greater.
{: tsResolve}

```
{
  "name": "myapp",
  "description": "this is my app",
  "version": "0.1",
  "engines": {
     "node": "4.2.4",
     "npm": "3.10.10"
  }
}
```
{: codeblock}

## Application restarts due to memory contraints
{: #oom}

Node.js does not know how much memory is available to the application, so the garbage collector might not run before memory is exhausted.

```
2017-09-01T11:00:42.19-0400 [APP/PROC/WEB/0]OUT Exit status 137
2017-09-01T11:00:42.23-0400 [CELL/0]     OUT Exit status 0
2017-09-01T11:00:42.27-0400 [CELL/0]     OUT Destroying container
2017-09-01T11:00:42.34-0400 [API/0]      OUT Process has crashed with type: "web"
2017-09-01T11:00:42.36-0400 [API/0]      OUT App instance exited with guid eecfba3b-430c-4a6b-b71f-ac72816fe152 payload: {"instance"=>"77dbb981-16d0-3a05-3235-9a4b", "index"=>0, "reason"=>"CRASHED", "exit_description"=>"2 error(s) occurred:\n\n* 2 error(s) occurred:\n\n* Exited with status 137 (out of memory)\n* cancelled\n* cancelled", "crash_count"=>1, "crash_timestamp"=>1504278042244633291, "version"=>"6497b5b5-67d4-4c5a-b1af-362e522a029d"}
2017-09-01T11:00:43.35-0400 [CELL/0]     OUT Successfully destroyed container
```
{: codeblock}

A possible solution is to set the `--max_old_space_size` option on the application's start command in the package.json file. This option represents part of the application's memory footprint and should be set to a value less than the total memory available to the application. Read about [Large memory spikes and Heroku](https://github.com/nodejs/node/issues/3370) for a more in-depth discussion of this topic.
```
  "scripts": {
    "start": "node --max_old_space_size=800 server.js"
  }
```
{: codeblock}
