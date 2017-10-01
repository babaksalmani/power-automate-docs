---
title: Use Azure Active Directory with a custom connector | Microsoft Docs
description: Learn how to create a custom connector for Azure Resource Manager, with Azure Active Directory authentication.
services: ''
suite: flow
documentationcenter: ''
author: msftman
manager: anneta
editor: ''

ms.service: flow
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2016
ms.author: deonhe

---
# Use Azure Active Directory with a custom connector in Microsoft Flow
Azure Resource Manager (ARM) enables you to manage the components of a solution on Azure - components like databases, virtual machines, and web apps. This tutorial demonstrates how to enable authentication in Azure Active Directory, register one of the ARM APIs as a custom connector, and then connect to it in Microsoft Flow. This would be useful if you want to manage Azure resources as part of a flow. For more information about ARM, see [Azure Resource Manager Overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## Prerequisites
* An [Azure subscription](https://azure.microsoft.com/free/).
* A [Microsoft Flow account](https://flow.microsoft.com).
* The [sample OpenAPI file](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json) used in this tutorial.

## Enable authentication in Azure Active Directory
First, we need to create an Azure Active Directory (AAD) application that will perform the authentication when calling the ARM API endpoint.

1. Sign in to the [Azure portal](https://portal.azure.com).  If you have more than one Azure Active Directory tenant, make sure you're logged into the correct directory by looking at your username in the upper-right corner.
   
    ![User Name](./media/customapi-azure-resource-manager-tutorial/current-user.png)
2. On the left-hand menu, click **More services**.  In the **Filter** textbox, type **Azure Active Directory**, and then click **Azure Active Directory**.
   
    ![Azure Active Directory](./media/customapi-azure-resource-manager-tutorial/azureaad.png)
   
    The Azure Active Directory blade opens.   
3. In the menu on the Azure Active Directory blade, click **App registrations**.
   
    ![App registrations](./media/customapi-azure-resource-manager-tutorial/azureapplication.png)
4. In the list of registered applications, click **Add**.
   
    ![Add button](./media/customapi-azure-resource-manager-tutorial/add-app-btn.png)   
5. Type a name for your application, leave **Web app / API** selected, and then for **Sign-on URL** type `https://login.windows.net`.  Click **Create**.  
   
    ![New app form](./media/customapi-azure-resource-manager-tutorial/newapplication.png)
6. Click the new application in the list.
   
    ![New app in list](./media/customapi-azure-resource-manager-tutorial/newapplication2.png)
   
    The Registered app blade opens.  Make a note of the **Application ID**.  We'll need it later.
7. The Settings blade should have opened, as well.  If it didn't, click the **Settings** button.
   
    ![Settings button](./media/customapi-azure-resource-manager-tutorial/settings-btn.png)
8. In the Settings blade, click **Reply URLs**. In the list of URLs, add `https://msmanaged-na.consent.azure-apim.net/redirect` and click **Save**.
   
    ![Reply URLs](./media/customapi-azure-resource-manager-tutorial/reply-urls.png)
9. Back on the Settings blade, click **Required permissions**.  On the Required permissions blade, click **Add**.
   
    ![Required permissions](./media/customapi-azure-resource-manager-tutorial/permissions.png)
   
    The Add API access blade opens.
10. Click **Select an API**. In the blade that opens, click the option for the Azure Service Management API and click **Select**.
    
    ![Select an API](./media/customapi-azure-resource-manager-tutorial/permissions2.png)
11. Click **Select permissions**.  Under *Delegated permissions*, click **Access Azure Service Management as organization users**, and then click **Select**.
    
    ![Delegated permissions](./media/customapi-azure-resource-manager-tutorial/permissions3.png)
12. On the Add API access blade, click **Done**.
13. Back on the Settings blade, click **Keys**.  In the Keys blade, type a description for your key, select an expiration period, and then click **Save**.  Your new key will be displayed.  Make note of the key value, as we will need that later, too.  You may now close the Azure portal.
    
    ![Create a key](./media/customapi-azure-resource-manager-tutorial/configurekeys.png)

## Add the connection in Microsoft Flow
Now that the AAD application is configured, let's add the custom connector.

1. In the [Microsoft Flow web app](https://flow.microsoft.com/), click the **Settings** button at the upper right of the page (it looks like a gear).  Then click **custom connectors**.
   
    ![Find custom connectors](./media/customapi-azure-resource-manager-tutorial/finding-custom-apis.png)  
2. Click **Create custom connector**.  
   
    You will be prompted for the properties of your API.  
   
   | Property | Description |
   | --- | --- |
   | Name |At the top of the page, click **Untitled** and give your flow a name. |
   | OpenAPI file |Browse to the [sample ARM OpenAPI file](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json). |
   | Upload API icon |Cick **Upload icon** to select an image file for the icon. Any PNG or JPG image less than 1 MB in size will work. |
   | Description |Type a description of your custom connector (optional). |
   
    ![Create custom connector](./media/customapi-azure-resource-manager-tutorial/create-custom-api.png)  
   
    Select **Continue**.
3. On the next screen, because the OpenAPI file uses our AAD application for authentication, we need to give Flow some information about our application.  Under **Client id**, type the AAD **Application ID** you noted earlier.  For client secret, use the **key**.  And finally, for **Resource URL**, type `https://management.core.windows.net/`.
   
   > [!IMPORTANT]
   > Be sure to include the Resource URL exactly as written above, including the trailing slash.
   > 
   > 
   
    ![OAuth settings](./media/customapi-azure-resource-manager-tutorial/oauth-settings.png)
   
    After entering security information, click the check mark (**&#x2713;**) next to the flow name at the top of the page to create the custom connector.
4. Your custom connector is now displayed under **custom connectors**.
   
    ![Available APIs](./media/customapi-azure-resource-manager-tutorial/list-custom-apis.png)  
5. Now that the custom connector is registered, you must create a connection to the custom connector so it can be used in your apps and flows.  Click the **+** to the right of the name of your custom connector and then complete complete the sign-on screen.

> [!NOTE]
> The sample OpenAPI does not define the full set of ARM operations and currently only contains the [List all subscriptions](https://msdn.microsoft.com/library/azure/dn790531.aspx) operation.  You can edit this OpenAPI or create another OpenAPI file using the [online OpenAPI editor](http://editor.swagger.io/).
> 
> This process can be used to access any RESTful API authenticated using AAD.
> 
> 

## Next steps
For more detailed information about how to create a flow, see [Start to build with Microsoft Flow](get-started-logic-flow.md).

To ask questions or make comments about custom connectors, [join our community](https://aka.ms/flow-community).
