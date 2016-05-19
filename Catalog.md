## Catalog

The key part of the Musement catalogue is events. Events are organized by type using _Categories_ and _Verticals_ and by geographical location using _Cities_ and _Venues_.

To find events in the catalogue you can use the _Search_ or you can navigate to it by using some predefined collection.

![Catalog](http://musement.s3.amazonaws.com/documentation_images/catalog.png)

### Event endpoints

All these endpoints return events. All these endpoints support different parameters that allow you to further filter the events.

* ```GET /api/v3/verticals/{id}/events``` | All the events for a specific _Vertical_  | [Test it](http://developers.musement.com/documentation#get--api-v3-verticals-{id}-events.{_format})
* ```GET /api/v3/categories/{id}/events``` | All the events for a specific _Category_ | [Test it](http://developers.musement.com/documentation#get--api-v3-categories-{id}-events.{_format})
* ```GET /api/v3/venues/{id}/events``` | All the events for a specific _Venue_ | [Test it](http://developers.musement.com/documentation#get--api-v3-venues-{id}-events.{_format})
* ```GET /api/v3/cities/{id}/events``` | All the events for a specific _City_ | [Test it](http://developers.musement.com/documentation#get--api-v3-cities-{id}-events.{_format})
* ```GET /api/v3/cities/{id}/events/{today|tomorrow}``` | Return all events available in a specific city `today` or `tomorrow` | [Test it](http://developers.musement.com/documentation#get--api-v3-cities-{id}-events-{happening}.{_format})
* ```GET /api/v3/countries/{id}/events``` | All the events for a specific _Country_ | [Test it](http://developers.musement.com/documentation#get--api-v3-countries-{id}-events.{_format})

Full list: http://api.musement.com/documentation
