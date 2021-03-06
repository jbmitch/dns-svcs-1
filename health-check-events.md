---

copyright:
  years: 2020
lastupdated: "2020-11-03"

keywords:

subcollection: dns-svcs

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Creating health check events
{:#health-check-events}

When using global load balancing, you can create a health check to specify how the origin's health is monitored. Health check events are status changes from origin pools that have connected health checks and their associated origin servers. If an origin's status changes, a new event is recorded with the event's description.

IBM Log Analysis with LogDNA manages system and application logs in the IBM Cloud. You can use this service to access health check events for your origin pools and origin servers. For more information, see the [Getting started tutorial](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-getting-started) for IBM Log Analysis with LogDNA.

## Health check event properties
{:#health-check-event-properties}

Health check event records have the following properties:
 - `event_time`: The time at which the event was recorded. For example, `2020-10-28T15:04:05.00+0000`.
 - `logSourceCRN`: CRN of the DNS Services instance in which the origin pools and orgin servers were created.
   For example: `crn:v1:bluemix:public:dns-svcs:global:a/bcf1865e99742d38d2d5fc3fb80a5496:85ed7b9d-cd48-4881-b354-52eb1d8e9011::`
 - `type`: The value of this property is `health_check_event` for health check events.
 - `origins`: An array of objects representing the origin servers associated with the orgin pool. For example, 
   
   ```
   [
        {
            "name": "web-server1",
            "address": "10.240.0.4",
            "enabled": true,
            "healthy": true,
            "overall_health": true,
            "health_failure_reason": "SUCCESS",
            "changed": true
        },
        {
            "name": "web-server2",
            "address": "10.240.100.4",
            "enabled": true,
            "overall_health": false,
            "health_failure_reason": "CONNECTION_ERROR",
            "changed": false
        }
    ]
   ```
   {:codeblock}

 - `pool`: An object that represents the orgin pool for which the health check event was generated. For example,
    
    ```
    {
        "id": "987f3f96-c077-4124-87eb-846dc026e383",
        "name": "us-south-pool",
        "healthy_origins_threshold": 1,
        "enabled": true,
        "healthy": "DEGRADED",
        "changed": false
    }
    ```
    {:codeblock}

## Viewing health check events
{:#view-health-check-events}


### Before you begin
{: #logdna-preparation}

To view health check events in LogDNA, make sure that the following prerequisites are met.

* An IBM Log Analysis with LogDNA instance is created in the Frankfurt region in the account.


### Receiving health check events with IBM Log Analysis with a LogDNA instance
{: #receiving-health-check-events-logdna}

To view health check events in a LogDNA instance, you must configure the LogDNA instance to receive platform logs with the following steps.

1. In the {{site.data.keyword.cloud_notm}} UI, click the **Menu** icon ![Menu icon](../icons/icon_hamburger.svg) &gt; **Observability**. The Observability dashboard appears.
2. Select **Logging**. The list of logging instances appears.
3. Click **Configure platform logs** button.
4. Select the `Frankfurt` region, and then select one LogDNA instance where you want to receive the platform logs.
5. Click **Save**.

#### Viewing health check events in the IBM Log Analysis with LogDNA instance
{: #viewing-healtch-check-events-logdna}

To view health check events, use the UI of the LogDNA instance that you configured to receive the platform logs in the previous steps.

For more information, see [Launching the LogDNA web UI through the IBM Cloud UI](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-launch#launch_cloud_ui).

To filter health check events from within the logging instance, enter the health check event type `health_check_event` in the **Search** field.

![LogDNA Source filter](images/health-check-type-filter.png)

You can also filter the events you want by combining other event fields. For example:

- Filter health check events for a specific DNS Services instance.
![filter events by CRN](images/health-check-type-filter-crn.png)

- Filter health check events for a pool by specifying its `name` and its DNS Services instance.
![filter events by CRN and pool name](images/health-check-type-filter-crn-pool.png)

- Filter health check events for an origin by specifying its `name` and its DNS Services instance.
![filter events by CRN and origin name](images/health-check-type-filter-crn-origin.png)

- Filter health check events for an origin when its status becomes healthy by specifying its `name` and `overall_health` fields and also its DNS Services instance.
![filter events by CRN, origin name and health](images/health-check-type-filter-crn-origin-health.png)

- Filter health check events for a pool when its status becomes DEGRADED by specifying its `name` and `healthy` fields and also its DNS Services instance.
![filter events by CRN, pool name and health](images/health-check-type-filter-crn-pool-health.png)


 


