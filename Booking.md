## 3. API for Booking

For a successful booking you have to: 

1. ```GET /events/{id}```
2. ```GET /events/{id}/dates```
3. ```GET /events/{id}/dates/{YYYY-MM-DD}```
4. ```POST /cart```
5. Create the order
6. Finalize the order with or without payment (depending on your contract)
7. Retrieve the voucher

### 3.1 GET Event

To navigate the catalog refer to [paragraph 2] (https://github.com/musement/api-documentation/blob/master/Catalog.md). 

### 3.2 GET Event Available Dates

**DOCs**: [http://api.musement.com/documentation#get--api-v3-events-{id}-dates.{_format}]
(http://api.musement.com/documentation#get--api-v3-events-{id}-dates.{_format})

You can specify:

* from_date
* to_date

otherwise you will get the current year. 

**Example Request:**

```
GET /api/v3/events/497/dates HTTP/1.1
Host: api.musement.com
Authorization: Bearer MWFkZWE5YWUyZTZiNzM3NzFjMzkwZmI3ZDgyM2E2ZWQ0YWFmNDQ1NTM4ZDM4Mzc0MDkyNzMyZWMzNWNkNjQzOA
```

**Example Response:**

```
[
  {
    "date": "2016-01-14"
  },
  {
    "date": "2016-01-15"
  }
]
```

### 3.3 Event Available Ticket on a Date

Request 

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

### 3.4 Cart APIs

Cart APIs are useful to check the amount of the order, add promotions and discounts and check ticket availability. Every time you add something to it we block the ticket and calculate the final price of the order.

#### 3.4.1 POST /carts - Create a new Cart

DOCs: http://api.musement.com/documentation#post--api-v3-carts.{_format}

Check Documentation for Request Payload. 

NB. The Product id is the SeatPrice id. 

#### 3.4.2 GET /cart/{id} - Retrive the cart
#### 3.4.3 PUT/PATCH /cart/{id} - To Update the cart

With this API you can update your cart, adding or removing tickets or populating it with customer info.
The main difference between PUT and PATCH is that with the first you will update the model of the cart with the body you send, with the PATCH you will update just the field you sent in body. 

For example - in the cart: 

```
{
  "id": 1,
  "tickets": [
    {
      "product": {
        "id": "4488624",
        "type": "musement",
        "title": "Torre Eiffel: biglietti salta-la-coda con accesso sommità",
        "datetime": "2015-08-27T17:30:00+0000",
        "tourist_category": "Prezzo intero",
        "activity_name": "Ingresso",
        "event": {
          "id": 2700,
          "title": "Torre Eiffel: biglietti salta-la-coda con accesso sommità"
        },
        "image": "https://static1.musement.com/cover/0001/19/skip-the-line-tickets-to-the-eiffel-tower-with-summit-acc..."
      },
      "code": "BLU27-08-17-92138",
      "quantity": 1,
      "metadata": {},
      "status": "PENDING",
      "vouchers": []
    }
  ],
  "retail_price": {
    "currency": "EUR",
    "value": 26.6,
    "formatted_value": "€ 26.60"
  }
}
```

If you PATCH it with this request:

```
PATCH /cart/1
BODY:
{
  "customer" : {
        "email": "guest136a@fakemail.com",
        "firstname": "Guest136a",
        "lastname": "surname136a",
        "country": {
          "id": 123
        }
  }
}
```

You will get the cart with the customer info ```GET /cart/1```:

```
{
  "id": 1,
  "tickets": [
    {
      "product": {
        "id": "4488624",
        "type": "musement",
        "title": "Torre Eiffel: biglietti salta-la-coda con accesso sommità",
        "datetime": "2015-08-27T17:30:00+0000",
        "tourist_category": "Prezzo intero",
        "activity_name": "Ingresso",
        "event": {
          "id": 2700,
          "title": "Torre Eiffel: biglietti salta-la-coda con accesso sommità"
        },
        "image": "https://static1.musement.com/cover/0001/19/skip-the-line-tickets-to-the-eiffel-tower-with-summit-acc..."
      },
      "code": "BLU27-08-17-92138",
      "quantity": 1,
      "metadata": {},
      "status": "PENDING",
      "vouchers": []
    },
    {
	    "email": "guest136a@fakemail.com",
	    "firstname": "Guest136a",
	    "lastname": "surname136a",
	    "country": {
	      "id": 123,
	      "name": "Nepal"
	    }
	}
  ],
  "retail_price": {
    "currency": "EUR",
    "value": 26.6,
    "formatted_value": "€ 26.60"
  }
}
```

Check, all original data is still there plus the new customer info.



#### 3.4.3 DELETE Cart - Delete the cart
#### 3.4.4 Customer Info needed for cart

Docs: http://api.musement.com/documentation#get--api-v3-carts-{id}-form.{_format}

With this form you can retrieve all information needed for a successful booking, there are three big categories:

* Customer Data
* Extended Fields
* Passenger Info

Customer Data and Extended Fields are related to the customer, Passenger info is related to each traveler.

Example:
Request: ```GET /api/v3/carts/{id}/form```
Response:

```
{
  "children": {
    "firstname": {
      "type": "text",
      "required": true,
      "label": "Name"
    },
    "lastname": {
      "type": "text",
      "required": true,
      "label": "Surname"
    },
    "email": {
      "children": {
        "email": {
          "type": "email",
          "required": true,
          "label": "Email Address"
        },
        "email_confirm": {
          "type": "email",
          "required": true,
          "label": "Confirm Email"
        }
      },
      "type": "repeated",
      "required": true,
      "label": "Email Address"
    },
    "customer_data": {
      "children": {
        "do_you_need_invoice": {
          "type": "checkbox",
          "required": false,
          "label": "Do you need an invoice?"
        },
        "birthdate": {
          "type": "date",
          "required": false,
          "label": "Date of birth"
        },
        "country": {
          "type": "entity",
          "required": true,
          "label": "Country"
        },
        "city": {
          "type": "text",
          "required": false,
          "label": "City"
        },
        "address": {
          "type": "text",
          "required": false,
          "label": "Address"
        },
        "zipcode": {
          "type": "text",
          "required": false,
          "label": "ZIP Code"
        },
        "mobile": {
          "type": "number",
          "required": false,
          "label": "Mobile #"
        },
        "id_number": {
          "type": "text",
          "required": false,
          "label": "text.profile.id_number"
        }
      },
      "type": "musement_user_customer_data",
      "required": true,
      "label": "text.profile.customer_data"
    },
    "extended_fields": {
      "children": {
        "2700": {
          "children": {
            "159": {
              "children": {
                "phone_number": {
                  "type": "text",
                  "required": true,
                  "label": "Phone number"
                }
              },
              "type": "form",
              "required": true,
              "label": "text.profile.159"
            }
          },
          "type": "form",
          "required": true,
          "label": "text.profile.2700"
        }
      },
      "type": "form",
      "required": true,
      "label": "text.profile.extended_fields"
    }
  }
}
```

Just required fields are needed for a successful booking. With a response like the previous one you have to Update your cart with this body:

```
{
        "firstname": "Guest136a",
        "lastname": "surname136a",
        "email": "guest136a@fakemail.com",
        "customer_data": 
        {
                "country": {
                  "id": 123,
                  "name": "Nepal"
                }
        },
        "extended_fields": {
                "phone_number": 123
        }
}
```


### 3.5 Create the order

When the cart is completed (**with customer info filled in**) and you want to finalize it (proceed with payments and check the availability one last time) 

```POST /api/v3/order```

### 3.6 Finalize the order


If the order amount is equal to 0 or you use your payment gateway, use the flow without payment (ask for activation). Otherwise follow the Payment flow in 4.4.2.


#### 3.6.1 Without Payment Flow

Request ```POST /api/v3/payments/no-payment```

#### 3.6.2 With Payment Flow

##### Braintree Payments

You have to integrate Braintree SDK

Note: in QA endpoint use credit card numbers contained in this link https://developers.braintreepayments.com/reference/general/testing/ruby#credit-card-numbers 

###### Step 1 
GET the token

Request ```POST /api/v3/payments/braintree/token```
Response ```{ "nonce": "eyJ2ZXJzaW9uIjoyLCJhdXRob3JpemF0aW9uRmluZ2VycHJpbnQiOiI3MDVjY2JlZThmZDRkNzI3MWQ2NTJjZDVjOTZjZDAyOTA5YWEzZDA4ZTJhN2ExNDVlNzc3MTI3MWYxMTUzMDIxfGNyZWF0ZWRfYXQ9MjAxNi0wMi0wM1QxMzowNzowOS43NjQ5NDQ2NjIrMDAwMFx1MDAyNm1lcmNoYW50X2lkPXE5MjdzdGt5cjRtZHc1MnhcdTAwMjZwdWJsaWNfa2V5PWNzMjJkMjJzbjc2ano4YzYiLCJjb25maWdVcmwiOiJodHRwczovL2FwaS5icmFpbnRyZWVnYXRld2F5LmNvbTo0NDMvbWVyY2hhbnRzL3E5MjdzdGt5cjRtZHc1MngvY2xpZW50X2FwaS92MS9jb25maWd1cmF0aW9uIiwiY2hhbGxlbmdlcyI6WyJjdnYiXSwiZW52aXJvbm1lbnQiOiJwcm9kdWN0aW9uIiwiY2xpZW50QXBpVXJsIjoiaHR0cHM6Ly9hcGkuYnJhaW50cmVlZ2F0ZXdheS5jb206NDQzL21lcmNoYW50cy9xOTI3c3RreXI0bWR3NTJ4L2NsaWVudF9hcGkiLCJhc3NldHNVcmwiOiJodHRwczovL2Fzc2V0cy5icmFpbnRyZWVnYXRld2F5LmNvbSIsImF1dGhVcmwiOiJodHRwczovL2F1dGgudmVubW8uY29tIiwiYW5hbHl0aWNzIjp7InVybCI6Imh0dHBzOi8vY2xpZW50LWFuYWx5dGljcy5icmFpbnRyZWVnYXRld2F5LmNvbSJ9LCJ0aHJlZURTZWN1cmVFbmFibGVkIjpmYWxzZSwicGF5cGFsRW5hYmxlZCI6dHJ1ZSwicGF5cGFsIjp7ImRpc3BsYXlOYW1lIjoiTXVzZW1lbnQiLCJjbGllbnRJZCI6IkFkcTdxSTJrS2NNTEFIQjRGRDRac1JxNC13VGp0eUxnckhRdllSTGMtQ1k0MGFmZlh2dUYwUHpLRUJSSVZfRktJeTVaNmJvREZpY1A2ZmIzIiwicHJpdmFjeVVybCI6Imh0dHBzOi8vd3d3Lm11c2VtZW50LmNvbS9lbi9wL3ByaXZhY3kiLCJ1c2VyQWdyZWVtZW50VXJsIjoiaHR0cHM6Ly93d3cubXVzZW1lbnQuY29tL2VuL3AvdGVybXMtY29uZGl0aW9ucyIsImJhc2VVcmwiOiJodHRwczovL2Fzc2V0cy5icmFpbnRyZWVnYXRld2F5LmNvbSIsImFzc2V0c1VybCI6Imh0dHBzOi8vY2hlY2tvdXQucGF5cGFsLmNvbSIsImRpcmVjdEJhc2VVcmwiOm51bGwsImFsbG93SHR0cCI6ZmFsc2UsImVudmlyb25tZW50Tm9OZXR3b3JrIjpmYWxzZSwiZW52aXJvbm1lbnQiOiJsaXZlIiwidW52ZXR0ZWRNZXJjaGFudCI6ZmFsc2UsImJyYWludHJlZUNsaWVudElkIjoiQVJLcllSRGgzQUdYRHpXN3NPXzNiU2txLVUxQzdIR191V05DLXo1N0xqWVNETlVPU2FPdElhOXE2VnBXIiwiYmlsbGluZ0FncmVlbWVudHNFbmFibGVkIjpmYWxzZSwibWVyY2hhbnRBY2NvdW50SWQiOiJtdXNlbWVudEVVUiIsImN1cnJlbmN5SXNvQ29kZSI6IkVVUiJ9LCJjb2luYmFzZUVuYWJsZWQiOmZhbHNlLCJtZXJjaGFudElkIjoicTkyN3N0a3lyNG1kdzUyeCIsInZlbm1vIjoib2ZmIiwiYXBwbGVQYXkiOnsic3RhdHVzIjoicHJvZHVjdGlvbiIsImNvdW50cnlDb2RlIjoiVVMiLCJjdXJyZW5jeUNvZGUiOiJFVVIiLCJtZXJjaGFudElkZW50aWZpZXIiOiJtZXJjaGFudC5jb20ubXVzZW1lbnQubXVzZW1lbnRwcm9kIiwic3VwcG9ydGVkTmV0d29ya3MiOlsidmlzYSIsIm1hc3RlcmNhcmQiXX18" }```

###### Step 2 

Using Braintree SDK, ask the user for payment information (credit card, paypal etc). For more details please visit: https://developers.braintreepayments.com/

###### Step 3 

After the user fills out the braintree payment form, ask us to process the payment and close the order 

Request ```POST /api/v3/payments/braintree/payment```

Request Body 
```
{
  "nonce" : "nonce_text",
  "orderId" : 123456
}
```

### 3.7 Retrieve the voucher

If you used the login 2.1.2 you can retrieve the voucher with the Request

```GET /customer/me/orders```

Otherwise we will send it to your email address.
