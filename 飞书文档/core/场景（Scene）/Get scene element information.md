# Get scene element information

### Get scene element information
Used to support customers in customization

## Request

### POST ip:port/queryTopoInfo
## Request JSON field

## Name

## Required field

## Description

## robotGroup

## Yes

## Robot group

## topoElement

## Yes

## Element name

## elementType

## Yes

## Element type
## Element type
Currently supports two types of elements: point, edge 
WhenelementType is point, the corresponding topoElement is the point name; when it is edge , the topoElement is the name of the edge
## Element name
## Point: LM123
Route: LM123-AP456 represents a one-way route from LM123 to AP456 .
## Response
When the request succeeds, the info field in the response contains the corresponding scene element information. When it fails, the error message contains the error description.
## Point information

## Name

## Type

## Meaning

## name

## string

## Point name

## dir

## double

### Point orientation, unit rad

## ignore_dir

## bool

Whether to ignore point orientation
## Route information

## Name

## Type

## Meaning

## end

## int

## End of route

## start

## int

### Starting point of route

## length

## double

## Route length in m

## max_speed

## double

The maximum speed configured on the route, in m/s, is not set when it is negative

## start_dir

## double

Orientation at the beginning of the route, unit rad

## end_dir

## double

Orientation at the end of the route, unit rad

### Request and response examples
Query RIL_H robot group map, LM10848 to AP10860 route information
## 请求
## {
    "robotGroup": "RIL_H",
    "topoElement": "LM10848-AP10860",
### "elementType": "edge"
## }
## 响应
## {
## "info": {
        "end": 10860,
        "end_dir": -1.5707963267948966,
        "length": 1.7719999999999998,
        "max_speed": -1.0,
        "start": 10848,
        "start_dir": -1.5707963267934772
## }
## }
Query RIL_H robot group map, LM10848 point information
## 请求
## {
    "robotGroup": "RIL_H",
    "topoElement": "LM10848",
### "elementType": "point"
## }
## 响应
## {
## "info": {
        "dir": -1.5707963267948966,
        "ignore_dir": false,
## "name": "LM10848"
## }
## }
Query scene element information of non-existent robot group
## 请求
## {
    "robotGroup": "SEER GROUP",
    "topoElement": "LM10848",
### "elementType": "point"
## }
## 响应
## {
    "error": "No such group: SEER GROUP"
## }
Query non-existent element information
## 请求
## {
    "robotGroup": "RIL_H",
    "topoElement": "F",
### "elementType": "point"
## }
## 响应
## {
### "error": "No such point: F"
## }
Query non-existent element type
## 请求
## {
    "robotGroup": "RIL_H",
    "topoElement": "LM10848",
### "elementType": "region"
## }
## 响应
## {
    "error": "Not supported element type: region"
## }
