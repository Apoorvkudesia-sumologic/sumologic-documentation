---
id: nginx-ingress
title: Nginx Ingress
sidebar_label: Nginx Ingress
description: The Sumo Logic app for Nginx Ingress helps you monitor webserver activity in Nginx Ingress Controller.
---

import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('img/integrations/web-servers/nginx-ingress.png')} alt="Thumbnail icon" width="75"/>

The Nginx Ingress app is a unified logs and metrics app that helps you monitor the availability, performance, health, and resource utilization of your Nginx Ingress web servers. Preconfigured dashboards and searches provide insight into connections, requests, ingress controller metrics, visitor locations, visitor access types, traffic patterns, errors, web server operations, and access from known malicious sources.

This app is tested with the following Nginx Ingress versions:
* For Kubernetes environments: Nginx version 1.21.3

## Log and metrics types

The Sumo Logic app for Nginx Ingress assumes the NCSA extended/combined log file format for Access logs and the default Nginx error log file format for error logs.

All [Dashboards](#viewing-nginx-ingress-dashboards) (except the Error logs Analysis dashboard) assume the Access log format. The Error Logs Analysis Dashboard assumes both Access and Error log formats, so as to correlate information between the two.

For more details on Nginx logs, see [Module ngx_http_log_module](http://nginx.org/en/docs/http/ngx_http_log_module.html).

The Sumo Logic app for Nginx Ingress assumes Prometheus format Metrics for Requests, Connections, and Ingress controller.

For more details on Nginx Ingress Metrics, see [Prometheus](https://docs.nginx.com/nginx-ingress-controller/logging-and-monitoring/prometheus/).


## Collecting logs and metrics for Nginx Ingress

This section provides instructions for configuring log and metric collection for the Sumo Logic app for Nginx Ingress.

In the Kubernetes environment, we use our Sumo Logic Kubernetes collection. You can learn more about this [here](/docs/observability/kubernetes/collection-setup).

Configuring log and metric collection for the Nginx Ingress app includes the following tasks:

### Configure Nginx Ingress Logs and Metrics Collection

Sumo Logic supports the collection of logs and metrics data from Nginx Ingress in Kubernetes environments.

:::note Prerequisites
It’s assumed that you are using the latest helm chart version if not please upgrade using the instructions [here](/docs/send-data/kubernetes).
:::

1. Before you can configure Sumo Logic to ingest metrics, you must enable the Prometheus metrics in the Nginx Ingress controller and annotate the Nginx Ingress pods, so Prometheus can find the Nginx Ingress metrics. For instructions on Nginx Open Source, refer to [this documentation](https://docs.nginx.com/nginx-ingress-controller/logging-and-monitoring/prometheus/).
2. Ensure you have deployed the [Sumologic-Kubernetes-Collection](https://github.com/SumoLogic/sumologic-kubernetes-collection), to send the logs and metrics to Sumologic. For more information on deploying Sumologic-Kubernetes-Collection, [visit here](/docs/send-data/kubernetes/install-helm-chart). Once deployed, logs will automatically be picked up and sent by default. Prometheus will scrape the Nginx Ingress pods, based on the annotations set in Step 1, for the metrics. Logs and Metrics will automatically be sent to the respective [Sumo Logic Distribution for OpenTelemetry Collector](https://github.com/SumoLogic/sumologic-otel-collector) instances, which consistently tag your logs and metrics, then forward them to your Sumo Logic org.
3. Apply the following labels to the Nginx Ingress pod.
  ```sql
  environment="prod_CHANGEME"
  component="webserver"
    webserver_system="nginx_ingress"
    webserver_farm="<farm_CHANGEME>"
  ```
  Enter in values for the following parameters (marked in bold and **`CHANGE_ME`** above):
     * `environment`. This is the deployment environment where the Nginx Ingress farm identified by the value of servers resides. For example:- dev, prod, or QA. While this value is optional we highly recommend setting it.
     * `webserver_farm` - Enter a name to identify this Nginx Ingress farm. This farm name will be shown in the Sumo Logic dashboards. If you haven’t defined a farm in Nginx Ingress, then enter ‘default’ for `webserver_farm`.

  **Do not modify** the following values set by this configuration as it will cause the Sumo Logic app to not function correctly.
     * `component: “webserver”`. This value is used by Sumo Logic apps to identify application components.
     * `webserver_system: “nginx_ingress”`. This value identifies the database system.
<br/>
**FER to normalize the fields in Kubernetes environments**. Labels created in Kubernetes environments automatically are prefixed with `pod_labels`. To normalize these for our app to work, a Field Extraction Rule named **AppObservabilityNginxIngressWebserverFER** is automatically created for Nginx Application Components.
<br/>
## Installing the Nginx Ingress app
import AppInstall2 from '../../reuse/apps/app-install-sc-k8s.md';

<AppInstall2/>

Additionally, if you're using Nginx Ingress in the Kubernetes environment, the following fields will be created automatically during the app installation process:

* `pod_labels_component`
* `pod_labels_environment`
* `pod_labels_webserver_system`
* `pod_labels_webserver_farm`

## Viewing Nginx Ingress dashboards
import ViewDashboards from '../../reuse/apps/view-dashboards.md';

<ViewDashboards/>

### Overview

The **Nginx Ingress - Overview** dashboard provides an at-a-glance view of the NGINX server access locations, error logs, and connection metrics.

Use this dashboard to:
* Gain insights into originated traffic location by region. This can help you allocate computer resources to different regions according to their needs.
* Gain insights into your Nginx health using Critical Errors and Status of Nginx Server.
* Get insights into Active and dropped connections.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Overview.png')} alt="Nginx-Overview" />

### Error Logs


The **Nginx Ingress - Error Logs Analysis** Dashboard provides a high-level view of log level breakdowns, comparisons, and trends. The panels also show the geographic locations of clients and clients with critical messages, new connections, outliers, client requests, request trends, and request outliers.

Use this dashboard to:

* Track requests from clients. A request is a message asking for a resource, such as a page or an image.
* To track and view client geographic locations generating errors.
* Track critical alerts and emergency error alerts.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Error-Logs.png')} alt="Nginx-Ingress-Error-Logs" />

### Trends

The **Nginx Ingress - Logs Timeline Analysis** dashboard provides a high-level view of the activity and health of Nginx servers on your network. Dashboard panels display visual graphs and detailed information on traffic volume and distribution, responses over time, as well as time comparisons for visitor locations and server hits.

Use this dashboard to:

* To understand the traffic distribution across servers, provide insights for resource planning by analyzing data volume and bytes served.
* Gain insights into originated traffic location by region. This can help you allocate compute resources to different regions according to their needs.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Trends.png')} alt="Nginx-Ingress-Trends" />

### Outlier Analysis

The **Nginx Ingress -  Outlier Analysis** dashboard provides a high-level view of Nginx server outlier metrics for bytes served, number of visitors, and server errors. You can select the time interval over which outliers are aggregated, then hover the cursor over the graph to display detailed information for that point in time.

Use this dashboard to:

* Detect outliers in your infrastructure with Sumo Logic’s machine-learning algorithm.
* To identify outliers in incoming traffic and the number of errors encountered by your servers.

You can use schedule searches to send alerts to yourself whenever there is an outlier detected by Sumo Logic.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Outlier-Analysis.png')} alt="Nginx-Ingress-Outlier-Analysis" />

### Threat Intel

The **Nginx Ingress - Threat Intel** dashboard provides an at-a-glance view of threats to Nginx servers on your network. Dashboard panels display the threat count over a selected time period, geographic locations where threats occurred, source breakdown, actors responsible for threats, severity, and a correlation of IP addresses, method, and status code of threats.

Use this dashboard to:
* To gain insights and understand threats in incoming traffic and discover potential IOCs. Incoming traffic requests are analyzed using Sumo Logic [threat intelligence](/docs/security/threat-intelligence/).

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Threat-Intel.png')} alt="Nginx-Ingress-Threat-Intel" />

### Web Server Operations

The Nginx - Web Server Operations dashboard provides a high-level view combined with detailed information on the top ten bots, geographic locations, and data for clients with high error rates, server errors over time, and non 200 response code status codes. Dashboard panels also show information on server error logs, error log levels, error responses by a server, and the top URIs responsible for 404 responses.

Use this dashboard to:
* Gain insights into Client and Server Responses on the Nginx Server. This helps you identify errors in the Nginx Server.
* To identify geolocations of all Client errors. This helps you identify client locations causing errors and helps you to block client IPs.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Web-Server-Operations.png')} alt="Nginx-Ingress-Web-Server-Operations" />

### Visitor Access Types

The **Nginx Ingress - Visitor Access Types** dashboard provides insights into visitor platform types, browsers, and operating systems, as well as the most popular mobile devices, PC and Mac versions used.

Use this dashboard to:
* Understand which platform and browsers are used to gain access to your infrastructure.
* These insights can be useful for planning in which browsers, platforms, and operating systems (OS) should be supported by different software services.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Visitor-Access-Types.png')} alt="Nginx-Ingress-Visitor-Access-Types" />

### Visitor Locations

The **Nginx Ingress - Visitor Locations** dashboard provides a high-level view of Nginx visitor geographic locations both worldwide and in the United States. Dashboard panels also show graphic trends for visits by country over time and visits by  US region over time.

Use this dashboard to:
* Gain insights into the geographic locations of your user base.  This is useful for resource planning in different regions across the globe.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Visitor-Locations.png')} alt="Nginx-Ingress-Visitor-Locations" />

### Visitor Traffic Insight

The **Nginx Ingress - Visitor Traffic Insight** dashboard provides detailed information on the top documents accessed, top referrers, top search terms from popular search engines, and the media types served.

Use this dashboard to:
* To understand the type of content that is frequently requested by users.
* It helps in allocating IT resources according to the content types.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Visitor-Traffic-Insight.png')} alt="Nginx-Ingress-Visitor-Traffic-Insight" />

### Connections and Requests Metrics

The **Nginx Ingress - Connections and Requests Metrics** dashboard provides insight into active, dropped connections, reading, writing, and waiting requests.

Use this dashboard to:

* Gain information about active and dropped connections. This helps you identify the connection rejected by the Nginx Server.
* Gain information about the total requests handled by Nginx Server per second. This helps you understand read, and write requests on the Nginx Server.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Connections-and-Requests-Metrics.png')} alt="Nginx-Ingress-Connections-and-Requests-Metrics" />

### Controller Metrics

The **Nginx Ingress - Ingress Controller Metrics** dashboard gives you insight into the status, reloads, and failure of the Kubernetes Nginx ingress controller.

Use this dashboard to:
* Gain information about Nginx ingress Controller status and reloads. This helps you understand the availability of Nginx Ingress controllers.
* Gain information about Nginx reload time and any reload errors.

<img src={useBaseUrl('img/integrations/web-servers/Nginx-Ingress-Controller-Metrics.png')} alt="Nginx-Ingress-Controller-Metrics" />

## Create monitors for Nginx Ingress app

import CreateMonitors from '../../reuse/apps/create-monitors.md';

<CreateMonitors/>

## Nginx Ingress alerts

<details>
<summary>Here are the alerts available for Nginx Ingress (click to expand).</summary>
| Alert Type (Metrics/Logs) | Alert Name | Alert Description | Trigger Type (Critical / Warning) | Alert Condition | Recover Condition |
|:---|:---|:---|:---|:---|:---|
| Logs | Nginx Ingress - Access from Highly Malicious Sources | This alert fires when an Nginx Ingress server is accessed from highly malicious IP addresses. | Critical | > 0 | < = 0 |
| Logs | Nginx Ingress - High Client (HTTP 4xx) Error Rate | This alert fires when there are too many HTTP requests (>5%) with a response status of 4xx. | Critical | > 0 | 0 |
| Logs | Nginx Ingress - High Server (HTTP 5xx) Error Rate | This alert fires when there are too many HTTP requests (>5%) with a response status of 5xx. | Critical | > 0 | 0 |
| Logs | Nginx Ingress - Critical Error Messages | This alert fires when we detect critical error messages for a given Nginx Ingress server. | Critical | > 0 | 0 |
| Metrics | Nginx Ingress - Dropped Connections | This alert fires when we detect dropped connections for a given Nginx Ingress server. | Critical | > 0 | 0 |
</details>
