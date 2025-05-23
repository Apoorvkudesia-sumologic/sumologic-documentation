---
title: Microsoft EWS Daemon
description: ''
---

import useBaseUrl from '@docusaurus/useBaseUrl';

<img src={useBaseUrl('/img/platform-services/automation-service/app-central/logos/microsoft-ews-daemon.png')} alt="microsoft-defender-atp" width="100"/>

***Version: 2.5  
Updated: May 9, 2024***

:::sumo Cloud SOAR
This integration is only for Cloud SOAR.
:::

Process emails with EWS Daemon.

## Actions

* **Microsoft EWS Incoming Mail Daemon** *(Daemon)* - Automatically retrieve emails from EWS.

## Microsoft EWS configuration

Each application you want the Microsoft identity platform to perform identity and access management (IAM) needs to be registered. Registering it establishes a trust relationship between your application and the identity provider, the Microsoft identity platform.

### Register an application

[Registering your application](https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/how-to-authenticate-an-ews-application-by-using-oauth) establishes a trust relationship between your app and the Microsoft identity platform. The trust is unidirectional: your app trusts the Microsoft identity platform, and not the other way around.

Follow these steps to create the app registration:

1. Sign in to the [Azure portal](https://portal.azure.com/).
1. If you have access to multiple tenants, use the Directory + subscription filter  <br/><img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/microsoft-ews-daemon/microsoft-ews-daemon-1.png')} style={{border:'1px solid gray'}} alt="microsoft-ews-daemon" width="30"/> in the top menu to select the tenant in which you want to register an application.
1. Search for and select the **Azure Active Directory**.
1. Under **Manage**, select **App registrations > New registration**.
1. Enter a Name for your application. Users of your app might see this name, and you can change it later.
1. Select Register to complete the initial app registration.
1. Don't enter anything for **Redirect URI (optional)**. <br/><img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/microsoft-ews-daemon/microsoft-ews-daemon-2.png')} style={{border:'1px solid gray'}} alt="microsoft-defender-atp" width="800"/>

When registration completes, the Azure portal displays the app registration's Overview pane, which includes its Application (client) ID. Also referred to as just client ID, this value uniquely identifies your application in the Microsoft identity platform.

The client ID as one aspect in validating the security tokens it receives from the identity platform.
<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/microsoft-ews-daemon/microsoft-ews-daemon-3.png')} style={{border:'1px solid gray'}} alt="microsoft-defender-atp" width="800"/>

**Add credentials**

Credentials are used by confidential client applications that access an API. Examples of confidential clients are web apps, or service- and daemon-type applications. Credentials allow your application to authenticate as itself, requiring no interaction from a user at runtime.

You can add client secrets (a string) as credentials to your confidential client app registration.<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/microsoft-ews-daemon/microsoft-ews-daemon-4.png')} style={{border:'1px solid gray'}} alt="microsoft-defender-atp" width="800"/>

**Add a client secret**

The client secret, known also as an application password, is a string value of your app.

1. Select your application in App registrations in the Azure portal.
2. Select **Certificates & secrets > New client secret**.
3. Add a description for your client secret.
4. Select a duration.
5. Select **Add**.
6. Record the secret's value for use in your client application code - it's never displayed again after you leave this page.

**Add permissions to API**

1. Select your application in App registrations in the Azure portal.
2. Select **API permissions > Add a permission**.
3. Delegated permissions are selected by default. Delegated permissions are appropriate for client apps that access an API as the signed-in user, and whose access should be restricted to the permissions you select in the next step.
4. Application permissions are for service- or daemon-type applications that need to access API as themselves, without user interaction for sign-in or consent. Unless you've defined application roles for your API.
5. Select Add a permission, and add the following permissions (as we can see from the picture).<img src={useBaseUrl('/img/platform-services/automation-service/app-central/integrations/microsoft-ews-daemon/microsoft-ews-daemon-5.png')} style={{border:'1px solid gray'}} alt="microsoft-defender-atp" width="800"/>

**EWS API need to be configure these permissions**

Applications are authorized to call APIs when they are granted permissions by users/admins as part of the consent process. The list of configured permissions should include all the permissions the application needs.

**API / Permissions**

Microsoft Graph (7)

* Mail.Read
    + Type: Delegated
    + Description: Read user mail
    + Admin: -
* Mail.Read
    + Type: Application
    + Description: Read mail in all mailboxes
    + Admin: Yes
* Mail.ReadBasic
    + Type: Delegated
    + Description: Read user basic mail
    + Admin: -
* Mail.ReadBasic
    + Type: Application
    + Description: Read basic mail in all mailboxes
    + Admin: Yes
* Mail.ReadBasic.All
    + Type: Application
    + Description: Read basic mail in all mailboxes
    + Admin: Yes
* profile
    + Type: Delegated
    + Description: View users' basic profile
    + Admin: -
* User.Read
    + Type: Delegated
    + Description: Sign in and read user profile
    + Admin: Yes

Office 365 Exchange Online (3)

* EWS.AccessAsUser.All
    + Type: Delegated
    + Description: Access mailboxes as the signed-in user via Exchange Web Services
    + Admin: -
* Exchange.ManageAsApp
    + Type: Application
    + Description: Manage Exchange As Application
    + Admin: Yes
* full\_access\_as\_app
    + Type: Application
    + Description: Use Exchange Web Services with full access to all mailboxes
    + Admin: Yes

full\_access\_as\_app Use Exchange Web Services with full access to all mailboxes

Once API permission are added then Admin must consent to a grant these permissions, [Learn more about permissions and consent](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent?WT.mc_id=Portal-Microsoft_AAD_RegisteredApps).

:::info Important Note
When using the Microsoft EWS Daemon action within an automation rule, note that it will only pull in emails that are marked "Unread" within the respective mailbox scope. To ensure all relevant alerts are processed correctly, keep this mailbox a dedicated entity and avoid any manual reviews by other stakeholders.
:::

## Configure Microsoft EWS Daemon in Cloud SOAR

import IntegrationsAuth from '../../../../reuse/integrations-authentication.md';

<IntegrationsAuth/>

For information about Microsoft EWS, see [Microsoft Exchange Web Services documentation](https://learn.microsoft.com/en-us/exchange/client-developer/exchange-web-services/how-to-authenticate-an-ews-application-by-using-oauth).

## Category

Email Gateway

## Change Log

* October 27, 2021 - First upload
* March 10, 2022 - Logo
* October 6, 2023 (v2.2) - Integration Updated
* March 4, 2024 (v2.3) - Updated code for compatibility with Python 3.12
* March 21, 2024 (v2.4) - Resolved an issue related to the Email Body
* May 9, 2024 (v2.5) - A new field has been added to the integration resource for specifying the folder or path to search within 
