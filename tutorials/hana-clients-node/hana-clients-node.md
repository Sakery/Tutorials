---
title: Connect Using the SAP HANA Node.js Interface
description: Create and debug a Node.js application that connects to SAP HANA using the SAP HANA client.
auto_validation: true
time: 15
tags: [ tutorial>beginner, products>sap-hana\,-express-edition, products>sap-hana-cloud]
primary_tag: products>sap-hana
---

## Prerequisites
 - You have completed the first 3 tutorials in this mission.

## Details
### You will learn
  - How to install Node.js and the SAP HANA client Node.js driver
  - How to create a Node.js application that queries a SAP HANA database

Node.js provides a JavaScript runtime outside of the browser and uses an asynchronous event driven programming model.  For more details, see [Introduction to Node.js](https://nodejs.dev/).  

---

[ACCORDION-BEGIN [Step 1: ](Install Node.js)]

The first step is to check if you have Node.js installed and what version it is.  Enter the following command:

```Shell
node -v  
```  

If Node.js is installed, it will return the currently installed version, such as v12.16.3.  

If node is not installed, download the long-term support (LTS) version of Node.js from [Node.js Download](https://nodejs.org/en/download/).

>An install is not provided on Linux.  The installation instructions can be found [here](https://nodejs.org/en/download/).

During the installation, there is no need to check the following box as you do not need to install Chocolatey.  

![Chocolatey](Chocolatey.png)

>The SAP HANA client provides a 32-bt and a 64-bit install, as does Node.js.  The Node.js driver provIded with the SAP HANA client is available for 64-bit only.  For additional details see SAP note [2939501 - SAP HANA Client Supported Platforms for 2.5 and later](https://launchpad.support.sap.com/#/notes/2939501).

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Install SAP HANA client for Node.js from NPM)]

Node.js packages are available using [NPM](https://www.npmjs.com/), which is the standard package manager for Node.js.  

1. Enter `@sap/hana-client`, and click **Search**.

    ![Search for hana-client](search-hana-client.png)  

    The page for the SAP HANA Node.js package on npm is shown below.

    ![npm page for hana-client](npm-hana-client.png)  

     It contains additional sample code, a weekly download counter, information about previous versions and the command to install the package using the npm command line interface (`cli`).

2. Create a folder named `node` and enter the newly created directory.

    ```Shell (Microsoft Windows)
    mkdir %HOMEPATH%\HANAClientsTutorial\node
    cd %HOMEPATH%\HANAClientsTutorial\node
    ```

    ```Shell (Linux or Mac)
    mkdir $HOME/HANAClientsTutorial/node
    cd $HOME/HANAClientsTutorial/node
    ```

3. Initialize the project and install the `hana-client` driver from NPM.

    ```Shell
    npm init -y
    npm install @sap/hana-client
    ```

    >The hana-client driver is also available from the HANA client install folder.  The install location was set during the install.

    >```Shell
    >npm install C:\SAP\hdbclient\node
    ```

    >If you encounter an error about permissions, on Microsoft Windows, run or open the command prompt as an administrator, or use `sudo` on Linux or Mac.

The following command will list the Node.js modules that are now installed locally into the `HANAClientsTutorial\node` folder.  Note that the extraneous message can be ignored.  

```Shell
npm list
```

> ### Some Tips

>At this point, the SAP HANA client module has been installed into the `HANAClientsTutorials\node\node_modules` folder and added as a dependency in the `packages.json` file.  The following is some extra optional information on NPM.  

> ---

>Node.js modules can also be installed globally. To see the list of node modules installed globally enter the following command.  

>The depth parameter can be used to specify the number of levels to show when displaying module dependencies.  By setting depth=x, a tree-structure is outputted that shows modules that are x levels below the top-level module.

>```Shell
>npm list -g
>npm list -g --depth=0
>```  

>Command line help for NPM is available.  A few examples of this are shown below.

>```Shell
>npm help
>npm help list
>```  

>Additional information can be found out for a module, such as the debug module, via the info command.

>```Shell
>npm info @sap/hana-client
>```  

>The following commands can be used to view the latest available version of a package, remove a package, add a specific version of a package and then update it to the latest version.

>```Shell
>npm view @sap/hana-client version
>npm uninstall @sap/hana-client
>npm install @sap/hana-client@2.4.167
>npm list @sap/hana-client
>npm update @sap/hana-client
>npm list @sap/hana-client
>```

>SAP also maintains a NPM registry.  The following steps show some commands on how to configure NPM to use the registry npm.sap.com.
>
>```Shell
>npm config list
>npm config set @sap:registry="https://npm.sap.com"
>npm info @sap/hana-client
>npm config set @sap:registry=
>```

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Create a Node.js application that queries SAP HANA)]

1. Open an editor on a file named nodeQuery.js.

    ```Shell (Microsoft Windows)
    cd %HOMEPATH%\HANAClientsTutorial\node
    notepad nodeQuery.js
    ```

    Substitute `pico` below for your preferred text editor.  

    ```Shell (Linux or Mac)
    cd $HOME/HANAClientsTutorial/node
    pico nodeQuery.js
    ```

2. Add the code below to `nodeQuery.js` and update the `serverNode` values in the `connOptions` to match your SAP HANA host and port.

    ```JavaScript
    'use strict';
    const { PerformanceObserver, performance } = require('perf_hooks');
    var t0 = performance.now();
    var util = require('util');
    var hana = require('@sap/hana-client');

    var connOptions = {
        serverNode: '@User1UserKey',
        //serverNode: 'your host:your port',
        //UID: 'USER1',
        //PWD: 'Password1',
        encrypt: 'true',  //Must be set to true when connecting to SAP HANA Cloud
        sslValidateCertificate: 'false',  //Must be set to false when connecting
        //to an SAP HANA, express edition instance that uses a self signed certificate.

        //Below setting is used to specify where the trust store is
        //ssltruststore: '/home/dan/.ssl/trust2.pem',

        //Alternatively provide the contents of the certificate directly (DigiCertGlobalRootCA.pem)
        //ssltruststore: '-----BEGIN CERTIFICATE-----MIIDrzCCApegAwIBAgIQCDvgVpBCRrGhdWrJWZHHSjANBgkqhkiG9w0BAQUFADBhMQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBDQTAeFw0wNjExMTAwMDAwMDBaFw0zMTExMTAwMDAwMDBaMGExCzAJBgNVBAYTAlVTMRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxGTAXBgNVBAsTEHd3dy5kaWdpY2VydC5jb20xIDAeBgNVBAMTF0RpZ2lDZXJ0IEdsb2JhbCBSb290IENBMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4jvhEXLeqKTTo1eqUKKPC3eQyaKl7hLOllsBCSDMAZOnTjC3U/dDxGkAV53ijSLdhwZAAIEJzs4bg7/fzTtxRuLWZscFs3YnFo97nh6Vfe63SKMI2tavegw5BmV/Sl0fvBf4q77uKNd0f3p4mVmFaG5cIzJLv07A6Fpt43C/dxC//AH2hdmoRBBYMql1GNXRor5H4idq9Joz+EkIYIvUX7Q6hL+hqkpMfT7PT19sdl6gSzeRntwi5m3OFBqOasv+zbMUZBfHWymeMr/y7vrTC0LUq7dBMtoM1O/4gdW7jVg/tRvoSSiicNoxBN33shbyTApOB6jtSj1etX+jkMOvJwIDAQABo2MwYTAOBgNVHQ8BAf8EBAMCAYYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUA95QNVbRTLtm8KPiGxvDl7I90VUwHwYDVR0jBBgwFoAUA95QNVbRTLtm8KPiGxvDl7I90VUwDQYJKoZIhvcNAQEFBQADggEBAMucN6pIExIK+t1EnE9SsPTfrgT1eXkIoyQY/EsrhMAtudXH/vTBH1jLuG2cenTnmCmrEbXjcKChzUyImZOMkXDiqw8cvpOp/2PV5Adg06O/nVsJ8dWO41P0jmP6P6fbtGbfYmbW0W5BjfIttep3Sp+dWOIrWcBAI+0tKIJFPnlUkiaY4IBIqDfv8NZ5YBberOgOzW6sRBc4L0na4UU+Krk2U886UAb3LujEV0lsYSEY1QSteDwsOoBrp+uvFRTp2InBuThs4pFsiv9kuXclVzDAGySj4dzp30d8tbQkCAUw7C29C79Fv1C5qfPrmAESrciIxpg0X40KPMbp1ZWVbd4=-----END CERTIFICATE-----'
    };

    var connection = hana.createConnection();
    connection.connect(connOptions, function(err) {
        if (err) {
            return console.error(err);
        }
        var sql = 'select TITLE, FIRSTNAME, NAME from HOTEL.CUSTOMER;';
        var rows = connection.exec(sql, function(err, rows) {
            if (err) {
                return console.error(err);
            }
            console.log(util.inspect(rows, { colors: false }));
            var t1 = performance.now();
            console.log("time in ms " +  (t1 - t0));
            connection.disconnect(function(err) {
                if (err) {
                    return console.error(err);
                }   
            });
        });
    });
    ```  


4. Run the app.  

    ```Shell
    node nodeQuery.js
    ```
![SAP HANA Express result](Node-query.png)

Note the above app makes use of some of the SAP HANA client Node.js driver methods, such as [connect](https://help.sap.com/viewer/f1b440ded6144a54ada97ff95dac7adf/latest/en-US/d7226e57dbd943aa9d8cd0b840da3e3e.html), [execute](https://help.sap.com/viewer/f1b440ded6144a54ada97ff95dac7adf/latest/en-US/ef5564058b1747ce99fd3d1e03266b39.html) and [disconnect](https://help.sap.com/viewer/f1b440ded6144a54ada97ff95dac7adf/latest/en-US/fdafeb1d881947bb99abd53623996b70.html).

In nodeQuery.js, the asynchronous versions of these methods are used because the optional callback function is provided.  For readers that are unfamiliar with synchronous and asynchronous operations, see [The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/de/docs/guides/event-loop-timers-and-nexttick/).

>To enable debug logging of the SAP  HANA Node.js client, enter the following command and then rerun the app.

>```Shell (Microsoft Windows)
>set DEBUG=*
>```  

>```Shell (Linux or Mac)
export DEBUG=*
>```    

> ![debug output](debug_flag.png)

> The value of the environment variable DEBUG can be seen and removed with the commands below.

>```Shell (Microsoft Windows)
>set DEBUG
>set DEBUG=
>set DEBUG
>```  

>```Shell (Linux or Mac)
>printenv | grep DEBUG
>unset DEBUG
>printenv | grep DEBUG
>```


[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 4: ](Debug the application)]

Visual Studio Code can be used to run and debug a Node.js application.  

1. [Download Visual Studio Code.](https://code.visualstudio.com/Download)

2. In Visual Studio Code, choose **File | Add Folder to Workspace** and then add the `HANAClientsTutorial` folder.

    ![Workspace](workspace.png)

3. Open the file `nodeQuery.js`.

4. Place a breakpoint inside the `connection.exec` callback.  Select **Run | Start Debugging**.  

    Notice that the debug view becomes active and that the RUN option is Node.js.  

    Notice that the program stops running at the breakpoint that was set. Observe the variable values in the leftmost pane.  Step through code.

    ![VS Code Debugging](debugging.png)


Congratulations! You have created and debugged a Node.js application that connects to and queries an SAP HANA database.

[VALIDATE_1]
[ACCORDION-END]


---
