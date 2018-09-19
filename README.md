# Lightning Component BRK

If you need another scratch org, click this button:  [![Deploy](https://deploy-to-sfdx.com/dist/assets/images/DeployToSFDX.svg)](https://sfdx-deployer-app.herokuapp.com/launch?template=https://github.com/garazi/Dreamhouse_BRK)

The following code snippets are for use with the Lightning Components BRK.

### Step 1

  ```xml
	<aura:component implements="flexipage:availableForRecordHome,force:hasRecordId" access="global">
	    <aura:attribute name="recordId" type="Id" />
	    <aura:attribute name="similarProperties" type="Object[]" />
	    <aura:attribute name="property" type="Property__c" />
	    <aura:attribute name="searchCriteria" type="String" default="Price" />
	    <aura:attribute name="priceRange" type="String" default="100000" />
	    
	    <force:recordData 
	        aura:id="propertyService" 
	        recordId="{!v.recordId}" 
	        targetFields="{!v.property}" 
	        layoutType="FULL" />
	
		<lightning:card iconName="custom:custom85" title="{! 'Similar Properties by ' + v.searchCriteria}" class="slds-is-relative">
        <div class="slds-p-horizontal_medium">
            CONTENT GOES HERE
        </div>
		</lightning:card>
	
	</aura:component>
  ```
  
### Step 2

  ```xml
	<aura:component controller="MyPropertyController" implements="flexipage:availableForRecordHome,force:hasRecordId" access="global">
		<aura:attribute name="recordId" type="Id" />
		<aura:attribute name="similarProperties" type="Object[]" />
	    <aura:attribute name="property" type="Property__c" />
	    <aura:attribute name="searchCriteria" type="String" default="Price" />
	    <aura:attribute name="priceRange" type="String" default="100000" />
	
	    <force:recordData 
	            aura:id="propertyService" 
	            recordId="{!v.recordId}" 
	            targetFields="{!v.property}" 
	            layoutType="FULL" 
	            recordUpdated="{! c.doSearch}" />
	
		<lightning:card iconName="custom:custom85" title="{! 'Similar Properties by ' + v.searchCriteria}" class="slds-is-relative">
	        <div class="slds-p-horizontal_medium">
	            CONTENT GOES HERE
	        </div>
		</lightning:card>
	</aura:component>
  ```
  
### Step 3

  ```js
({
    doSearch : function(component, event, helper) {
        var action = component.get("c.getSimilarProperties");
        action.setParams({
            recordId: component.get("v.recordId"),
            beds: component.get("v.property.fBeds__c"),
            price: component.get("v.property.Price__c"),
            searchCriteria: component.get("v.searchCriteria"),
            priceRange: parseInt(component.get("v.priceRange"), 10)
        });
        action.setCallback(this, function(response){
            var similarProperties = response.getReturnValue();
            component.set("v.similarProperties", similarProperties);
        });
        $A.enqueueAction(action);
    }
})
  ```
  
### Step 4

	```xml
	<ul class="slds-grid slds-wrap slds-has-dividers_top-space">
	    <aura:iteration items="{!v.similarProperties}" var="item">
	        <li class="slds-item slds-size_1-of-1">
	            {! item.Name}
	        </li>
	    </aura:iteration>
	</ul>
	```
 
### Step 5

  ```xml
	<aura:component controller="MyPropertyController" implements="flexipage:availableForRecordHome,force:hasRecordId" access="global">
		<aura:attribute name="recordId" type="Id" />
		<aura:attribute name="similarProperties" type="Object[]" />
	    <aura:attribute name="property" type="Property__c" />
	    <aura:attribute name="searchCriteria" type="String" default="Price" />
	    <aura:attribute name="priceRange" type="String" default="100000" />
	    <aura:attribute name="fields" type="String[]" default="Beds__c,Baths__c,Price__c,Status__c,Broker__c"/>
	
	    <force:recordData 
	            aura:id="propertyService" 
	            recordId="{!v.recordId}" 
	            targetFields="{!v.property}" 
	            layoutType="FULL" 
	            recordUpdated="{! c.doSearch}" />
	
		<lightning:card iconName="custom:custom85" title="{! 'Similar Properties by ' + v.searchCriteria}" class="slds-is-relative">
	        <div class="slds-p-horizontal_medium">
	                <ul class="slds-grid slds-wrap slds-has-dividers_top-space">
	                        <aura:iteration items="{!v.similarProperties}" var="item">
	                            <li class="slds-item slds-size_1-of-1">
	                                <div class="slds-media slds-p-vertical_medium">
	                                    <div class="slds-media__figure">
	                                        <img src="{!item.Thumbnail__c}" class="slds-avatar_large slds-avatar_circle" alt="{!v.targetFields.Title_c}" />
	                                    </div>
	                                    <div class="slds-media__body">
                                            <a onclick="{!c.navToRecord}" data-value="{! item.Id}">
                                                    <h3 class="slds-text-heading_small slds-m-bottom_xx-small">{! item.Name}</h3>
                                            </a>
	                                    	    <lightning:recordForm recordId="{! item.Id}" objectApiName="Property__c" fields="{!v.fields}" columns="2" mode="View" />
	                                    </div>
	                                </div>
	                            </li>
	                        </aura:iteration>
	                    </ul>
	        </div>
		</lightning:card>
	</aura:component>
  ```

### Step 6

  ```js
	({
	    doSearch : function(component, event, helper) {
	        var action = component.get("c.getSimilarProperties");
	        action.setParams({
	            recordId: component.get("v.recordId"),
	            beds: component.get("v.property.Beds__c"),
	            price: component.get("v.property.Price__c"),
	            searchCriteria: component.get("v.searchCriteria"),
	            priceRange: parseInt(component.get("v.priceRange"), 10)
	        });
	        action.setCallback(this, function(response){
	            var similarProperties = response.getReturnValue();
	            component.set("v.similarProperties", similarProperties);
	        });
	        $A.enqueueAction(action);
	    },
	    navToRecord : function (component, event, helper) {
	        var remoteId = event.currentTarget;
	        var navEvt = $A.get("e.force:navigateToSObject");
	        navEvt.setParams({
	            "recordId": remoteId.dataset.value
	        });
	        navEvt.fire();
	    }
	})
  ```
