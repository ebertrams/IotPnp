<!-- markdownlint-disable MD033 -->
# IoT Plug and Play Samples Bug Bash instructions for the GA Sept 30th 2020 release

## Intro

IoT Plug and Play Platform, SDK and tools will be GA the Sept 30th 2020.

This bug bash is focused on the  IoT Hub features, SDKs, and tooling. 
We have quickstarts in various languages ready, for device and service scenarios but the focus of this edition is **Managed languages**.

We'll hope you will have fun with:

1. C# and Java device PnP samples
1. C# and Java service PnP samples
1. C# sample of IoT MQTT Bridge 
1. C# generic PnP Module
1. C# Edge PnP Module

If you are more welling to test other languages or sample on constrained device, please have a look at the [previous bugbash instruction](bugbash-09-11.md) (Azure RTOS, Embedde C, C SDK ...)

## BugBash support and feedback

- Use the teams channel [PnP GA BugBash](https://teams.microsoft.com/l/meetup-join/19%3ameeting_MDg5OWQ4ZGItNThmMS00M2Y0LTkxOGEtYTkxNDVmMmRmNjg0%40thread.v2/0?context=%7b%22Tid%22%3a%2272f988bf-86f1-41af-91ab-2d7cd011db47%22%2c%22Oid%22%3a%22a43f0cf6-a7bc-4985-aa0b-37503f8ea92a%22%7d) to ask for help. There will be team members answering questions during the BugBash.

- If you have bugs or feature request, please use this [Bug Template](https://msazure.visualstudio.com/One/_workitems/create/Bug?templateId=588f0905-1848-4c0a-9525-8e0be8cae7f0&ownerId=f0be8f47-90b7-4440-852e-4d5401b257cf). You can query existing bugs in this [PnP BugBash Query](https://msazure.visualstudio.com/One/_queries/query-edit/a922de67-413f-4f4b-9187-29739cc310b8/)

## Getting started, environment and tooling

#### Docs

- The pnp docs are available URL: [IoT Plug and Play documentation]( https://review.docs.microsoft.com/en-us/azure/iot-pnp/overview-iot-plug-and-play?branch=pr-en-us-130249). To provide feedback use this [PR in docs](https://github.com/MicrosoftDocs/azure-docs-pr/pull/130249), your github account must be registered in the MicrosoftDocs org.
- Reviewers can use the PR to leave feedback. You could also leave docs feedback in the teams channel and we'll collate it after the bash.

To access the PR, you must join the MicrosoftDocs organization. Self-service instructions are [here](https://review.docs.microsoft.com/en-us/help/contribute/contribute-get-started-setup-github)


#### IoT Hub

- The required hub version is available for all regions now:
- Create S1 IoTHub in any of these regions to get started.
- FYI, this last version use API version: 2020-09-30

To create the hub using the `az` CLI replace the **hubname** and **region** and run the script below:

```bash
az extension add --name azure-iot
az login
az account set -s IOTPNP_TEST_BY_MAIN
az iot hub create --resource-group BugBash --sku S1 --location <Region> --partition-count 4 --name <alias-hub-name>
```

To create the hub from the portal make sure you select the right subscription, region and resource group.

> Note. Make sure you get the IoT Hub connection string to be able to configure IoT explorer.

#### Digital Twin Definition Language

The DTDL v2 Spec can be found at [https://aka.ms/dtdl](https://aka.ms/dtdl) can be used as a reference for the language. Use the [samples](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL/v2/samples) we've provided to get started.

You can install the VSCode or Visual Studio extension for editing your DTDL files:
- [DTDL VS Code extension](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl)
- [DTDL VS 2019 extension](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16dtdllanguagesupport)

#### PnP related tools

Tools available as internal previews are:

There is no Model repository for GA, this will land later.

- [Azure IoT Explorer]( https://github.com/YingXue/azure-iot-explorer/releases/tag/v0.11.5) (use Azure.IoT.Explorer.preview.0.11.5.msi)
 See more about IoT Explorer in our [documentation](https://review.docs.microsoft.com/en-us/azure/iot-pnp/?branch=pr-en-us-129259/howto-use-iot-explorer#use-azure-iot-explorer)

- [Model Parser] (https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-parser) The parser is available in NuGet.org with the ID: Microsoft.Azure.DigitalTwins.Parser. For the bugbash we have an internal version: GA Release Candidate, 3.12.4,  available via the NuGet feed:

https://dev.azure.com/azure-iot-pnp/azure-iot-pnp-libraries/_packaging?_a=package&feed=azure.iot.pnp.libraries&package=Microsoft.Azure.DigitalTwins.Parser&version=3.12.4&protocolType=NuGet 

**Note**

This package corresponds to the following commit on the next_generation branch of the parser library:

https://github.com/Azure/azure-iot-digitaltwin-libraries/commit/f7c732020b2675bf5448f8d655f0992f58cd0956

## Option 1. Review and run SDK samples

All samples implement the Themorstat and TemperatureController models available in the [DTDL spec repo](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL/v2/samples), and use the same validation flow:

1. Prepare source code for each language.
2. Create a device and provide the connection string.
3. Configure IoT Explorer to find the models and to interact with the device.

Focus on Managed languages:
1. You can test the 2 quickstart with Device SDK  (C# and Java)
1. You can test the 2 new quickstart with Service SDK (C# and Java)
1. You can also test the 2 Tutorial with multi component devices ( C# and Java)
1. Test and use the DTDL Model Parser (C# only, [see this sample](https://docs.microsoft.com/en-us/azure/iot-pnp/concepts-model-parser))

All quickstarts and tutorials are in the [IoT Plug and Play documentation](https://docs.microsoft.com/en-us/azure/iot-pnp/)

**Note** We encourage you to use the device samples with DPS connection.

## Option 2. Exploratory testing

Create simulated device and solution from scratch

- You can follow the instructions in this [deck](https://microsoft.sharepoint.com/:p:/t/PnPCross-TeamCore/Ed9pGHB_AaxIgisKioEHSygB2mADMo2vPSQJZK0lKBnFKQ?e=VZ1ztQ) to create your own simulated device and custom solution from scratch. **The instructions in that deck are specific to C# but can be generalized to the language of your choice** as needed.
