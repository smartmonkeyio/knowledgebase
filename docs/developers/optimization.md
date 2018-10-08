# SmartMonkey's Route Optimization API

## Getting Started

To use **SmartMonkey's Route Optimization API** you'll require to:
1. Sign up at [SmartMonkey's Developers Console](https://console.smartmonkey.io)
2. Create an API key.

## Endpoint

All optimization requests should be addressed to the following endpoint:

> POST https://services.smartmonkey.io/api/v1/optimize?key=API_KEY_HERE

## Input

The input of an optimization request consists of:
1. Vehicles definition
2. Services definition
3. Reward regions
4. Configuration

### Input format
The input is structured in 4 main blocks:
* **Vehicles**: vehicles have to visit different places on the map to perform services.
* **Services**: jobs that have to be done by vehicles.
* **Reward zones**: regions in which services have a modified reward.
* **Configuration**: Setup of the optimization call

The input data should be in JSON format in the following way:
```json
{
  "vehicles": [ // Define the vehicles
    {
      "capacity": [84, 71],
      "timewindow": [32400, 57600],
      "start": {
        "lat": 41.374752,
        "lng": 2.14145
      },
      "provides": ["coke", "beer", "sprite"],
      "end": {
        "lat": 41.374752,
        "lng": 2.14145
      },
      "id": "v97499"
    }
  ],
  "services": [ // Define the services
    {
      "requires": ["beer"],
      "timewindows": [[28800, 61200]],
      "location": {
        "lat": 41.376458,
        "lng": 2.128609
      },
      "duration": 1800,
      "reward": 100,
      "optional": true,
      "id": "s2557",
      "size": [7, 5]
    }
  ]
}
```

### Vehicles

A vehicle is defined as follows:

```json
{
  "id": "seller 1",
  "start": {
    "lat": 1.234,
    "lng": 4.324
  },
  "end": {
    "lat": 1.234,
    "lng": 4.324
  },
  "capacity": [14],
  "timewindow": [36000, 68400],
  "provides": ["coal", "fuel"]
}
```

#### Attributes:
* id (required): Unique id for the vehicle.
* start (optional): GPS coordinate of the vehicle's starting point.
* end (optional): GPS coordinate of the vehicle's finish point.
* capacity (optional): Capacity of the vehicle. It's an array of `n` dimensions.
* timewindows (optional): Used to define the working hours of the vehicle.
* provides (optional): List of types of services that can be performed by the vehicle.

### Services

A service is defined as follows:

```json
{
  "id": "client 1",
  "location": {
    "lat": 3.564,
    "lng": 6.543
  },
  "size": [1],
  "timewindows": [[36000, 50400], [57600, 68400]],
  "duration": 3600,
  "reward": 100,
  "optional": true,
  "requires": ["fuel"]
}
```

#### Attributes:
* id (required): Unique id for the service
* location (required): Place to execute the service.
* size (optional): Size or space required in the vehicle for this job. It should have as many as dimensions as the vehicles have.
* timewindows (optional): Time when the tasks can be executed. List of start and end times defined in seconds since the start of the day. The example above has 2 time windows to make the visit from 10am to 2pm and from 4pm to 7pm. 
* duration (optional): Duration of the service in seconds
* reward (optional): Reward obtained by executing the service. Useful when there are different optional tasks and some are more important than the others.
* optional (optional): States whether the task is mandatory or not (optional). When it is set to false, it will fail to find a solution if the service is can’t be placed in any route.
* requires(optional): The service will be performed only by vehicles providing all the features included in this field.

### Reward regions
Modify the reward of the services inside the defined region by adding the region's reward to service's reward.

```json
{
  "lat": 41.31,
  "lng": 2.17,
  "radius": 5000,
  "reward": 10
}
```

#### Attributes:
* lat (required): GPS latitude of the center of the circle
* lng (required): GPS longitude of the center of the circle
* radius (required): Radius of the circle
* reward (required): Reward offset in the region, can be positive or negative

### Configuration

```json
{
  "wait": false,
  "callback": "http://callback.com/result"
}
```
#### Attributes:
* wait (optional): True in order to wait for the optimization to finish. Defaults to true when a callback is not defined.
* callback (optional): Callback address to submit the optimization result once finished. `wait` should be false.

In case of setting the configuration field `wait` to `false` the result of the optimization can be fetched with a GET request to:

> GET https://services.smartmonkey.io/api/v1/optimize?job_id=JOB_ID_HERE&key=API_KEY_HERE

The format of the response to this request is as defined in the Output section.

In case of defining a `callback` value in the configuration, the response will be sent to the specified callback but it can also be retrieved using the same GET request.

## Output
```json
{
  "status": "success",
  "processing_time": 632,
  "job_id": "5bbb107698318200160faf1e",
  "solution": {
    "routes": [
      {
        "geometry": "e_p{FkgaLiBAChC...",
        "vehicle_id": "v97499",
        "steps": [
          { // The first step includes the starting time of the route
            "dep_time": 35426,
            "type": "start"
          },
          { // There is an step for the service in the route
            "distance": 1326,
            "dep_time": 37348,
            "type": "stop",
            "id": "s2557",
            "arr_time": 35548
          },
          { // The last step indicate the arrival to specified place if any.
            "distance": 824,
            "type": "end",
            "arr_time": 56502
          }
        ]
      }
    ],
    "missing": []
  }
}
```

#### Attributes

* status: Status of the optimization
* process_time: Time spend computing the result
* job_id: Unique identifier of the job
* solution: Solution consisting of 0 or more routes
  * routes: 
    * geometry: Geometry of the route as a [polyline](https://developers.google.com/maps/documentation/utilities/polylinealgorithm)
    * vehicle_id: Id of the vehicle that has to execute the route
    * steps: Array of steps to execute the route
      * id: Id of the service
      * dep_time: Departure time in seconds since the start of the day.
      * type: Type of stop
      * distance: Distance from the previous step to the current one.
      * arr_time: Arrival time in seconds since the start of the day.
  * missing: Ids of the services that could not be fit in any route
