## Customer data

To read information about the user 

```
GET /api/v3/customers/me
Authorization: Bearer HereABearerObtainedUsingPasswordGrantType
```

the response contains all user info

```
{
  "email": "user_1234@gmail.com",
  "firstname": "Foo",
  "lastname": "Bar",
  "avatar": "http://www.gravatar.com/avatar/9b466218341f7a8e57e29f8841b06361?s=120&d=identicon&r=g",
  "country": {
    "id": 64,
    "name": "Germany",
    "iso_code": "DE"
  },
  "currency": "EUR",
  "zipcode": "22-222",
  "id_number": "11111",
  "mobile": "2421414",
  "city": "Brzesko"
}
```

##### Order data

```
GET /api/v3/customers/me/orders
Authorization: Bearer HereABearerObtainedUsingPasswordGrantType
```

the response contains all orders for the authenticated customer

```
[
  {
    "id": "120241",
    "date": "2016-05-02T14:27:43+0000",
    "status": "OK",
    "tickets": [
      {
        "id": 324506,
        "product": {
          "id": "22446056",
          "type": "musement",
          "title": "Eiffel Tower priority access tickets with summit option + host",
          "datetime": "2016-05-20T10:30:00+0000",
          "tourist_category": "adult",
          "activity_name": "2nd_floor_summit",
          "event": {
            "id": 5866,
            "title": "Eiffel Tower priority access tickets with summit option + host",
            "print_voucher": false,
            "meeting_point": "8-10 Avenue de la Bourdonnais, 75007 Paris, FranceDo not go directly to the Eiffel Tower. Meet your tour representative at 10 avenue de la Bourdonnais 75007.Your representative will be wearing a black jacket and holding a sign reading \"GUIDE Smart Tour Paris\". Please be sure to arrive at least 25 minutes before your booked timeEiffel Tower Priority Access Tickets Map Download",
            "open": false,
            "ticket_not_included": false,
            "exclusive": false,
            "daily": false,
            "cover_image_url": "https://static1.musement.com/cover/0001/33/thumb_32355_cover_event.jpeg?event",
            "latitude": 48.860012,
            "longitude": 2.296424
          },
          "image": "https://static1.musement.com/cover/0001/33/thumb_32355_cover_event.jpeg?event"
        },
        "code": "-13088",
        "quantity": 1,
        "metadata": {},
        "status": "OK",
        "vouchers": [
          "http://qual.musement.com/voucher/view/147921?pdf=1"
        ]
      }
    ]
  }
...
]
```

