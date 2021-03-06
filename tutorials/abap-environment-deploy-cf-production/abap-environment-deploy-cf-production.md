---
auto_validation: true
title: Develop and Run SAP Fiori Application With SAP Business Application Studio
description: Develop and run your SAP Fiori application with SAP Business Application Studio
primary_tag: products>sap-cloud-platform--abap-environment
tags: [  tutorial>beginner, topic>abap-development, products>sap-cloud-platform, products>sap-business-application-studio ]
time: 25
author_name: Merve Temel
author_profile: https://github.com/mervey45
---

## Prerequisites  
- You need a SAP Cloud Platform ABAP Environment trial user or a license.


## Details
### You will learn  
- How to assign role collections
- How to create dev spaces
- How to set up organization and space
- How to create list report object pages
- How to run SAP Fiori applications
- How to deploy applications
- How to check BSP library in Eclipse
- How to create IAM apps and business catalogs
- How to create index.html
- How to run index.html

---
[ACCORDION-BEGIN [Step 1: ](Assign role collection to user)]

  1. Login to [SAP Cloud Platform trial cockpit](https://cockpit.hanatrial.ondemand.com/) and click **Enter Your Trial Account**.

      ![assign role collection](bas1.png)

  2. Select your subaccount **trial**.

      ![assign role collection](bas2.png)

  3. Click **Trust Configuration** to set up your trust.

      ![assign role collection](bas3.png)

      HINT: If you are using a licensed system, make sure you have the trust administrator role assigned to your user.

  4. Select **sap.default**.

      ![assign role collection](bas4.png)

  5. Enter your e-mail address and click **Show Assignments**.

      ![assign role collection](bas5.png)

  6. Click **Assign Role Collection** .

      ![assign role collection](bas6.png)

  7. Select **`Business_Application_Studio_Developer`** and click **Assign Role Collection**.

      ![assign role collection](bas7.png)

  8. Check your result. Now your user should have the **`Business_Application_Studio_Developer`** role collection assigned.

      ![assign role collection](bas8.png)

      You are now able to develop on SAP Business Application Studio.


[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Create dev space)]

  1.  Select **trial** > **Subscriptions** > **SAP Business Application Studio** and click **Go to Application**.

      ![dev](studio.png)

  2.  Check the privacy statement and click **OK**.

      ![dev](studio2.png)

  3. Now the SAP Business Application Studio has started. Click **Create Dev Space**.

      ![dev](studio3.png)

  4. Create a new dev space:
       - Name: **Fiori**
       - Type: **SAP Fiori**
       - Additional SAP Extensions: **Launchpad Module**

       Click **Create Dev Space**.

     ![dev](studio4.png)

  5. When your status is **Running**, select your dev space **Fiori**.

      ![dev](studio5.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Set up organization and space)]

  1. Now you are in your **Fiori** dev space in SAP Business Application Studio.
     Select **Open Workspace** to set your workspace.

      ![organization](studio6.png)

  2. Select **projects** and click **Open**.

      ![organization](studio7.png)

  3. Select **The organization and space in Cloud Foundry have not been set.**

      ![organization](studio8.png)

  4. Press enter to set your Cloud Foundry endpoint.

      ![organization](studio9.png)

  5. Enter the same e-mail address you set in your trial instance and press enter.
      ![organization](studio10.png)

  6. Enter your password and press enter.

      ![organization](studio11.png)

  7. Select your global account and press enter.

      ![organization](studio12.png)

  8. Select dev as your space and press enter.

      ![organization](studio13.png)

  9. Check your result. Now your organization and space have been set.

     ![organization](studio14.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 4: ](Create list report object page)]

  1. Select **View** > **Find Command**.

    ![object](studio15.png)

  2. Search for **Yeoman UI Generators** and select it.

    ![object](studio16.png)

  3. Select **SAP Fiori elements application** and click **Next >**.

    ![object](studio17.png)

  4. Select **List Report Object Page** and click **Next >**.

    ![object](studio18.png)

  5. Configure data source, system and service:
     - Data source: **Connect to SAP System**
     - System: **`New System`**
     - ABAP Environment: **`abap-cloud-default_abap-trial`**
     - Service: **`ZUI_C_TRAVEL_M_XXX`**

     Click **Next >**.

     The destination service is: **`abap-cloud-<your_abap_trial_instance>(SCP)`**.

    ![object](studio19.png)

  6. Select your main entity **`TravelProcessor`** and click **Next >**.

    ![object](studio20.png)

  7. Configure project attributes:
     - Name: **`ztravel_app_xxx`**
     - Title: **Travel App XXX**
     - Description: **A Fiori application.**

     Click **Finish**.

    ![object](studio21.png)

    HINT: Your **application name must** begin with a `z letter` and **must** be in **lowercase letters**.

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 5: ](Run SAP Fiori application for data preview)]

  1. Close the wizard.

      ![run](studio22.png)

  2. Press the run button and select press button for **`Start ztravel_app_xxx`**  to run your SAP Fiori application.

      ![run](studio24.png)

      HINT: An alternative to run the application is to open the terminal and enter: `npm start`.

  3. Click **Expose and Open**.

      ![run](studio25.png)

  4. Enter **travel** and press enter.

      ![run](studio26.png)

  5. Select **`test/`**.

      ![run](studio27.png)

  6. Select **`flpSandbox.html`**.

      ![run](studio28.png)

  7. Now your SAP Fiori application runs. Select your application **Travel App XXX**.

      ![run](studio29.png)

  8. Click **Go** to see your result.

      ![run](studio30.png)

  9. Check your result.

     ![run](studio31.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 6: ](Deploy your application)]

  1. Go back to SAP Business Application Studio, select projects, right-click on your project **`ztravel_app_xxx`** and select **Open in Terminal**.

      ![deploy](deploy.png)

  2. To add Fiori Launchpad content use this command, enter **`npx fiori add flp-config`**.

     Add following information:

       - Semantic Object: **`ztravel_app_xxx`**
       - Action: display
       - Title: Travel App XXX
       - Subtitle (optional): press enter

       ![deploy](deploy2.png)

  3.  Open eclipse, search your package **`ZTRAVEL_APP_XXX`** and open it. Open your transport organizer to see your transport request. Copy your transport request for later use. You can find your **transport request** underneath the **Modifiable** folder.

      ![deploy](deploy3.png)

  4. To add `deploy config` details enter **`npx fiori add deploy-config`**.

     Add following information:

      - Please choose the target: ABAP
      - Is this an ABAP Cloud System?: Y
      - Destination: press enter for default
      - Name: press enter for default
      - Package: **`ztravel_app_xxx`**
      - Transport Request: **`<your_transport_request>`**
      - Generate standalone index.html during deployment: y

      ![deploy](deploy4.png)

      The `ui5-deploy.yaml` will be generated as part of this `deploy config` command.

  5. Enter **`npm run deploy`** to deploy your application.
     When prompted, check deployment configuration and press y.
     Open the URL at the end of the deployment log in browser to preview the application.

      ![deploy](deploy5.png)

      When the deployment is successful, you will get these two information back as a result: **UIAD details** and **deployment successful**.


[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 7: ](Check BSP library and SAP Fiori Launchpad app descriptor item in Eclipse)]

  1. Open Eclipse and check the **BSP library** and **SAP Fiori Launchpad app descriptor item folder** in your package **`ZTRAVEL_APP_XXX`**.

    ![library](library.png)

[DONE]
[ACCORDION-END]


[ACCORDION-BEGIN [Step 8: ](Create IAM App and business catalog)]

  1. In Eclipse right-click on your package **`ZTRAVEL_APP_XXX`** and select **New** > **Other Repository Object**.

      ![iam](iam.png)

  2. Search for **IAM App**, select it and click **Next >**.

      ![iam](iam2.png)

  3. Create a new IAM App:
     - Name: **`ZTRAVEL_IAM_XXX`**
     - Description: IAM App

      ![iam](iam3.png)

      Click **Next >**.

  4. Click **Finish**.

      ![iam](iam4.png)

  5. Select **Services** and add a new one.

      ![iam](iam5.png)

  6. Select following:
      - Service Type: `OData V2`
      - Service Name: `ZUI_C_TRAVEL_M_XXX_0001`    

      ![iam](iam6.png)

      Click **OK**.

      **Save** and **activate** your IAM app.

  7. Right-click on your package **`ZTRAVEL_APP_XXX`** and select  **New** > **Other Repository Object**.

      ![catalog](catalog.png)

  8. Search for **Business Catalog**, select it and click **Next >**.

      ![catalog](catalog2.png)

  9. Create a new business catalog:
     - Name: **`ZTRAVEL_BC_XXX`**
     - Description: Business catalog

      ![catalog](catalog3.png)

      Click **Next >**.

 10. Click **Finish**.

      ![catalog](catalog4.png)

 11. Select **Apps** and add a new one.

      ![catalog](catalog5.png)

 12. Create a new business catalog:
     - IAM App: `ZTRAVEL_IAM_XXX_EXT`
     - Name: `ZTRAVEL_BC_XXX_0001`

      ![catalog](catalog6.png)

      Click **Next >**.

 13. Click **Finish**.

       ![catalog](catalog7.png)

 14. Click **Publish Locally** to publish your business catalog.

       ![catalog](catalog8.png)


[DONE]
[ACCORDION-END]


[ACCORDION-BEGIN [Step 9: ](Create index.html and run SAP Fiori application)]

  1. Go back to SAP Business Application Studio, select projects and open your project **`ztravel_app_xxx`**.Right-click your **`webapp`** folder and select **New File**.

      ![index](index.png)

  2. Create a new file:
     - Name: **`index.html`**

      ![run](index2.png)

      Click **OK**.

  3. Copy and paste following code:

    ```ABAP
    <!DOCTYPE html>
    <html>
    <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BonusPlan_MD_CXT_TECH_Standalone</title>
    <!-- Bootstrapping UI5 -->
    <script id="sap-ui-bootstrap"
            src="resources/sap-ui-core.js"
            data-sap-ui-libs="sap.m"
            data-sap-ui-theme="sap_bluecrystal"
            data-sap-ui-compatVersion="edge"
            data-sap-ui-preload="async"
            data-sap-ui-resourceroots='{"ztravel_app_xxx": "."}'
            data-sap-ui-frameOptions="trusted">
    </script> 
    <script>
        sap.ui.getCore().attachInit(function () {
            sap.ui.require([
                "sap/m/Shell",
                "sap/ui/core/ComponentContainer"
            ], function (Shell, ComponentContainer) {
                // initialize the UI component
                new Shell({
                    app: new ComponentContainer({
                        height : "100%",
                        name : "ztravel_app_xxx"
                    })
                }).placeAt("content");
            });
            });
     </script>
    </head>
    <!-- UI Content -->
    <body class="sapUiBody" id="content">
    </body>
    </html>

    ```

  5. Save **`index.html`**.

  6. Deploy your changes, therefore right-click on your project **`ztravel_app_xxx`** again and select **Open in Terminal**.

    ![url](url.png)


  7.  Enter **`npm run deploy`**. When prompted, check deployment configuration and press y.

      ![url](url2.png)

  7. Press **`CTRL and click on the following link`** to open the URL in a browser.

      ![url](url3.png)

  8. Login to ABAP Trial.

      ![url](url4.png)

  9. Click **Go**.

      ![url](url5.png)

 10. Check your result.

      ![url](url6.png)

[DONE]
[ACCORDION-END]


[ACCORDION-BEGIN [Step 10: ](Test yourself)]

[VALIDATE_1]
[ACCORDION-END]

<p style="text-align: center;">Give us 55 seconds of your time to help us improve</p>

<p style="text-align: center;"><a href="https://sapinsights.eu.qualtrics.com/jfe/form/SV_0im30RgTkbEEHMV?TutorialID=abap-environment-deploy-cf-production" target="_blank"><img src="https://raw.githubusercontent.com/SAPDocuments/Tutorials/master/data/images/285738_Emotion_Faces_R_purple.png"></a></p>
