# AnalyzeReceipt

Using the instructions here, you can analyze your own receipts using the Azure Document Intelligence service, PowerApps, and PowerAutomate.

## Prerequisites
- [Azure Document Intelligence](https://azure.microsoft.com/en-us/products/ai-services/ai-document-intelligence) resource 
- [PowerApps](https://www.microsoft.com/tr-tr/power-platform/products/power-apps) 
- [Power Automate](https://www.microsoft.com/en-us/power-platform/products/power-automate/) 
  
**Note** : For Power Automate & Power Apps free trial enough

## Step 1: 
Go to [Make.powerapps.com](http://Make.powerapps.com) and sign in then create blank app.

## Step 2:
Add camera and button from the Insert section.
![image](https://github.com/user-attachments/assets/90d0afa3-d0dc-48a8-ac98-47e30579b68a)
a. Change the false function of the button to **UpdateContext({locPhoto:Camera1.Stream})**.
  ![image](https://github.com/user-attachments/assets/9472f771-6b8f-4723-b400-aadb6ace6086)
  
b. You can change the name of the button to **Take photo** from the right panel.

c. Make the **StreamRate** of **Camera1** **1000** from the advanced section of the right panel.
![image](https://github.com/user-attachments/assets/052af6fa-3f71-493f-938b-5f0fe9a40041)



## Step 3:
Add Second **button** from the **Insert** section.

a. Change the function of second button to **UpdateContext({locDevice:If(Camera1.Camera<Max(Camera1.AvailableDevices,Id),Camera1.Camera+1,0)})**.
![image](https://github.com/user-attachments/assets/7b7e72c6-5a65-447a-8642-e34475019442)

## Step 4:
Change the function of **Camera1** to **locDevice**.

## Step 5:
Add **Image** from the **Insert** section.

![image](https://github.com/user-attachments/assets/1b0aaa06-9074-4c8d-9cbe-a9dafcba2dea)

a. Change the function from **SampleImage** to **locPhoto**. (We are changing it with the variable we defined before.)

## Step 6:
Duplicate the **Take photo** button with copy and paste.
a. Change the name of the copy to **Sent to Cognitive Services**.

## Step 7:
Go to  [Make.powerautomate.com](https://make.powerautomate.com/) and sign in.

## Step 8:
Create **Instant cloud Flow** from the **Create** section.

## Step 9:
Give the Flow a name you want. Select the Trigger **Power Apps** like **AnalyzeReceipt**.

Note: If you are working with New Designer, you may see it with a different name.

a. Add a text input inside. You can fill it as follows.
![image](https://github.com/user-attachments/assets/48542c5a-2797-4e30-b944-410d959526b4)

b. Add a new step, select **Initialize variable**. Fill it as follows. For its value, take the variable you defined in power apps from **dynamic content**.
![image](https://github.com/user-attachments/assets/d4c5e72d-cf36-42c8-a645-0d2d016e7730)

c. Add a new step, select **Initialize variable**. Fill it as follows. We will use this to remove the extra base64 part of the image we get in the next step.
![image](https://github.com/user-attachments/assets/ff928cbb-b128-4ae9-b845-f7abd3a16b88)

d. Add a new step, select **Initialize variable**. Fill it as follows. For its value, add **last(split(variables('varImage'),variables('varSplittextValue')))** to the function section in the **expression** section.
![image](https://github.com/user-attachments/assets/eb9ffa9f-8eca-4683-8459-5e35417652ad)

e. Add a new step, select **Compose** component. Fill the Input section as follows. This text in JSON format is used to process an image, especially with Base64 encoding.
![image](https://github.com/user-attachments/assets/7fe8e96e-43f4-47ff-81de-38d848f6e634)


f. Add a new step, select **Analyze Receipt**. After entering the key and endpoints, add the output of the previous **Compose** component to the Content section.
![image](https://github.com/user-attachments/assets/bc3c3155-f4dc-4e7e-a88e-d3972151df63)

Note: This requires an Azure Document Intelligence source. If you don’t have one, you can create one from Azure AI Document Intelligence | Microsoft Azure.

g. Save the Flow and return to the Power Apps screen.

## Step 10:
Click on the **Power Automate** icon on the left panel in Power Apps. Here, select your own flow from the In your app section.
![image](https://github.com/user-attachments/assets/bb1ac0a4-2b96-413a-b8eb-cccc49861daf)


## Step 11:
Change the function of the Send to cognitive services button to the following. This way, we can call the flow.
Note: You should write your own flow name in the **Analyzedoc** section.

**UpdateContext({locFlowResponse:Analyzedoc.Run(locPhoto)});**
**UpdateContext({locCaptions: Table(ParseJSON(locFlowResponse.captions))});**
**UpdateContext({locTagnames: Table(ParseJSON(locFlowResponse.'tag-names'))});**

## Step 12:
After saving everything, you can start testing by taking photos from the camera in preview mode with f5.

## Step 13: 
You can check if the flow is working correctly from Power Automate.
![image](https://github.com/user-attachments/assets/4da2b281-0ff8-4801-b5be-2a20b936d9f1)

