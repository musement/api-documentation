### Version 3.1.0

 - `POST /orders` returns an `Order` DTO and not only the order id. 
 
 Now the response is like
 
```
{
  "id": "6",
  "date": "2016-06-20T11:06:09+0200",
  "status": "PENDING"
}
```

 - The endpoint ~POST /orders/{order_id}/no-payment` is deprecated in favour of `POST /api/v3/payments/no/payment`.
 
The payload is

```
{
  "order_id" : 123456
}
```

Please note that also the payload has changed. The field with the order id is now `order_id` instead of `orderId`
