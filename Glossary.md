# Musement Glossary

* **Category**: a collection of events with shared themes or characteristics.

* **Customer**: the person who makes the purchase. He may or may not be one of the passengers. In any cart there must be one and only one customer.

* **Event**: a tour, ticket, or general product to purchase.

* **Open ticket**: the event does not have an exact starting time. Musement events can have a specific date/time, e.g. a football match, or can be valid for a limited time span, e.g. an entrance ticket valid for the next 365 days. In the first case the property open_ticket would be "no", in the second case would be "yes". The API dates for open tickets will always return the last day of availability no matter the range of days you passed. This property is printed at the end of each "Single date" API request.

* **Passenger**: the person who will use the ticket. In any cart there can be multiple passengers or none. Not every product requires info about passengers.

* **Seat price**: this is a single product that can be added to the cart. The seat price identifies a specific time slot, with a specific ticket holder for a specific event.

* **Venue**: a collection of events for a particular building, attraction, or location.
