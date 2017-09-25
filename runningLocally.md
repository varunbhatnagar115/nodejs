---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Run the Node.js application locally
{: #hints}

Set a port number to run your Node.js application locally without causing conflicts when you run it on {{site.data.keyword.Bluemix}}.
{: shortdesc}

When the application is running on Bluemix, the PORT environment variable is allocated by Cloud Foundry. However, when the application is running locally, PORT is not defined, so you can define the port for your application. To avoid conflicts, define the port that your app listens to locally something different than the port used by Bluemix.

In the following example for a **js** file, **3000** is used as the port number. By using **3000**, you can run the application locally for testing purposes and on Bluemix without making changes.

```
var port = (process.env.PORT || 3000);
```
{: codeblock}
