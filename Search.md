## 4. API for Search
If you want to search for a specific place or event, use this endpoint:

```GET /api/v3/events/search-extended```

You can pass your query string of keywords with the parameter ```"q"```

```GET /api/v3/events/search-extended?q=sagrada+familia```

```limit``` : numeric value to limit the number of results, default is 10

```sorting``` : you can sort the results by "popularity", "price" or "rating"

### 4.1 Filters
You can also filter the results using this list of parameters:

```language``` : events that are translated in one of the langauges passed as a comma separated array

```vertical``` : events that are associated with one of the specified verticals

```category``` : events that are associated with one of the specified categories

```topFeature``` : events that have one of these properties: "Top seller", "Must see" or "Best seller"

```country``` : events that are associated with one of the specified countries

```city``` : events that are associated with one of the specified cities

```minPrice``` : events that have a default price higher than the numeric value passed

```maxPrice``` : events that have a default price lower than the numeric value passed
