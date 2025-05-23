---
title: CyberArk AAM
description: ''
---
import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/cyberark-aam.png')} alt="cyberark-aam" width="100"/>

***Version: 1.2  
Updated: Jul 18, 2023***

CyberArk Application Access Manager interaction for widely used application types and non-human identities. CyberArk AAM is a credentials retrieval integration. 

## Actions

* **Update Certificate** (one required field: Upload file).
* **Get Application Details** (4 required fields: APP ID, Safe, Folder, Object).

## Configure CyberArk AAM in Automation Service and Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';

<IntegrationsAuth/>

For information about CyberArk, see [CyberArk documentation](https://docs.cyberark.com/portal/latest/en/docs.htm). For information about CyberArk APIs, see their [REST APIs documentation](https://docs.cyberark.com/pam-self-hosted/latest/en/content/webservices/implementing%20privileged%20account%20security%20web%20services%20.htm).

## Change Log

* October 5, 2020 - First upload
* June 26, 2023 (v1.1) - Updated the integration with Environmental Variables
* July 18, 2023 (v1.2) - Code refactoring
