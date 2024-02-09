# Developing Transactional App Based on Managed Scenario

* [Anubhav Trainings Official Website](https://www.anubhavtrainings.com)
* [Anubhav Trainings Official Channel](https://www.youtube.com/@AnubhavOberoy)


# Developing Validations, Actions, Determinations and Feature Control

* [Exercise 1 - Implementing the Action createTravelByTemplate]
* [Exercise 2 - Modeling Static and Dynamic Feature Control]
* [Exercise 3 - Implementing Validations]
* [Exercise 4 - Implementing the Determination]


<a id="exercise-1"></a>
# Exercise 1 - Implementing the Action createTravelByTemplate

This action provides a template for creating a new travel based on an already existing travel instance.

Actions are specified as non-standard operations in behavior definitions by using the following syntax:

```
[implementation] managed;
define behavior for CDSEntity [alias AliasedEntityName]
implementation in class ABAP_ClASS_NAME [unique] 
...
{
 [static] [factory] [internal] action ActionName 
           [parameter {InputParameterEntity | $self)} ] 
           [result [cardinality] {OutputParameterEntity | $self}]; 
...      
}
```

1.	Make below changes to the behavior definition `ZITC_AO_I_FE_TRAVEL`, 
   - Change the class name in the first line to `zbp_itc_ao_i_fe_travel`
   - Add action `createTravelByTemplate` as defined below
   - Add behavior implementation class to the Booking behavior
         `implementation in class zcl_bp_booking_m_AO unique`
   - provide alias names for both travel and booking behavior as mentioned below
   - Add `field ( readonly ) travel_id;` to booking behavior

```
managed implementation in class zbp_itc_ao_i_fe_travel unique;

define behavior for ZITC_AO_I_FE_TRAVEL alias Travel
…
{
….
  factory action createTravelByTemplate [1];
…

}

```

2.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 

3. Now behavior Implementation class should be created to implement action createTravelByTemplate. In your ABAP project (or ABAP Cloud Project), select the relevant behavior definition node ZITC_AO_I_FE_TRAVEL in Project Explorer.

![OData](images/0Project.png)

4.	Open the context menu and choose `New Behavior Implementation` to launch the creation wizard.

![OData](images/1NBehImp.png)

5.	Give behavior implementation name `zbp_itc_ao_i_fe_travel`(Replace AO with your Initials)  as below and click on Finish.

![OData](images/2BehImpName.png)

6.	Select the `Global class` section to see the generated code.

![OData](images/3GlobalClass.png)

Compared to the standard ABAP class, the generated behavior pool (in our case `zbp_itc_ao_i_fe_travel`) provides you with an extension `FOR BEHAVIOR OF`.

The real substance of a behavior pool is in `Local Types` (there is currently no implementation yet). Here you can implement special local classes, namely handler classes for additional operations (such as actions), validations and determinations that are triggered at specific points in time within the interaction phase.

Note that these classes can be instantiated or invoked only by the ABAP runtime environment (virtual machine).

7.	Click the activation button or use the shortcut `Ctrl + F3` to activate the class.

8.	To Implement action `createTravelByTemplate` open behavior implementation class `zbp_itc_ao_i_fe_travel` `Local Types` as shown below and paste the below code.

```
CLASS lhc_travel DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS createtravelbytemplate FOR MODIFY
      IMPORTING keys FOR ACTION travel~createtravelbytemplate.


ENDCLASS.

CLASS lhc_travel IMPLEMENTATION.

  METHOD createTravelByTemplate.
    SELECT MAX( travel_id ) FROM zitc_atrav INTO @DATA(lv_travel_id). "#EC CI_NOWHERE

    READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
      ENTITY travel
         ALL FIELDS WITH CORRESPONDING #( keys )
         RESULT    DATA(lt_read_result)
         FAILED    failed
         REPORTED  reported.

    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    DATA lt_create TYPE TABLE FOR CREATE ZITC_AO_I_FE_TRAVEL\\travel.

    lt_create = VALUE #( FOR row IN  lt_read_result INDEX INTO idx

                             ( %cid = row-TravelUUID
                               TravelUUID     = cl_uuid_factory=>create_system_uuid(  )->create_uuid_x16(  )
                               travelid      = lv_travel_id + idx
                               agencyid      = row-agencyid
                               customerid    = row-customerid
                               begindate     = lv_today
                               enddate       = lv_today + 30
                               bookingfee    = row-bookingfee
                               totalprice    = row-totalprice
                               currencycode  = row-currencycode
                               description    = 'Created by Anubhav'
                               OverallStatus = 'O' ) ). " Open


    MODIFY ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
        ENTITY travel
           CREATE FIELDS (
                              TravelUUID
                              travelid
                              agencyid
                              customerid
                              begindate
                              enddate
                              bookingfee
                              totalprice
                              currencycode
                              description
                              OverallStatus )
           WITH lt_create
         MAPPED   mapped
         FAILED   DATA(failed_modify)
         REPORTED DATA(reported_modify).

    failed-travel   = CORRESPONDING #( BASE ( failed-travel )   failed_modify-travel   MAPPING TravelUUID = %cid ).
    reported-travel = CORRESPONDING #( BASE ( reported-travel ) reported_modify-travel MAPPING TravelUUID = %cid ).


*    READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
*      ENTITY travel
*        ALL FIELDS WITH
*        CORRESPONDING #( mapped-travel )
*    RESULT DATA(lt_read_created).
*
*    mapped-travel = VALUE #( FOR key IN  mapped-travel  INDEX INTO idx
*                               ( %cid_ref = keys[ KEY entity %key = key-%cid ]-%cid_ref
*                                 %key     = key-%cid
*                                 %param-%tky   = key-%tky ) ) .
*
*    result = CORRESPONDING #( result FROM lt_read_created USING KEY entity  %key = %param-%key MAPPING %param = %data EXCEPT * ).



  ENDMETHOD.


ENDCLASS.
```

Replace AO with your initials.

9.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 
10.	To enable action in UI, add action using below syntax in Travel Processor behavior Definition `ZITC_AO_C_FE_TRAVEL`.

```
projection;

define behavior for ZITC_AO_C_FE_TRAVEL //alias <alias_name>
{
….
use action createTravelByTemplate;
…
}
```

11.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition.
12.	To enable action for UI consumption following UI annotation needs to be exposed to the consumer in Travel Processor Projection CDS View `ZITC_AO_C_FE_TRAVEL`.

```
@UI: {
          lineItem:       […..
                            { type: #FOR_ACTION, dataAction: 'createTravelByTemplate', label: 'Create Travel by Template' } ]
         …. }
```
This is already done in our case.

13.	Click the `preview` button by selecting the Entity type or double click the entity in the service binding to check how the UI of a Fiori application looks like or refresh the browser where Fiori app is displayed.

![OData](images/1Preview.png)

14.	The UI screen provides the button `Create Travel by Template` for the action as shown in the figure below.

![OData](images/2ButtonCreateTravel.png)

15.	Click on `Go` button and select any travel item and click on button `Create Travel by Template` button to copy the travel instance.

![OData](images/3Go.png)

16.	In change mode (after the user clicks the `Edit` button on Fiori UI’s object page), the end user can change the relevant travel fields as shown in the figure below.

![OData](images/4Edit.png)

![OData](images/5EditandSave.png)

17.	As soon as the user chooses the `Save` button on the object page, the data is persisted in the corresponding database table and a travel instance with a new `Travel ID` is created.

![OData](images/6Display.png)


# Exercise 2 - Modeling Static and Dynamic Feature Control

As an application developer you may want to determine, which entities of your business object should be create-, delete- and update-enabled, so that they can be modified during consumption using EML or OData services. In addition, you also may want control which (UI) fields of an entity are read-only or which actions in which usage scenarios are enabled or disabled for execution by the end users.

In ABAP RESTful programming model, the feature control is precisely the means of accomplishing such tasks. It allows you to control the visibility and changeability of fields, operations or entire entities.

Depending on whether feature control refers to specific instances or is independent of each entity instance, we distinguish between `instance-bound` and `static feature control`.

The availability of feature control values is modelled in a behavior definition. Unlike static, instance-bound feature control requires not only a definition but also an implementation in a handler class of the behavior pool. Therefore, we also talk about `dynamic feature control` in case of instance-bound feature control.

Both, static and dynamic feature control is defined for different levels (entity, field, or action level) in the behavior definition by using the following syntax:

```
[implementation] managed;
define behavior for CDSEntity [alias AliasName]
implementation in class ABAP_ClASS [unique] 
...
{
/* (1) Feature control at entity level */
/* Static operation control*/
  internal create
  internal update
  internal delete

/* or (instance-based) dynamic operation control: implementation required! */
  create (features: instance);
  update (features: instance);
  delete (features: instance);

/* (2) Feature control at field level */
/* Static field control */
  field (read only | mandatory) f1[, f2, ..., fn];

/* or dynamic field control: implementation required! */
  field (features: instance) f1[, f2, ..., fn];

/* (3) Feature control for actions */
/* Static action control */
  internal action ActionName [...]

/* or dynamic action control: implementation required! */
  action ( features: instance ) ActionName [... ]      
}
```

1.	Add below feature control to Behavior Definition `ZITC_AO_I_FE_TRAVEL`. Also add `implementation in class zcl_bp_booking_m_AO unique` after define behavior for Booking `define behavior for ZITC_AO_C_FE_BOOKING alias booking`.
```
managed implementation in class zbp_itc_ao_i_fe_travel unique;

define behavior for ZITC_AO_I_FE_TRAVEL alias Travel
persistent table /DMO/TRAVEL
lock master
//authorization master ( instance )
//etag <field_name>
{
  ….
  
  // administrative fields: read only
  field ( readonly ) lastchangedat, lastchangedby, createdat, createdby;

  // mandatory fields that are required to create a travel
  field ( mandatory ) agencyid, OverallStatus, bookingfee, currencycode;

  // dynamic field control
  field (features : instance ) BookingFee;
  
…
  
}

For the travel entity, we define all admin fields as read-only, whereas the fields that are required for creating a travel instance are defined as mandatory.

The field `bookingFee` plays a special role and is intended for dynamic field control. In the corresponding implementation, we will not allow editing of the booking fee if agency code is 70041

```
CLASS lhc_travel DEFINITION INHERITING FROM cl_abap_behavior_handler.

….
  METHODS get_instance_features FOR INSTANCE FEATURES
        IMPORTING keys REQUEST requested_features FOR travel RESULT result.


…


ENDCLASS.

CLASS lhc_travel IMPLEMENTATION.


    ….
METHOD get_instance_features.

    READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
        ENTITY Travel
        FIELDS ( AgencyID )
        WITH CORRESPONDING #( keys )
        RESULT DATA(lt_travel_result)
        FAILED failed.

        result = VALUE #( FOR ls_travel IN lt_travel_result
                           (         %tky                   = ls_travel-%tky
                             %field-BookingFee               = COND #( WHEN ls_travel-AgencyID = 70041
                                                                        THEN if_abap_behv=>fc-f-read_only
                                                                         ELSE if_abap_behv=>fc-f-unrestricted  )


                          ) ).

  ENDMETHOD.
…

ENDCLASS.

```

Replace AO with your initials 

4.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition.

# Exercise 3 - Implementing Validations

A validation is an implicitly executed function intended to check the data consistency of an existing instance of an entity (consistency validation). It is implicitly invoked by the business object’s framework as soon as a trigger condition at a predefined point in time is fulfilled. Validation can return messages to the consumer and reject inconsistent instance data from being saved.

Validations never modify any instance data but return the messages and keys of failed (inconsistent) entity instances.

Validations are specified for individual business object’s entities in the behavior definition by using the following syntax:

```
[implementation] managed;
define behavior for CDSEntity [alias AliasedName]
implementation in class ABAP_ClASS [unique] 
...
{
  validation ValidationName on save { field f1[, f2, ..., fn]; }
...      
}
```

**Example**

A validation is implemented to check if the customer ID referenced in the Travel order is valid. This validation is assigned to the "Travel" entity having defined update as the trigger operation on entity "Travel". As soon as a customer ID is updated by the consumer, the validation will check it and return a warning message if the ID is unknown.

1.	Add below validations to Behavior Definition `ZITC_AO_I_FE_TRAVEL`.

```
managed implementation in class zbp_itc_ao_i_fe_travel unique;

define behavior for ZITC_AO_I_FE_TRAVEL alias Travel
persistent table /DMO/TRAVEL
lock master
//authorization master ( instance )
//etag <field_name>
{
  ….
  
  validation validateCustomer on save { field customer_id; create; }
…
  
}

```

2.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 
3.	To Implement valdiation for travel open behavior implementation class `zbp_itc_ao_i_fe_travel` `Local Types` as shown below and paste the below code.

```
CLASS lhc_travel DEFINITION INHERITING FROM cl_abap_behavior_handler.

….
 METHODS validateCustomer FOR VALIDATE ON SAVE
      IMPORTING keys FOR Travel~validateCustomer.

…


ENDCLASS.

CLASS lhc_travel IMPLEMENTATION.


    ….
 METHOD validateCustomer.

    READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
        ENTITY Travel
          FIELDS ( customerid )
          WITH CORRESPONDING #( keys )
        RESULT DATA(lt_travel)
        FAILED DATA(lt_failed).

    failed =  CORRESPONDING #( DEEP lt_failed  ).

    DATA lt_customer TYPE SORTED TABLE OF /dmo/customer WITH UNIQUE KEY customer_id.

    " Optimization of DB select: extract distinct non-initial customer IDs
    lt_customer = CORRESPONDING #( lt_travel DISCARDING DUPLICATES MAPPING customer_id = customerid EXCEPT * ).
    DELETE lt_customer WHERE customer_id IS INITIAL.

    IF  lt_customer IS NOT INITIAL.
      " Check if customer ID exists
      SELECT FROM /dmo/customer FIELDS customer_id
                                FOR ALL ENTRIES IN @lt_customer
                                WHERE customer_id = @lt_customer-customer_id
      INTO TABLE @DATA(lt_customer_db).
    ENDIF.

    " Raise message for non existing customer id
    LOOP AT lt_travel INTO DATA(ls_travel).

      APPEND VALUE #(  %tky                 = ls_travel-%tky
                       %state_area          = 'VALIDATE_CUSTOMER' ) TO reported-travel.

      IF ls_travel-customerid IS  INITIAL.
        APPEND VALUE #( %tky = ls_travel-%tky ) TO failed-travel.

        APPEND VALUE #( %tky                = ls_travel-%tky
                        %state_area         = 'VALIDATE_CUSTOMER'
                        %msg                = NEW /dmo/cm_flight_messages(
                                                                textid = /dmo/cm_flight_messages=>enter_customer_id
                                                                severity = if_abap_behv_message=>severity-error )
                        %element-customerid = if_abap_behv=>mk-on ) TO reported-travel.

      ELSEIF ls_travel-customerid IS NOT INITIAL AND NOT line_exists( lt_customer_db[ customer_id = ls_travel-customerid ] ).
        APPEND VALUE #(  %tky = ls_travel-%tky ) TO failed-travel.

        APPEND VALUE #(  %tky                = ls_travel-%tky
                         %state_area         = 'VALIDATE_CUSTOMER'
                         %msg                = NEW /dmo/cm_flight_messages(
                                                                customer_id = ls_travel-customerid
                                                                textid = /dmo/cm_flight_messages=>customer_unkown
                                                                severity = if_abap_behv_message=>severity-error )
                         %element-customerid = if_abap_behv=>mk-on ) TO reported-travel.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.

…

ENDCLASS.
```

Replace AO with your initials.

4.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 
5.	Click the `preview` button by selecting the Entity type or double click the entity in the service binding to check how the UI of a Fiori application looks like or refresh the browser where Fiori app is displayed.

![OData](images/1Preview.png)

6.	If the user enters an invalid number as the `Customer ID`, the validation is initiated at the `Save` time and, as a result, which rejects saving of the instance data and returns a corresponding message to the user.

![OData](images/2EditCustomerID.png)

![OData](images/3ErrorCustomerID.png)


<a id="exercise-6"></a>
# Exercise 4 - Implementing the Determination

A determination is an implicitly executed function that handles side effects of modified entity instances. It is invoked by the business object’s framework as soon as a determination’s trigger condition at a predefined point in time, the determination time, is fulfilled.

Determinations are triggered internally based on changes made to the entity instance of a business object. The trigger conditions are checked by business object framework at different points during the transaction cycle, depending on the determination time and the changing operations on the relevant entity instances. For each determination, it is necessary to specify both the determination time and the changes that form the trigger condition. A trigger condition consists of a list of fields belonging to the same entity the determination is assigned to and the changing operations that include creating or updating entity instances. We call this operations trigger operations.

In case a field is changed (after creation or update), the condition is fulfilled. The framework evaluates the triggering condition of all determinations at certain points in time (determination time). For determinations, this is today either after each modification or during the save phase.

You can use a determination primarily to compute data that is derived from the values of other fields. The determined fields and the determining fields either belong to the same entity or to different entities of a business object.

As a result, determination can modify entity instances and return transition messages (error, warning, information, success).

Determinations are specified for individual business object’s entities in the behavior definition by using the following syntax:

```
[implementation] managed;
define behavior for CDSEntity [alias AliasedName]
implementation in class ABAP_ClASS [unique] 
...
{
  // (1) determination for triggers at field level 
  determination DeterminationName on (save | modify) { field f1[, f2, ..., fn] ; }

  // (2) determination for triggers at entity level 
  determination DeterminationName on (save | modify) { create; }
...      
}

```

1.	In the behavior definition `ZITC_AO_I_FE_TRAVEL`, the determination on the booking entity is defined as follows:

```
…
define behavior for ZITC_AO_C_FE_BOOKING alias booking
implementation in class zcl_bp_booking_m_AO unique
persistent table /dmo/booking
lock dependent ( travel_id = travel_id )
//authorization dependent( <local_field_name> = <target_field_name> )
//etag <field_name>
{
  
  …
  determination calculateTotalFlightPrice on modify { field flight_price, currency_code; create; }
…
}
```

The determination `calculateTotalFlightPrice` on the booking entity is intended to handle the calculation of total price of all flight bookings that belong to the selected travel. The determination will be triggered by on modify as determination time when creating new booking instances or updating the flight price value or when changing the currency. In other words: both fields `flight_price` and `currency_code` serve as trigger fields and form together with create and update operations the trigger condition for the determination.

2.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 

3.	To Implement determination for booking open behavior implementation class `zcl_bp_booking_m_AO` `Local Types` as shown below and paste the below code.

```
CLASS lhc_booking DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

….
    METHODS calculate_total_flight_price FOR DETERMINATION booking~calculateTotalFlightPrice
      IMPORTING keys FOR booking.

ENDCLASS.

CLASS lhc_booking IMPLEMENTATION.


  ….

  METHOD calculate_total_flight_price.



 TYPES: BEGIN OF ty_amount_per_currencycode,
             amount        TYPE /dmo/total_price,
             currency_code TYPE /dmo/currency_code,
           END OF ty_amount_per_currencycode.

    DATA: amount_per_currencycode TYPE STANDARD TABLE OF ty_amount_per_currencycode.


    " Read all relevant travel instances.
    READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
         ENTITY Travel
            FIELDS ( booking_fee currency_code )
            WITH CORRESPONDING #( keys )
         RESULT DATA(lt_travel).


    DELETE lt_travel WHERE currency_code IS INITIAL.

    LOOP AT lt_travel ASSIGNING FIELD-SYMBOL(<fs_travel>).
      " Set the start for the calculation by adding the booking fee.
      amount_per_currencycode = VALUE #( ( amount        = <fs_travel>-booking_fee
                                           currency_code = <fs_travel>-currency_code ) ).

      " Read all associated bookings and add them to the total price.
      READ ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
        ENTITY Travel BY \_Booking
          FIELDS ( flight_price currency_code )
        WITH VALUE #( ( %tky = <fs_travel>-%tky ) )
        RESULT DATA(lt_booking).

      LOOP AT lt_booking INTO DATA(booking) WHERE currency_code IS NOT INITIAL.
        COLLECT VALUE ty_amount_per_currencycode( amount        = booking-flight_price
                                                  currency_code = booking-currency_code ) INTO amount_per_currencycode.
      ENDLOOP.



      CLEAR <fs_travel>-total_price.
      LOOP AT amount_per_currencycode INTO DATA(single_amount_per_currencycode).
        " If needed do a Currency Conversion
        IF single_amount_per_currencycode-currency_code = <fs_travel>-currency_code.
          <fs_travel>-total_price += single_amount_per_currencycode-amount.
        ELSE.
          TRY  .
              /dmo/cl_flight_amdp=>convert_currency(
                 EXPORTING
                   iv_amount                   =  single_amount_per_currencycode-amount
                   iv_currency_code_source     =  single_amount_per_currencycode-currency_code
                   iv_currency_code_target     =  <fs_travel>-currency_code
                   iv_exchange_rate_date       =  cl_abap_context_info=>get_system_date( )
                 IMPORTING
                   ev_amount                   = DATA(total_booking_price_per_curr)
                ).
            CATCH cx_amdp_execution_failed.

          ENDTRY.
          <fs_travel>-total_price += total_booking_price_per_curr.
        ENDIF.
      ENDLOOP.
    ENDLOOP.

    " write back the modified total_price of travels
    MODIFY ENTITIES OF ZITC_AO_I_FE_TRAVEL IN LOCAL MODE
      ENTITY travel
        UPDATE FIELDS ( total_price )
        WITH CORRESPONDING #( lt_travel ).

  ENDMETHOD.

..

ENDCLASS.

```

Replace AO with your initials.

4.	Click the activation button or use the shortcut `Ctrl + F3` to activate the behavior definition. 

5.	Click the `preview` button by selecting the Entity type or double click the entity in the service binding to check how the UI of a Fiori application looks like or refresh the browser where Fiori app is displayed.

![OData](images/1Preview.png)

6.	The figure below refers to the starting point of viewing with a newly created travel instance with the initial amount (Total Price) and the travel currency 0.00 EUR.

![OData](images/2existingTravel.png)

7.	If a user adds a flight booking to the travel, then also the travel’s `Total Price` is updated.

![OData](images/3displaytravelBooking.png).

8.	If the user switches the booking’s object page to `edit` mode and then changes the Flight Price, then the Total Price is also updated at root level.

![OData](images/4EditBookingPrice.png)

![OData](images/5DisplayTotalPrice.png)
