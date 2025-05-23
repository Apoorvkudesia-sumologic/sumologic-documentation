---
title: FireEye Email Security (EX)
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/fireeye-email-security-ex.png')} alt="fireeye-email-security-ex" width="100"/>

***Version: 1.1  
Updated: Jul 03, 2023***

Full stack email security solution for email analysis.

## Actions

* **Get Alert Info** (*Enrichment*) - Query FireEye EX for alert details.
* **Get ATI Details** (*Enrichment*) - Get the ATI details for the specified alert Id.
* **Add YARA Rule** (*Containment*) - Add a new YARA rule from the specified file.

## Configure FireEye Email Security (EX) in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';

<IntegrationsAuth/>

For information about Trellix Email Security - Server (formerly FireEye Email Security), see [Trellix Email Security - Server documentation](https://docs.trellix.com/bundle/fe-email-server-landing/page/UUID-ca2a7502-d2aa-2294-6577-2592319a5837.html).

## Change Log

* June 12, 2019 - First upload
* July 3, 2023 (v1.1) - Updated the integration with Environmental Variables
