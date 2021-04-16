![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Leveraging Azure Digital Twins in a supply chain
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
April 2021
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2021 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents** 

<!-- TOC -->
- [Leveraging Azure Digital Twins in a supply chain hands-on lab step-by-step](#leveraging-azure-digital-twins-in-a-supply-chain-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
  - [Exercise 1: Authoring Digital Twins Definition Language (DTDL) models](#exercise-1-authoring-digital-twins-definition-language-dtdl-models)
    - [Task 1: The components of a model](#task-1-the-components-of-a-model)
    - [Task 2: Ontologies overview](#task-2-ontologies-overview)
    - [Task 3: Validating the storeroom model](#task-3-validating-the-storeroom-model)
  - [Exercise 2: Loading models into Azure Digital Twins](#exercise-2-loading-models-into-azure-digital-twins)
    - [Task 1: Configure Azure Digital Twins permissions](#task-1-configure-azure-digital-twins-permissions)
    - [Task 2: Loading models using the CLI](#task-2-loading-models-using-the-cli)
    - [Task 3: Setup the Azure Digital Twins Explorer application](#task-3-setup-the-azure-digital-twins-explorer-application)
    - [Task 4: Loading models using the Azure Digital Twins Explorer](#task-4-loading-models-using-the-azure-digital-twins-explorer)
  - [Exercise 3: Creating digital twin instances to build an environment knowledge graph](#exercise-3-creating-digital-twin-instances-to-build-an-environment-knowledge-graph)
    - [Task 1: Create digital twin instances using the CLI](#task-1-create-digital-twin-instances-using-the-cli)
    - [Task 2: Create a digital twin instance using the Azure Digital Twins Explorer](#task-2-create-a-digital-twin-instance-using-the-azure-digital-twins-explorer)
    - [Task 3: Importing digital twin instances using a spreadsheet](#task-3-importing-digital-twin-instances-using-a-spreadsheet)
  - [Exercise 4: Querying and visualizing the Azure Digital Twins graph](#exercise-4-querying-and-visualizing-the-azure-digital-twins-graph)
    - [Task 1: Run digital twin queries using the CLI](#task-1-run-digital-twin-queries-using-the-cli)
    - [Task 2: Run digital twin queries using Azure Digital Twins Explorer](#task-2-run-digital-twin-queries-using-azure-digital-twins-explorer)
  - [Exercise 5: Keeping Azure Digital Twin instances up-to-date](#exercise-5-keeping-azure-digital-twin-instances-up-to-date)
    - [Task 1: Manually update the state of a digital twin using the CLI](#task-1-manually-update-the-state-of-a-digital-twin-using-the-cli)
    - [Task 2: Manually update the state of a digital twin using the Azure Digital Twins Explorer](#task-2-manually-update-the-state-of-a-digital-twin-using-the-azure-digital-twins-explorer)
    - [Task 3: Using a logic application to trigger Azure Digital Twin updates](#task-3-using-a-logic-application-to-trigger-azure-digital-twin-updates)
  - [Exercise 6: Simulating real-world telemetry](#exercise-6-simulating-real-world-telemetry)
    - [Task 1: Task name](#task-1-task-name)
  - [Exercise 7: Visualizing incoming data with Azure Time Series Insights](#exercise-7-visualizing-incoming-data-with-azure-time-series-insights)
    - [Task 1: Task name](#task-1-task-name-1)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete resource group](#task-1-delete-resource-group)
<!-- /TOC -->

# Leveraging Azure Digital Twins in a supply chain hands-on lab step-by-step

## Abstract and learning objectives

\[Insert what is trying to be solved for by using this workshop. . . \]

## Overview

\[insert your custom workshop content here . . . \]

## Solution architecture

![](media/solution_architecture.png)

## Requirements

1. Azure Subscription
2. [Azure CLI 2.3.1+](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
3. PowerShell ([macOS](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6&preserve-view=true)) (Windows: PowerShell is built in)
4. [Visual Studio Code](https://code.visualstudio.com/)
5. [.NET Core 3.1](https://dotnet.microsoft.com/download)
6. [C# Visual Studio Code Extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
7. [Node.js 10+](https://nodejs.org/)

## Before the hands-on lab

Refer to the Before the hands-on lab setup guide manual before continuing to the lab exercises.

## Exercise 1: Authoring Digital Twins Definition Language (DTDL) models

Duration: X minutes

The Azure Digital Twins PaaS service's core offering is the ability to create knowledge graphs representing an entire environment based on digital models. An environment could be a store, a city, factory, farm, or even the whole planet. Contained within an environment, you will find people, places, and things. Each model defined is significant to a business and is entirely customizable. They can be as large as the world and as small as a tiny sensor. The environment, the components found within, and the relationships between them are represented digitally using a model expressed using Digital Twin Definition Language (DTDL). Digital Twin Definition Language is based on JSON-LD (JavaScript Object Notation for Linked Data). Azure Digital Twins uses DTDL version 2. You can think of a model definition as being similar to a class in object-oriented programming. Later, you will use these model definitions to create digital twins instances representing specific entities in an environment.

In this exercise, you will be assisting Contoso Apparel in defining a model representing a storeroom. In Contoso's environment, we must track the temperature and humidity telemetry coming storeroom as well as the current stock level.

### Task 1: The components of a model

At the top-level, a DTDL model defines an interface that encapsulates the rest of the model definition. A DTDL model interface may contain zero, one, or many of each of the following fields:

| Field | Description |
|-------|-------------|
| Property | Properties are data fields that represent the state of an entity (like the properties in many object-oriented programming languages). Properties have backing storage and can be read at any time. If the property is writeable, you can also store a value in the property.|
| Telemetry | Telemetry is a set of data messages that have short lifespans. If you don't set up listening for the event and actions to take when it happens, there is no trace of the event at a later time. You can't come back to it and read it later. Telemetry is typically a single measurement sent by a device. |
| Component |Components allow you to build your model interface as an assembly of other interfaces. Use a component to describe something that is an integral part of your solution but doesn't need a separate identity, and doesn't need to be created, deleted, or rearranged in the twin graph independently. |
| Relationship |  Relationships let you represent how a digital twin can be involved with other digital twins. Relationships can represent different semantic meanings, such as contains ("floor contains room"), cools ("hvac cools room"), isBilledTo ("compressor is billed to user"), etc. Relationships allow the solution to provide a graph of interrelated entities. |

It is essential to distinguish between properties and telemetry. With properties, it has a backing store so that you can read and query the data fields. Telemetry is the stream of data from an IoT device that contains things like temperature and humidity values. These values are not stored on the device itself. You are not able to query for the latest temperature value from a telemetry field. Instead, you'll need to have a data ingress function listen to the device's messages and take actions as they arrive. Based on the business rules defined by the logic of the ingress function, you can then update a property based on the incoming telemetry (or property events) from the device using the Azure Digital Twins API.

Here is an example of a Planet model. Model files should be saved with the **.json** extension. This example demonstrates the proper syntax in defining the Planet interface, properties, telemetry, relationship, and component. Take note that when defining a component interface (Crater in this example), it should be defined in the same array as the interface that uses it (Planet).

```JavaScript
[
    {
      "@id": "dtmi:com:contoso:Planet;1",
      "@type": "Interface",
      "@context": "dtmi:dtdl:context;2",
      "displayName": "Planet",
      "contents": [
        {
          "@type": "Property",
          "name": "name",
          "schema": "string"
        },
        {
          "@type": "Property",
          "name": "mass",
          "schema": "double"
        },
        {
          "@type": "Telemetry",
          "name": "Temperature",
          "schema": "double"
        },
        {
          "@type": "Relationship",
          "name": "satellites",
          "target": "dtmi:com:contoso:Moon;1"
        },
        {
          "@type": "Component",
          "name": "deepestCrater",
          "schema": "dtmi:com:contoso:Crater;1"
        }
      ]
    },
    {
      "@id": "dtmi:com:contoso:Crater;1",
      "@type": "Interface",
      "@context": "dtmi:dtdl:context;2"
    },
    {
      "@id": "dtmi:com:contoso:Moon;1",
      "@type": "Interface",
      "@context": "dtmi:dtdl:context;2"
    }
  ]
```

> **TIP**: DTDL also supports model inheritance using the **extends** field.

Let's now continue this exercise by defining a model for a storeroom using DTDL. Feel free to refer to the [DTDL specification](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) as a reference.

1. Let's begin by defining the interface for a storeroom. Open Visual Studio Code to the `Hands-on lab/Resources/models` folder, and create a new file named **storeroom.json**. Add the following text to this file:

    ```JavaScript
    [
        {
            "@id": "dtmi:com:contoso:storeroom;1",
            "@context": "dtmi:dtdl:context;2",
            "@type": "Interface",
            "displayName": "StoreRoom"
        }
    ]
    ```

    | Field | Description |
    |-------|-------------|
    | @id | required field, must take on the following standard format: **dtmi:DOMAIN:UNIQUE MODEL IDENTIFIER;MODEL VERSION NUMBER** |
    | @context | indicates that we are using DTDL version 2 |
    | @type | indicates the kind of information being described |
    | displayName | the friendly name of the model being described |

2. Now that we've defined our interface, we are able to define the contents of the model. Let's start with the properties of a storeroom. The current stock level is a property that Contoso Apparel needs to track and query. Continuing in the same file, add the following beneath the **displayName** field (remember to add a comma after the displayName value!):

    ```JavaScript
    "contents": [
        {
            "@type": "Property",
            "name": "StockLevel",
            "schema": "integer",
            "writable": true
        }
    ]
    ```

3. Let's define the telemetry of a storeroom. In the case of Contoso Apparel's sensors, a storeroom sends a consistent stream of integer temperature and humidity values. Within the **contents** array, add the following telemetry definitions (remember to add a comma after the StockLevel object definition):

    ```JavaScript
    {
      "@type": "Telemetry",
      "name": "Humidity",
      "schema": "integer"
    },
    {
      "@type": ["Telemetry", "Temperature"],
      "name": "Temperature",
      "schema": "integer",
      "unit": "degreeCelsius"
    }
    ```

    You'll notice a slight difference in the definition of the Temperature telemetry type. In this case, the telemetry is defined with a [semantic type](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md#semantic-types). When using semantic types, the unit property must be an instance of the corresponding unit type, and the schema type must be a numeric type (double, float, integer, or long). The use of semantic types is optional. We could have defined the Humidity **@type** as **["Telemetry", "RelativeHumidity"]** (in the DTDL specification **RelativeHumidity** doesn't have a corresponding **unit** therefore need not be described in the model).

4. Review the final `storeroom.json` file and ensure it matches the following:

    ```JavaScript
    [
        {
            "@id": "dtmi:com:contoso:storeroom;1",
            "@context": "dtmi:dtdl:context;2",
            "@type": "Interface",
            "displayName": "StoreRoom",
            "contents": [
                {
                    "@type": "Property",
                    "name": "StockLevel",
                    "schema": "integer",
                    "writable": true
                },
                {
                    "@type": "Telemetry",
                    "name": "Humidity",
                    "schema": "integer"
                },
                {
                    "@type": ["Telemetry", "Temperature"],
                    "name": "Temperature",
                    "schema": "integer",
                    "unit": "degreeCelsius"
                }
            ]
        }
    ]
    ```

5. Now we need to add StoreRoom as a relationship in the Factory model. Open the `Hands-on lab/Resources/models/factory.json` file, and add the following to the **contents** array:

  ```JavaScript
  {
      "@type": "Relationship",
      "name": "containsStoreRoom",
      "target": "dtmi:com:contoso:storeroom;1"
  }
  ```

### Task 2: Ontologies overview

In the previous task, we created a model definition of a storeroom manually. In this task, we'll investigate pre-existing model sets aligned to specific industries, also referred to as an ontology. Currently there exists two open-source DTDL ontologies that leverage industry standards in their model definitions. These are the [real estate industry smart building](https://github.com/Azure/opendigitaltwins-building), and the [smart city industry](https://github.com/Azure/opendigitaltwins-smartcities). You can take advantage of these existing ontologies to form the basis of your model definitions. You have the ability to adopt them as-is and to extend them as needed using model inheritance.

1. An ontology provides a common representation of places, infrastructure, and assets to enable interoperability and data sharing across multiple domains. In a new browser window or tab, visit the following URL [https://github.com/Azure/opendigitaltwins-building](https://github.com/Azure/opendigitaltwins-building).

2. This ontology is representative of the **RealEstateCore** ontology for smart buildings. Take a moment and review the structure as well as the motivations behind this ontology by reading through the README.md article found at this URL. This article also describes how to extend and contribute to this ontology.

3. Remaining on this website, navigate into the **Ontology/Space/Building/Building.json** file to view the definition of a building. Note that this model extends the **Space** definition located at **Ontology/Space/Space.json**.

4. You are also able to visualize and interact with the **RealEstateCore** ontology by opening the following url in a new tab or window [http://www.visualdataweb.de/webvowl/#iri=https://w3id.org/rec/full/3.3/](http://www.visualdataweb.de/webvowl/#iri=https://w3id.org/rec/full/3.3/).

    ![The RealEstateCore model visualization graph is shown.](media/realestatecorevisualization.png "RealEstateCore model visualization")

5. When you have finished investigating the **RealEstateCore** ontology, you may close this tab. You may optionally investigate the [**Smart Cities** ontology](https://github.com/Azure/opendigitaltwins-smartcities).

### Task 3: Validating the storeroom model

It is recommended that you validate your DTDL models offline prior to loading them into the Azure Digital Twins service. We will validate our model definitions using the [DTDL validator](https://github.com/Azure-Samples/DTDL-Validator/tree/master/) to run validation on the models folder.

1. In a new browser tab or window, access the [DTDL Validator code sample](https://docs.microsoft.com/en-us/samples/azure-samples/dtdl-validator/dtdl-validator/) webpage.

2. Select the **Download ZIP** button.

3. Extract the contents of the downloaded ZIP file to the location of your choice. For example, I've extracted mine to `C:\SourceCode\DTDL_Validator`.

4. Using Visual Studio Code, open the folder where you extracted the files.

5. If you are prompted with notifications about missing assets and unresolved dependencies, select **Yes** and **Restore**.

    ![Two Visual Studio notifications are shown indicating the required assets to build and debug are missing from the Validator project as well as a notification indicating there are unresolved dependencies.](media/validatorprojectdependencieswarning.png "Visual Studio Code notifications")

6. Open a terminal window by selecting **View > Terminal** from the Visual Studio Code menu.

7. In the terminal, navigate to the application directory by executing the following command:

   ```PowerShell
   cd .\DTDLValidator-Sample\DTDLValidator\
   ```

8. Copy the full path to the **models** folder of this lab (`C:\PATH_TO_LAB_FILES\Hands-on lab\Resources\models`) to a text editor for use in the next step.

9. In the Visual Studio Code terminal window, execute the following command (replacing **MODEL_FOLDER_PATH** with the full path to your models folder copied in the previous step, keeping the quote characters intact):

    ```PowerShell
    dotnet run -d "MODEL_FOLDER_PATH"
    ```

10. You should get a message indicating that all files in the models folder are valid.

  ![A portion of the Visual Studio Code terminal window is shown with a message indicating that all files have been validated and that the DTDL is valid.](media/validationtoolresults.png "Visual Studio Code Terminal Window Output")

## Exercise 2: Loading models into Azure Digital Twins

Duration: X minutes

In the previous exercise we learned how to author and validate DTDL models. In this exercise, we will load these models into the Azure Digital Twins Azure service.

### Task 1: Configure Azure Digital Twins permissions

Specific permissions are required to have the ability to maintain models in the Azure Digital Twins service. In this exercise, you will add yourself as an **Azure Digital Twins Data Owner** using role-based access control (RBAC).

1. [Azure portal](https://portal.azure.com), open the resource group you created for this lab.

2. Select the Azure Digital Twins service from the list named **{PREFIX}digtwins**, where `PREFIX` is the generated value or the prefix you specified in the **Before the HOL** steps.

    ![The resource group service listing is shown with the Azure Digital Twins service selected.](media/resourcegrouplist_digitaltwins.png "Resource group service listing")

3. From the left menu, select **Access control (IAM)**, then expand the **+ Add** button in the toolbar and select **Add role assignment**.

    ![In the Azure Digital Twins service screen, Access control (IAM) is selected from the left menu. The +Add button is expanded with the Add role assignment option highlighted.](media/digtwins_iam_menu.png "Azure Digital Twins IAM")

4. In the **Add role assignment** blade, select the **Azure Digital Twins Data Owner** role, and search for and select your Azure Active Directory account (search for your login email address). Select **Save**.

    ![The Add role assignment blade is shown with an email in the Select field and a selected member chosen. The Save button is highlighted.](media/digtwins_addroleassignment.png "Add role assignment blade")

### Task 2: Loading models using the CLI

Azure Digital Twins has a command set for the Azure CLI that you can use to perform most management functions with the service. The commands relative to digital twins are part of the larger [Azure IoT extension for Azure CLI](https://github.com/Azure/azure-iot-cli-extension). You can review the digital twins specific command reference here: [az dt command reference](https://docs.microsoft.com/en-us/cli/azure/dt).

1. In the [Azure portal](https://portal.azure.com), open the resource group you created for this lab.

2. Select the **Azure Digital Twins** service from the list named **{PREFIX}digtwins**, where `PREFIX` is the generated value or the prefix you specified in the **Before the HOL** steps.

    ![The resource group service listing is shown with the Azure Digital Twins service selected.](media/resourcegrouplist_digitaltwins.png "Resource group service listing")

3. On the **Overview** screen, make note of the values for **Resource group**, **Host name**, and the name of the Azure Digital Twins instance. You should store these values in a text editor for later use. Please note that your values will differ from the screenshot below.

   ![The Azure Digital Twins Overview screen is shown with the resource name, Resource group, and Host name values highlighted.](media/digitaltwins_overview.png "Azure Digital Twins Overview")

4. Open a command prompt on your local machine.

5. Issue the following Azure CLI command to ensure that you are using the latest version of the Azure IoT extension for Azure CLI.

    ```Bash
    az extension add --upgrade -n azure-iot
    ```

6. Next, you will need to authenticate to Azure by executing the following command and following the instructions provided in the prompts:

    ```Bash
    az login
    ```

7. Change the current directory to the **models** folder of this lab (`C:\PATH_TO_LAB_FILES\Hands-on lab\Resources\models`) by issuing the following command(replacing **MODEL_FOLDER_PATH** with the full path to your models folder, retaining the quote characters):
  
    ```Bash
    cd "MODEL_FOLDER_PATH"
    ```

8. Upload the storeroom model using the CLI with the following command (replace ADT_INSTANCE_NAME with your Azure Digital Twins instance name):

    ```Bash
    az dt model create -n ADT_INSTANCE_NAME --models storeroom.json
    ```

    After a few seconds, you should see output similar to the following:

    ![Output from a console window is displayed showing a JSON definition of the storeroom model that was added to the Azure Digital Twins instance](media/cli_addmodel_output.png "CLI console output")

9. Verify that your model was uploaded successfully by executing the following command that lists registered models with your Azure Digital Twins instance (replace ADT_INSTANCE_NAME with your Azure Digital Twins instance name):
  
    ```Bash
    az dt model list -n ADT_INSTANCE_NAME
    ```

10. Keep this console window open for further CLI tasks later on in this lab.

### Task 3: Setup the Azure Digital Twins Explorer application

The Azure Digital Twins Explorer is a Node.js based single page web application that you will run on your computer. This application connects to an Azure Digital Twins instance and provides features to manage models and twins instances (along with their properties). Azure Digital Twins Explorer provides visualizations of the twins graph, the ability to edit twin instance properties, and to run queries across the twins graph.

1. In a new tab or web browser window, visit the [Azure Digital Twins Explorer GitHub repository](https://github.com/Azure-Samples/digital-twins-explorer/tree/main/).

2. Follow the steps in the [**Getting Started** -> Running digital-twins-explorer locally](https://github.com/Azure-Samples/digital-twins-explorer/tree/main/#running-digital-twins-explorer-locally) section of this page, beginning with step 2 (we already have an Azure Digital Twins service instance) to step 6.

3. With the Azure Digital Twins Explorer application running locally, the web application will display a modal window where you will enter the host name value for your Azure Digital Twins service instance. Be sure to pre-pend the host name value with **https://**, then select **Save**.

   ![A modal window is displayed prompting for the host name URL of the Azure Digital Twins service instance.](media/adtexplorer_hostnameentry.png "Azure Digital Twins Explorer Host Name Entry")

>**NOTE**: Prior to running the web application from the command prompt, remember to execute the **az login** command to ensure appropriate authentication.

### Task 4: Loading models using the Azure Digital Twins Explorer

The Azure Digital Twins Explorer provides an intuitive user interface to manage our Azure Digital Twins service instance. In this task, we will use this interface to upload the remainder of the models for our environment.

1. In the **MODELS** blade of the Azure Digital Twins Explorer, you should see the StoreRoom model that we uploaded via the CLI earlier.

    ![The Azure Digital Twins Explorer MODELS blade displays the storeroom model.](media/modelsblade_adt_storeroom.png "Azure Digital Twins Explorer MODELS blade")

2. In the **MODELS** blade, select the **Upload a Model** button.

    ![The Azure Digital Twins Explorer MODELS blade is shown with the Upload a Model button highlighted.](media/adte_uploadamodelbutton.png "Azure Digital Twins Explorer Upload a Model")

3. Navigate to the lab **models** folder (`C:\PATH_TO_LAB_FILES\Hands-on lab\Resources\models`). Select all the files with the ***.json** extension in this folder. The dialog allows uploading multiple files at once.

4. Verify that you have all 11 models loaded to the Azure Digital Twins instance.

    ![The Azure Digital Twins Explorer MODELS blade is shown with 11 models listed.](media/adte_elevenmodels.png "Azure Digital Twins Explorer models listing")

5. Keep the Azure Digital Twins Explorer web application open for future tasks.

## Exercise 3: Creating digital twin instances to build an environment knowledge graph

Duration: X minutes

Now that we've modeled the entities in our environment, we are ready to create digital twin instances based on these models to represent the real world artifacts. In this exercise, we will continue assisting Constoso Apparel with defining four of the storeroom instances that exist in their environment.

### Task 1: Create digital twin instances using the CLI

1. Return to your CLI command prompt window.

2. The first storeroom that we will define has the identifier of **SR90636**. To create this digital twin instance, execute the following CLI command  (replace ADT_INSTANCE_NAME with your Azure Digital Twins instance name):
  
    ```Bash
    az dt twin create -n ADT_INSTANCE_NAME --dtmi "dtmi:com:contoso:storeroom;1" --twin-id "SR90636"
    ```

3. The second storeroom that we will define has the identifier of **SR90637**. To create this digital twin instance, execute the following CLI command (replace ADT_INSTANCE_NAME with your Azure Digital Twins instance name):
  
    ```Bash
    az dt twin create -n ADT_INSTANCE_NAME --dtmi "dtmi:com:contoso:storeroom;1" --twin-id "SR90637"
    ```

4. Query for all defined digital twins by executing the following command (replace ADT_INSTANCE_NAME with your Azure Digital Twins instance name):

    ```Bash
    az dt twin query -n ADT_INSTANCE_NAME -q "select * from digitaltwins"
    ```

    ![A portion of a command window output is displayed showing a results array with two storeroom digital twins instances returned.](media/cli_queryresults_alltwins.png "Digital Twins query output")

5. Keep this command window open for additional CLI tasks later on in this lab.

### Task 2: Create a digital twin instance using the Azure Digital Twins Explorer

We will use the Azure Digital Twins Explorer web application to create two additional instances of a storeroom.

1. Return to the Azure Digital Twins Explorer web application.

2. In the **MODELS** blade, locate the **StoreRoom** model, and select the **Create a Twin** button.

    ![A section of the MODELS blade is shown with the Create a Twin button highlighted on the StoreRoom model item.](media/adte_createatwinbutton_storeroom.png "Create a Twin button")

3. We will now create a digital twin for the storeroom identified by **SR90638**. In the modal dialog that displays, enter `SR90638`, then select **Save**.

    ![A modal dialog is shown with the value SR90638 entered in the New Twin Name text field.](media/adte_createtwin_SR90638.png "New Twin Name modal dialog")

4. Repeat the previous step and create a digital twin for the storeroom identified by **SR90639**.

5. You should see the two storeroom instances show on the **TWIN GRAPH** canvas in the web application.

   ![The TWIN GRAPH canvas is shown with two circles representing both storerooms that were defined in this task.](media/adte_twingraph_twostorerooms.png "TWIN GRAPH")

6. You may be wondering where the other two storerooms are that we had defined in the CLI. To refresh the TWIN GRAPH, select the **Run Query** button from the QUERY EXPLORER section near the top of the screen. This will query the Azure Digital Twins instance and return all digital twins instances.

    ![The TWIN GRAPH canvas is shown with four circles representing all the storeroom digital twins that have been defined.](media/adte_twingraph_fourstorerooms.png "TWIN GRAPH")

7. Keep the Azure Digital Twins Explorer web application open for future lab tasks.

### Task 3: Importing digital twin instances using a spreadsheet

Creating digital twin instances one-by-one via CLI and in the Azure Digital Twins could be a lengthy task. Lucky for us, we are able to define and provide initialization details for bulk import via a spreadsheet.

1. In Azure Digital Twins Explorer select the **Import Graph** button from the **TWIN GRAPH** canvas upper right toolbar.

    ![The TWIN GRAPH toolbar is shown with the Import Graph button highlighted.](media/adte_importgraph_button.png "TWIN GRAPH toolbar")

2. In the open file dialog, navigate to the lab **models** folder (`C:\PATH_TO_LAB_FILES\Hands-on lab\Resources\models`). Select the `FullTwins.xlsx` file.

3. Select the **Save** icon at the upper right of the IMPORT canvas, this canvas shows a preview of the graph.

    ![The IMPORT graph preview is shown with many additional nodes.](media/adte_graphimport_result.png "IMPORT graph")

4. Return to the **TWIN GRAPH**, and select the **Run Query** button from the **QUERY EXPLORER** to refresh the graph. Give this refresh a few moments to run.

5. Allow a few moments for the import to complete. You will then see a dialog indicating a successful import.

    ![The Import Successful dialog is shown indicating 152 twins were imported along with 162 relationships](media/adte_graphimportsuccess_dialog.png "Azure Digital Twins Explorer Import Successful dialog")

6. Your graph should look substantially larger now.

   ![The TWINS GRAPH is updated with many additional nodes.](media/adte_fulltwinsgraph.png "Updated TWINS GRAPH")

7. The graph can be made even more representational of the physical world using easy to identify iconography. To add icons representing each of our models, select the **Upload Model Images** button on the **MODELS** blade toolbar.

    ![The MODELS blade toolbar menu is shown with the Upload Model Images button highlighted.](media/adte_modelstoolbar_importimages.png "Upload Model Images")

8. In the open file dialog, navigate to the **icons** folder found in the **models** folder of the lab(`C:\PATH_TO_LAB_FILES\Hands-on lab\Resources\models\icons`). Select all image files in this folder. Note the name of the icons match the model identifiers and version of the models they represent.

9. Once uploaded, the **TWIN GRAPH** will refresh with the icons.

    ![A portion of the TWIN GRAPH is displayed with icons representing the models shown.](media/adte_filltwinsgraph_icons.png "TWIN GRAPH")

10. Keep the Azure Digital Twins Explorer web application open for future lab tasks.

## Exercise 4: Querying and visualizing the Azure Digital Twins graph

Duration: X minutes

X

### Task 1: Run digital twin queries using the CLI
### Task 2: Run digital twin queries using Azure Digital Twins Explorer

## Exercise 5: Keeping Azure Digital Twin instances up-to-date

Duration: X minutes

ASDF

### Task 1: Manually update the state of a digital twin using the CLI

1. S

### Task 2: Manually update the state of a digital twin using the Azure Digital Twins Explorer

1. X

### Task 3: Using a logic application to trigger Azure Digital Twin updates

## Exercise 6: Simulating real-world telemetry

Duration: X minutes

ASDF

### Task 1: Task name

1. Asdf.

## Exercise 7: Visualizing incoming data with Azure Time Series Insights

Duration: X minutes

ASDF

### Task 1: Task name

1. Asdf.

## After the hands-on lab

Duration: X minutes

\[insert your custom Hands-on lab content here . . .\]

### Task 1: Delete resource group

1. In the Azure Portal, delete the resource group you created for this lab.

You should follow all steps provided *after* attending the Hands-on lab.
