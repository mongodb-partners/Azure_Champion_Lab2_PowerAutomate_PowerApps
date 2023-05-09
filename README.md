# Lab 2 - MongoDB Atlas Integration with Power Platform (Power Apps and Power Automate)
## Introduction:
Digitalization is putting pressure on organizations to develop apps, features and workflows faster. And the fact is that there is a skill shortage to meet these demands. Thus, citizen developers are using Low-code , No Code solutions to meet the TTM demands. [Power Platform](https://powerplatform.microsoft.com/en-us/) offers a suite of tools that can enable developers to build rich applications and workflows without having to master any coding skills. To build these distributed applications and workflows faster, you need a robust and flexible Data platform that provides the necessary services and tools at the performance and scale users demand. [MongoDB Atlas](https://www.mongodb.com/atlas) is the industry’s first unified developer data platform that allows you to accelerate and simplify how you build with data using the Power Platform for modern applications.

## MongoDB Atlas and Power Apps / Power Automate integration:
In this Lab, we will create a single page application using Power Apps to capture some details from the user and save it in MongoDB. 
We will also create a Power Automate flow that scans a document uploaded to a blob storage using AI and saves the extracted details into MongoDB. 

### Power Apps 
MongoDB Atlas is easily integrated with Power Apps using a certified custom connector. You can add the certified MongoDB custom connector by using the “Import from GitHub'' option and the “master” branch of the “Production” Github repo. Select and Test from the available MongoDB Data APIs that you would use in your Power Apps solution.

img1

### Power Automate
MongoDB Atlas is easily integrated with Power Automate using a Premium connector. You can add the Premium MongoDB custom connector by using the Connections -> New Connection option and searching for MongoDB connector. Once selected, it will ask you for the Base url and the API Key to be used while invoking the APIs. You can use the MongoDB connection from Power automate by selecting the operation and passing the required parameters from the UI itself.

img2

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



      
      
      
