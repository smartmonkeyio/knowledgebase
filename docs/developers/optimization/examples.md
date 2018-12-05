# Route Optimization examples

Below we describe different scenarios that can be solved with SmartMonkey's Route Optimization API.

## Basic optimization

In this scenario we assume there is **one vehicle that has to visit 2 places**. We know the starting and ending points of the vehicle and the coordinates of the different stops.

### Input
**Description**

In the following JSON input we define:
* First the vehicles: Specifying the vehicle's id and it's start and end coordinates.
* Secondly the services: In this case we define two visits with their corresponding ids and locations.

**Raw**
```json
{
    "vehicles": [{
        "id": "vehicle1",
        "start": { "lat": 41.3870154, "lng": 2.1678584 },
        "end": { "lat": 41.3870154, "lng": 2.1678584 }
    }],
    "services": [{
        "id": "task1",
        "location": { "lat": 41.3892478, "lng": 2.1706213 }
    }, {
        "id": "task2",
        "location": { "lat": 41.3855048, "lng": 2.161903 }
    }]
}
```
### Output

**Description**

In the execution output we find the following information:
* job_id: Id of the optimization job.
* status: _Success_ indicates that the optimization has finished properly.
* processing_time: time spent in seconds since the request is received until the response in generated.
* solution: contains two fields:
    * routes: contains an array providing the id of the vehicle and the assigned route as a sequence of steps with the arrival and departure times.
    * missing: an empty array means that all services have been assigned to some route.

**Raw**
```json
{
    "job_id": "5c0547c8832ce921f312e1be",
    "status": "success",
    "processing_time": 1117,
    "solution": {
        "routes": [
            {
                "vehicle_id": "vehicle1",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 0
                    },
                    {
                        "id": "task2",
                        "type": "stop",
                        "arr_time": 66,
                        "dep_time": 66,
                        "distance": 571
                    },
                    {
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 209,
                        "dep_time": 209,
                        "distance": 1394
                    },
                    {
                        "type": "end",
                        "arr_time": 274,
                        "distance": 626
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@J^j@pBZhAPl@Rr@\\d@FFfA~AXb@DFPTk@v@ILMRORKPGHLLnApBl@|@\\j@PWHMHQJK[e@yBiDSYOWqBsCW_@DIX_@JQBMHaB?KFsABa@TaG@eA@W@_@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            }
        ],
        "missing": []
    }
}
```

## Optimization with time windows

In this scenario we describe how to setup the input for the optimizer defining 

### Input
**Description**

In the input data we find three main fields:
* vehicles: here we define the vehicles providing an id, initial and final coordinate and the time availability of the vehicle. In this case the vehicle can work from 1am to 2am, as it is expressed as the number of seconds since the start of the day.
* services: For each service we state the position, the time required by the service specified in the duration field and finally the list of time windows in which the service can be carried out. In the case of task on, the task has to be performed from 01:04:00am to 01:04:10am.
* options: here we specify the maximum waiting for any vehicle to perform a service as 1000 seconds, i.e., 16 minutes and 40 seconds.

**Raw**

```json
{
    "vehicles": [
        {
            "id": "vehicle1",
            "start": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "end": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "timewindow": [
                3600,
                7200
            ]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.3892478,
                "lng": 2.1706213
            },
            "duration": 10,
            "timewindows": [
                [
                    3850,
                    3860
                ]
            ]
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.3855048,
                "lng": 2.161903
            },
            "duration": 10,
            "timewindows": [
                [
                    4470,
                    4480
                ]
            ]
        }
    ],
    "options": {
        "max_wait_time": 1000
    }
}
```

### Output

**Raw**
```json
{
    "job_id": "5c054d24a3c367289e8c64dd",
    "status": "success",
    "processing_time": 856,
    "solution": {
        "routes": [
            {
                "vehicle_id": "vehicle1",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 3600
                    },
                    {
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 3850,
                        "dep_time": 3860,
                        "distance": 1040
                    },
                    {
                        "id": "task2",
                        "type": "stop",
                        "arr_time": 4470,
                        "dep_time": 4480,
                        "distance": 1198
                    },
                    {
                        "type": "end",
                        "arr_time": 4578,
                        "distance": 940
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@fBoC`@q@DS@i@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@XbAPl@VbA^xAHVJ`@J^j@pBZhAPl@Rr@\\d@FFfA~AXb@DFPTk@v@ILMRORKPGHLLnApBl@|@\\j@PWHM[c@}A_Ce@i@QQOWaBeCQUQYQYgCyDYc@o@y@aDqEi@a@NO@i@Va@HKnBwCHMRYPw@XhADJPv@HT"
            }
        ],
        "missing": []
    }
}
```

## Capacited routing problem

### Input
**Raw**

```json
{
    "vehicles": [
        {
            "id": "vehicle1",
            "start": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "end": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "timewindow": [
                3600,
                7200
            ],
            "capacity": [
                4
            ]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.3892478,
                "lng": 2.1706213
            },
            "duration": 50,
            "size": [
                4
            ]
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.3855048,
                "lng": 2.161903
            },
            "duration": 50,
            "size": [
                5
            ]
        }
    ]
}
```

### Output

**Raw**

```json
{
    "job_id": "5c0552c313e3f32f9e715cbe",
    "status": "success",
    "processing_time": 881,
    "solution": {
        "routes": [
            {
                "vehicle_id": "vehicle1",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 3600
                    },
                    {
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 3702,
                        "dep_time": 3752,
                        "distance": 1040
                    },
                    {
                        "type": "end",
                        "arr_time": 3817,
                        "distance": 626
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@fBoC`@q@DS@i@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            }
        ],
        "missing": [
            {
                "id": "task2"
            }
        ]
    }
}
```

## Specifying service's vehicle restrictions
### Input

**Raw**
```json
{
    "vehicles": [
        {
            "id": "vehicle1",
            "start": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "end": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "timewindow": [
                3600,
                7200
            ],
            "provides": [
                "fanta",
                "coke"
            ]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.3892478,
                "lng": 2.1706213
            },
            "duration": 1800,
            "requires": [
                "coke"
            ]
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.3855048,
                "lng": 2.161903
            },
            "duration": 1800,
            "requires": [
                "pepsi"
            ]
        }
    ]
}
```
### Output

**Raw**
```json
{
    "job_id": "5c07bb6c638c4020ec77d6e2",
    "status": "success",
    "processing_time": 498,
    "solution": {
        "routes": [
            {
                "vehicle_id": "vehicle1",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 3600
                    },
                    {
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 3702,
                        "dep_time": 5502,
                        "distance": 1040
                    },
                    {
                        "type": "end",
                        "arr_time": 5567,
                        "distance": 626
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@fBoC`@q@DS@i@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            }
        ],
        "missing": [
            {
                "id": "task2"
            }
        ]
    }
}
```
## Defining pickup places

### Input

**Raw**
```json
{
    "vehicles": [
        {
            "id": "vehicle1",
            "start": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "end": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "timewindow": [
                3600,
                7200
            ]
        },
        {
            "id": "vehicle2",
            "start": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "end": {
                "lat": 41.3870154,
                "lng": 2.1678584
            },
            "timewindow": [
                3600,
                7200
            ]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.3892478,
                "lng": 2.1706213
            },
            "duration": 1800,
            "pickups": [
                {
                    "location": {
                        "lat": 41.3855048,
                        "lng": 2.161903
                    },
                    "duration": 10,
                    "timewindows": [
                        [
                            3600,
                            7200
                        ]
                    ]
                }
            ]
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.3855048,
                "lng": 2.161903
            },
            "duration": 1800,
            "pickups": [
                {
                    "location": {
                        "lat": 41.3892478,
                        "lng": 2.1706213
                    }
                }
            ]
        }
    ]
}
```

### Output

**Raw**
```json
{
    "job_id": "5c07b764af6f2f1684e895a0",
    "status": "success",
    "processing_time": 576,
    "solution": {
        "routes": [
            {
                "vehicle_id": "vehicle1",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 3600
                    },
                    {
                        "id": "pickup-task1-0",
                        "type": "pickup",
                        "arr_time": 3666,
                        "dep_time": 3676,
                        "distance": 571
                    },
                    {
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 3819,
                        "dep_time": 5619,
                        "distance": 1394
                    },
                    {
                        "type": "end",
                        "arr_time": 5684,
                        "distance": 626
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@J^j@pBZhAPl@Rr@\\d@FFfA~AXb@DFPTk@v@ILMRORKPGHLLnApBl@|@\\j@PWHMHQJK[e@yBiDSYOWqBsCW_@DIX_@JQBMHaB?KFsABa@TaG@eA@W@_@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            },
            {
                "vehicle_id": "vehicle2",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 3600
                    },
                    {
                        "id": "pickup-task2-0",
                        "type": "pickup",
                        "arr_time": 3702,
                        "dep_time": 3702,
                        "distance": 1040
                    },
                    {
                        "id": "task2",
                        "type": "stop",
                        "arr_time": 3832,
                        "dep_time": 5632,
                        "distance": 1198
                    },
                    {
                        "type": "end",
                        "arr_time": 5730,
                        "distance": 940
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@fBoC`@q@DS@i@^iIIgAK[EOOa@SUQQ_DiCSQIKEMK[]b@EFgB|BEHa@h@UTIZKJw@fAORQUQWeAaBw@iAo@cA_@g@^m@vBeDR_@HVbC~IBJFRH\\DRFTH\\XbAJ\\V`AL`@Nd@XhADJPv@XbAPl@VbA^xAHVJ`@J^j@pBZhAPl@Rr@\\d@FFfA~AXb@DFPTk@v@ILMRORKPGHLLnApBl@|@\\j@PWHM[c@}A_Ce@i@QQOWaBeCQUQYQYgCyDYc@o@y@aDqEi@a@NO@i@Va@HKnBwCHMRYPw@XhADJPv@HT"
            }
        ],
        "missing": []
    }
}
```

