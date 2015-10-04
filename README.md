# Fleek Parser

[![Join the chat at https://gitter.im/fleekjs/fleek-parser](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/fleekjs/fleek-parser?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Parser module that parses [swagger documentation](http://swagger.io/) json into a single fully dereferenced object. Acts as the linch pin to the [Fleekjs](https://github.com/fleekjs) environment of micro-utilities.

[![Build Status](https://travis-ci.org/fleekjs/fleek-parser.svg)](https://travis-ci.org/fleekjs/fleek-parser) [![npm](https://img.shields.io/npm/l/express.svg)](https://github.com/fleekjs/fleek-parser/blob/master/LICENSE)  [![Dependencies](https://img.shields.io/david/fleekjs/fleek-parser.svg)](https://david-dm.org/fleekjs/fleek-parser)

`$ npm install fleek-parser`

Beyond basic JSON parse:
  - Render all `$ref` within the JSON
  - Render all `$ref` referring to a separate file - TODO
  - Merge `allOf` objects - TODO
  - provide various utilities to access the Swagger document in a non-standard way

## Key

- [Usage](#usage)
  - [Basic](#basic)
- [Utilities](#utilities)
- [Reference Material](#reference-material)
  - [Swagger](#swagger)
- [Authors](#authors)

## Usage

### Basic


_app.js_
```javascript
var parser = require('fleek-parser');

// parse stringified JSON as json object
var swaggerFile = fs.readFileSync('./path/to/swagger.json')
var swagger     = parser.parse(swaggerFile);

// OR

// parse JSON file from path
var swagger = parser.parse("./path/to/swagger.json");
```

_swagger.json_
```JSON
{
  "swagger": "2.0",
  "info": {
    "version": "2.0.0",
    "title": "Swagger Petstore",
    "contact": {
      "name": "Swagger API Team",
      "url": "http://swagger.io"
    }
  },
  "host": "petstore.swagger.io",
  "basePath": "/api",
  "schemes": [
    "http"
  ],
  "paths": {
    "/pets": {
      "get": {
        "tags": [ "Pet Operations" ],
        "summary": "finds pets in the system",
        "responses": {
          "200": {
            "description": "pet response",
            "schema": {
              "type": "array",
              "items": {
                "$ref": "#/definitions/Pet"
              }
            }
          }
        }
      }
    }
  },
  "definitions": {
    "Pet": {
      "type": "object",
      "required": [
        "id"
      ],
      "properties": {
        "id": {
          "type": "integer",
          "format": "int64"
        }
      }
    }
  }
}
```

#### Dereference example

```javascript
console.log(swagger.paths['/pets'].get.responses['200'].schema.items)
// {
//   "type": "object",
//   "required": [
//     "id"
//   ],
//   "properties": {
//     "id": {
//       "type": "integer",
//       "format": "int64"
//     }
//   }
// }
```

## Utilities

Additional properties are attached to the result to simplify usage

```javascript
var parser  = require('fleek-parser');
var swagger = parser.parse('./swager.json');

console.log(swagger.controllers);
console.log(swagger.routeValidationMap);
console.log(swagger.sanitizedRoutes);
```


## Reference Material

### Swagger

- [Home](http://swagger.io/)
- [Editor Demo](http://editor.swagger.io/)
- [Documentation](https://github.com/swagger-api/swagger-spec/blob/master/versions/2.0.md)

## By the authors

- [Hart Engineering](http://engineering.hart.com/)

## Authors

- [John Hofrichter](https://github.com/johnhof)
- [Lan Nguyen](https://github.com/lan-nguyen91)
