# Lightning Components BRK

 Contact: **Greg Rewis, Director, Product Management, Lightning, [grewis@salesforce.com](mailto:grewis@salesforce.com), +1-602-451-4858**


## **Change Log**

|8/20/2018	|Greg Rewis	|[grewis@salesforce.com](mailto:grewis@salesforce.com)	|Updated for Winter '19 
on 9/18/2018	|
|---	|---	|---	|---	|
|3/6/2018	|Greg Rewis	|[grewis@salesforce.com](mailto:grewis@salesforce.com)	|Updated for Spring '18	|
|11/3/2017	|Greg Rewis	|[grewis@salesforce.com](mailto:grewis@salesforce.com)	|Notes	|

## Objectives

* Introduce Lightning Components
* Understand <lightning> namespaced components
* Create a Lightning Component

## Preparation

For this demo, you can either use the Developer Console, or VS Code with SFDX. All the necessary software should already be installed, but if you are using your own laptop, you will need to have SFDX (Salesforce CLI) installed and VS Code. If you have your own Winter'19 devhub, please use it.

* Create a folder with your name on the demo machine where you will be  downloading the source for the demo org.
* Using the CLI, navigate into the folder you created and execute the following command:
    * `git clone https://github.com/garazi/Dreamhouse_BRK.git `
* Using the CLI, navigate into the newly created folder “Dreamhouse_BRK”.
* Log into the devhub with the following:
    * `sfdx force:auth:web:login -a dfDevHub`
    * Username: [demo@devhub2019.com](mailto:demo@devhub2019.com)
    * Password: sfdc1234
        * Close the browser window once you have authenticated
* Run the following command, where YOUR_ORG_NAME is whatever you want:
    * `sfdx force:org:create -f config/project-scratch-def.json -s -a YOUR_ORG_NAME -d 30 -w 10`
* Once the scratch org has been created, execute the following commands:
    * `sfdx force:org:open`
    * `sfdx force:source:pull`
    * `sfdx force:source:push`
    * `sfdx force:user:permset:assign -n All_Access`
* In the org, open the App Launcher and choose **Dreamhouse Lightning**.
* Choose the **Data Import** tab, and click the **Initialize Sample Data** button.
* Click on the **Properties** tab to confirm that you have a list of properties.
* Open the following in another browser tab to allow you to copy/paste code: https://github.com/garazi/LightningComponentBRK

## Devices

None


## Monitor Setup

 Nothing of note


## Demo Script


 Actions and scripts for the Demo. 


* At the completion of each demo:
    * In App Builder, remove the relatedProperties component that you create from the page.
    * In the VS Code, delete the component.
    * Push the changes back up to the org.

### Talk track

|**DO**	|**SAY**	|
|---	|---	|
|Click on the Properties tab and choose a property.	|The Dreamhouse app is an app for a ficticious real estate company to help them manage their inventory of houses, brokers, prospects, etc. The default Property Record page has been modified with several Lightning Components using Lightning App Builder.	|
|Point out the Lightning Components in the righthand column.	|Each of the items in the righthand column is a Lightning Component. In other words, they are each a piece of standalone functionality. But just because the individual components are independant of one another on the page, they can still communicate with each other, as well as pull data from the record itself.	|
|Point to the **Days on the Market** component at the top of the column.	|For example, this component is calculating the number of days on the market using the **Date Listed** field in the record.	|
|Click on the** Map** tab.	|The Property Map is picking up the address from the record and using it to place a pin on the map in the proper location. This is using a brand new component in Winter '19, called lightning:map. You can pass a single marker, or even a list of addresses that you want plotted.	|
|Click back on the Details tab, then point to the **Brokers Card** component in the righthand column.	|In addition to grabbing fields from a record when the page loads, Lightning Components can also "listen" for events to occur. For example, this component has looked up details about this property's broker using the ID of the broker from the property record.	|
|In the Details pane, click to change the broker for the property. Choose any other broker, and click **Save**.	|Notice if we assign a different broker to the property and save, the component updates with the details for the new broker. This is all through the magic of Lightning Data Service, which allows a component to create, read, update and delete a single record without writing server-side Apex.	|
|Point to the **Neighborhood Explorer** component.	|Lightning Components are not limited to talking to Salesforce. In the Neighborhood Explorer component, we are again capturing the address for the Property Record like we did in the Property Map component. But this time, we are passing it off to a 3rd party app running on Heroku which is proxying the Yelp! API to gather information about schools, shopping and restaurants around this property.	
|Click on **Setup > Edit Page**.	|In addition, as a developer, I can expose properties of my component to App Builder. This way, when a person is adding the component to a page, they can specify their own values. These are know as Design Parameters.	|
|Click to select the **Neighborhood Explorer** component in the righthand column.	|For example, this component uses Design Parameters to expose the title for the three tabs of the component, as well as specify the height of the component on the page.	|
|Click the **Back** button in the upper righthand corner to return to the record detail page.	|	|
|Click on the **Mortgage Calculator** in the Utility Bar.	|Lightning Components can be used almost everywhere in the Lightning Experience. In this case, we're using a Lightning Component in the Utility Bar so that it's available throughout the Dreamhouse app. Lightning Components can also be used as Quick Actions or as Standard Button Overrides.
|	|Now that we've looked at several components in action, let's talk about how you actually build a Lightning Component.
|	|We're going to create a component which lists other properties that are similar in price to the property that we are currently looking at.	|
|Switch to **VS Code**. NOTE: you should already have your project folder open in VS Code.	|We're going to use VS Code and Salesforce DX (SFDX) to help us build the component. For questions about SFDX, I'd encourage you to stop by their booth here in the Developer Forest.	|
|Choose **Command + Shift + P**to open VS Code's command palette, then type **SFDX**.	|The SFDX team has created plugins for VS Code that give us the ability to do a lot of things, for example, push and pull source code to/from our scratch org, as well as create new Components.	|
|Choose **SFDX Create Lightning Component**, and give it a name of **RelatedProperties**.	|Let's give this component a name of "RelatedProperties" and save it in the default location for this project.	|
|Point out the newly created component files.	|A Lightning Component consists of different files containing HTML CSS and JavaScript which all work together to give us the final rendered component.	|
|Copy the code from **Step 1** on the github page and replace the default contents of the component with the copied code, then choose **Save**.	|In the interest of time, I'm just going to copy a bit of code into the component to get us started. 
|	|The Lightning Framework uses several key concepts that we can see here.	|
|Point out the **implements** attribute.	|First, a Lightning Component uses the concept of interfaces, which provide additional "super powers" for the component, based on where it is going to be used. Our's is using an interface called "flexipage:availableForRecordHome" which indicates it can be used on a Record Detail page. Additionally, the force:hasRecordId interface provides the ID for the current record to the component.	|
|Highlight the three **<aura:attribute>** tags.	|Next, is the concept of **aura:attributes**. These are simply value providers. In other words, they are a place where we store data that can be displayed or manipulated by the component. The force:hasRecordId interface will put the current record's ID into the recordId attribute. We'll use the other two attributes to store other data.	|
|Highlight the **<force:recordData>** tag.	|I mentioned Lightning Data Services earlier. The <force:recordData> tag is how we access Lightning Data Service within a Lightning Component. When the component loads, it uses the force:hasRecordId interface to retrieve the Id of the current record page and assigns it to the recordId attribute of force:recordData. 
|	|This, in turn, tells Lightning Data Service to retrieve the rest of the fields for the designated record, based upon the layoutType - in this case the full record. Once the fields have been retrieved, they are stored in the attribute defined by the targetRecord attribute; here that is the attribute with the name "property".	|
|Highlight the **<lightning:card>** tag.	|In order to help us build components faster, the Lightning Framework uses UI elements called Base Lightning Components. These all start with **lightning:** as the first part of the tag name. These Base Lightning Components use the markup from the Salesforce Lightning Design System, or SLDS. The lightning:card creates a white box with an icon, which also comes from SLDS, and title.	|
|Open a new tab in the browser and navigate to https://developer.salesforce.com/docs/component-library	|So, the question is, where do I find these components, and learn how to use them? The answer lies in the Component Library that we can find on the Saleforce Developer Site under the Resources section.	|
|Locate lightning:card and select it in the left-hand navigation.	|We can see all of the components available to use, for example, the lightning:card that we were just looking at. We can see the code needed, as well as documentation and the spec list for the component. In addition, if you login into your org, the component library will not only show you the list of standard lightning components, but also custom components in your own org.	|
|Choose **Command + Shift + P** and then choose **SFDX Push Source to Default Scratch Org**	|Let's go ahead and push the new component to our scratch org using SFDX.	|
|Switch back to the Property detail page and choose **Setup > Edit Page**.	|In order to see our component, we need to put it on a page, and to do that, we will use Lightning App Builder. 	|
|Locate the **RelatedProperties** component in the list of Custom components.	|Lightning App Builder gives us access to Standard Components, as well as the components which we have created, like some of the other components that we saw earlier. 	|
|Drag the component into the righthand column of the page layout.	|Let's place the component in the righthand column of the page.	|
|Click **Save**, and then click the Back link after the page is saved. 
|	|Point out the title of the RelatedProperties component.	|We'll save the page, and we can see that it is, in fact, appearing on the page. 	|
|**REPLACE** the component's code with the code from **Step 2** on the github pag, and highlight **controller="MyPropertyController"** in the aura:component tag.	|But, of course, we need some data. In the Lightning Component world, when we want to talk to Salesforce, we do it similar to Visualforce. Here, we have specified that we will be using an Apex Controller with the name "MyPropertyController". 	|
|Highlight the **recordUpdated="{! c.doSearch}" **attribute of the force:recordData tag.	|We are going to use Lightning Data Service to trigger the execution of a JavaScript function called "doSearch", which in turn, will retrieve properties with a price that is similar to the property we are viewing. The "c." indicates that this function is located in the client-side controller — in other words, in this component.<p><p>Lightning Data Service will fire whatever function is defined in the recordUpdated attribute any time a field in the watched record changes.	|
|Switch to the **RelatedPropertiesController.js** file in VS Code, and replace the boilerplate code with the code from **Step 3** in the github page.	|For the doSearch function, you can see that we are passing in three parameters: component, event, helper. The component parameter allows the JavaScript Controller to access the markup of the component itself.<p><p>First we are declaring a variable "action" that indentifies which method we will be calling in our Apex Controller, and establishing parameters to pass to the method. These parameters are all being retrieved from attributes in the component.<p><p>Finally, when the data is retrieved, a callback function is executed and sets the value of the similarProperties attribute in the component to the retrieved records.	|
|Switch back to the **relatedProperties.cmp** file, and **REPLACE** "CONTENT GOES HERE" with the contents of **Step 4** in the github page.	|Now we simply need to display the records that were retrieved by the Apex Controller.<p><p>Using an aura:iteration, we can loop over all of the records that were retrieved and stored in the similarProperties attribute, displaying the Name of the record in a list.	|
|Save the files (both the cmp and controller files), and then use **Command + Shift + P**to select **SFDX Push Source to Default Scratch Org.**
Reload the Property page to see the component populate with properties.	|Let's save and upload our changes.<p><p>And after reloading, you can see that we are seeing property names being returned by our call to Salesforce.
|**REPLACE** the contents of **relatedProperties.cmp** with the contents of **Step 5** in the github page, and **Save**.	
|Highlight the code you just pasted.	|Let's make each property record look a bit better.<p><p>We want to add a thumbnail image, a larger title for each result and a nice 2 column layout for the details of each property.<p><p>You'll notice that we are using some HTML markup to layout the results. The markup was simply copied from the SLDS site to get the initial HTML structure and CSS classes to give us the look we are after.<p><p>You can see that the image and title are being retrieved each time the iteration happens.	|
|Highlight the `<a>` tag surrounding the Name of the property. 	|The Name of the property needs to be a link so that our agents can quickly move to that record. Therefore, we've added an onclick method to it named "NavToRecord", as well as a data-attribute to store the ID of the individual record.	|
|Switch to the relatedPropertiesController.js file, and replace the contents with the contents of **Step 6** in the github page, and **Save**.	|The navToRecord function uses a built-in function to navigate to the correct record by retrieving the correct recordId from the data attribute for the individual record.	|
|Highlight the **lightning:recordForm**	|In the Summer'18 release, we introduced the lightning:recordForm component. It gives developers the ability to create an editable form for a record by simply providing a recordId and ObjectApiName, along with the fields you want to display (much like force:recordData).<p><p>We've added an aura:attribute with the name "fields", listing the fields we are interested in, which in this case are "Beds, Baths,Price,Status,Broker".	|
|Save the file(s) and upload to the org **Command + Shift + P to select SFDX Push Source to Default Scratch Org**. 
|Reload the Property page to see the component populate with a different layout.	|Now the component is looking much better.	|
|Click on a record name in the component to navigate to that particular record.	|You can see that we can click to navigate from property to property.	|
|Click any field to switch into edit mode.	|Using lightning:recordForm, we are able to switch between a View form and an Edit form very easily. And, as a developer, I don't have to write any additional code.	|
|Click to remove the Broker for a property, and choose a different Broker, then click **Save**.	|Even lookup fields are automatically created for me, and we can easily assign a record to a different broker.	|
|	|So there you have it. We've seen how Lightning Components are the building blocks of the Lightning Experience, and how developers can utilize Base Lightning Components to quickly build their own custom components.	|

If you have additional time and/or questions, feel free to open any of the components in the org to show their code.

## Break Glass in Case of Emergency

In case of emergency, there is a finished version of the component named “SimilarProperties” that you can drag onto the page, and/or use to walk through the code.

## Call to Action

 What would you like the developer to do after the demo?


* **REQUIRED**: Earn the Lightning Components badge: https://trailhead.salesforce.com/modules/lex_dev_lc_basics
* Download the Lightning Components Developer guide: https://developer.salesforce.com/docs/atlas.en-us.208.0.lightning.meta/lightning/intro_framework.htm


