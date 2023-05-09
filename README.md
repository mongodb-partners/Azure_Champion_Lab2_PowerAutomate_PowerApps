# Lab 2 - MongoDB Atlas Integration with Power Platform (Power Apps and Power Automate)
## Introduction:
Digitalization is putting pressure on organizations to develop apps, features and workflows faster. And the fact is that there is a skill shortage to meet these demands. Thus, citizen developers are using Low-code , No Code solutions to meet the TTM demands. [Power Platform](https://powerplatform.microsoft.com/en-us/) offers a suite of tools that can enable developers to build rich applications and workflows without having to master any coding skills. To build these distributed applications and workflows faster, you need a robust and flexible Data platform that provides the necessary services and tools at the performance and scale users demand. [MongoDB Atlas](https://www.mongodb.com/atlas) is the industry’s first unified developer data platform that allows you to accelerate and simplify how you build with data using the Power Platform for modern applications.

## MongoDB Atlas and Power Apps / Power Automate integration:
In this Lab, we will create a single page application using Power Apps to capture some details from the user and save it in MongoDB. 
We will also create a Power Automate flow that scans a document uploaded to a blob storage using AI and saves the extracted details into MongoDB. 

### Power Apps 
MongoDB Atlas is easily integrated with Power Apps using a certified custom connector. You can add the certified MongoDB custom connector by using the “Import from GitHub'' option and the “master” branch of the “Production” Github repo. Select and Test from the available MongoDB Data APIs that you would use in your Power Apps solution.

![Picture 1](https://user-images.githubusercontent.com/104025201/236997363-2b3c8718-06ed-4dc6-84bf-86f41256adeb.png)

### Power Automate
MongoDB Atlas is easily integrated with Power Automate using a Premium connector. You can add the Premium MongoDB custom connector by using the Connections -> New Connection option and searching for MongoDB connector. Once selected, it will ask you for the Base url and the API Key to be used while invoking the APIs. You can use the MongoDB connection from Power automate by selecting the operation and passing the required parameters from the UI itself.

![Picture 2](https://user-images.githubusercontent.com/104025201/236997384-ffc65ba8-93a8-48e5-886b-d9c870ccf78b.png)

### Objectives:
In this Lab you will explore : -  
  1. Create a Power Automate workflow   
      a. Scans a document uploaded in blob storage using AI  
      b. AI extracted information is inserted as a document to MongoDB collection  
      c. Sends an email notification with the extracted information  
 
#### OPTIONAL: 
  2. Create a single page Power Apps application  
      a. Takes user inputs    
      b. Inserts the input as a document in MongoDB collection 

### Prerequisites:

  1. Spin up a MongoDB Atlas cluster 
 
      a. Configure Atlas Environment  
      Register for a new Atlas Account [here](https://www.mongodb.com/docs/atlas/tutorial/create-atlas-account/#register-a-new-service-account). Follow steps from 1 and 2 (Create an Atlas account, Deploy a Free cluster) to set up the Atlas environment. In Step 2 (Deploy a Free cluster), Give the cluster name as **“Sandbox”.**  
      b. Enable the Data API in Atlas [here](https://www.mongodb.com/docs/atlas/api/data-api/#1.-enable-the-data-api).  
     Under Data API Access dropdown, choose **Read And Write** option.   
     Save the **URL Endpoint** which will be used at a later stage to configure **MongoDBDataAPI.**  
      c. Create a Data API Key [here](https://www.mongodb.com/docs/atlas/api/data-api/#2.-create-a-data-api-key)   
  **Note: Save it as this is the only time you can retrieve the full private key.** 
  2. Create a Blob Storage  
     a. Login to your Azure account. If you don't have an Azure account, subscribe to the free account using the link [here](https://azure.microsoft.com/en-in/free/).  
     b. Create a blob storage account. You can follow the steps provided in [Microsoft Link](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal).   
     c. Create 1 container called **"documents"**. Select **Public access level** as **“Container (anonymous read access for containers and blobs)”**.  
     <img width="302" alt="Picture 3" src="https://user-images.githubusercontent.com/104025201/237000734-50341393-ccf4-406e-8233-953acd2636fe.png">

     d. Note the **Storage account name** and also Save one of the **Access keys** from the **“Access keys”** option in the storage account menu on the left side.
     ![Picture 4](https://user-images.githubusercontent.com/104025201/237000775-c1d3fd02-3812-4951-bfe8-17fed9b5e41e.png)


### Integration Steps:
#### A. Create a Power Automate workflow which scans the document uploaded and emails the results

##### Create the Power Automate Flow
  1. SignIn to Power Apps/Power Automate Environment
For the Labs, we will provide an exclusive environment for each user .
Open a new tab in Incognito or Private browsing, create a new browser profile using the credentials provided by the instructor, Open - “www.powerapps.com” and SignIn to the logged in account. *Microsoft default environments have many policies that block access and will not work for these Labs.* 

Or you can also create a free Power Apps account (Developer Plan) using a personal email id. The [landing page for the plan](https://powerapps.microsoft.com/en-us/developerplan/) and the page with details on [how to Sign up](https://learn.microsoft.com/en-us/power-apps/maker/developer-plan) can be of help to get started. For a licensed version, refer to the link [here](https://powerapps.microsoft.com/en-us/pricing/).  

  2. Create a New Automated Cloud Flow. Select **My Flows -. +New flow -> Automated Cloud flow**. The flow consists of 5 actions.  

<img width="356" alt="Picture 5" src="https://user-images.githubusercontent.com/104025201/236997731-72e11826-811f-4a40-8b18-59ac381018a8.png">

**a. Step 1 - Set Up Trigger**   
  * Give a name and choose flow’s trigger as **“When a blob is added or modified”**. (Hint : Just type blob in the search). Click **“Create”** to create the Flow.	 

 ![Picture 6](https://user-images.githubusercontent.com/104025201/236997763-78b7c94a-d8c1-4ea1-ba0b-9fead51cabfe.png)
                                                                                                                                             
  * Select **Authentication type** as **“Access key”**. Give the name of the storage account created in the first step and one of the two access keys and click **“Create”**.  
                                                                                                                                             
<img width="331" alt="Picture 7" src="https://user-images.githubusercontent.com/104025201/236997826-5cab15fa-6c1b-4bb6-8e4e-c1df4f3eed99.png">

  * After the connection is successful, it will ask for the storage account and container. Select as the storage account from the connection created just now and select the container as **documents** by selecting the folder icon.  

<img width="452" alt="Picture 8" src="https://user-images.githubusercontent.com/104025201/236997856-6603d13a-a79d-4018-b81a-3dc09815df49.png">

**b. Step 2 - Get uploaded blob’s content**   
  * Add a new step by selecting the **“+ New step”** button. This time select the action as **“Get blob content”**

![Picture 9](https://user-images.githubusercontent.com/104025201/236997872-0af49b99-aa37-4907-87c0-8d7784249894.png)

  * Select the added storage account connection by selecting “Use connection settings(\<name of the storage account\>)” against the Storage account name/blob endpoint. For specifying the **blob**, type in  **“/documents/”** and then select **DisplayName** from **“Dynamic content”** to indicate that it applies to all blobs in the **documents** container.  

![Picture 10](https://user-images.githubusercontent.com/104025201/236997896-6e52b6f2-672b-4c91-a21b-0ab260aa2a54.png)

  * Finally, it will look as below:  

<img width="433" alt="Picture 11" src="https://user-images.githubusercontent.com/104025201/236997910-fcdcf657-e1ba-4b2e-80cb-86abad4c35e8.png">

**c. Step 3 - Use AI Builder action to extract information from the blob**   
  * Add a new step, this time selecting the **AI Builder** and the action as **“Extract Information from identity documents”** 

<img width="384" alt="Picture 12" src="https://user-images.githubusercontent.com/104025201/236997933-006054db-ca2b-49c4-9503-fc98ccee7a57.png">

  When using AI Builder for the first time, a pop up will appear asking to start a trial to use the AI Builder. Select **“Start trial”** to begin using AI Builder.  

  Select the identity document as the **File content** of the blob , by selecting **File content** from the **"Dynamic content list”**.

![Picture 13](https://user-images.githubusercontent.com/104025201/236997949-03a5c4fe-6780-4391-9e54-f29e824b704d.png)

**d. Step 4 - Insert the extracted details into MongoDB collection**   
  * Add a new step and this time select MongoDB operation and action as Insert document.  

 <img width="428" alt="Picture 14" src="https://user-images.githubusercontent.com/104025201/236997976-3709915d-29b7-4226-b11f-ac066ba5e5fa.png">

  Give the MongoDB Atlas connection a name and provide the base url and the API key created in the Prerequisites. Click **“Create”** to create the MongoDB Atlas connection.

<img width="452" alt="Picture 15" src="https://user-images.githubusercontent.com/104025201/236997994-9f5eac18-8748-449b-aae9-abcc929813a6.png">

  Once the connection is created, provide values as below:
  MongoDB Cluster Name:     “Sandbox”
  MongoDB Database Name: “XYZBank”
  MongoDB Collection Name: “onboarding”
  Document to be inserted:	
```
  {
  "firstname": <From previous AI step - First name>,
  "lastname": <From previous AI step - Last name>,
  "documentno": <From previous AI step- Identity document number>,
  "country": <From previous AI step -Country/ Region>,
  "dateofbirth": <From previous AI step - Date of birth>
}
```
*The values for each field should be the output of the AI Builder step and thus should be selected from the dynamic content. Copy above json and replace the angle brackets and the content in it by selecting appropriate dynamic content.
Follow the steps shown below to complete the json document.*

![Picture 16](https://user-images.githubusercontent.com/104025201/236998039-7ea3ec94-a388-45d3-b129-37483d1564b8.png)

After entering the details the form should look as below:

<img width="425" alt="Picture 17" src="https://user-images.githubusercontent.com/104025201/236998054-764f7873-c60c-4a12-8d03-5b920b928f07.png">


**e. Step 4 - Insert the extracted details into MongoDB collection** 
  * Add a new step to send an email notification. If you have a **Outlook365 account**, you can use the below steps, else use **work Gmail account** or create an **Outlook.com** account and use that to send a mail. *Note : this will not work with personal gmail accounts.*

  * Select **Office 365 Outlook** operation and **“Send an email (v2)"** action.

<img width="452" alt="Picture 18" src="https://user-images.githubusercontent.com/104025201/236998101-9ad4f20e-3fdb-4c97-8f50-a7fa70f85108.png">

  * Add below information to the **Office 365 Outlook  -> Send an email (V2)** form
    * **To**: *Give a valid email address which you have access to receive mails*
    * **Subject**: *“Info from the Uploaded document”*
    * **Body**:
```
Hi,
    Your uploaded document has been analysed and the details are as below:

  First Name: 		<From previous AI step - First name>,

  Last Name: 		<From previous AI step - Last name>,

  Document Number: 	<From previous AI step- Identity document number>,

  Country/Region: 	<From previous AI step -Country/ Region>,

  Date of birth: 		<From previous AI step - Date of birth>

Please revert if the details are inaccurate, else we have updated our records with your information.
Have a nice day!

Regards,
Document Analyser
```
  The form should be as below after all inputs are provided.
  
<img width="452" alt="Picture 19" src="https://user-images.githubusercontent.com/104025201/236998138-bc09fd46-35bb-4984-ac85-d54ba9b7573f.png">

Save the flow using the Save option at the bottom or the top right corner. Once saved, you will see a message as below on the top left corner.


##### Test the Power Automate Flow

1. Download the specimen passport from wikipedia, link [here](https://drive.google.com/file/d/1nsvZS8xpOTKS0Neml9nC7oEVQ4AinrpW/view?usp=share_link).

![Picture 20](https://user-images.githubusercontent.com/104025201/236998226-ccdf49dc-8f75-4e6d-9f3d-a35f96426bde.png)

2. Test the flow by clicking the **“Test”** option on the top right corner of the Power Automate flow.
  * Select option Manually and click the **“Test”** button.

<img width="258" alt="Picture 21" src="https://user-images.githubusercontent.com/104025201/236998262-03f5bed7-0ce8-48c8-8646-79fbdff2ad6a.png">

You will see below, the message hinting to start the trigger action.

<img width="262" alt="Picture 22" src="https://user-images.githubusercontent.com/104025201/236998275-6b166193-35da-4d2a-a9ea-ed8c33fca8f8.png">

  * Go to the blob storage account and Upload the downloaded Passport file. Select the “Upload” option in the documents container.

![Picture 23](https://user-images.githubusercontent.com/104025201/236998295-abd5176f-d7f7-4e85-a0ae-5663a6043a8e.png)

Select **“browse files”** and select the downloaded **specimen passport** file and Upload it to the blob storage documents container.

  * Back to Power Automate, You will see that your flow was triggered and finally a message **“Your flow has run successfully”** appears. All stages executed will have  a green tick.

<img width="452" alt="Picture 24" src="https://user-images.githubusercontent.com/104025201/236998315-828bfffe-19a2-4d76-acfe-26c7044713e8.png">

  * Verify that the AI extracted details were inserted into the MongoDB collection **XYZBank.onboarding**

<img width="412" alt="Picture 25" src="https://user-images.githubusercontent.com/104025201/236998338-6defda55-50ab-476e-9aec-3303e9f4fd6c.png">

  * Verify that the email was received in the mailbox of the email account which was given in the **“To”** field of the email action form.

<img width="452" alt="Picture 26" src="https://user-images.githubusercontent.com/104025201/236998351-5107d33a-bbbb-4707-98ed-b57631448d08.png">


**Congratulations ! You have successfully created a Power automate flow that scans the document uploaded using AI and sends a mail and inserts a record with the extracted details to MongoDB Atlas.**







   



  
