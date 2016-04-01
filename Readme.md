# Musement API Documentation

Current latest version 3.0.0

Here you can find documentation to help you start integrating with Musement. Explore our events catalog step by step and easily set up the checkout flow into your app or website.

The Musement API gives you a set of endpoints to GET all the information about our products such as locations, opening hours, availability, prices, discounts and more. Also your application can create and manage carts, tickets and orders and make payments.

Clicking on the links below you can find all step by step instructions that you need

1. [API for Authentication] (https://github.com/musement/api-documentation/blob/master/Authentication.md)
2. [API for Catalog] (https://github.com/musement/api-documentation/blob/master/Catalog.md)
3. [API for Booking] (https://github.com/musement/api-documentation/blob/master/Booking.md)
4. API for Search - documentation in progress

### Swagger sandbox tool

[https://api.musement.com/documentation](https://api.musement.com/documentation)

### Endpoints

* *Production*: https://api.musement.com/api/v3
* *QA-testing*: https://api-qa.musement.com/api/v3

### Shared Headers

* Accept-Language
* X-Musement-Currency
* X-Musement-Version
 
Note: when you are creating a new project with Musement API set X-Musement-Version to the current latest version. You can check it at the top of this document. Once you have set the version you will remain with a stable data model. When a new version is released you can check the documentation and choose to upgrade your header or remain with your version. If you don't set the header it will call by default the latest version and it might change the data model without you knowing it. 
