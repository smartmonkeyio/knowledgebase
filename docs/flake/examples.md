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
        "id": "taskA",
        "location": { "lat": 41.3892478, "lng": 2.1706213 }
    }, {
        "id": "taskB",
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

## Capacitated routing problem

In this example we show how to setup an optimization problem with vehicles with one dimension. This dimension can interpreted as liters, kilograms, gallons... it is just a number. Make sure all vehicles and services have the same number of dimensions.

### Input

**Description**

In the example below we defined a vehicle with one dimension and capacity 4 in that dimension. Also we define two services with required dimensions of 4 and 5 respectively.

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

**Description**

In this case we have a service that could not be assigned because it does not fit in the vehicle as it has size 5 and the vehicle has size 4.

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

This example shows how vehicle restrictions can be defined and how services are assigned to vehicles accordingly.

### Input

**Description**

We setup the problem with a vehicle delivering _Fanta_ and _Coke_ and two service requiring _Coke_ and _Pepsi_ respectively.

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

**Description**

The result contains a route with one stop because the stop requiring _Pepsi_ could not be served as the vehicle only provides _Fanta_ and _Coke_.

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

This scenario has 2 vehicles and 2 services. Each service has a pickup place, therefore, 4 stops are required.

### Input

**Description**

Two services are defined both with a pickup to perform before being executed. The first service's pickup defines a pickup time of 10 seconds and also defines a time window for the pickup to be carried out.

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

**Description**

The solution has two routes, each vehicle performs one service with the corresponding pickup. The pickups are identified with step type _pickup_ and its _id_ is of the the form _pickup-service.id-index_.

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

## Load balancing

This scenario has 2 vehicles and 2 services. We'll show how to balance the load among vehicles.

In this scenario, we have 2 inputs with the corresponding output. We have the same tasks and vehicles in both cases but the latter case has been slightly modified in order to balance the workload.

### Input 1

**Description**

There are two services far from the starting point and the optimal solution is to send only a vehicle to perform both services. Nonetheless, we would like to distribute the workload among vehicles.

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
            "capacity": [1500]
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
            "capacity": [1500]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.5398348,
                "lng": 2.4448926
            },
            "size": [500],
            "duration": 1800
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.6079555,
                "lng": 2.2876008
            },
            "size": [500],
            "duration": 1800
        }
    ]
}
```

### Output 1

**Description**

The solution includes two routes: one with both services and the other one with none.

**Raw**
```json
{
    "job_id": "5c8f9d055137a40021a2d5d5",
    "status": "success",
    "processing_time": 324,
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
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 1576,
                        "dep_time": 3376,
                        "distance": 31753
                    },
                    {
                        "id": "task2",
                        "type": "stop",
                        "arr_time": 4400,
                        "dep_time": 6200,
                        "distance": 18621
                    },
                    {
                        "type": "end",
                        "arr_time": 7847,
                        "distance": 29824
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@jBwCBKAOKg@{@oDg@iBQe@GQK[aBtAIFSNGFU`@Qv@SXILoBvCIJW`@[g@sByCKOGIKI?GGUOMKAEUGOmDoFQYYa@qByC[g@s@cAaBcCW_@Y_@yBaDSWKEU[yBcDa@UgAsAIc@Ge@@a@Ae@Kc@O[KMSMWIQA[D]Q_@QGG{@w@Y[_CiDIMIKW_@{BgDU]W]{BiDU]U]cCmDQWOU}CcFeD{Ei@y@QUo@_As@cA[a@Yg@g@_A_@_AY}@{@qB]m@EIi@u@w@u@y@u@}@q@]]QKy@kAi@u@kAiBeDaFkC{DUYMUYc@sByCs@cA}@kAcEeGyD{FcKcOuDsF}Te\\gLuPgRyX_DwE{A_C{@}AkAsDo@eBMUUm@KYg@wA[{@_@{@y@aB_@m@sAiB}@cAm@i@i@g@qA_Ai@g@]YMM_EgDsEsFyDeEcAgAaByAmCiCkBmASGgBo@uBc@}BQyBS}BWyBa@sBi@sBs@{DkByAcA_BkAaD}CkDeDqCwC{AuAiT{SiHiHeF}EwAaBiAwAeB_CyBaCiJgLcGoHuAqBs@sAe@iAw@mBwBoGwAmDuAqCeDqGyBkEsCwFi@kA]aAe@gBKe@Kk@[cBc@}Ck@eE}D_XuAeHsBiJyDiOeFgQ{CoJUo@iFkNU{@WgAW_BMgAs@oHQeAO}@Ki@]qAeAsCsAaCoEgE{FmD}@i@SMyDgDgFuG{DsDsCcBmDqB{EeD}ByBcB{AuBwCgB}CuA_DqC_HiAuCiBcEuBaFeFyHkBiB{ImGqGkDgGuD{CgCeAoAaAuAMOk@aA_@u@Sa@c@_A[w@Ww@WaAa@}A[eBMs@Y{BOmBIeBCiBAcA@wAD{AFkAR{CZ}Ax@cElBwHrBiIrA}Gd@uC^yCTyBLyBJuBDuBCkEQaEm@eF_C}IeA{Bc@_A_CgEmDmEsDeEy@wA{@mBk@}Ak@eBa@}Ac@sB[oBUaBSaCYaEQmEGmEEqQCyDCqCIoCSkE[qDe@oDk@oCqByHy@oDo@_Ei@aEg@}Cs@gDaAcDwAkDcBaDkBiCqBuB{BiBgGiDyEaCeCaBaC{BiCsDg@s@kB{DsA{DgA{C}@qCSm@y@uCm@gCs@{Co@qCOsCS{DGoAEa@Me@w@{BcCqHkAkEw@iCi@iB}A{FiBsFiBiFaAcCeAmCiB}DoB}DaFqJ_FaK_CmFs@iBu@iBoGkRyB{IoBuIi@oCoAyG_B_K_@cCiAiIkAyIkAaJ_@aCWsAi@kCcA}EkAgEkAiEaAmCkB}EyAkD_F_LiBqDwAmDy@qBu@gBoB_F_BgE}BqGqAcEq@}B{@oD_@cBQcASuAMiAO_BMcBM}CKeBKcBMgBSoBQqAQsASoAc@wCU}A[wBUiBOkBKyAIsBEyAGgE?kEBgDFiDBaF?k@?KBSFOJUFKDU@]Ac@Iy@EWGUGSGQMWMQWMQCSBMFORIVEV?b@@`@@NQb@OFwBr@}CfAOFyAf@m@Rk@RqE|Ak@Rm@RgBn@g@PmBl@}Aj@e@PeA\\[LKIYCQHKPYSaBkAMKOOaAaAaBiBUU_A_AUWsAsA]_@a@a@MS_AsA_@k@Wi@Wm@S]POFKBK?UAMEQOQWCKBMHQ[{CeEQWy@iAEe@OqAmAmIq@{EM{@BMYgBGOEQpAe@xAk@xAg@x@]h@SVSLCZ@d@Bb@BtAD??lAjHeBrBKLoAtAGH?`@@Z@pADlD?TDbD?R?TDpE?t@Af@SBMHGLEVQZm@^_BjA[ZQNc@b@i@n@KLIJU\\W\\u@`A_@`@ONk@f@uBvA_A`@aAZ}@Lu@BO_@KMMIKEYCS@WLKLGJEJELALKDKD[H_@Fg@D}@FQ@sFVmFTU@}@DQ@wABqADESGQKOMKMEOCK@K@KDIHMPITEXMPIDIBw@BW@aBFQ@_@CUGMKKOIWAQA[BOHIJCJBLLVXJLR\\\\tA\\vA^bB^dBPhAVtARjAZfAN|@N~@jBfLt@~EhA~F|AtG|AjFpBfGn@bB`@|AhAjCt@|Aj@jANVT`@\\f@PVZn@Xh@HZDV@V?XCXCPETGPGNKLKN[Zq@j@iAdAq@j@_@Z_@Tm@^_BjAwBlAgB|@}CfBk@ZaBpA{BhB_C~AgClAq@Xo@R{A`@{A\\}AVoBb@sBj@yB|@eFxCmBtBsBtCyAbDyAlEeB|G}AzEuBvDkBrCaC|BiHzE_K|EeKpF{G~CoCxByAzAyAnBo@hA{@nBu@zB]nAUbAc@|BYnBYbB_@~Ak@nB}@bC_AlBiB~C{BnCoCdC_DbCsD~D{@jAu@nAaAjBaAbCaAzCg@lB_BvHoBzJsAvGmBrNKv@_Nv`A_Gj[_Jb\\m@|Bg@rBi@tBcBvEcAhC_A|BqAhCkBjDmDvFcIdMyCvE}@vA_CfDyAbCiA~BeAzBm@nBk@`Ci@fD_@xDS~DKdCMrCQ`B]jBKn@a@dBq@bCO^_@fA_AnBg@dAu@pAk@bAk@hA_@fAYpAQvAGl@CjCLzA`@pCpIva@h@nCpC`KlBfG`@v@HNn@d@j@Tb@\\X`@BXJVDX?`@Ez@SnBQtAc@~Be@`Be@|@]p@a@h@i@p@{@~@m@z@aAjAq@v@{AfBYJUDa@?UHSPS\\I^BVJXRLJBL?h@XzBdB`F|G\\`@n@dBxBlHv@bHNrA^pF^|EApHq@rEu@dC{BvEk@hAo@dB_@fAWz@Sz@S`AOz@UbBMrAUtCS~CUf@GHGLAR?NJTB^UzCSdCUzCYpDI@IDGFK\\i@Ie@EiAM[EaC[OAm@IMCgAO}De@yB[gAOaAMwDi@sCa@}BYcH{@OrCeCBIhBOjCMxBuA?u@AG_AYu@??zDAnB?jDAvCA~D?RlB@JHx@ZfDJjADh@BVBP^bETxBT`Cp@nH|@tIZ|AJf@R\\\\l@V\\K\\?PBTHPLJPDRCLKb@n@x@pAl@h@b@l@AHBXBNJNLHPPLZb@v@dBhCLN`@f@JHtAzBt@rAfArBzAfDvAhDjAxCt@rBFX~DnML`@l@nBdBrFPd@dEpNb@vAz@lCZp@^\\f@RXDTATGx@Mh@Kj@G`@?VBj@Lz@ZvB|@`JfG~CvBrGrE`N~ItPtKdRjKpNtHdDdBXF`JxIlBrBxBfCbAdAtDzDpArAhFnFfOhPlTzU|BhCfBfCf@lAzDhIf@bAhCvD~BtCvI~IhHtGtDlEr@`At@bAtAbBl@p@l@l@nDrDrApAh@b@l@`@r@\\r@Zn@Pl@LvHRfFChBOjB]|D{@bCWxCGvBDzDf@dB`@xBt@hBhAbH|EnDdC|DpCtDdCdE|CvBdB~DzC|DrCdDtBv@v@PNxAvBrAbCrCdFx@xArD|FzAlAd@VpI|C|@X|@Zd@RX\\Lh@Cl@K`@]X]Fg@KYg@Eo@A_@Fm@h@gBtAaD^w@LM^i@l@s@dA_AxAi@hAChAHtBp@bLjFLClEjCpPlJdCtAxDvB`B|@zI`GrBzAtLnIvD~BlBnAbBhAvPfR~FdHrA`BvBhCbJnKxGxGlA`ApB|Av@j@v@f@lBjAbAj@hAf@dBt@tAd@xAb@x@Pz@PxB^tBX`BTvAPxAX`AT`A^jAj@~@j@b@^b@`@`@`@`@d@f@t@f@v@Xf@Vf@r@vAj@jAXl@`@v@j@~@v@fAr@x@bA~@r@h@~@j@r@Zt@Z|@Xv@Pv@Lb@Fd@Dl@Fp@BbA@pAA~ACxAItAKr@G|BSrD]vAGzAGv@Av@?bA@dAFdAFfAPvAVv@P`AZvAd@nAj@rAr@xA~@z@j@|@l@~ApA~AtA~AzAbAbAlAnAfErEdBhBz@|@dA~@~@t@fAn@d@Vf@Th@Pv@TjARr@Hr@D`A?p@An@Gn@Ij@Kr@Qn@Sb@Qd@Ux@e@h@]l@g@nAkAn@q@b@q@p@s@bA}@h@k@LOt@c@vA{@bCqA~@k@h@a@pAy@bAw@j@k@`@_@f@i@r@}@z@iAf@s@bA_Bj@_An@}@dAgBfAwBxAkCz@}AVc@^e@b@g@^_@^]j@_@h@YfBy@z@YlA[`AMvAOdAOZBhCKzB@xCIlDJtBTbB^lAVfB`@fAT\\Fd@LhDt@xA\\l@L|A\\jDt@jAVzD|@jDx@XDfB\\fEv@hIdBp@LjRpEf@JnLtC~EbApEdAjAXv@Bf@A`@Ch@IVKj@Y^YXUdCaBf@YhBoA\\Q`Ai@nAw@t@c@`E{BzCgBb@Yd@Sx@]`@Ub@Kt@Sh@Ip@Kh@GTANAl@?n@?x@@\\?pDAdF@b@@L?L?r@?rG?b@?~F@l@?rICrD@dB@lI@l@@jG?rABl@Dn@DnB@bCALNlCvD|AxBnApBZb@vBzCZb@Vb@pB|C\\d@Zf@Zb@nAjBZb@\\h@lBpCXb@\\h@bBbC\\j@\\b@t@dAl@z@d@p@Zd@vB`Dd@x@NRf@r@NRV`@x@jANTPVzB`D^l@b@b@bBhC`@n@\\h@~BpDFDjCrD^j@`@j@fBlC^j@HJV\\X^r@dAZ`@`@h@^l@dBnCb@n@NV|@pAz@pAXb@^h@nDbFRVjD}EHMfDkEhCcDn@a@L?JALMDM@O?GRu@xByCPWNSv@gAJKVLH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            },
            {
                "vehicle_id": "vehicle2",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 0
                    },
                    {
                        "type": "end",
                        "arr_time": 0,
                        "distance": 0
                    }
                ],
                "geometry": "mlr{FwkfL??"
            }
        ],
        "missing": []
    }
}
```

### Input 2

**Description**

There are two services far from the starting point and the optimal solution is to send only a vehicle to perform both services. Nonetheless, we would like to distribute the workload among vehicles. For this purpose, we add and extra dimension to each vehicle and service so that each task can be done only by one vehicle.

**NOTE**: The capacities and sizes of vehicles and services has been modified by adding an extra dimension to the end.

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
            "capacity": [1500, 1]
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
            "capacity": [1500, 1]
        }
    ],
    "services": [
        {
            "id": "task1",
            "location": {
                "lat": 41.5398348,
                "lng": 2.4448926
            },
            "size": [500, 1],
            "duration": 1800
        },
        {
            "id": "task2",
            "location": {
                "lat": 41.6079555,
                "lng": 2.2876008
            },
            "size": [500, 1],
            "duration": 1800
        }
    ]
}
```

### Output 2

**Description**

The solution includes two routes, each one with a service.

**Raw**
```json
{
    "job_id": "5c8f9e0f5137a40021a2d5d7",
    "status": "success",
    "processing_time": 335,
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
                        "id": "task1",
                        "type": "stop",
                        "arr_time": 1576,
                        "dep_time": 3376,
                        "distance": 31753
                    },
                    {
                        "type": "end",
                        "arr_time": 4978,
                        "distance": 31823
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@jBwCBKAOKg@{@oDg@iBQe@GQK[aBtAIFSNGFU`@Qv@SXILoBvCIJW`@[g@sByCKOGIKI?GGUOMKAEUGOmDoFQYYa@qByC[g@s@cAaBcCW_@Y_@yBaDSWKEU[yBcDa@UgAsAIc@Ge@@a@Ae@Kc@O[KMSMWIQA[D]Q_@QGG{@w@Y[_CiDIMIKW_@{BgDU]W]{BiDU]U]cCmDQWOU}CcFeD{Ei@y@QUo@_As@cA[a@Yg@g@_A_@_AY}@{@qB]m@EIi@u@w@u@y@u@}@q@]]QKy@kAi@u@kAiBeDaFkC{DUYMUYc@sByCs@cA}@kAcEeGyD{FcKcOuDsF}Te\\gLuPgRyX_DwE{A_C{@}AkAsDo@eBMUUm@KYg@wA[{@_@{@y@aB_@m@sAiB}@cAm@i@i@g@qA_Ai@g@]YMM_EgDsEsFyDeEcAgAaByAmCiCkBmASGgBo@uBc@}BQyBS}BWyBa@sBi@sBs@{DkByAcA_BkAaD}CkDeDqCwC{AuAiT{SiHiHeF}EwAaBiAwAeB_CyBaCiJgLcGoHuAqBs@sAe@iAw@mBwBoGwAmDuAqCeDqGyBkEsCwFi@kA]aAe@gBKe@Kk@[cBc@}Ck@eE}D_XuAeHsBiJyDiOeFgQ{CoJUo@iFkNU{@WgAW_BMgAs@oHQeAO}@Ki@]qAeAsCsAaCoEgE{FmD}@i@SMyDgDgFuG{DsDsCcBmDqB{EeD}ByBcB{AuBwCgB}CuA_DqC_HiAuCiBcEuBaFeFyHkBiB{ImGqGkDgGuD{CgCeAoAaAuAMOk@aA_@u@Sa@c@_A[w@Ww@WaAa@}A[eBMs@Y{BOmBIeBCiBAcA@wAD{AFkAR{CZ}Ax@cElBwHrBiIrA}Gd@uC^yCTyBLyBJuBDuBCkEQaEm@eF_C}IeA{Bc@_A_CgEmDmEsDeEy@wA{@mBk@}Ak@eBa@}Ac@sB[oBUaBSaCYaEQmEGmEEqQCyDCqCIoCSkE[qDe@oDk@oCqByHy@oDo@_Ei@aEg@}Cs@gDaAcDwAkDcBaDkBiCqBuB{BiBgGiDyEaCeCaBaC{BiCsDg@s@kB{DsA{DgA{C}@qCSm@y@uCm@gCs@{Co@qCOsCS{DGoAEa@Me@w@{BcCqHkAkEw@iCi@iB}A{FiBsFiBiFaAcCeAmCiB}DoB}DaFqJ_FaK_CmFs@iBu@iBoGkRyB{IoBuIi@oCoAyG_B_K_@cCiAiIkAyIkAaJ_@aCWsAi@kCcA}EkAgEkAiEaAmCkB}EyAkD_F_LiBqDwAmDy@qBu@gBoB_F_BgE}BqGqAcEq@}B{@oD_@cBQcASuAMiAO_BMcBM}CKeBKcBMgBSoBQqAQsASoAc@wCU}A[wBUiBOkBKyAIsBEyAGgE?kEBgDFiDBaF?k@?KBSFOJUFKDU@]Ac@Iy@EWGUGSGQMWMQWMQCSBMFORIVEV?b@@`@@NQb@OFwBr@}CfAOFyAf@m@Rk@RqE|Ak@Rm@RgBn@g@PmBl@}Aj@e@PeA\\[LKIYCQHKPYSaBkAMKOOaAaAaBiBUU_A_AUWsAsA]_@a@a@MS_AsA_@k@Wi@Wm@S]POFKBK?UAMEQOQWCKBMHQ[{CeEQWy@iAEe@OqAmAmIq@{EM{@BMYgBGOEQpAe@xAk@xAg@x@]h@SVSLCZ@d@Bb@BtAD??lAjHeBrBKLoAtAGH?`@@Z@pADlD?TDbD?R?TDpE?t@Af@SBMHGLEVDb@JRJFL@LAR\\Vl@Vh@^j@~@rALR`@`@\\^rArATV~@~@TT`BhB`A`ANNLJ`BjAXREJAN?PBRFJVRH@RENMFOBU?WRKdA_@b@OjEwAh@SdBm@n@Uh@QpE_Bj@Ql@UhBm@NGlC_AvBu@RGJ?NFDNL\\NVPPNd@BP?RAVAr@CpCGnDAvACtB@vA@zABtAFtAHtBFjAHlAPbBVnBZrBb@tCh@`DPnARrARzALzAFz@Bh@JbBJhBHrBLvBHz@H|@LhARpAPbA^fB|@nDp@xBf@fBn@lBfA`Dr@jBn@bBrAdDxBdGl@rBj@zBh@|Cx@tEThA\\jAl@zAl@lAhA|AlA|A|AhB|BtE`BvDhA|D~@~Dt@|Df@vCLt@hAbIhAnIlBrMzAzJlAnGh@vCrB|IzBxIhDjLbBtEv@rBpDvIbF~J~ExJzExJhBvEVr@lBnFX|@nArDhAnEjAhEDVb@nBZxDZlD\\jBh@jBr@xBp@bB^|@t@hDv@jDf@|Bt@bDrChJxCpIrB~Dd@v@nCtDbC`CjChB`FbCxFxCtBhBpBrBjBlCdBxCnAdD|@zCr@dDj@nDh@|Dj@rDx@nDrBvHp@rD\\rCZhDR`EFvB@pBHxVHrGThGXhDPpBr@pEd@xB`@zAd@|An@bBbAxBx@|A|CzEpD~DnCvDdBrDzBvIl@`FPzD?fEOdFMpBSpBg@zDq@tD_ClKu@tCgBrHs@dD[tBOxAMbBInAAdACz@?nB@xAJzBNvBLbAZpBP`Ah@|BNh@Vv@`@jAf@jAZn@d@z@p@dAhAxAdAjAlC`CrG~DrGjD~IbGlBdChDdFvBrEdAtCjB|G`@vA`B~E|AtDjB~CxBrChBdBhCnBjAp@fJdE|Ax@~A`AnChC~EdGpArAb@`@x@n@NLTNv@j@|FhDlEzDpA~B`AlC\\pAP`ALz@PnAj@tFHbAN~@TjALl@Rt@HVj@`BRj@tCdIXv@jDxJ`FdQvDfOtBlJvApH~DbXj@|Df@pDX|Ab@hBZbAVr@p@zAfCdF|BlEbDzG|A|CbBbEdBbFx@pBd@tAr@pA~AxBxFhHhJdLlHzIhEtEpFlF|HpHrLlLbB`BtAnALNbCbCr@p@l@j@tArApArA|ArAhAz@fBlA`Bz@r@Zp@VlBp@nBf@z@P|@LxBVzBRzBTxB^~Af@XJhBrArEhEjCpC`BdBbBbCbBxC|AhD^`A`BhEVdAVr@h@rAx@|AVp@p@dB`@p@d@f@d@^p@Xn@ZjAn@hA|@~@r@j@j@vArAxAnBdBdCf@t@lBpCbAvALJ|A`C^d@rB|Ci@z@_BvDILINqCfE{BvCo@~@MTIJGFVZZb@rC~Dr@bAnB|CpBvCz@pAJL`DnEfD|EfA~AJN`@n@n@dApAlBLPjA`BnAfBJNrApBhA`B`B~BFJ|CtEnDfFfD|EzFlINVNR\\d@v@bAv@hAxAxBtArBT\\JJJJ\\XPRLNlCvD|AxBnApBZb@vBzCZb@Vb@pB|C\\d@Zf@Zb@nAjBZb@\\h@lBpCXb@\\h@bBbC\\j@\\b@t@dAl@z@d@p@Zd@rBzCh@~@NRf@r@NRV`@x@jAT\\JNzB`D^l@b@b@bBhC`@n@\\h@~BpDJJfClD^j@`@j@fBlC^j@HJV\\X^r@dAZ`@`@h@^l@dBnCb@n@NV|@pAz@pAXb@^h@nDbFRVjD}EHMfDkEhCcDn@a@L?JALMDM@O?GRu@xByCPWNSv@gAJKVLH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            },
            {
                "vehicle_id": "vehicle2",
                "steps": [
                    {
                        "type": "start",
                        "dep_time": 0
                    },
                    {
                        "id": "task2",
                        "type": "stop",
                        "arr_time": 1658,
                        "dep_time": 3458,
                        "distance": 31708
                    },
                    {
                        "type": "end",
                        "arr_time": 5105,
                        "distance": 29824
                    }
                ],
                "geometry": "mlr{FwkfLNl@Pl@VbA^xAHVJ`@Xe@jBwCBKAOKg@{@oDg@iBQe@GQK[aBtAIFSNGFU`@Qv@SXILoBvCIJW`@[g@sByCKOGIKI?GGUOMKAEUGOmDoFQYYa@qByC[g@s@cAaBcCW_@Y_@yBaDSWKEU[yBcDa@UgAsAIc@Ge@@a@Ae@Kc@O[KMSMWIQA[D]Q_@QGG{@w@Y[_CiDIMIKW_@{BgDU]W]{BiDU]U]cCmDQWOU}CcFeD{Ei@y@QUo@_As@cA[a@Yg@g@_A_@_AY}@{@qB]m@EIi@u@w@u@y@u@}@q@]]QKy@kAi@u@kAiBeDaFkC{DUYMUYc@sByCs@cA}@kAcEeGyD{FcKcOuDsF}Te\\gLuPgRyX_DwE{A_C{@}AkAsDo@eBMUUm@KYi@iA]s@]y@c@}@k@}@[[YW[SWMYK]C[Aa@@[DWJUHYRUR]ZWZ_@n@Wn@Sr@Mx@Kx@Kx@Kb@M^QZSTc@\\e@ZeAr@eAn@cBhAg@l@kCbBkDzBmDxBc@VuBjAwBhA}Ar@iAf@iAd@oAd@oA`@eBd@uSlDw@Lw@PgBb@u@TmA`@kAb@oAf@sAn@oAn@eAn@}@l@UNw@h@o@f@a@^mAdAoBdBqBdBiB`B{ApA_Az@m@f@yB~BsBhCo@hAg@fAo@bBQl@Oj@Op@A`@[hBQ|ASdBYhBSz@Y|@]~@]p@W`@[`@]\\[X_@RUL[LWHo@Pk@Jm@Hg@Fk@BcBReAPYAi@Ni@TWLUNSLQNc@b@a@h@Yf@Uf@Q`@Od@Od@Md@]tAU`AMx@Ij@Gt@C`@Al@CjACx@El@Gj@Kf@K`@K^Qf@Wf@Y`@w@bA{@|@Y\\k@l@i@n@]RiBhCeAhBaAhBuAjCk@bAm@`Aa@l@}@rAe@n@aAfAk@h@q@l@eBvAuC~Bs@j@kA`Ay@t@aA`A{A~AgAjAs@v@QXoAvAoAjAi@b@i@`@w@b@_Ab@{@Xm@Nk@Jm@Hq@Fq@By@As@Eq@IiAQu@Ui@Sg@Sc@WcAm@}@q@_A{@}@}@cBiBoE{EoAqA}@}@cB}AsAkAmB{A}@o@}@m@yA}@sAs@wAo@sAe@cAY{@SsAUiAOiAKeAGcAAw@?w@@}ADuAJuDX}BRw@FoAJ}AHyADqA?cAAo@Co@Ga@Ca@Ey@Ow@S}@Ys@Yu@_@{@i@u@k@{@y@s@y@q@_Ag@{@c@y@_@s@c@aAq@wA[k@Yg@g@y@i@s@c@i@c@c@e@a@e@a@}@k@o@[e@ScA_@gAY{AWwASwAQ_C]wB]y@Qw@QyAa@wAg@cBs@gAg@cAi@gBgAy@k@w@m@kA{@a@]aA{@u@q@kCgCoFeG_IsJsA_BaEaFsKiMoEiEi@g@_A{@}@{@kFaEqNaJ}@k@iMoH{D{BePmJAOaFqDsCaCwFcFuAyAcBq@y@Eu@FiBdAqAhAaBvB}AxAi@h@_AZUAu@[q@SYE_@Fo@DqAHgADsABi@GWGm@YmAw@_BuCaGsKuCkE}@q@gD{BoBwAaGoEaHmFaEqCcD}BmA}@oAy@iHiFy@i@aAi@WMmFiB_Ce@}DKeBByAJsEp@eCl@eCZmD?gGKiA?w@Gi@Iq@Qc@QMGs@_@k@_@g@c@gAeAoCoCyBcCeBoBq@_Aq@}@wAeBcBoBeHsGsFeGyA_BwCeDeBmCc@_AoDkHoAgCaB}Bi@u@mQyR{RwSu@{@qDcEiBcBcBkBeFmFaCcCcDeDSSeE}Dk@i@}OoI{IuE_FoC}@i@WO}EwC}T_OuDcC_P_LmCmBgDuBeAs@uAy@qEaBeBo@qAc@c@W_@Y_@g@[m@Oc@M_@_@iAqDqLqBwGsDoL{AkE{CuH_BmDgBeD}AiC_@w@[o@c@w@i@w@Uc@O_@I[EWDQ@Q?QEUIOKKOEO?QFKHq@Em@i@y@qAc@o@HQFQAOAUIQKMQGUBSRW]]m@S]Kg@[}A}@uIq@oHUaCUyB_@cECQCWEi@KkA[gDIy@iA@o@?}A?yC@kBAW?aD@uA?u@AG_AYu@??zDAnB?jDAvCA~D?RlB@JHx@ZfDJjADh@BVBP^bETxBT`Cp@nH|@tIZ|AJf@R\\\\l@V\\K\\?PBTHPLJPDRCLKb@n@x@pAl@h@b@l@AHBXBNJNLHPPLZb@v@dBhCLN`@f@JHtAzBt@rAfArBzAfDvAhDjAxCt@rBFX~DnML`@l@nBdBrFPd@dEpNb@vAz@lCZp@^\\f@RXDTATGx@Mh@Kj@G`@?VBj@Lz@ZvB|@`JfG~CvBrGrE`N~ItPtKdRjKpNtHdDdBXF`JxIlBrBxBfCbAdAtDzDpArAhFnFfOhPlTzU|BhCfBfCf@lAzDhIf@bAhCvD~BtCvI~IhHtGtDlEr@`At@bAtAbBl@p@l@l@nDrDrApAh@b@l@`@r@\\r@Zn@Pl@LvHRfFChBOjB]|D{@bCWxCGvBDzDf@dB`@xBt@hBhAbH|EnDdC|DpCtDdCdE|CvBdB~DzC|DrCdDtBv@v@PNxAvBrAbCrCdFx@xArD|FzAlAd@VpI|C|@X|@Zd@RX\\Lh@Cl@K`@]X]Fg@KYg@Eo@A_@Fm@h@gBtAaD^w@LM^i@l@s@dA_AxAi@hAChAHtBp@bLjFLClEjCpPlJdCtAxDvB`B|@zI`GrBzAtLnIvD~BlBnAbBhAvPfR~FdHrA`BvBhCbJnKxGxGlA`ApB|Av@j@v@f@lBjAbAj@hAf@dBt@tAd@xAb@x@Pz@PxB^tBX`BTvAPxAX`AT`A^jAj@~@j@b@^b@`@`@`@`@d@f@t@f@v@Xf@Vf@r@vAj@jAXl@`@v@j@~@v@fAr@x@bA~@r@h@~@j@r@Zt@Z|@Xv@Pv@Lb@Fd@Dl@Fp@BbA@pAA~ACxAItAKr@G|BSrD]vAGzAGv@Av@?bA@dAFdAFfAPvAVv@P`AZvAd@nAj@rAr@xA~@z@j@|@l@~ApA~AtA~AzAbAbAlAnAfErEdBhBz@|@dA~@~@t@fAn@d@Vf@Th@Pv@TjARr@Hr@D`A?p@An@Gn@Ij@Kr@Qn@Sb@Qd@Ux@e@h@]l@g@nAkAn@q@b@q@p@s@bA}@h@k@LOt@c@vA{@bCqA~@k@h@a@pAy@bAw@j@k@`@_@f@i@r@}@z@iAf@s@bA_Bj@_An@}@dAgBfAwBxAkCz@}AVc@^e@b@g@^_@^]j@_@h@YfBy@z@YlA[`AMvAOdAOZBhCKzB@xCIlDJtBTbB^lAVfB`@fAT\\Fd@LhDt@xA\\l@L|A\\jDt@jAVzD|@jDx@XDfB\\fEv@hIdBp@LjRpEf@JnLtC~EbApEdAjAXv@Bf@A`@Ch@IVKj@Y^YXUdCaBf@YhBoA\\Q`Ai@nAw@t@c@`E{BzCgBb@Yd@Sx@]`@Ub@Kt@Sh@Ip@Kh@GTANAl@?n@?x@@\\?pDAdF@b@@L?L?r@?rG?b@?~F@l@?rICrD@dB@lI@l@@jG?rABl@Dn@DnB@bCALNlCvD|AxBnApBZb@vBzCZb@Vb@pB|C\\d@Zf@Zb@nAjBZb@\\h@lBpCXb@\\h@bBbC\\j@\\b@t@dAl@z@d@p@Zd@vB`Dd@x@NRf@r@NRV`@x@jANTPVzB`D^l@b@b@bBhC`@n@\\h@~BpDFDjCrD^j@`@j@fBlC^j@HJV\\X^r@dAZ`@`@h@^l@dBnCb@n@NV|@pAz@pAXb@^h@nDbFRVjD}EHMfDkEhCcDn@a@L?JALMDM@O?GRu@xByCPWNSv@gAJKVLH\\XbAJ\\V`AL`@Nd@XhADJPv@HT"
            }
        ],
        "missing": []
    }
}
```

