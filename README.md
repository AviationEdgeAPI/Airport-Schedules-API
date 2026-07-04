# Real-time-Airport-Schedules-API

[Real-time Airport Schedules API](https://aviation-edge.com/flight-schedule-and-timetable-of-airlines-and-airports/) is a JSON REST API that provides live airport timetable data for airport departures and arrivals worldwide. The API is designed for developers who need real-time airport schedule data, live flight status information, airport arrival data, airport departure data, delay information, terminal and gate details, airline information, flight numbers, codeshare details, and aircraft data through a simple API request.

The API is designed to return one airport schedule at a time. This can be either the departure schedule or the arrival schedule of a specific airport. It is possible to filter flights in the response based on airline, flight number, flight status, codeshare information, and more.

### Documentation

You may find input parameters, output examples with explanations for each item, filter list, and more in the [documentation](https://aviation-edge.com/developers/#schedulesa).

### Example Fields of Use

* Websites, tools, or apps that display live airport departure and arrival schedules
* Flight tracking platforms using real-time airport timetable data
* Travel, airport greeting, and transportation services tracking flight arrivals
* Tools monitoring flight delays, cancellations, diversions, and active flights
* Airport traffic analysis based on live departure and arrival data
* Aviation data platforms that need structured real-time flight schedule data
* Internal dashboards for airport, airline, or operational monitoring

### Request

For the departure schedule of a certain airport:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=departure`

For the arrival schedule of a certain airport:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=arrival`

For the departure schedule of a certain airport filtered by airline IATA code:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=departure&airline_iata=AA`

For the arrival schedule of a certain airport filtered by flight status:

**GET** `https://aviation-edge.com/v2/public/timetable?key=[API_KEY]&iataCode=JFK&type=arrival&status=landed`

### Filters

```
&iataCode=          (mandatory) The IATA code of the airport you'd like to request data from.

&type=              (mandatory) Flight type: departure or arrival


&status=            The status of the flight: scheduled, active, landed, cancelled, unknown, diverted

&airline_name=      Name of the airline, for example Air France, American Airlines, Delta Air Lines
                    Airline names may need URL encoding, for example air%20france or american%20airlines

&airline_iata=      IATA code of the airline

&airline_icao=      ICAO code of the airline

&flight_num=        The flight number based on 1 to 4 digits, for example: 171

&flight_iata=       The flight IATA number consisting of digits and letters, usually including the airline IATA code.
                    For example: AA171

&flight_icao=       The flight ICAO number consisting of digits and letters, usually including the airline ICAO code.
                    For example: AAL171


&codeshared=        If the flight is codeshared, this data will be included.
                    If you don't want codeshared flights, you can input null.
```

### Useful Code Examples

1. Fetching data from the API with JavaScript using the Axios library

```
const axios = require("axios");

const API_KEY = "YOUR_API_KEY_HERE";
const API_ENDPOINT = `https://aviation-edge.com/v2/public/timetable?key=${API_KEY}&iataCode=JFK&type=arrival`;

async function fetchAirportSchedule() {
    try {
        const response = await axios.get(API_ENDPOINT);
        return response.data;
    } catch (error) {
        console.error("Error fetching airport schedule:", error);
        return null;
    }
}

// Test fetching the data
fetchAirportSchedule().then(data => {
    if (data) {
        console.log("Received Airport Schedule Data:", data);
    }
});
```

2. Display selected flight details, including aircraft data

```
function displayFlightDetails(flight) {
    console.log("Flight Number:", flight.flight.iataNumber);
    console.log("Flight Status:", flight.status);
    console.log("Schedule Type:", flight.type);

    console.log("Airline:", flight.airline.name);
    console.log("Airline IATA:", flight.airline.iataCode);
    console.log("Airline ICAO:", flight.airline.icaoCode);

    if (flight.aircraft) {
        console.log("Aircraft ICAO Code:", flight.aircraft.icaoCode);
        console.log("Aircraft Registration:", flight.aircraft.regNumber);
        console.log("Aircraft ICAO24:", flight.aircraft.icao24);
    }

    console.log("Departure Airport:", flight.departure.iataCode);
    console.log("Departure Scheduled Time:", flight.departure.scheduledTime);
    console.log("Departure Actual Time:", flight.departure.actualTime);
    console.log("Departure Delay:", flight.departure.delay);

    console.log("Arrival Airport:", flight.arrival.iataCode);
    console.log("Arrival Scheduled Time:", flight.arrival.scheduledTime);
    console.log("Arrival Actual Time:", flight.arrival.actualTime);
    console.log("Arrival Delay:", flight.arrival.delay);

    console.log(
        "Codeshare Flight:",
        flight.codeshared ? flight.codeshared.flight.iataNumber : "No codeshare information available"
    );
}

// Test display function with the first flight from fetched data
fetchAirportSchedule().then(data => {
    if (data && data.length > 0) {
        displayFlightDetails(data[0]);
    }
});
```

3. Filter flights with current departure or arrival delays

```
function getFlightsWithDelays(data) {
    return data.filter(flight => {
        const departureDelay = flight.departure && flight.departure.delay !== null;
        const arrivalDelay = flight.arrival && flight.arrival.delay !== null;

        return departureDelay || arrivalDelay;
    });
}

// Test delay filter with the fetched data
fetchAirportSchedule().then(data => {
    if (data) {
        const delayedFlights = getFlightsWithDelays(data);
        console.log("Flights With Delays:", delayedFlights);
    }
});
```

4. Filter flights by status

```
function getFlightsByStatus(data, status) {
    return data.filter(flight => flight.status === status);
}

// Example statuses: scheduled, active, landed, cancelled, unknown, diverted
fetchAirportSchedule().then(data => {
    if (data) {
        const landedFlights = getFlightsByStatus(data, "landed");
        console.log("Landed Flights:", landedFlights);
    }
});
```

### Response

```
[
  {
    "aircraft": {
      "icao24": "A3D326",
      "icaoCode": "A21N",
      "regNumber": "N34562"
    },
    "airline": {
      "iataCode": "NZ",
      "icaoCode": "ANZ",
      "name": "Air New Zealand"
    },
    "arrival": {
      "actualRunway": "2026-07-03T00:32:00.000",
      "actualTime": "2026-07-03T00:32:00.000",
      "baggage": "C2",
      "delay": null,
      "estimatedRunway": "2026-07-03T00:32:00.000",
      "estimatedTime": "2026-07-03T00:32:00.000",
      "gate": "14",
      "iataCode": "IAH",
      "icaoCode": "KIAH",
      "scheduledTime": "2026-07-03T00:41:00.000",
      "terminal": "E"
    },
    "codeshared": {
      "airline": {
        "iataCode": "ua",
        "icaoCode": "ual",
        "name": "united airlines"
      },
      "flight": {
        "iataNumber": "ua1253",
        "icaoNumber": "ual1253",
        "number": "1253"
      }
    },
    "departure": {
      "actualRunway": "2026-07-02T21:37:00.000",
      "actualTime": "2026-07-02T21:37:00.000",
      "baggage": null,
      "delay": "27",
      "estimatedRunway": "2026-07-02T21:37:00.000",
      "estimatedTime": "2026-07-02T21:31:00.000",
      "gate": "B16",
      "iataCode": "DEN",
      "icaoCode": "KDEN",
      "scheduledTime": "2026-07-02T21:10:00.000",
      "terminal": null
    },
    "flight": {
      "iataNumber": "NZ6508",
      "icaoNumber": "ANZ6508",
      "number": "6508"
    },
    "status": "landed",
    "type": "arrival"
  }
]
```

### Developer Notes

The Real-time Airport Schedules API can be used to retrieve live airport departure schedules and live airport arrival schedules. Each request returns one schedule type at a time for a selected airport.

The `status` field indicates the current flight status. Possible status values include `scheduled`, `active`, `landed`, `cancelled`, `unknown`, and `diverted`.

Delayed is not a separate flight status in this API. Current departure and arrival delay information is available in the response through the delay fields under the departure and arrival arrays.

### Access & Support

[Contact us](https://aviation-edge.com/contact/) via email for any questions, support requests, custom requirements, or sample data requests.

[Get your API key](https://aviation-edge.com/premium-api/) in a minute and start testing the data right away. The API is provided through API subscriptions. All plans grant access to the Current Airport Schedules API and other APIs with a difference of the monthly API call limit. Choose the best plan for you and upgrade, downgrade or cancel your plan anytime without commitments.

### License

The use of the API is subject to Aviation Edge [Terms and Conditions](https://aviation-edge.com/api-terms-of-service/).
