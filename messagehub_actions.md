---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-13"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}

# {{site.data.keyword.messagehub}} package
{: #openwhisk_catalog_message_hub}

A package that enables communication with [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub) instances for publishing and consuming messages by using the native high-performance Kafka API.
{: shortdesc}

## Setting up a {{site.data.keyword.messagehub}} package using {{site.data.keyword.Bluemix_notm}}
{: #create_message_hub_ibm}

1. Create an instance of {{site.data.keyword.messagehub}} service under your current organization and space that you are using for {{site.data.keyword.openwhisk}}.

2. Verify that the topic you want to listen to is available in {{site.data.keyword.messagehub}} or create a new topic, for example, titled **mytopic**.

3. Refresh the packages in your namespace. The refresh automatically creates a package binding for the {{site.data.keyword.messagehub}} service instance that you created.
  ```
  ibmcloud fn package refresh
  ```
  {: pre}

  Example output:
  ```
  created bindings:
  Bluemix_Message_Hub_Credentials-1
  ```
  {: screen}

4. List the packages in your namespace to show that your package binding is now available.
  ```
  ibmcloud fn package list
  ```
  {: pre}

  Example output:
  ```
  packages
  /myBluemixOrg_myBluemixSpace/Bluemix_Message_Hub_Credentials-1 private
  ```
  {: screen}

  Your package binding now contains the credentials that are associated with your {{site.data.keyword.messagehub}} instance.

## Setting up a {{site.data.keyword.messagehub}} package outside {{site.data.keyword.Bluemix_notm}}

If you want to set up your {{site.data.keyword.messagehub}} outside of {{site.data.keyword.Bluemix_notm}}, you must manually create a package binding for your {{site.data.keyword.messagehub}} service. You need the {{site.data.keyword.messagehub}} service credentials and connection information.

Create a package binding that is configured for your {{site.data.keyword.messagehub}} service.
```
ibmcloud fn package bind /whisk.system/messaging myMessageHub -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p kafka_admin_url https://kafka-admin-prod01.messagehub.services.us-south.bluemix.net:443
```
{: pre}

## Listening for messages using events

For detailed information about how to use triggers in {{site.data.keyword.messagehub}} to listen for messages, see the following
[{{site.data.keyword.messagehub}} events source](./openwhisk_messagehub.html) topic where the following tasks are covered:
* [Creating a trigger that listens to a {{site.data.keyword.messagehub}} instance](./openwhisk_messagehub.html#create_message_hub_trigger)
* [Creating a trigger for a {{site.data.keyword.messagehub}} package outside {{site.data.keyword.Bluemix_notm}}](./openwhisk_messagehub.html#create_message_hub_trigger_outside)
* [Listening for messages](./openwhisk_messagehub.html#message_hub_listen)
* [Examples](./openwhisk_messagehub.html#examples)

## Producing messages to {{site.data.keyword.messagehub}}
{: #producing_messages}

The `/messaging/messageHubProduce` action is deprecated and will be removed at a future date. To maintain optimal performance, migrate your usage of the `/messaging/messageHubProduce` action to use a persistent connection when data is produced to {{site.data.keyword.messagehub}}/Kafka.
{: tip}

If you would like to use a {{site.data.keyword.openwhisk_short}} action to conveniently produce a message to {{site.data.keyword.messagehub}}, you can use the `/messaging/messageHubProduce` action. This action takes the following parameters:

|Name|Type|Description|
|---|---|---|
|kafka_brokers_sasl|JSON Array of Strings|This parameter is an array of `<host>:<port>` strings that comprise the brokers in your {{site.data.keyword.messagehub}} instance.|
|user|String|Your {{site.data.keyword.messagehub}} username.|
|password|String|Your {{site.data.keyword.messagehub}} password.|
|topic|String|The topic that you would like the trigger to listen to.|
|value|String|The value for the message you would like to produce.|
|key|String (Optional)|The key for the message you would like to produce.|

While the first three parameters can be automatically bound by using `ibmcloud fn package refresh`, see the following example that invokes the action with all necessary parameters:
```
ibmcloud fn action invoke /messaging/messageHubProduce -p kafka_brokers_sasl "[\"kafka01-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka02-prod01.messagehub.services.us-south.bluemix.net:9093\", \"kafka03-prod01.messagehub.services.us-south.bluemix.net:9093\"]" -p topic mytopic -p user <your {{site.data.keyword.messagehub}} user> -p password <your {{site.data.keyword.messagehub}} password> -p value "This is the content of my message"
```
{: pre}

## References
- [{{site.data.keyword.messagehub_full}}](https://developer.ibm.com/messaging/message-hub/)
- [Apache Kafka](https://kafka.apache.org/)
