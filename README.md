# zOS-Connect
This Journey describes the steps to try out the z/OS Connect Enterprise Edition trials.

## Overview
IBM® z/OS® Connect Enterprise Edition is a framework that enables z/OS based programs and data to participate fully in the new API economy for mobile and cloud applications.

IBM z/OS Connect Enterprise Edition V2.0 provides access to z/OS subsystems, such as CICS®, IMS™, WebSphere® MQ, DB2®, and Batch, that use RESTful APIs with JSON formatted messages. The framework provides concurrent access, through a common interface, to multiple z/OS subsystems.

z/OS Connect EE can help to deliver benefits for an enterprise in two ways.
* It provides an intuitive workstation-based tool, the z/OS Connect EE API Editor, that enables a developer, with or without z/OS skills, to create RESTful APIs from traditional z/OS based assets. The core business assets that run on z/OS can easily be adapted to the latest mobile and cloud communication techniques and message protocol formats.
* Mobile and cloud application developers, inside or outside the enterprise, can incorporate z/OS data and transactions into their applications without the need to understand z/OS subsystems. The z/OS resources appear as any other RESTful API. 

## Included components
[IBM® z/OS® Connect Enterprise Edition](https://www.ibm.com/us-en/marketplace/connect-enterprise-edition)    
[IBM® CICS®](https://www-01.ibm.com/software/data/enterprise-application-servers/cics/)    
IBM® IMS® ???   

## Prerequisites

To request a trial, go to [IBM Trial homepage](https://www-03.ibm.com/systems/z/resources/trials.html). On this page, navigate to the **z/OS Connect Enterprise Edition** panel on the right. In the panel, click **"register now"** button and follow the steps. If everything goes through, you shoud see a screen titled "Congradulations, your environment is on its way".
Wait for an email notification from "zTrial".
The waiting normally lasts several hours to one business day.   
The trial is available through a Windows-based remote desktop environment.

In order to access the trial environment, you must be able to connect to a remote system over a network connection.
Windows users should use the built-in Remote Desktop Connection application.       
Mac users should use the Remote Desktop app, available from the App Store.       
Linux users have several choices of remote desktop application, which might vary between distribution.   

## Expose a CICS COBOL program as a RESTful API.

IBM® z/OS Connect Enterprise Edition (z/OS Connect EE) makes exposing a CICS® application through a RESTful API quick and easy.

![](img/topology_cics.png)

This scenario guides you through the steps in roughly 30 minutes. By the end of the session, you'll know how to:

*   Create and deploy an API from within IBM Developer for z Systems™.
*   Associate and map an API to a service representation of a CICS application, no code required.
*   Test an API by using the built-in Swagger UI.

No previous knowledge of CICS, z/OS Connect EE, or API design is needed, but some awareness of API terminology might help.

Please wait a moment while your development environment loads (this takes about 20 seconds). 


1.  [Creating the API Project](#create_api_project)  
    Create an API project that will contain your API and service mapping.
2.  [Creating the API](#create_api)  
    Create a basic API that uses RESTful principles.
3.  [Associating a service with the API](#associate_service_with_api)  
    Associate your API to a Service Archive file.
4.  [Mapping the request](#map_request)  
    Map your API parameters to fields in the associated service.
5.  [Mapping the response](#map_response)  
    Remove irrelevant values from the response so that your API returns relevant fields only.
6.  [Deploying the API](#deploy_api)  
    Package and deploy your API from within the API Editor.
7.  [Testing the API](#test_api)  
    Test your API by using the built-in Swagger UI.

### Creating the API Project  <a name="create_api_project"></a>


Create an API project that will contain your API and service mapping.


Before you create your API, you must create a new API project. A z/OS® Connect EE API Project contains files that represent the API, and the mappings from the API to the services.

<div class="note note note_note"><span class="note__title">Note:</span> You will connect to a CICS® application that holds inventory information about office stationary (for example, price and stock levels), so let's call our project <kbd class="ph userinput">catalog</kbd>.</div>


1.  <span class="ph cmd">On the development environment menu bar, click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">New</span> > <span class="ph uicontrol">Project...</span> to open the New Project wizard.</span>
2.  <span class="ph cmd">Expand the <span class="ph uicontrol">z/OS Connect Enterprise Edition</span> folder, select <span class="ph uicontrol">z/OS Connect EE API Project</span>, and click <span class="ph uicontrol">Next</span>.</span>
3.  <span class="ph cmd">Complete the fields as follows:</span>


    *   type <kbd class="ph userinput">catalog</kbd> in the <span class="ph uicontrol">Project name</span> and <span class="ph uicontrol">API name</span> fields.
    *   type <kbd class="ph userinput">/catalogManager</kbd> in the <span class="ph uicontrol">Base path</span> field.    

 ![A screen shot that shows the API project wizard, with the required text boxes filled in.](img/new_proj_zc_cics.png)  

4.  <span class="ph cmd">Click <span class="ph uicontrol">Finish</span> to create the project in the Project Explorer.</span>

    <div class="itemgroup info">The z/OS Connect EE API Editor dialog opens.</div>

<section class="section result">Your API project is created.</section>

<section class="section postreq">The next step is to create your API: let's do that now.</section>


### Creating the API <a name="create_api"></a>

Create a basic API that uses RESTful principles.

<section class="section context">You'll create an API that a consumer can use to query the stationery catalog. The API will connect through a service to the COBOL application in CICS®.</section>

1.  <span class="ph cmd">In the <span class="ph uicontrol">Path</span> field, replace `/newPath1` with the value <kbd class="ph userinput">/items/{itemID}</kbd>.</span>

    <div class="itemgroup info">The parameter `{itemID}` is a placeholder that represents the value that is provided by an API consumer when they make a request.

    For example, the item `24-pack of black ball pens` has an ID of `10`, so you would specify <kbd class="ph userinput">/items/10</kbd> in your API request.

    </div>

2.  <span class="ph cmd">Remove the POST, PUT, and DELETE methods for this path by clicking the <span class="ph uicontrol">X</span> icon associated with each method.</span>

This leaves the GET method. The GET method is typically used for retrieving data, which is the purpose of your API.       
![Screen capture that shows correctly configured Paths and Methods.](img/leave_get_cics.gif)  

3.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>

<section class="section result">You've created the front end of your API! You must now connect the API to the application in CICS. In z/OS Connect EE, this connection is through a service, which represents the logic of the CICS application in a more consumable form.</section>

<section class="section postreq">Next, you'll associate your API with that relevant service.</section>

### Associating a service with the API <a name="associate_service_with_api"></a>

Associate your API to a Service Archive file.

In z/OS Connect EE, a service archive file (`.sar` file) represents the underlying z/OS® asset.

z/OS Connect EE provides tooling to generate `.sar` files for its compatible subsystems (including CICS®, IMS™, DB2®, and IBM® MQ).

In this scenario, the `.sar` file is already generated, so you can focus on creating, deploying, and testing your API.


1.  <span class="ph cmd">Click <span class="ph uicontrol">Service...</span>.</span>

    <div class="itemgroup info">The <span class="ph uicontrol">Select a z/OS Connect EE Service</span> dialog box opens.</div>

2.  <span class="ph cmd">Click <span class="ph uicontrol">File System</span>.</span>
3.  <span class="ph cmd">Browse to `C:/Service Archive Files`.</span>
4.  <span class="ph cmd">Select the `inquireSingle.sar` file and click <span class="ph uicontrol">Open</span>.</span>
5.  <span class="ph cmd">In the dialog box that opens, click <span class="ph uicontrol">OK</span> to confirm the import.</span>
6.  <span class="ph cmd">Click <span class="ph uicontrol">OK</span>.</span>

The `inquireSingle` service is now associated with the get method of your API.     
![Screen capture that shows the inquireSingle service that is correctly associated with the GET method of your API.](img/serv_assoc.png)  

7.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>

<section class="section result">Great, the GET method of your API is now configured to connect to the CICS application through the `inquireSingle` service.</section>

In this scenario, the connection between z/OS Connect EE and CICS is configured for you (using the WOLA service provider).

The final step is to configure the mappings between your new API and the `inquireSingle` service, which represents the CICS COBOL application logic.


### Mapping the request <a name="map_request"></a>


Map your API parameters to fields in the associated service.

<section class="section context">Use the request mapping to associate parameters in your API with fields in the `inquireSingle` service, and to remove redundant fields from the API documentation.</section>

1.  <span class="ph cmd">Next to the GET method, click <span class="ph uicontrol">Mapping...</span> then click <span class="ph uicontrol">Open Request Mapping</span>.</span>

    <div class="itemgroup info">The <span class="ph uicontrol">request</span> tab opens.</div>

2.  <span class="ph cmd">Right-click `ca_request_id`, then click <span class="ph uicontrol">Add Assign transform</span>.</span>

    <div class="itemgroup info">A static value is assigned to the field in the request schema.</div>

3.  <span class="ph cmd">In the <span class="ph uicontrol">Properties</span> view, at the bottom of the window, click <span class="ph uicontrol">General</span> and set <span class="ph uicontrol">Value</span> to <kbd class="ph userinput">01INQS</kbd>.</span>

![Screen capture showing the Properties view](img/properties_cics.gif)

Leave the <span class="ph uicontrol">Omit from interface</span> option checked to exclude this value from the API documentation.

4.  <span class="ph cmd">Expand the field `ca_inquire_single` so that the sub-field `ca_single_item` is visible.</span>
5.  <span class="ph cmd">Select the fields `ca_return_code`, `ca_response_message`, and `ca_single_item`.</span>

    <div class="itemgroup info">Press Ctrl and click to select multiple fields.</div>

6.  <span class="ph cmd">Right-click one of the selected fields, then click <span class="ph uicontrol">Add Remove transform</span>.</span>

    <div class="itemgroup info">The fields are now removed from the API documentation.</div>

7.  <span class="ph cmd">Connect the path parameter <span class="ph uicontrol">itemID</span> to the field `ca_item_ref_req` by clicking and dragging one to the other.</span>


    A <span class="ph uicontrol">Move</span> action is created that assigns the path parameter `itemID` in the HTTP request to the field `ca_item_ref_req` in the service's JSON content.

    ![](img/move_action_cics.gif)  

    <div class="note note note_note"><span class="note__title">Note:</span> This mapping defines the flow of a value, which is passed in by an API consumer, to the service associated with the API. The service then passes the value to the CICS application. At each stage, the data is transformed into formats and structures that each participant can understand.</div>


8.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>
9.  <span class="ph cmd">Close the <span class="ph uicontrol">request</span> tab.</span>

<section class="section postreq">The next step is to clean up the API response by removing fields that are not related to the API request.</section>


### Mapping the response <a name="map_response"></a>

Remove irrelevant values from the response so that your API returns relevant fields only.

<section class="section context">The API you're creating is designed to pass back information about a requested item in the catalog application. However, the service `inquireSingle` contain several fields that aren’t relevant to that request.

You can safely remove these fields to make the API response and the API documentation clearer.

</section>

1.  <span class="ph cmd">Click <span class="ph uicontrol">Mapping...</span> for the GET method, then click <span class="ph uicontrol">Open Response Mapping</span>.</span>
2.  <span class="ph cmd">Expand the field `ca_inquire_single` so that the sub-field `ca_item_ref_req` is visible.</span>
3.  <span class="ph cmd">On the right side of the window, select the `ca_request_id` and `ca_item_ref_req` fields.</span>

    <div class="itemgroup info">Press Ctrl and click to select multiple fields.</div>

4.  <span class="ph cmd">Right-click one of the selected fields, and select <span class="ph uicontrol">Add Remove transform</span>.</span>

This excludes these fields from the body of the response.  
![Screen capture that shows the correct configuration for the response tab for this scenario.](img/remove_action_cics.png)  

5.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>
6.  <span class="ph cmd">Close the <span class="ph uicontrol">response</span> tab.</span>

<section class="section result">You completed the request and response mappings.</section>

<section class="section postreq">You completed your API front end, your service association, and respective API-to-service mappings, so go ahead and make this API available.</section>


### Deploying the API <a name="deploy_api"></a>


Package and deploy your API from within the API Editor.

<section class="section context">Deploying your API is a quick, simple process that you can complete without leaving the developer environment.</section>

1.  <span class="ph cmd">In the <span class="ph uicontrol">Project Explorer</span> view, select your API project (<span class="ph uicontrol">catalog</span>) and right-click to select <span class="ph menucascade"><span class="ph uicontrol">z/OS Connect EE</span> > <span class="ph uicontrol">Deploy API to z/OS Connect EE Server</span></span>.</span>
2.  <span class="ph cmd">Click <span class="ph uicontrol">OK</span>.</span>

![Screen capture showing the Deploy API dialog box](img/deploy_cics.png)Your API is being deployed.

3.  <span class="ph cmd">When deployment completes, click <span class="ph uicontrol">OK</span> on the Result dialog box.</span>

    <div class="itemgroup stepresult">Your API is now successfully deployed!</div>

<section class="section result">Creating and deploying this API using z/OS Connect EE gives your API consumers an easy, programmable way to interact with a CICS® application without having to work with unfamiliar data structures, or understand CICS.</section>

<section class="section postreq">You can test your work now, by calling the API from within the development environment by using the built-in interactive API documentation.</section>


### Testing the API <a name="test_api"></a>

Test your API by using the built-in Swagger UI.

<section class="section context">The OpenAPI (previously Swagger) specification is one of the most popular frameworks for documenting APIs. Any API that you create by using the z/OS Connect EE API Editor is automatically OpenAPI 2.0 compliant.

You can use the built-in Swagger UI to test out an API from within the developer environment. Let's try that now by opening up the Swagger UI for your new API, and request information about an item in the catalog.

</section>

1.  <span class="ph cmd">In the <span class="ph uicontrol">z/OS Connect EE Servers</span> view, expand the API folder, right-click your API, and click <span class="ph uicontrol">Open in Swagger UI</span>.</span>

    <div class="itemgroup info">The Swagger UI for your API opens.</div>

2.  <span class="ph cmd">In the tab that opens, click <span class="ph uicontrol">default</span>, and then <span class="ph uicontrol">/items/{itemID}</span></span>

    <div class="itemgroup info">Take a look at the Example Value JSON. You'll notice that the fields that you removed as part of the mapping are not present.</div>

The CICS application that you have exposed by creating this API contains an inventory of office stationary. A 24-pack of black ball pens has an `itemID` of <kbd class="ph userinput">10</kbd>.

You can use your new API to check the price and stock levels of this item.

1.  <span class="ph cmd">Type the value <kbd class="ph userinput">10</kbd> in the relevant text box, then click <span class="ph uicontrol">Try it out</span>.</span>

    <div class="itemgroup info">Scroll down to see the response. By inspecting the Response Body, you can see that this item costs $2.90 and 129 items are in stock.

         {
                                "DFH0XCMNOperationResponse": { "ca_return_code": 0, "ca_response_message":
                                "RETURNED ITEM: REF =0010", "ca_inquire_single": { "ca_single_item": {
                                    "in_sngl_stock": 129, "ca_sngl_description": "Ball Pens Black
                                24pk", "ca_sngl_item_ref": 10, "on_sngl_order": 0, "ca_sngl_cost":
                                    "002.90", "ca_sngl_department": 10 } } } } 

    </div>
Congratulations! You've successfully exposed a CICS® application as a RESTful API by using z/OS C


## Expose an IMS application as a RESTful API

IBM® z/OS Connect Enterprise Edition (z/OS Connect EE) makes exposing an IMS™ application through a RESTful API quick and easy.

![](img/topology_ims.png)

This scenario guides you through the steps in roughly 30 minutes. By the end of the session, you'll know how to:

*   Create and deploy an API from within IBM Developer for z Systems™.
*   Associate and map an API to a service representation of a CICS application, no code required.
*   Test an API by using the built-in Swagger UI.

No previous knowledge of IMS, z/OS Connect EE, or API design is needed, but some awareness of API terminology might help.

Please wait a moment while your development environment loads (this takes about 20 seconds). 

1.  [Creating the API Project](#createapi_project)  
    Create an API project that will contain your API and service mapping.
2.  [Creating the API](#createapi)  
    Create a basic API that uses RESTful principles.
3.  [Associating a service with the API](#associateservice_with_api)  
    Associate your API to a Service Archive file.
4.  [Mapping the request](#maprequest)  
    Map your API parameters to fields in the associated service.
5.  [Mapping the response](#mapresponse)  
    Remove irrelevant values from the response so that your API returns relevant fields only.
6.  [Deploying the API](#deployapi)  
    Package and deploy your API from within the API Editor.
7.  [Testing the API](#testapi)  
    Test your API by using the built-in Swagger UI.

### Creating the API Project <a name="createapi_project"></a>


Create an API project that will contain your API and service mapping.

Before you create your API, you must create a new API project. A z/OS® Connect EE API Project contains files that represent the API, and the mappings from the API to the services.

<div class="note note note_note"><span class="note__title">Note:</span> You will connect to an IMS™ application that holds contact information, so let's call our project <kbd class="ph userinput">phonebook</kbd>.</div>

</section>

1.  <span class="ph cmd">On the development environment menu bar, click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">New</span> > <span class="ph uicontrol">Project...</span> to open the New Project wizard.</span>
2.  <span class="ph cmd">Expand the <span class="ph uicontrol">z/OS Connect Enterprise Edition</span> folder, select <span class="ph uicontrol">z/OS Connect EE API Project</span>, and click <span class="ph uicontrol">Next</span>.</span>
3.  <span class="ph cmd">Complete the fields as follows:</span>

    <div class="itemgroup info">

    *   enter <kbd class="ph userinput">phoneBook</kbd> in the <span class="ph uicontrol">Project name</span> field and <span class="ph uicontrol">API name</span> field.
    *   enter <kbd class="ph userinput">/phoneBook</kbd> in the <span class="ph uicontrol">Base path</span> field.

    ![A screen shot that shows the API project wizard, with the required text boxes filled in.](img/new_proj_zc_ims.png)  
    </div>

4.  <span class="ph cmd">Click <span class="ph uicontrol">Finish</span> to create the project in the Project Explorer.</span>

    <div class="itemgroup info">The z/OS Connect EE API Editor dialog opens.</div>

<section class="section result">Your API project is created.</section>

<section class="section postreq">The next step is to create your API: let's do that now.</section>

### Creating the API <a name="createapi"></a>

Create a basic API that uses RESTful principles.

You'll create an API that a consumer can use to add contact information to the phone book program. The API will connect through a service to the program in IMS™.

1.  <span class="ph cmd">In the <span class="ph uicontrol">Path</span> field, replace <span class="ph filepath">/newPath1</span> with the value <span class="ph filepath">/contacts</span>.</span>

    <div class="itemgroup info">The contact record values are sent in the HTTP message body of our POST request. In this case, no parameters are stored in the URL.</div>

2.  <span class="ph cmd">Remove the GET, PUT and DELETE methods for this path by clicking on the <span class="ph uicontrol">X</span> icon associated with each method.</span>

This leaves the POST method. The POST method is typically used for submitting data, which is the purpose of your API.  
![Screen capture that shows correctly configured Paths and Methods.](img/leave_post_ims.gif)  


3.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>

<section class="section result">You've created the front end of your API! You must now connect the API to the program in IMS. In z/OS Connect EE, this connection is through a service, which represents the logic of the IMS program in a more consumable form.</section>

<section class="section postreq">Next, you'll associate your API with that relevant service.</section>

### Associating a service with the API <a name="associateservice_with_api"></a>

Associate your API to a Service Archive file.


In z/OS Connect EE, a service archive file (`.sar` file) represents the underlying z/OS® asset.

z/OS Connect EE provides tooling to generate `.sar` files for its compatible subsystems (including CICS®, IMS™, DB2®, and IBM® MQ).

In this scenario, the `.sar` file is already generated, so you can focus on creating, deploying, and testing your API.


1.  <span class="ph cmd">Click <span class="ph uicontrol">Service...</span>.</span>

    <div class="itemgroup info">The <span class="ph uicontrol">Select a z/OS Connect EE Service</span> dialog box opens.</div>

2.  <span class="ph cmd">Click <span class="ph uicontrol">File System</span>.</span>
3.  <span class="ph cmd">Browse to `C:/Service Archive Files`.</span>
4.  <span class="ph cmd">Select the `contacts.sar` file and click <span class="ph uicontrol">Open</span>.</span>
5.  <span class="ph cmd">In the dialog box that opens, click <span class="ph uicontrol">OK</span> to confirm the import.</span>
6.  <span class="ph cmd">Click <span class="ph uicontrol">OK</span>.</span>

The `contacts` service is now associated with the get method of your API.  
![Screen shot that shows the contacts service correctly associated with the POST method of your API.](img/serv_assoc_ims.png)  
 

7.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>

<section class="section result">Great, the POST method of your API is now configured to connect to the IMS program through the `contacts` service.</section>

In this scenario, the connection between z/OS Connect EE and IMS is configured for you (using the IMS service provider).

The final step is to configure the mappings between your new API and the `contacts` service, which represents the IMS program logic.


### Mapping the request <a name="maprequest"></a>

Map your API parameters to fields in the associated service.

<section class="section context">Use the request mapping to associate parameters in your API with fields in the `contacts` service, and to remove redundant fields from the API documentation.</section>

1.  <span class="ph cmd">Next to the POST method, click <span class="ph uicontrol">Mapping...</span> then click <span class="ph uicontrol">Open Request Mapping</span>.</span>

    <div class="itemgroup info">The <span class="ph uicontrol">request</span> tab opens.</div>

2.  <span class="ph cmd">Expand the `IVTNO_INPUT_MSG` section on the left side of the tab.</span>

![Screen capture that shows how to expand the IVTNO_INPUT_MSG section in the request tab.](img/expand_section_ims.gif)

This shows fields that are exposed through the API and made available to the REST clients. By default, it is a one-to-one mapping to the fields that are exposed by the service unless you change the mapping.

3.  <span class="ph cmd">Right-click the `IN_COMMAND` field on right side of the tab, in the service definition, then click <span class="ph uicontrol">Add Assign transform</span>.</span>
4.  <span class="ph cmd">In the <span class="ph uicontrol">Properties</span> view, at the bottom of the window, click <span class="ph uicontrol">General</span> and set <span class="ph uicontrol">Value</span> to <kbd class="ph userinput">ADD</kbd>.</span>

![Screen capture showing the Properties view](img/properties_ims.gif)

Leave the <span class="ph uicontrol">Omit from interface</span> option checked to exclude this value from the API documentation.

Note: This mapping defines the flow of a value, which is passed in by an API consumer, to the service associated with the API. The service then passes the value to the IMS program. At each stage, the data is transformed into formats and structures that each participant can understand.

5.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>
6.  <span class="ph cmd">Close the <span class="ph uicontrol">request</span> tab.</span>

<section class="section postreq">The next step is to clean up the API response by removing fields that are not related to the API request.</section>

### Mapping the response <a name="mapresponse"></a>

Remove irrelevant values from the response so that your API returns relevant fields only.

The API you're creating is designed to pass back information about a requested item in the phone book program. However, the `contacts` service contain several fields that aren’t relevant to that request.

You can safely remove these fields to make the API response and the API documentation clearer.

1.  <span class="ph cmd">Click <span class="ph uicontrol">Mapping...</span> for the POST method, then click <span class="ph uicontrol">Open Response Mapping</span>.</span>
2.  <span class="ph cmd">Right-click `OUT_COMMAND`, and select <span class="ph uicontrol">Add Remove transform</span>.</span>

This excludes this field from the body of the response.  
![Screen shot that shows the correct configuration for the response tab for this scenario.](img/response_map_ims.png)  


3.  <span class="ph cmd">Click <span class="ph uicontrol">File</span> > <span class="ph uicontrol">Save</span> from the menu to save your progress.</span>
4.  <span class="ph cmd">Close the <span class="ph uicontrol">response</span> tab.</span>

<section class="section result">You completed the request and response mappings.</section>

<section class="section postreq">You completed your API front end, your service association, and respective API-to-service mappings, so go ahead and make this API available.</section>

### Deploying the API <a name="deployapi"></a>

<div class="body taskbody">

Package and deploy your API from within the API Editor.

<section class="section context">Deploying your API is a quick, simple process that you can complete without leaving the developer environment.</section>

1.  <span class="ph cmd">In the <span class="ph uicontrol">Project Explorer</span> view, select your API project (<span class="ph uicontrol">phonebook</span>) and right-click to select <span class="ph menucascade"><span class="ph uicontrol">z/OS Connect EE</span> > <span class="ph uicontrol">Deploy API to z/OS Connect EE Server</span></span>.</span>
2.  <span class="ph cmd">Click <span class="ph uicontrol">OK</span>.</span>

![Screen capture showing the Deploy API dialog box](img/deploy_ims.png)Your API is being deployed.

3.  <span class="ph cmd">When deployment completes, click <span class="ph uicontrol">OK</span> on the Result dialog box.</span>

    <div class="itemgroup stepresult">Your API is now successfully deployed!</div>

<section class="section result">Creating and deploying this API using z/OS Connect EE gives your API consumers an easy, programmable way to interact with a IMS™application without having to work with unfamiliar data structures, or understand IMS.</section>

<section class="section postreq">You can test your work now, by calling the API from within the development environment by using the built-in interactive API documentation.</section>


### Testing the API <a name="testapi"></a>

Test your API by using the built-in Swagger UI.

<section class="section context">The OpenAPI (previously Swagger) specification is one of the most popular frameworks for documenting APIs. Any API that you create by using the z/OS Connect EE API Editor is automatically OpenAPI 2.0 compliant.

You can use the built-in Swagger UI to test out an API from within the developer environment. Let's try that now by opening up the Swagger UI for your new API, and adding a contact to the phone book.

</section>

1.  <span class="ph cmd">In the <span class="ph uicontrol">z/OS Connect EE Servers</span> view, expand the API folder, right-click your API, and click <span class="ph uicontrol">Open in Swagger UI</span>.</span>

    <div class="itemgroup info">The Swagger UI for your API opens.</div>

2.  <span class="ph cmd">In the tab that opens, click <span class="ph uicontrol">default</span>, and then <span class="ph uicontrol">/contacts</span></span>

    <div class="itemgroup info">Take a look at the Example Value JSON. You'll notice that the fields that you removed as part of the mapping are not present.</div>

The IMS program that you have exposed by creating this API contains contact information.

You can use your new API to add a new contact.

1.  <span class="ph cmd">Click the <span class="ph uicontrol">Example Value</span> box to copy the request message format into your phone book POST request.</span>

    <div class="itemgroup info">Specify some values for the last name, first name, zip code, and extension.</div>

2.  <span class="ph cmd">Click <span class="ph uicontrol">Try it out!</span>.</span>

    Information about the request URL, request headers, response body, response code, and response headers are provided. The response body contains the output message. The `OUT_MESSAGE` would contain one of the following messages:

    *   ENTRY WAS ADDED
    *   ADDITION OF ENTRY HAS FAILED (this message indicates that the IN_LAST_NAME value you specified already exists)


Congratulations! You've successfully exposed a IMS™ program as a RESTful API by using z/OS Connect EE!



