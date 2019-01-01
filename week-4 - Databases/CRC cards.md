# CRC Cards- Class Responsibility Collaboration

```
----------------------------------------------------------------------
Class/object                      |
----------------------------------------------------------------------
Responsibilities of               | Collaborators - what other classes
the class (methods)               | will it interact with
                                  |
                                  |
                                  |
                                  |
----------------------------------------------------------------------                                  
```


```
As a customer
so I can get a bicycle
I want to withdraw a bicycle from a docking station
```
```
As a customer
So I can complete my trip
I want to dock a bicycle back at a docking station
```
```
As a customer
So I can have the best cycling experience
I want to only get good bikes from the dock
```
```
As an administrator
so I can tell how many bikes are at each docking station
I want to get a count of the number of bikes at the docking station
```

```
----------------------------------------------------------------------
Bike                              |
----------------------------------------------------------------------
working?                          |
                                  |
                                  |
                                  |
                                  |
                                  |
----------------------------------------------------------------------                                  
```

```
----------------------------------------------------------------------
Docking Station                   |
----------------------------------------------------------------------
withdraw                          |  Bike
dock                              |
@storage                          |
                                  |
                                  |
                                  |
----------------------------------------------------------------------                                  
```

```
Database: Boris_Bikes

Tables

Bike                              |    Docking station
----                              |    ---------------
id                                |    id
working                           |    storage
docking_station_id = FK           |

docking_station_id references bike id
* reference goes in the table there are many of

```
