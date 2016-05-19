## Booking

The booking process allows you to book an event. Once you have found the _Event_ you want to book, using the search or browsing the catalogues, for a successful booking you have to follow this flow.

We assume you already retrieved the all the infomation for the _Event_ using the call:

```
GET /api/v3/events/{id}
```

### Booking flow

![Booking](http://musement.s3.amazonaws.com/documentation_images/booking.png)

1. Search for the available dates for the _Event_. | [Test it](https://developers.musement.com/documentation#get--api-v3-events-{id}-dates.{_format})

Returns a list of all days with availabitiy for a specific _Event_ over a period of time.

#### _Request_
```
GET /api/v3/events/{id}/dates
```

To specify the period you can use the parameters `from_date` and `end_date`. If none of these parameters are specified `from_date` is set to the current day. If `end_date` is not specified then `end_date` is set to the same as `from_date`.

**Examples:**

*** No parameters *** 

`from_date` and `end_date` are set as the current day. This call returns the current date if there is availabitiy, an empty array otherwise.

```
GET /api/v3/events/497/dates
```

*** With parameters *** 

Search for available dates for December 2016

```
GET /api/v3/events/497/dates?from_date=2016-12-01&end_date=2016-12-31
```

**Example Response:**

The response is a collection of dates

```
[
  {
    "date": "2016-01-14"
  },
  {
    "date": "2016-01-25"
  }
]
```

### 3 Find tickets available for selected day

Once you have found the date, the next step is to search for all available tickets for that day. Please note that for the same event different days can have different types of tickets.

#### _Request_
```GET /events/{id}/dates/{YYYY-MM-DD}``` | [Test it]

The response is a collection of items containing : 

 - `datetime` - The date and time. For a single day an event may have multiple starting times.
 - `seats` - A collection of all available `seats` (AKA `tickets`). Here you can find the seat structure.
 - `open_ticket` - If true the event is an `Open ticket`
 - `availability` - Number of place available
 - `languages` - The event may be available in different languages. Different times may also have different languages

`seat` structure:

 - `id` - Unique seat identifier
 - `price_tag` - Info about the `seat` (person) the ticket is for: ticket type (`name`) and type of person (`group`). Please note that for the same date and time the combination of `name` and `group`  is unique but you can have multiple seats with the same `group` or `name`
 - `max_buy` and `min_buy` - Maximum and minimum number of ticket purchasable in a single transaction
 - `raw_price` and `retail_price` - Internal and public price

#### _Example_

```GET /api/v3/events/497/dates/2016-01-15```

Response

``` 
[
  {
    "datetime": "2016-01-14T13:45:00+0000",
    "seats": [
      {
        "id": 8216618,
        "price_tag": {
          "name": "Full price",
          "group": "Tour",
          "locale": "en"
        },
        "max_buy": 15,
        "min_buy": 1,
        "raw_price": {
          "currency": "EUR",
          "value": 40,
          "formatted_value": "€ 40.00"
        },
        "retail_price": {
          "currency": "EUR",
          "value": 40,
          "formatted_value": "€ 40.00"
        }
      }
    ],
    "open_ticket": false,
    "availability": 9,
    "languages": [
      "en"
    ]
  }
]
```

### Create the order

When the cart is completed (**with customer info filled in**) and you want to finalize it (proceed with payments and check the availability one last time) 

```
POST /api/v3/orders

{
    "cart_id": 123456
}
```

[(See api docs for more details)](https://thack.musement.com/documentation#post--api-v3-orders.{_format})

You'll get the order id as response. Once you have the order id you can pay your order. If the order amount is equal to 0 or you use your payment gateway, use the flow without payment (ask for activation). Otherwise follow the _Braintree Payments flow_

#### Without Payment Flow

```
POST /api/v3/payments/no-payment

{
    "orderId": 123456
}
```
[(See api docs for more details)](https://thack.musement.com/documentation#post--api-v3-orders-{orderId}-no-payment.{_format})


#### Braintree Payments flow

You have to integrate [Braintree SDK](https://developers.braintreepayments.com/)

**Note** 
When testing use credit card numbers contained in this link https://developers.braintreepayments.com/reference/general/testing/php#credit-card-numbers 

###### Step 1

Get [client token](https://developers.braintreepayments.com/reference/request/client-token/generate/php) :

```
POST /api/v3/payments/braintree/token
```

Response 
```
{
  "token": "eyJ2ZXJzaW9uIjoyLCJhdXRob3JpemF0aW9uRmluZ2VycHJpbnQiOiI3MDVjY2JlZThmZDRkNzI3MW" 
}
```
[(See api docs for more details)](https://thack.musement.com/documentation#post--api-v3-payments-braintree-token.{_format})


###### Step 2 

Using Braintree SDK, ask the user for payment information (credit card, paypal etc) and generate "payment method nonce". Look at the [Braintree documentation for custom integrations](https://developers.braintreepayments.com/reference/client-reference/javascript/v2/credit-cards) or [Drop-in Payment UI](https://developers.braintreepayments.com/guides/drop-in/javascript/v2) for more details.

###### Step 3 

After the user fills out the Braintree payment form and you have "payment method nonce", ask us to process the payment and close the order by sending a 

```
POST /api/v3/payments/braintree/payment

{
  "nonce" : "nonce_string",
  "orderId" : 123456
}
```
[(See api docs for more details)](https://thack.musement.com/documentation#post--api-v3-payments-braintree-payment.{_format})