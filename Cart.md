## Cart

### Create the Cart

**NOTE**
- All calls to carts endpoints return a _Cart_, see `GET /api/v3/carts/{id}` for details.

Creating a new _Cart_ is as easy as sending a `POST` to the `/api/v3/carts` endpoint. The information needed to create a cart is divided in two groups: those related to the _Tickets_ and those related to the _Customer_.

```
POST /api/v3/carts

{
  "tickets":[
   {
     "product": {
      "id": 8216618
     },
     "quantity":2,
     "metadata":{"language":"es"}
   }
  ]
}
```

Of course you can also add more items with a single call

```
POST /api/v3/carts

{
 "tickets":[
  {
    "product": {
     "id": 8216618
    },
    "quantity": 2,
    "metadata":{"language":"es"}
  },
  {
    "product": {
      "id": 7372773
    },
    "quantity": 12
  }
 ]
}
```

#### Add info to a cart

Once a cart is created you can add information sending `PATCH` to the endpoint `/api/v3/carts/{id}`, where `{id}` is replaced with the integer you get from `id` field in response body.

_A call that add customer data to an existing cart_
```
PATCH /api/v3/carts/{id}

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

**NOTE**
- `email`, `firstname`, `lastname` and `country` are all mandatory.
- For the list of available countries [see](https://developers.musement.com/documentation#get--api-v3-countries.{_format})

#### Update/change info in a cart

To completely replace the data in the cart you must use `PUT` verbs and pass the full new payload.

```
PUT /api/v3/carts/{id}

{
    "tickets":[
      {
        "product": {
          "id": 8216618
        },
        "quantity":3,
        "metadata":{"language":"fr"}
      }     
    ],
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

> The main difference between PUT and PATCH is that with the first you will update the model of the cart with the body you send, with the PATCH you will update just the field you send in body. 

#### Retrive the cart

#### _Request_
```GET /api/v3/carts/{id}```

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

#### Extra info needed for cart

Once the cart has items and customer information, depending on the items you have in the cart, some extra info might be needed to complete the purchase.

To retrieve this info you need to call ```GET /api/v3/carts/{id}/form``` 

With this form you can retrieve all information needed for a successful booking, there are three big categories:

- Customer Data - Standard info related to the customer
- Extended Fields - Custom info related to the customer 
- Passenger Info - Info *for each* passanger

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
          "label": "Passport number"
        }
      },
      "type": "musement_user_customer_data",
      "required": true,
      "label": "Customer data"
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
      "label": "Extended fields"
    }
  }
}
```

Just required fields are needed for a successful booking. With a response like the previous one, you have to update your cart with this body:

```
{
  "firstname": "Guest136a",
  "lastname": "surname136a",
  "email": "guest136a@fakemail.com",
  "customer_data": 
  {
    "country": {
      "id": 123
    }
  },
  "extended_fields": {
    "phone_number": 123
  }
}
```
