---
id: username-and-hostname-normalization
title: Username and Hostname Normalization
sidebar_label: Username and Hostname Normalization
description: Learn about how Cloud SIEM normalizes usernames and hostnames during mapping and parsing.
---

import useBaseUrl from '@docusaurus/useBaseUrl';

Cloud SIEM normalizes usernames and hostnames in records during the parsing and mapping process. This allows for common name forms among Active Directory, AWS, and fully qualified domain names to be normalized into a domain and username form.

## Getting data into Cloud SIEM for normalization

Data that is normalized comes into Cloud SIEM from several sources:
* **Active Directory**. Configure the [Windows Active Directory Inventory Source](/docs/send-data/installed-collectors/sources/windows-active-directory-inventory-source/). The username, hostname, and domain information is pulled in directly from Active Directory to Cloud SIEM for normalization. For information on the computer and user data that is obtained, see the [Windows Active Directory Inventory Source](/docs/cse/administration/inventory-sources-and-data/#windows-active-directory-inventory-source) section in [Inventory Sources and Data](/docs/cse/administration/inventory-sources-and-data).
* **AWS**. Configure AWS sources and ensure that the **Forward to SIEM** option is set to true. For more information, see [Cloud SIEM Ingestion Best Practices](/docs/cse/ingestion/cse-ingestion-best-practices/) and [Ingestion Sources for Cloud SIEM](/docs/cse/ingestion/ingestion-sources-for-cloud-siem/).
* **Other sources**. See [Inventory Sources and Data](/docs/cse/administration/inventory-sources-and-data).

## Normalization process

The name normalization process normalizes a complete username into a base username and a domain. Some of the common forms for username are:

* `username` (no domain)
* `AD-DOMAIN\username`
* `username@AD-DOMAIN.X` (the AD domain format that’s more fully qualified)
* `username@EMAIL-DOMAIN` (FQDN)
* `arn:aws(-us-gov)?:iam::<USER_ID>:user/username` or `arn:aws(-us-gov)?:iam::<USER_ID>:user/username@FQDN`
* `AROA<AWS_ID>:username@FQDN` or `arn:aws:iam::<USER_ID>:user/username@FQDN`

Some of the common forms of hostname are:

* `hostname` (no domain)
* `AD-DOMAIN\hostname`
* `hostname@AD-DOMAIN.X` and `hostname$@AD-DOMAIN.X`
* `hostname.DOMAIN` (or `EMAIL-DOMAIN`)

The following fields of the schema are normalized.

| Schema Attribute     | Normalization Type |
|:----------------------|:--------------------|
| `device_hostname`    | hostname           |
| `dstDevice_hostname` | hostname           |
| `fromUser_username`  | username           |
| `srcDevice_hostname` | hostname           |
| `user_username`      | username           |

When a username is normalized, the original, un-normalized name is placed in a `_raw` name attribute, for example,  `user_useraname_raw`. The normalized name is placed in the attribute field `user_username`. The rules engine allows the `_raw` username forms to be used in rule creation.

If a name normalization configuration exists, the name attribute will be populated with the form `<username>:<friendly_domain>` where the `<friendly domain name>` portion is not populated for the normalized default domain.  When name normalization is enabled all name fields (not-raw) will be lowercase. For more information, see [Single domain example](#single-domain-example) and [Multiple domains example](#multiple-domains-example) below.

:::note
If no name normalization configuration exists, the name attribute will consist of the original (non-normalized) form and the system will continue to operate as it does today, with the exception that that `_raw` attribute will also be populated. 
:::

## Configure entity normalization

1. [**Classic UI**](/docs/get-started/sumo-logic-ui-classic). In the top menu select **Configuration**, and then under **Entities** select **Normalization**. <br/>[**New UI**](/docs/get-started/sumo-logic-ui). In the top menu select **Configuration**, and then under **Cloud SIEM Entities** select **Normalization**. You can also click the **Go To...** menu at the top of the screen and select **Normalization**.
1. Select the **Domain** tab. (For information about the **Lookup Tables** tab, see [Configure an Entity Lookup Table](/docs/cse/records-signals-entities-insights/configure-entity-lookup-table/)). 
1. You can configure just **Username Normalization**, just **Hostname Normalization**, or both. We recommend you enable both. 
1. Under **Normalization Formats** there are configuration options to normalize names from:
   * **FQDN**. Normalize names in the form `user@somedomain.net` or `hostname.somedomain.net`.
   * **Active Directory**. Normalize active directory domains username and hostname formats.
   * **AWS**. Normalize AWS ARN and usernames.
1. In **Normalized Default Domain**, provide a default domain name. 
1. In **Domain Name Mappings**, enter mappings for secondary domains.
1. Click **Save**.

Following is an example configuration:

<img src={useBaseUrl('img/cse/Configuration.png')} alt="Configuration dialog" style={{border: '1px solid gray'}} width="600"/>

### Warnings and issues

If no name normalization is configured, the system will continue to operate as it does today.  If normalization is then enabled, any signals already created in the system will use the non-normalized form of the name.  Any new signals will use the normalized name.  This means there is potential for insights to be uncorrelated between the two different name forms for one insight window.  This is especially true as all usernames will now be lowercase.


## Domain normalization

### Default domains

When normalization is configured, at least one domain must be configured and a “Normalized Default Domain” must be provided. The default name will never show up in normalized names, as it’s assumed, and username forms with no domain portion will be considered part of that domain. In our example above, we’ve assumed the name “sumo”.

Next, the user should enter the domain name forms that will be seen in the customer's environment.  This should include:

* The fully qualified domain name, for example, `sumologic.com`.
* The active directory domain, for example, `SUMO` or `SUMOLOGIC`.
* The active directory domain name with the `.LOCAL` extension, for example, `SUMO.LOCAL`.
* Any other domains or sub-domains.  

These domains should all have the “Normalized Name” that matches the “Normalized Default Domain”, for example, `sumo`.   

### Secondary domains

The normalization configurations also supports secondary domains that may not map users in a different namespace to the same name. For example, if `bob@sumologic.com` is not the same as `bob@jask.com`, then a secondary domain should be configured.  In this case a second set of configurations should be populated to maps to a different “normalized domain” (`jask`). 

For example, in this case a configuration could be:

* The fully qualified secondary domain (`jask.com`)
* Any secondary domains (`JASK`)

In this case, these domains map to a different normalized domain (`jask`).  When one of these domains is normalized, it will show up as `bob:jask` in the normalized name form.

### Single domain example

In this example, it is assumed you have configured the system for “Primary domain” and you configured the domains `SUMO` and `sumologic.com`. In this case, assume a log line has a username field:

`bob`

When that’s mapped and normalized, the normalized username is set in the username field:

`user_username = bob`

The raw name is populated in the un-normalized name field:

`user_username_raw = bob`

The same logic would apply to a hostname:

`device_hostname = desktop1 `

would be populated with the normalized name and the raw name would be
populated:

`device_hostname_raw would be = desktop1`

Now if an AD logon event was identified with the username:

`user_username_raw = SUMO\bob`

In this case, the domain portion (SUMO) would be normalized and the name would be:

`user_username = bob `

The hostname example would be the same:

`device_hostname_raw = SUMO\desktop1`

would have a normalized name 

`device_hostname = desktop1`

This would also hold true for the username `bob@sumologic.com` and the hostname `desktop1@sumologic.com`.   In all of these cases, the usernames or hostnames would normalize to the same name respectively.

For any name not matching a configured domain name, the normalized name would just be set to the username, so in this example:

`user_username = jask\fred`

The normalized username would be:

`user_username_raw = JASK\fred`

### Multiple domains example

In this example, it is assumed you have configured the system for “Primary domain” and also introduced a sub-domain (JASK). In this case, the configuration looks like:

Normalized Default Domain: sumo

| Domain String | Base Domain |
|:---------------|:-------------|
| SUMO          | sumo        |
| sumologic.com | sumo        |
| JASK          | jask        |
| jask.com      | jask        |

Name forms matching the default domain would look like:

| Username String (raw) | Normalized Value      |
|:-----------------------|:-----------------------|
| `bob`                   | `bob`                   |
| `SUMO\bob`              | `bob`                   |
| `bob@sumologic.com`     | `bob`                   |
| `fred@jask.com`         | `fred:jask`            |
| `JASK\fred`             | `fred:jask`             |
| `bob@someothername.com` | `bob@someothername.com` |
| `OTHERDOMAIN\suzy`      | `otherdomain\suzy`     | 

### Active Directory domain example

Following is an example configuration for a case where the customer has a domain name `test.com` and an Active Directory domain named `test`. 

<img src={useBaseUrl('img/cse/Example_UI.png')} alt="Configuration example" style={{border: '1px solid gray'}} width="600"/>

