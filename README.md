# Real-time Airport-Schedules-API
Aviation Edge [Real-time Airport Schedules API](https://aviation-edge.com/flight-schedule-and-timetable-of-airlines-and-airports/) is a JSON REST API that provides live airport timetable data. It has global coverage with the exception of military and private airfields. We gather the data from multiple different sources such as our data partners and airlines and airports directly (when availabile). We collect the data, maintain it ourselves and present it to our clients in a real-time manner via an API with fast response rates.
The API is desiged to return one schedule at a time. This can be either the daparture or the arrival schedule of a specific airport. It is possible to filter the flights in the response based on airline, flight number, flight status and more.

### Documentation
You may find input parameters, output examples with explanations for each item, filter list, and more in the [documentation](https://aviation-edge.com/developers/).

### Example Fields of Use
- Websites, tools or apps on tracking flight status
- Tracking flight delays and cancellations
- Airport greeting and transportation services
- Flight time analysis
- Airport traffic analysis

### Request
For the departure schedule of a certain airport:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=departure`

For the arrival schedule of a certain airport:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=arrival`

### Filters
```
&iataCode=          (obligatory) The IATA code of the airport you'd like to request data from.

&type=              (obligatory) Flight type: departure or arrival


&status=            The status of the flight: landed, scheduled, cancelled, active, incident, diverted, redirected, unknown

&airline_name=      Name of the airline (Air France, American Airlines, Delta Air Lines, etc) (may need air%20france, american%20airlines, delta%20air%20lines)

&airline_iata=      IATA code of airline

&airline_icao=      ICAO code of airline 

&flight_num=        The flight number based on 1 to 4 digits, for example: 171

&flight_iata=       The flight iata number consisting of digits and letters, usually of the airline iata code. For example: AA171

&flight_icao=       The flight icao number consisting of digits and letters, usually the airline icao code. For example: AAL171


&codeshared=        If the flight is codeshared, this data will be included. If you don't want codeshared flights, you can input null
```

### Useful Code Examples

1.	Fetching data from the API (axios library used):

```
const axios = require('axios');

const API_KEY = 'YOUR_API_KEY_HERE'; // Replace this with your API key.
const API_ENDPOINT = `https://aviation-edge.com/v2/public/timetable?key=${API_KEY}&iataCode=JFK&type=departure`;

async function fetchDepartures() {
    try {
        const response = await axios.get(API_ENDPOINT);
        const departures = response.data;
        return departures;
    } catch (error) {
        console.error("Error fetching departures:", error);
        throw error;
    }
}

// Usage example:
fetchDepartures().then(departures => {
    console.log(departures);
}).catch(error => {
    console.error("Error:", error.message);
});
```

2.	Parsing the data

Get Departures by Airline:

```
function getDeparturesByAirline(departures, airlineName) {
    return departures.filter(departure => departure.airline.name === airlineName);
}

// Usage example:
// const departures = await fetchDepartures();
// const anaDepartures = getDeparturesByAirline(departures, 'ANA');
// console.log(anaDepartures);

Get Departures with Delays:

function getDelayedDepartures(departures) {
    return departures.filter(departure => departure.departure.delay !== null);
}

// Usage example:
// const departures = await fetchDepartures();
// const delayedDepartures = getDelayedDepartures(departures);
// console.log(delayedDepartures);
```

### Response
```
[
{
"airline":
{
"iataCode":"UA",
"icaoCode":"UAL",
"name":"United Airlines"
},
"arrival":
{
"actualRunway":null,
"actualTime":null,
"baggage":"7",
"delay":null,
"estimatedRunway":null,
"estimatedTime":"2022-11-03T10:02:00.000",
"gate":"107",
"iataCode":"EWR",
"icaoCode":"KEWR",
"scheduledTime":"2022-11-03T10:21:00.000",
"terminal":"C"
},
"codeshared":null,
"departure":
{
"actualRunway":"2022-11-03T06:12:00.000",
"actualTime":"2022-11-03T06:12:00.000",
"baggage":null,
"delay":"13",
"estimatedRunway":"2022-11-03T06:12:00.000",
"estimatedTime":"2022-11-03T06:10:00.000",
"gate":"34",
"iataCode":"IAH",
"icaoCode":"KIAH",
"scheduledTime":"2022-11-03T06:00:00.000",
"terminal":"C"
},
"flight":
{"iataNumber":"UA1268",
"icaoNumber":"UAL1268",
"number":"1268"
},
"status":"active",
"type":"departure"
},
…
]

```

### Access & Support
[Contact us](https://aviation-edge.com/contact/) via email for any questions or support requests.

[Get your API key](https://aviation-edge.com/premium-api/) in a minute and start testing the data right away. The API is provided through API subscriptions. All plans grant access to the Airport Schedules API and other APIs with a difference of the monthly API call limit. Choose the best plan for you and upgrade, downgrade or cancel your plan anytime without  commitments.

### License
The use of the API is subject to Aviation Edge [Terms and Conditions](https://aviation-edge.com/api-terms-of-service/).
