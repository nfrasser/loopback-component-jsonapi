# loopback-component-jsonapi

[![Join the chat at https://gitter.im/digitalsadhu/loopback-component-jsonapi](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/digitalsadhu/loopback-component-jsonapi?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://travis-ci.org/digitalsadhu/loopback-component-jsonapi.svg)](https://travis-ci.org/digitalsadhu/loopback-component-jsonapi)
[![npm version](https://badge.fury.io/js/loopback-component-jsonapi.svg)](http://badge.fury.io/js/loopback-component-jsonapi)
[![Dependency Status](https://david-dm.org/digitalsadhu/loopback-component-jsonapi.svg)](https://david-dm.org/digitalsadhu/loopback-component-jsonapi)
[![devDependency Status](https://david-dm.org/digitalsadhu/loopback-component-jsonapi/dev-status.svg)](https://david-dm.org/digitalsadhu/loopback-component-jsonapi#info=devDependencies)

JSONAPI support for loopback.

## JSON API Spec
[http://jsonapi.org/](http://jsonapi.org/)

## Status
This project is a work in progress. Consider it beta software. For ember users, the component
should now be basically feature complete. Please test and report any issues.
The functionality that is present is pretty well tested. 130+ integration tests and counting!

Currently supported:
- Find all records via GET
- Find a record by ID via GET
- Create via POST
- Update a record via PATCH
- Delete a record via DELETE
- All errors have now been transformed into the correct JSON API format
- Find related records via GET eg. /posts/1/comments (belongsTo, hasMany, hasOne)
- Find relationships via GET eg. /posts/1/relationships/author (belongsTo, hasMany, hasOne)
- Creating resource relationship linkages during a resource create
- Updating/deleting resource relationship linkages during a resource update
- [Side loading data](http://jsonapi.org/format/#fetching-includes) via `include` param

Not yet supported:
- manipulating relationships directly via:
  - POST /:resource/relationships/:relatedResource
  - PATCH /:resource/relationships/:relatedResource
  - DELETE /:resource/relationships/:relatedResource

## Requirements
- JSON API v1.0
- loopback ^v2.0.0
- strong-remoting ^v2.22.0

## Sample Project
We have created a sample project using [EmberJS](http://emberjs.com), [Loopback](http://loopback.io) and this compoment. It's called [emberloop](https://github.com/tsteuwer/emberloop).

## Helping out
We are VERY interested in help. See the [issue tracker](https://github.com/digitalsadhu/loopback-component-jsonapi/issues)

## Usage
In your loopback project:

1. `npm install --save loopback-component-jsonapi`
2. Create a `component-config.json` file in your server folder (if you don't already have one)
3. Add the following config to `component-config.json`
```json
{
  "loopback-component-jsonapi": {}
}
```

## Advanced usage:
In a fairly limited way, you can configure a how the component behaves.

Example:
```json
{
  "loopback-component-jsonapi": {
    "restApiRoot": "/api",
    "enable": true,
    "handleErrors": true,
    "exclude": [
      {"model": "comment"},
      {"methods": "find"},
      {"model": "post", "methods": "find"},
      {"model": "person", "methods": ["find", "create"]}
    ]
  }
}
```
### restApiRoot
Url prefix to be used in conjunction with host and resource paths.
eg. http://127.0.0.1:3214/api/people
Default: `/api`

### enable
Whether the component should be enabled or disabled.
Default: true

### handleErrors
When true, the component will unregister all other error handling and
register a custom error handler which always returns errors in jsonapi compliant
format. Validation errors include the correct properties in order to work
out of the box with ember.
Default: true

### exclude
Allows blacklisting of models and methods. (See example above)
Define an array of blacklist objects. Blacklist objects can contain "model" key
"methods" key or both. If just "model" is defined then all methods for the
specified model will not use jsonapi. If just the "methods" key is defined then
all methods specified on all models will be not use jsonapi. If a combination of
"model" and "methods" keys are used then the specific combination of model and methods
specified will not use jsonapi.

#### Please note
The default component behavior currently is to only modify the output of the following
methods on all models to be json api compliant:
- find
- create
- updateAttributes
- deleteById
- findById
- __get__.*
- __findRelationships__.*

And the default current behavior for modifying input only applies to the following methods on
all models:
- create
- updateAttributes

Type: array
Default: null

## Debugging
You can enable debug logging by setting an environment variable:
DEBUG=loopback-component-jsonapi

Example:
```
DEBUG=loopback-component-jsonapi node .
```
