# Developing Transactional App Based on Managed Scenario

* [Anubhav Trainings Official Website](https://www.anubhavtrainings.com)
* [Anubhav Trainings Official Channel](https://www.youtube.com/@AnubhavOberoy)


# Developing S/4HANA Embedded Analytics - Fiori Elements - Analytic List Page App

## Introduction
In this unit, you will learn how to create an SAP Fiori application using SAP Business Application Studio and configure the content area which includes a chart and a table.

## Step 1. Generate SAP Fiori elements project with an Analytical List Page Floor Plan
The first step is to create your SAP Fiori elements project using the Analytical List Page Floor Plan in SAP Business Application Studio.


1. On the welcome page click  **Start from template - Create a new Project**. 

    ![](fiori/unit1/img_000.png)


2. Select the template **SAP Fiori application** and click **Start**.

    ![](fiori/unit1/img_001.png)


3. Select the application type as **Analytical List Page** and click **Next**.

    ![](fiori/unit1/img_002.png)



4. In this template step we connect to the SAP backend system and select the service which we created earlier

   Choose **Connect to a System** from the dropdown field **Data source**.
    
    ![](fiori/unit1/img_003.png)
    <br>
    <br>
   To select the system choose **S4H** from the dropdown field **System** .     

    ![](fiori/unit1/img_004.png)
    <br>
    <br>
    To select the service choose **ZITC_FE_BOOKING_AO - OData V2** from the dropdown field **Service**.

    ![](fiori/unit1/img_005.png)
   <br>
   <br>

    Click  **Next**.

    ![](fiori/unit1/img_006.png)


5. In the Entity Selection screen choose **Booking** from the dropdown field **Main entity**.

    ![](fiori/unit1/img_007.png)
    <br><br>
    **Table type** should be **Analytical**. 

    ![](fiori/unit1/img_008.png)
   <br><br>
    Click  **Next**.

    ![](fiori/unit1/img_009.png)


6. Maintain the project attributes as follows:
    
    **Module name**: bookinganalysis
    
    **Application title**: Booking Analysis
    
    **Application Namespace**: sap.fe.demo
   
    **Project folder path**: /home/user/projects

    Click  **Finish**.

    ![](fiori/unit1/img_010.png)

    The new SAP Fiori elements application is now created from the template using the service and the configurations you provided in this step.


7. Now you can open and display the new application you have created in the folder structure of **Explorer**. To do so click **Open Folder**. 

    ![](fiori/unit1/img_011.png)
    <br><br>
    Click on  **bookinganalysis**. 

    ![](fiori/unit1/img_012.png)
    <br><br>
    Click on  **Open**.
   
    ![](fiori/unit1/img_013.png)
    <br><br>
    In the explorer on the left you can see the newly created application with its folder structure.

    ![](fiori/unit1/img_014.png)


## Step 2. Configuring the content area by adding a table and chart
In this section, you will define the content area of the Analytical List Page with SAP Fiori guided development. 
For this, you first have to open the guided development tool.

1. To open the guided development tool right click on the **Webapp Folder**.

    ![](fiori/unit1/img_015.png)


2. Click  **SAP Fiori tools - Open Guided Development**.
    >⚠️ Firefox currently has a bug that it does not treat subdomains of localhost as secure. When you open "Guided Development" or another tool of the Fiori tools and the tab stays empty please refresh the page by pressing F5 or click the reload button of the browser. After reloading the page the tool should show up.
   > 
    ![](fiori/unit1/img_016.png)

## Step 2.1 Configuring the table by adding columns to it
In this step, you will configure the columns displayed in the table of the Analytical List Page. UI annotation 'LineItem' is used to represent the table, with DataField records representing the columns. As you will use the guided development tool, you do not need to add this annotation manually, just follow the steps below to configure the table, and the respective annotation will be added to or updated in the local annotation file when you choose Insert Snippet (formerly Apply).

1. To find the guides related to tables, type **table** in the search field

    ![](fiori/unit1/img_017.png)


2. Scroll to the Analytical List Page group and select the guide **Add and edit table columns**.

    ![](fiori/unit1/img_018.png)


3. Click **Start Guide**.

    ![](fiori/unit1/img_019.png)

    This simple guide contains just one step adding a column (DataField record) to your table (LineItem). 


4. In the Entity Type field, choose your main entity type as: **BookingType**.

    ![](fiori/unit1/img_020.png)

    ![](fiori/unit1/img_021.png)


5. In the Property field, choose **BookingID** as your first column content. <br>

    ![](fiori/unit1/img_022.png)

    ![](fiori/unit1/img_023.png)
    The code snippet is adjusted accordingly. <br>

    Click **Insert Snippet** (by the time the screenshots were created the button was called Apply). <br>
    The annotation UI.LineItem is added to your local annotation file as configured in the code snippet. This file gets opened next to the guided development tool.

    ![](fiori/unit1/img_024.png)


6. The above step is repeated as many as times as needed to add other columns to the table.

    Choose **AgencyID** and click **Insert Snippet (formerly Apply)**.

    Choose **CustomerID** and click **Insert Snippet (formerly Apply)**.

    Choose **BookingDate** and click **Insert Snippet (formerly Apply)**.

    Choose **FlightDate** and click **Insert Snippet (formerly Apply)**.

    Choose **FlightPrice** and click **Insert Snippet (formerly Apply)**.
    
    Now we can see that the UI.LineItem annotation for all the table columns is added to your local annotation file as shown in the code snippet below.

    ```
   <Annotation Term="UI.LineItem">
        <Collection>
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="BookingID"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record>
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="AgencyID"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record> 
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="CustomerID"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record>
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="BookingDate"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record>
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="FlightDate"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record>
            <Record Type="UI.DataField">
                <PropertyValue Property="Value" Path="FlightPrice"/>
                <Annotation Term="UI.Importance" EnumMember="UI.ImportanceType/High"/>
            </Record>
        </Collection>
   </Annotation>        
    ```

7. When the records for all property are added to your LineItem, click **Exit Guide**  to get back to the list of available guides.

    ![](fiori/unit1/img_030.png)


8. Now lets have a look how the app. You can either launch the app like you learned it in week 2, unit 4 or use the context menu to start the preview. Right click on **webapp**.

    ![](fiori/unit1/img_031.png)


9. Click **Preview Application**.

    ![](fiori/unit1/img_032.png)


10. Click **Start fiori run —open ‘test/flpSandbox.html#bookinganalysis-tile’**

    ![](fiori/unit1/img_033.png)

    When the application is loaded click **Go**.

    ![](fiori/unit1/img_034.png)

    The Analytical list page application is now loaded with the above configured table.

    ![](fiori/unit1/img_035.png)

## Step 2.2: Configuring the interactive chart

In this step, you will configure the chart to be displayed in the Analytical List Page. As you will use the guided development tool, you do not need to add this annotation manually, just follow the steps below to configure the chart and the respective annotation will be added to or updated in the local annotation file when you choose Insert Snippet (formerly Apply).

1. Filter guides by chart in the search field. 
    Enter **chart** in the search field
    
    ![](fiori/unit1/img_036.png)


2. Select the guide **Add an interactive chart** in the Analytical List Page group.

    ![](fiori/unit1/img_037.png)


3. Click **Start Guide**
    
    This guide has 2 steps. In **Step 1** we will be configuring the chart and in **Step 2** the presentation variant for the chart.
    
    ![](fiori/unit1/img_038.png)


4. Configuring a chart

    In this step, you will configure the annotation term "UI.Chart". It is used to visualize the data in the chart format.
    

5. Select **BookingType** in the dropdown field **Entity Type**.

    ![](fiori/unit1/img_039.png)


6. Enter the following values in the respective fields

    | **Field**             |**Value**
    | --------------------- |----------------------------------
    | Chart Title           |Booking FlightPrice by CarrierID
    | Chart Description     |Booking FlightPrice by CarrierID
    | ChartType             |Column
    | Measures property     |FlightPrice
    | Dimensions Property   |CarrierID

    ![](fiori/unit1/img_040.png)


7. Click **Insert Snippet (formerly Apply)**.

    ![](fiori/unit1/img_041.png)


8. Configuring Presentation Variant

    In this step, you will configure a "UI.PresentationVariant" annotation that is used to display the main content of the Analytical List Page. Here you will assign the chart and table created earlier in this exercise as visualization artifacts and define the sorting order. To do so, you will use the **Step 2** of the **Add interactive chart** guide.


9. Click **Step 2** to configure the UI.PresentationVariant annotation term.

    ![](fiori/unit1/img_042.png)


10. Enter the following values in the respective fields

    |**Field**                 |**Value**
    |------------------------- |----------------------------------
    |Entity Type               |BookingType
    |Sort Order Property       |FlightPrice
    |Sort Order Descending?    |true
    |Include Grand Total?      |false

    ![](fiori/unit1/img_043.png)


11. Click **Insert Snippet (formerly Apply)**.

    ![](fiori/unit1/img_044.png)

    Now we can see the UI.Chart and UI.PresentationVariant annotation is added to your local annotation file as configured in the code snippet below.

    ```
    <Annotation Term="UI.Chart">
        <Record Type="UI.ChartDefinitionType">
            <PropertyValue Property="Title" String="Booking FlightPrice By CarrierID"/>
            <PropertyValue Property="Description" String="Booking FlightPrice By CarrierID"/>
            <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Column"/>
            <PropertyValue Property="Dimensions">
                <Collection>
                    <PropertyPath>CarrierID</PropertyPath>
                </Collection>
            </PropertyValue>
            <PropertyValue Property="Measures">
                <Collection>
                    <PropertyPath>FlightPrice</PropertyPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
    <Annotation Term="UI.PresentationVariant">
        <Record Type="UI.PresentationVariantType">
        <PropertyValue Property="Text" String="Default"/>
             <PropertyValue Property="SortOrder">
                <Collection>
                    <Record Type="Common.SortOrderType">
                        <PropertyValue Property="Property" PropertyPath="FlightPrice"/>
                        <PropertyValue Property="Descending" Bool="true"/>
                    </Record>
                </Collection>
            </PropertyValue>
            <PropertyValue Property="IncludeGrandTotal" Bool="false"/>
            <PropertyValue Property="InitialExpansionLevel" Int="NaN"/>
            <PropertyValue Property="Visualizations">
                <Collection>
                    <AnnotationPath>@UI.LineItem</AnnotationPath>
                    <AnnotationPath>@UI.Chart</AnnotationPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>

    ```
    ![](fiori/unit1/img_045.png)


12. Click **Exit Guide**.

    ![](fiori/unit1/img_046.png)


13. Let's start the preview again. Right click on **webapp** folder.

    ![](fiori/unit1/img_031.png)


14. Click on **Preview Application**. 

    ![](fiori/unit1/img_032.png)


15. Click on **Start fiori run —open ‘test/flpSandbox.html#bookinganalysis-tile’**.

    ![](fiori/unit1/img_033.png)


16. When the application is loaded click on **Go**.

    ![](fiori/unit1/img_034.png)

    The Analytical List Page application in now displayed with the above configured chart.

    ![](fiori/unit1/img_047.png)


    
## Introduction
In this unit, you will learn how to enhance the Analytical List Page application created in the previous unit by adding filtering capabilities.
In this exercise, you will define the filter area of the Analytical List Page using the guided development tool.

## Step 1. Configuring the Compact Filters 
The first step you will configure a UI.SelectionFields annotation that is used to display the filter fields in the compact filter bar of the Analytical List Page.
For this, you first have to open the guided development tool.

1. To open the guided development tool right click on the **webapp** folder.

    ![](fiori/unit2/img_0.png)


2. Click on **SAP Fiori tools - Open Guided Development**. 

    ![](fiori/unit2/img_000.png)


3. To find the guides related to filters, type **filter** in the search field.

    ![](fiori/unit2/img_001.png)


4. Scroll to the Analytical List Page group and expand the guide **Add a new filter field to the Filter Bar**.

    ![](fiori/unit2/img_002.png)


5. Click **Start Guide**.

    ![](fiori/unit2/img_003.png)

    This simple guide contains just one step adding a field (UI Selection Fields) to your compact filter bar. 


6. In the field **Entity Type**, choose your entity type as: **BookingType**.

    ![](fiori/unit2/img_004.png)
    
    ![](fiori/unit2/img_005.png)


7. In the field **Property**, choose **CurrencyCode** as your first column content. <br>

    ![](fiori/unit2/img_006.png)

    ![](fiori/unit2/img_007.png)

    The code snippet is adjusted accordingly. <br>


8. Click **Insert Snippet**.

    ![](fiori/unit2/img_008.png)
    <br>
    The annotation UI.SelectionFields is added to your local annotation file as configured in the code snippet. This file gets opened next to the guided development tool.

    ![](fiori/unit2/img_009.png) <br>


9. The above step is repeated as many as times as needed to add other filters to the filter bar.

    Choose **AgencyID**  and click **Insert Snippet**.

    Choose **AgencyName** and click **Insert Snippet**.

    Choose **CarrierID** and click **Insert Snippet**.

    Choose **CarrierName** and click **Insert Snippet**.

    Choose **CustomerID** and click **Insert Snippet**.

    Choose **CustomerName** and click **Insert Snippet**.

    Choose **BookingDate** and click **Insert Snippet**.

    Choose **BookingDateYear** and click **Insert Snippet**.
    
    Now we can see the UI.SelectionFields annotation for all the filters is added to your local annotation file as shown in the code snippet below.

    ```
    <Annotation Term="UI.SelectionFields">
        <Collection>
            <PropertyPath>CurrencyCode</PropertyPath>
            <PropertyPath>AgencyID</PropertyPath>
            <PropertyPath>AgencyName</PropertyPath>
            <PropertyPath>CarrierID</PropertyPath>
            <PropertyPath>CarrierName</PropertyPath>
            <PropertyPath>CustomerID</PropertyPath>
            <PropertyPath>CustomerName</PropertyPath>
            <PropertyPath>BookingDate</PropertyPath>
            <PropertyPath>BookingDateYear</PropertyPath>
        </Collection>
    </Annotation>      
    ```

10. When the records for all property are added to your LineItem, click **Exit Guide** to get back to the list of available guides.

    ![](fiori/unit2/img_018.png)


## Step 2. Preview the application to view the compact filters 

1. Right click **webapp folder**.

    ![](fiori/unit2/img_019.png)


2. Click on **Preview Application**.

    ![](fiori/unit2/img_020.png)


3. Click on **start fiori run --open 'test/flpSandbox.html#bookinganalysis-tile'**.

    ![](fiori/unit2/img_021.png)


4. To view the application in a new browser tab click **Open** in the dialog.

    ![](fiori/unit2/img_022.png)


5. The visual filters are set as the default filter mode in the Analytical List Page application. To view the compact filters click  ![](fiori/unit2/fieldicon_20.png) (Filter Toogle for compact filters)

    ![](fiori/unit2/img_023.png)


6. To view the chart and table click : **Go**.

    ![](fiori/unit2/img_024.png)


7. We could now see the Analytical List Page application loaded with the chart and the table data.

    ![](fiori/unit2/img_025.png)


## Step 3. Configuring the visual filters of type line

In this step, you will configure a UI.Chart, UI.PresentationVariant and Common.ValueList annotations that are used to display the visual filters. 

1. Expand the guide **Add a new visual filter** in the Analytical List Page group.

    ![](fiori/unit2/img_026.png)


2. Click **Start Guide**.

    ![](fiori/unit2/img_027.png)


3. **Visual Filter Guide(Step 1) - To configure a UI.Chart annotation** 

    At first, you define a chart to be displayed in the filter bar, using a UI.Chart annotation.<br>
   
    In the Entity Type field, choose your entity type as : **BookingType**. 

    ![](fiori/unit2/img_028.png)

    Enter the following values in the respective fields and click **Insert Snippet**.

    | **Field**             |**Value**
    | --------------------- |----------------------------------
    | Chart Qualifier       |FilterFlightPriceBookingDateYear
    | Chart Title           |Filter FlightPrice by BookingDateYear
    | Chart Description     |Filter FlightPrice by BookingDateYear
    | ChartType             |Line
    | Dimensions Property   |BookingDateYear
    | Measures property     |FlightPrice

    ![](fiori/unit2/img_029.png)

    The UI.Chart Annotation is now added to the annotation file.  

    Click **Step 2** <br/>
    Note: You can also click **Next** at the bottom of the guide to go to the next step.

    ![](fiori/unit2/img_031.png)


4. **Visual Filter Guide(Step 2) - To configure a UI.PresentationVariant annotation** 

    In this step, you will reference the chart in a presentation variant so that it could be displayed it in the filter bar.

    Enter the following values and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |----------------------------------
    |Presentation Variant Qualifier   |PVFilterFlightPriceBookingDateYear
    |Chart Reference                  |\@UI.Chart\#FilterFlightPriceBookingDateYear

    ![](fiori/unit2/img_033.png)

    The UI.PresentationVariant Annotation is now added to the annotation file.  
    
    Click **Step 3**.

    ![](fiori/unit2/img_034.png)


5. **Visual Filter Guide(Step 3) - To configure a ValueList annotation** : 

    In that step, you will define the mapping between dimensions in the visual filter and properties in the objects displayed in the main content, so that clicking on a dimension value the visual filter will filter the main content accordingly.For this, we use the Common.ValueList annotation.

    Enter the following values in the respective fields and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |------------------------------------
    |Entity Type                      |BookingType
    |Entity Type Property             |BookingDateYear
    |Collection Path                  |Booking
    |Presentation Variant Qualifier   |PVFilterFlightPriceBookingDateYear
    |Local Data Property              |BookingDateYear
    |Value List Property              |BookingDateYear

    ![](fiori/unit2/img_036.png)

    The Common.ValueList Annotation is now added to the annotation file.

    ![](fiori/unit2/img_037.png)


6. Click **Exit Guide** to get back to the Guides.

    ![](fiori/unit2/img_038.png)

    Now we can see the UI.Chart and UI.PresentationVariant and common.valuelist annotation is added to your local annotation file as configured in the code snippet below.

    ```
    <Annotation Term="UI.Chart" Qualifier="FilterFlightPriceBookingDateYear">
        <Record Type="UI.ChartDefinitionType">
            <PropertyValue Property="Title" String="Filter FlightPrice By BookingDateYear"/>
            <PropertyValue Property="Description" String="Filter FlightPrice By BookingDateYear "/>
            <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Line"/>
            <PropertyValue Property="Dimensions">
                <Collection>
                    <PropertyPath>BookingDateYear</PropertyPath>
                </Collection>
            </PropertyValue>
            <PropertyValue Property="Measures">
                <Collection>
                    <PropertyPath>FlightPrice</PropertyPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
    <Annotation Term="UI.PresentationVariant" Qualifier="PVFilterFlightPriceBookingDateYear">
        <Record Type="UI.PresentationVariantType">
            <PropertyValue Property="Visualizations">
                <Collection>
                    <AnnotationPath>@UI.Chart#FilterFlightPriceBookingDateYear</AnnotationPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
   ```
   ```
    <Annotations Target="cds_zfe_booking_analytics_######.BookingType/BookingDateYear">
        <Annotation Term="Common.ValueList">
            <Record Type="Common.ValueListType">
                <PropertyValue Property="CollectionPath" String="Booking"/>
                <PropertyValue Property="PresentationVariantQualifier" String="PVFilterFlightPriceBookingDateYear"/>
                <PropertyValue Property="Parameters">
                    <Collection>
                        <Record Type="Common.ValueListParameterInOut">
                            <PropertyValue Property="LocalDataProperty" PropertyPath="BookingDateYear"/>
                            <PropertyValue Property="ValueListProperty" String="BookingDateYear"/>
                        </Record>
                    </Collection>
                </PropertyValue>
            </Record>
        </Annotation>
    </Annotations>
    ```

## Step 4. Configuring the Visual Filters of type Donut Chart

In this step, let us try to configure a visual filter with a different Dimension using a Donut Chart

1. Expand the guide **Add a new visual filter** in the Analytical List Page group.

    ![](fiori/unit2/img_026.png)


2. Click **Start Guide**.

    ![](fiori/unit2/img_027.png)


3. **Visual Filter Guide(Step 1) - To configure a UI.Chart annotation**: 
   
    In the Entity Type field, choose your entity type as : **BookingType**. 

    ![](fiori/unit2/img_039.png)

    Enter the following values in the respective fields and Click **Insert Snippet**

    | **Field**             |**Value**
    | --------------------- |----------------------------------
    | Chart Qualifier       |FilterFlightPriceCustomerID
    | Chart Title           |Filter FlightPrice by CustomerID
    | Chart Description     |Filter FlightPrice by CustomerID
    | ChartType             |Donut
    | Dimensions Property   |CustomerID
    | Measures property     |FlightPrice

    ![](fiori/unit2/img_040.png)

    The UI.Chart annotation is now added to the annotation file. Click **Step 2** .

    ![](fiori/unit2/img_041.png)


4. **Visual Filter Guide(Step 2) - To configure a UI.PresentationVariant annotation** : 

    Enter the following values and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |----------------------------------
    |Presentation Variant Qualifier   |PVFilterFlightPriceCustomerID
    |Chart Reference                  |\@UI.Chart\#FilterFlightPriceCustomerID

    ![](fiori/unit2/img_042.png)

    The UI.PresentationVariant annotation is now added to the annotation file, click
    **Step 3**

    ![](fiori/unit2/img_043.png)


5. **Visual Filter Guide(Step 3) - To configure a ValueList annotation** : 

    Enter the following values in the respective fields and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |------------------------------------
    |Entity Type                      |BookingType
    |Entity Type Property             |CustomerID
    |Collection Path                  |Booking
    |Presentation Variant Qualifier   |PVFilterFlightPriceCustomerID
    |Local Data Property              |CustomerID
    |Value List Property              |CustomerID

    ![](fiori/unit2/img_044.png)

    The Common.ValueList Annotation is now added to the annotation file. <br>
    Click **Exit Guide** to get back to the Guides. <br/>

    ![](fiori/unit2/img_045.png) 

     Now we can see the UI.Chart and UI.PresentationVariant and common.valuelist annotation is added to your local annotation file as configured in the code snippet below.

    ```
    <Annotation Term="UI.Chart" Qualifier="FilterFlightPriceCustomerID">
        <Record Type="UI.ChartDefinitionType">
            <PropertyValue Property="Title" String="Filter FlightPrice By CustomerID"/>
            <PropertyValue Property="Description" String="Filter FlightPrice By CustomerID"/>
            <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Donut"/>
            <PropertyValue Property="Dimensions">
                <Collection>
                    <PropertyPath>CustomerID</PropertyPath>
                </Collection>
            </PropertyValue>
            <PropertyValue Property="Measures">
                <Collection>
                    <PropertyPath>FlightPrice</PropertyPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
    <Annotation Term="UI.PresentationVariant" Qualifier="PVFilterFlightPriceCustomerID">
        <Record Type="UI.PresentationVariantType">
            <PropertyValue Property="Visualizations">
                <Collection>
                    <AnnotationPath>@UI.Chart#FilterFlightPriceCustomerID</AnnotationPath>
                </Collection>
            </PropertyValue>
        </Record>
    </Annotation>
   ```
   ```
     <Annotations Target="cds_zfe_booking_analytics_######.BookingType/CustomerID">
        <Annotation Term="Common.ValueList">
            <Record Type="Common.ValueListType">
                <PropertyValue Property="CollectionPath" String="Booking"/>
                <PropertyValue Property="PresentationVariantQualifier" String="PVFilterFlightPriceCustomerID"/>
                <PropertyValue Property="Parameters">
                    <Collection>
                        <Record Type="Common.ValueListParameterInOut">
                            <PropertyValue Property="LocalDataProperty" PropertyPath="CustomerID"/>
                            <PropertyValue Property="ValueListProperty" String="CustomerID"/>
                        </Record>
                    </Collection>
                </PropertyValue>
            </Record>
        </Annotation>
    </Annotations>
    ```
## Step 5. Configuring the Visual Filters of type Bar Chart

In this step, Let us try to configure a visual filter with a different dimension using a bar chart

1. Expand the guide **Add a new visual filter** in the Analytical List Page group.

    ![](fiori/unit2/img_026.png)


2. Click **Start Guide**.

    ![](fiori/unit2/img_027.png)


3. **Visual Filter Guide(Step 1) - To configure a UI.Chart annotation** : 
   
    In the field **Entity Type**, choose your entity type as : **BookingType**. 

    ![](fiori/unit2/img_046.png)

    Enter the following values in the respective fields and click **Insert Snippet**

    | **Field**             |**Value**
    | --------------------- |----------------------------------
    | Chart Qualifier       |FilterFlightPriceAgencyID
    | Chart Title           |Filter FlightPrice by AgencyID
    | Chart Description     |Filter FlightPrice by AgencyID
    | ChartType             |Bar
    | Dimensions Property   |AgencyID
    | Measures property     |FlightPrice

    ![](fiori/unit2/img_047.png)

    The UI.Chart annotation is now added to the annotation file. Click **Step 2** .

    ![](fiori/unit2/img_048.png)


4. **Visual Filter Guide(Step 2) - To configure a UI.PresentationVariant annotation**
    
    Enter the following values and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |----------------------------------
    |Presentation Variant Qualifier   |PVFilterFlightPriceAgencyID
    |Chart Reference                  |\@UI.Chart\#FilterFlightPriceAgencyID

    ![](fiori/unit2/img_049.png)

    The UI.PresentationVariant annotation is now added to the annotation file, click **Step 3**

    ![](fiori/unit2/img_050.png)


5. **Visual Filter Guide(Step 3) - To configure a ValueList annotation** : 

    Enter the following values in the respective fields and click **Insert Snippet**.

    |**Field**                        |**Value**
    |-------------------------------- |------------------------------------
    |Entity Type                      |BookingType
    |Entity Type Property             |AgencyID
    |Collection Path                  |Booking
    |Presentation Variant Qualifier   |PVFilterFlightPriceAgencyID
    |Local Data Property              |AgencyID
    |Value List Property              |AgencyID

    ![](fiori/unit2/img_051.png)

    The Common.ValueList annotation is now added to the annotation file. <br>

    Click **Exit Guide** to get back to the Guides.

    ![](fiori/unit2/img_052.png) 

     Now we can see the UI.Chart and UI.PresentationVariant and common.valuelist annotation is added to your local annotation file as configured in the code snippet below.

    ```
    <Annotation Term="UI.Chart" Qualifier="FilterFlightPriceAgencyID">
            <Record Type="UI.ChartDefinitionType">
                <PropertyValue Property="Title" String="Filter FlightPrice By AgencyID"/>
                <PropertyValue Property="Description" String="Filter FlightPrice By AgencyID"/>
                <PropertyValue Property="ChartType" EnumMember="UI.ChartType/Bar"/>
                <PropertyValue Property="Dimensions">
                    <Collection>
                        <PropertyPath>AgencyID</PropertyPath>
                    </Collection>
                </PropertyValue>
                <PropertyValue Property="Measures">
                    <Collection>
                        <PropertyPath>FlightPrice</PropertyPath>
                    </Collection>
                </PropertyValue>
            </Record>
        </Annotation>
        <Annotation Term="UI.PresentationVariant" Qualifier="PVFilterFlightPriceAgencyID">
            <Record Type="UI.PresentationVariantType">
                <PropertyValue Property="Visualizations">
                    <Collection>
                        <AnnotationPath>@UI.Chart#FilterFlightPriceAgencyID</AnnotationPath>
                    </Collection>
                </PropertyValue>
            </Record>
        </Annotation>
    ```
   ```
    <Annotations Target="cds_zfe_booking_analytics_######.BookingType/AgencyID">
        <Annotation Term="Common.ValueList">
            <Record Type="Common.ValueListType">
                <PropertyValue Property="CollectionPath" String="Booking"/>
                <PropertyValue Property="PresentationVariantQualifier" String="PVFilterFlightPriceAgencyID"/>
                <PropertyValue Property="Parameters">
                    <Collection>
                        <Record Type="Common.ValueListParameterInOut">
                            <PropertyValue Property="LocalDataProperty" PropertyPath="AgencyID"/>
                            <PropertyValue Property="ValueListProperty" String="AgencyID"/>
                        </Record>
                    </Collection>
                </PropertyValue>
            </Record>
        </Annotation>
    </Annotations>
    ```

## Step 6. Preview the application to view the visual filters 

1. Right click **webapp folder**.

    ![](fiori/unit2/img_019.png)


2. Click on **Preview Application**.

    ![](fiori/unit2/img_020.png)


3. Click on **start fiori run --open 'test/flpSandbox.html#bookinganalysis-tile'**.

    ![](fiori/unit2/img_021.png)


4. To view the application in a new browser tab click **Open** in the dialog.

    ![](fiori/unit2/img_022.png)


5. To view the chart and table click : **Go**.

    ![](fiori/unit2/img_059.png)

    ![](fiori/unit2/img_060.png)


## Step 7. Add a default value to a filter field 
In this step, let us try to add a default value to a filter field. This value would be applied automatically to a field when the app is loaded. 

This can be achieved using the annotation : Common.FilterDefaultValue

Lets add a default value **2021** to the field **Booking Date Year**. The annotation is added to the value list annotation section.

1. In the property annotation section of the BookingDateYear, add the following annotation
    ```
    <Annotation Term="Common.FilterDefaultValue" String="2021"/>
    ```
    ![](fiori/unit2/img_053.png)
    
    ![](fiori/unit2/img_054.png)

## Step 8. Preview the application to view the default value added

1. Right click **webapp folder**.

    ![](fiori/unit2/img_019.png)


2. Click on **Preview Application**.

    ![](fiori/unit2/img_020.png)


3. Click on **start fiori run --open 'test/flpSandbox.html#bookinganalysis-tile'**.

    ![](fiori/unit2/img_021.png)


4. To view the application in a new browser tab click **Open** in the dialog. <br>

    You could now see a default filter is applied to the **BookingDateYear** field.

    ![](fiori/unit2/img_057.png)


5. To view the compact filters click  ![](fiori/unit2/fieldicon_20.png) (filter toogle for compact filters).

    ![](fiori/unit2/img_058.png)


## Summary
You have completed the exercise!

In this unit, you have learned how to 
- enhance the SAP Fiori application based on the Analytical List Page floor plan with filtering capabilities. 
- configure the compact and visual filter
- to add a default value to a filter field by configuration.
