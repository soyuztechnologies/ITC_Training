# Developing Transactional App Based on Managed Scenario

* [Anubhav Trainings Official Website](https://www.anubhavtrainings.com)
* [Anubhav Trainings Official Channel](https://www.youtube.com/@AnubhavOberoy)


# Developing Interface and Projection Layers for Analytics

* [Exercise 1 - Create Interface Analytics views]
* [Exercise 2 - Create Booking Projection View]

# Exercise 1 - Create Interface Analytics views

```
@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Travel Analytics'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZITC_AO_I_TRAVEL_ANALYTICS
  as select from zitc_atrav as Travel
  association [0..1] to /DMO/I_Agency   as _Agency   on $projection.AgencyID = _Agency.AgencyID
  association [0..1] to /DMO/I_Customer as _Customer on $projection.CustomerID = _Customer.CustomerID
{
  key travel_uuid           as TravelUUID,
      travel_id             as TravelID,
      agency_id             as AgencyID,
      customer_id           as CustomerID,
      begin_date            as BeginDate,
      end_date              as EndDate,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      booking_fee           as BookingFee,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      total_price           as TotalPrice,
      currency_code         as CurrencyCode,
      description           as Description,
      overall_status        as OverallStatus,
      @Semantics.user.createdBy: true
      created_by            as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      created_at            as CreatedAt,
      @Semantics.user.lastChangedBy: true
      last_changed_by       as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at       as LAST_CHANGED_AT,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_last_changed_at as LocalLastChangedAt,
      _Agency,
      _Customer
}
```


--

```
@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'booking analytics'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZITC_AO_I_BOOKING_ANALYTICS
  as select from zitc_abook as Booking
  association [0..1] to ZITC_AO_I_TRAVEL_ANALYTICS as _Travel     on  $projection.TravelUUID = _Travel.TravelUUID
  association [1..1] to ZITC_AO_I_FE_CONN          as _Connection on  $projection.CarrierID    = _Connection.AirlineID
                                                                  and $projection.ConnectionID = _Connection.ConnectionID
  association [1..1] to ZITC_AO_I_FE_CARR          as _Carrier    on  $projection.CarrierID = _Carrier.AirlineID
  association [0..1] to /DMO/I_Customer            as _Customer   on  $projection.CustomerID = _Customer.CustomerID
  association [0..1] to /DMO/I_Agency              as _Agency     on  $projection.AgencyID = _Agency.AgencyID
{
  key booking_uuid                 as BookingUUID,
      travel_uuid                  as TravelUUID,
      booking_id                   as BookingID,
      booking_date                 as BookingDate,
      substring (booking_date,1,4) as BookingDateYear,
      customer_id                  as CustomerID,
      _Customer.LastName           as CustomerName,
      carrier_id                   as CarrierID,
      _Carrier.Name                as CarrierName,
      connection_id                as ConnectionID,
      flight_date                  as FlightDate,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      flight_price                 as FlightPrice,
      currency_code                as CurrencyCode,
      _Travel.AgencyID             as AgencyID,
      _Travel._Agency.Name         as AgencyName,
      @Semantics.user.createdBy: true
      created_by                   as CreatedBy,
      @Semantics.user.lastChangedBy: true
      last_changed_by              as LastChangedBy,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_last_changed_at        as LocalLastChangedAt,
      _Travel,
      _Carrier,
      _Customer,
      _Connection

}
```

# Exercise 2 - Create Booking Projection View

```
@EndUserText.label: 'Booking Analytics'
@AccessControl.authorizationCheck: #NOT_REQUIRED
define root view entity ZITC_AO_C_BOOKING_ANALYTICS
  as projection on ZITC_AO_I_BOOKING_ANALYTICS
{
  key BookingUUID,
      TravelUUID,
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.9
      BookingID,
      BookingDate,
      @EndUserText.label: 'Booking Date (Year)'
      BookingDateYear,
      @EndUserText.label: 'Customer'
      @ObjectModel.text.element: [ 'CustomerName' ]
      CustomerID,
      CustomerName,
      @EndUserText.label: 'Airline'
      @ObjectModel.text.element: [ 'CarrierName' ]
      CarrierID,
      CarrierName,
      ConnectionID,
      FlightDate,
      @Semantics.amount.currencyCode: 'CurrencyCode'
      @Aggregation.default: #SUM
      FlightPrice,
      CurrencyCode,
      @EndUserText.label: 'Agency'
      @ObjectModel.text.element: [ 'AgencyName' ]
      AgencyID,
      AgencyName,
      CreatedBy,
      LastChangedBy,
      LocalLastChangedAt,
      _Travel,
      _Carrier,
      _Customer,
      _Connection

}
```

# Exercise 3: Create Service definition as below

```
@EndUserText.label: 'Service definition analytics'
define service ZITC_FE_BOOKING_XX {
  expose ZITC_AO_C_BOOKING_ANALYTICS as Booking;
  expose ZITC_AO_I_TRAVEL_ANALYTICS  as Travel;
  expose ZITC_AO_I_FE_CONN           as Connection;
  expose ZITC_AO_I_FE_FLIG           as Flight;
  expose ZITC_AO_I_FE_CARR           as Airline;
  expose I_Currency                  as Currency;
  expose /DMO/I_Customer             as Customer;
  expose /DMO/I_Agency               as Agency;
}
```