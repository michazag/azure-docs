---
title: 'Tutorial: Azure Active Directory integration with Workspot Control | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Workspot Control.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess

ms.assetid: 3ea8e4e9-f61f-4f45-b635-b0e306eda3d1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 3/11/2019
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Workspot Control

In this tutorial, you learn how to integrate Workspot Control with Azure Active Directory (Azure AD).
Integrating Workspot Control with Azure AD provides you with the following benefits:

* You can control in Azure AD who has access to Workspot Control.
* You can enable your users to be automatically signed-in to Workspot Control (Single Sign-On) with their Azure AD accounts.
* You can manage your accounts in one central location - the Azure portal.

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.

## Prerequisites

To configure Azure AD integration with Workspot Control, you need the following items:

* An Azure AD subscription. If you don't have an Azure AD environment, you can get a [free account](https://azure.microsoft.com/free/)
* Workspot Control single sign-on enabled subscription

## Scenario description

In this tutorial, you configure and test Azure AD single sign-on in a test environment.

* Workspot Control supports **SP** and **IDP** initiated SSO

## Adding Workspot Control from the gallery

To configure the integration of Workspot Control into Azure AD, you need to add Workspot Control from the gallery to your list of managed SaaS apps.

**To add Workspot Control from the gallery, perform the following steps:**

1. In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.

	![The Azure Active Directory button](common/select-azuread.png)

2. Navigate to **Enterprise Applications** and then select the **All Applications** option.

	![The Enterprise applications blade](common/enterprise-applications.png)

3. To add new application, click **New application** button on the top of dialog.

	![The New application button](common/add-new-app.png)

4. In the search box, type **Workspot Control**, select **Workspot Control** from result panel then click **Add** button to add the application.

	 ![Workspot Control in the results list](common/search-new-app.png)

## Configure and test Azure AD single sign-on

In this section, you configure and test Azure AD single sign-on with Workspot Control based on a test user called **Britta Simon**.
For single sign-on to work, a link relationship between an Azure AD user and the related user in Workspot Control needs to be established.

To configure and test Azure AD single sign-on with Workspot Control, you need to complete the following building blocks:

1. **[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Configure Workspot Control Single Sign-On](#configure-workspot-control-single-sign-on)** - to configure the Single Sign-On settings on application side.
3. **[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
4. **[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Create Workspot Control test user](#create-workspot-control-test-user)** - to have a counterpart of Britta Simon in Workspot Control that is linked to the Azure AD representation of user.
6. **[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.

### Configure Azure AD single sign-on

In this section, you enable Azure AD single sign-on in the Azure portal.

To configure Azure AD single sign-on with Workspot Control, perform the following steps:

1. In the [Azure portal](https://portal.azure.com/), on the **Workspot Control** application integration page, select **Single sign-on**.

    ![Configure single sign-on link](common/select-sso.png)

2. On the **Select a Single sign-on method** dialog, select **SAML/WS-Fed** mode to enable single sign-on.

    ![Single sign-on select mode](common/select-saml-option.png)

3. On the **Set up Single Sign-On with SAML** page, click **Edit** icon to open **Basic SAML Configuration** dialog.

	![Edit Basic SAML Configuration](common/edit-urls.png)

4. On the **Basic SAML Configuration** section, if you wish to configure the application in **IDP** initiated mode, perform the following steps:

    ![Workspot Control Domain and URLs single sign-on information](common/idp-intiated.png)

    a. In the **Identifier** text box, type a URL using the following pattern:
    `https://<INSTANCENAME>-saml.workspot.com/saml/metadata`

    b. In the **Reply URL** text box, type a URL using the following pattern:
    `https://<INSTANCENAME>-saml.workspot.com/saml/assertion`

5. Click **Set additional URLs** and perform the following step if you wish to configure the application in **SP** initiated mode:

    ![Workspot Control Domain and URLs single sign-on information](common/metadata-upload-additional-signon.png)

    In the **Sign-on URL** text box, type a URL using the following pattern:
    `https://<INSTANCENAME>-saml.workspot.com/`

	> [!NOTE]
	> These values are not real. Update these values with the actual Identifier, Reply URL and Sign-on URL. Contact [Workspot Control Client support team](mailto:support@workspot.com) to get these values. You can also refer to the patterns shown in the **Basic SAML Configuration** section in the Azure portal.

6. On the **Set up Single Sign-On with SAML** page, in the **SAML Signing Certificate** section, click **Download** to download the **Certificate (Base64)** from the given options as per your requirement and save it on your computer.

	![The Certificate download link](common/certificatebase64.png)

7. On the **Set up Workspot Control** section, copy the appropriate URL(s) as per your requirement.

	![Copy configuration URLs](common/copy-configuration-urls.png)

	a. Login URL

	b. Azure AD Identifier

	c. Logout URL

### Configure Workspot Control Single Sign-On

1. In a different web browser window, sign in to Workspot Control as a Security Administrator.

2. In the toolbar at the top of the page, click **Setup**, then navigate to **SAML**.

	![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_setup.png)

3. On the **Security Assertion Markup Language Configuration** page, perform the following steps:
 
	![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_saml.png)

	a. In the **Entity ID** textbox, paste the value of **Azure Ad Identifier** which you have copied from the Azure portal.   

	b.In the **Signon Service URL** textbox, paste the value of **Login URL** which you have copied from the Azure portal.

	c. In the **Logout Service URL** textbox, paste the value of **Logout URL** which you have copied from the Azure portal. 

	d. Click on **Update File** button to Upload the base-64 encoded certificate that you have downloaded from the Azure portal, into the X.509 certificate.

	e. Click **Save**.

### Create an Azure AD test user 

The objective of this section is to create a test user in the Azure portal called Britta Simon.

1. In the Azure portal, in the left pane, select **Azure Active Directory**, select **Users**, and then select **All users**.

    ![The "Users and groups" and "All users" links](common/users.png)

2. Select **New user** at the top of the screen.

    ![New user Button](common/new-user.png)

3. In the User properties, perform the following steps.

    ![The User dialog box](common/user-properties.png)

    a. In the **Name** field enter **BrittaSimon**.
  
    b. In the **User name** field type brittasimon@yourcompanydomain.extension. For example, BrittaSimon@contoso.com

    c. Select **Show password** check box, and then write down the value that's displayed in the Password box.

    d. Click **Create**.

### Assign the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workspot Control.

1. In the Azure portal, select **Enterprise Applications**, select **All applications**, then select **Workspot Control**.

	![Enterprise applications blade](common/enterprise-applications.png)

2. In the applications list, select **Workspot Control**.

	![The Workspot Control link in the Applications list](common/all-applications.png)

3. In the menu on the left, select **Users and groups**.

    ![The "Users and groups" link](common/users-groups-blade.png)

4. Click the **Add user** button, then select **Users and groups** in the **Add Assignment** dialog.

    ![The Add Assignment pane](common/add-assign-user.png)

5. In the **Users and groups** dialog select **Britta Simon** in the Users list, then click the **Select** button at the bottom of the screen.

6. If you are expecting any role value in the SAML assertion then in the **Select Role** dialog select the appropriate role for the user from the list, then click the **Select** button at the bottom of the screen.

7. In the **Add Assignment** dialog click the **Assign** button.

### Create Workspot Control test user

To enable Azure AD users to sign in to Workspot Control, they must be provisioned into Workspot Control. In Workspot Control, provisioning is a manual task.

**To provision a user account, perform the following steps:**

1. Sign in to Workspot Control as a Security Administrator.

2. In the toolbar at the top of the page, click **Users**, then navigate to **Add User**.

	![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_adduser.png)

3. On the **Add a New User** page, perform the following steps:

	![image](./media/workspotcontrol-tutorial/tutorial_workspotcontrol_addnewuser.png)

	a. In **First Name** text box, enter the first name of user like **Britta**.

	b. In **Last Name** text box, enter the last name of user like **simon**.

	c. In **Email** text box, enter the email of user like Brittasimon@contoso.com.

	d. Select the appropriate user role from the **Role** dropdown.

	e. Select the appropriate user group from the **Group** dropdown.

	f. Click **Add User**.

### Test single sign-on 

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Workspot Control tile in the Access Panel, you should be automatically signed in to the Workspot Control for which you set up SSO. For more information about the Access Panel, see [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## Additional Resources

- [ List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [What is conditional access in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

