# AI dev kit Hands On Lab 2019

## Setup your AI dev kit device

### Hardware setup
Make sure that your AI vision dev kit is plugged to power via USB and connected to an external screen via HDMI. Also please verify that your device 3 LEDs are displayed a solid green light.

### Get an Azure subscription
Use this account to login to Azure, see and deploy Azure resources:
- user: *Provided by the lab at your station*
- password: *Provided by the lab at your station*
- subscription: *Provided by the lab at your station*
- resource group: *Provided by the lab at your station*

### Set up Azure IoT resources
Your device has already been set up as an Azure IoT Edge device, connected to an IoT Hub. This will enable you to remotely manage it. Let's first verify that your setup looks correct:

- Let's connect to Azure via the portal to see your Edge device in your IoT Hub:
    - In a browser, login to [http://portal.azure.com](http://portal.azure.com)
    - Click on `All resources` from the left menu and search for your IoT Hub under your resource group
    - Click on your IoT Hub and then on `IoT Edge` section from the left menu
    - Verify that you can see your `visionkit` device is listed

### Deploy a sample AI model
We will now deploy an sample AI model to the camera. For that, we will use the AI Dev Kit get started module from the IoT Edge marketplace.

- Go to [this link](https://ms.portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=AIDevKitPreview#blade/Microsoft_Azure_Marketplace/GalleryFeaturedMenuItemBlade/selectedMenuItemId/home/searchQuery/AI%20vision%20dev%20kit/resetMenuId/) and click on the  `AI Vision Dev Kit Get Started Module (preview)` and  `Create`.

- Choose your subscription, your IoT Hub, find your device named `visionkit` and click on `Next`

- We'll use a preview version of IoT Edge for this device, click on `Configure advanced Edge runtime settings` replace the edgeHub URI from `mcr.microsoft.com/azureiotedge-hub:1.0` to `mcr.microsoft.com/azureiotedge-hub:1.0.7-rc2` and the edgeAgent URI from `mcr.microsoft.com/azureiotedge-agent:1.0` to `mcr.microsoft.com/azureiotedge-agent:1.0.7-rc2` and finally add the environment variable `OptimizeForPerformance` set to `False` for the edgeHub. 

- Confirm the deployment by clicking on `Next` twice then `Submit`

After a few minutes (time to download the module), you should now see objects being detected by the camera when looking at its HDMI output!

## Build your own AI model
We'll build our own AI model to detect when someone is wearing an hard hat. You have an hard hat on your desk which well be used to validate your model built with [Custom Vision](https://www.customvision.ai/).

### Setup up a new Custom Vision project
- Login to the Azure Custom Vision Service (Preview) at [https://www.customvision.ai](https://www.customvision.ai).

- Create a new project, use these recommended settings:
    - Give it a name like `Simulated HardHat Detector`
    - Project Type - [Classification]
    - Use the existing resource group
    - Classification Type - [Multiclass (Single tag per image)]
    - Domain - [General(compact)]
    - Export Capabilites - Vision AI Dev Kit

### Upload and tag your training data
Some training images have already been collected for you for the hard hat use case.

Download them and upload them to your Custom Vision project:
- Downlaod the .zip file at this location: [https://1drv.ms/u/s!AkzLzaBpSgoMo9dhS3q2jLb85H2-Fg?e=Rht04X](https://1drv.ms/u/s!AkzLzaBpSgoMo9dhS3q2jLb85H2-Fg?e=Rht04X)
- Uncompress it
- Upload images to custom vision per tag (HardHat/NoHardHat/Negative) and tag them appropriately them during upload. Upload all pictures names similarly (like HardHat) at the same time.

### Train and export your custom model
To train it with your new training data, go to your Custom Vision project and click on `Train`.

To export it, select tab `Performances` and `Export` button. Choose the `Vision AI Dev Kit` option to download your new custom model and finally uncompress them.

### Deploy your custom model to your device
To deploy your custom model, we will first store your model in a publicly accessible location and then update the configuration of the Get Started module to use this model instead of the default one. We will use a cloud blob store to store the model but a public OneDrive link would work just as well.

#### Uploading new custom model files
We'll start by creating a new storage account and then upload your model to it.
- Login to the [Azure portal ](http://portal.azure.com)
- Search for `Storage` and select the `Storage accounts` service
- Use your existing subscription and resource group
- Give it a unique name, upper case characters are not allowed
- Select the West-US 2 region
- Click on `Review+Create`
- Wait until until provisioning is complete and navigate to yoru new storage account
- From the `overview` tab, click on `Blobs` service
- `Add a new container`, give it a name like `model001` and make sure to select `Container (anonymous read access for containers and blobs)` for the `Public access level`
- Click on the container just created, Click on the `Upload` button and select your files from the custom vision service and `Upload`
- Copy the three URLs of these files by clicking on each of them and copying their URL to the clipboard

#### Updating the configuration of the Get Started module to use your new custom model

- Login to the [Azure portal ](http://portal.azure.com) and go to your ioT Hub resource
- Click on `IoT Edge` tab and then on your camera device named `visionkit`
- Click on the `AIVisionDevKitGetStartedModule` module name and click on `Module Identity Twin`
- Update the three desired properties (model, label, vam config) to map to your new URLs and hit `Save`

After a few minutes, your device should now be running your custom model!

### Test your new model
- Put your hard hat on and smile at the camera !
- Verify that the camera picks it up and correctly classify you as wearing the hard hat

## Going further
As a next step, you could re-use the same training images but build an object detection model instead of an image classifier. For that, please use this [environment of custom vision](https://iris-demo1.azurewebsites.net/projects) and use its labeling tool.

To learn more about this AI Dev Kit, visit https://azure.github.io/Vision-AI-DevKit-Pages/docs/Get_Started/


## Clean up
Before leaving, please:


- delete your download folder on your laptop

- delete your custom vision project 

- delete your storage account

- log out from all websites


Thank you!


