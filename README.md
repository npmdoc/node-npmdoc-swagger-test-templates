# api documentation for  [swagger-test-templates (v1.3.0)](https://github.com/apigee-127/swagger-test-templates#readme)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-swagger-test-templates.svg)](https://travis-ci.org/npmdoc/node-npmdoc-swagger-test-templates)
#### Generate test code from a Swagger spec

[![NPM](https://nodei.co/npm/swagger-test-templates.png?downloads=true)](https://www.npmjs.com/package/swagger-test-templates)

[![apidoc](https://npmdoc.github.io/node-npmdoc-swagger-test-templates/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-swagger_test_templates_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-swagger-test-templates/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-swagger-test-templates/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/apigee-127/swagger-test-templates/issues"
    },
    "dependencies": {
        "handlebars": "^3.0.3",
        "json-schema-deref-sync": "^0.3.1",
        "lodash": "^3.10.0",
        "sanitize-filename": "^1.3.0",
        "string": "^3.3.0"
    },
    "description": "Generate test code from a Swagger spec",
    "devDependencies": {
        "chai": "^3.0.0",
        "dotenv": "^1.2.0",
        "eslint": "^0.24.0",
        "js-yaml": "^3.3.1",
        "mocha": "^2.2.5",
        "mocha-eslint": "^0.1.7",
        "ncp": "^2.0.0",
        "rewire": "^2.3.4",
        "walk": "^2.3.9",
        "z-schema": "^3.12.0"
    },
    "directories": {},
    "dist": {
        "shasum": "fe5997234b999daaaf0bedb44d3001cbbc04ed08",
        "tarball": "https://registry.npmjs.org/swagger-test-templates/-/swagger-test-templates-1.3.0.tgz"
    },
    "gitHead": "f03f2cdb1a059c00d871e634342081c440cb5aac",
    "homepage": "https://github.com/apigee-127/swagger-test-templates#readme",
    "keywords": [
        "Swagger",
        "API",
        "Test"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "mohsen",
            "email": "me@azimi.me"
        },
        {
            "name": "swagger",
            "email": "mazimi+swagger@apigee.com"
        },
        {
            "name": "noahdietz",
            "email": "noahdietz24@gmail.com"
        },
        {
            "name": "elsapeng",
            "email": "elsa.peng@hotmail.com"
        }
    ],
    "name": "swagger-test-templates",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+ssh://git@github.com/apigee-127/swagger-test-templates.git"
    },
    "scripts": {
        "pretest": "npm install && node pretest.js",
        "test": "node mocha.js"
    },
    "swagger-test-templates@1.0.0": [
        {
            "name": "Mohsen Azimi",
            "email": "me@azimi.me",
            "url": "http://azimi.me"
        },
        {
            "name": "Linjie Peng",
            "email": "elsa.peng@hotmail.com"
        },
        {
            "name": "Noah Dietz",
            "email": "noahdietz24@gmail.com"
        }
    ],
    "version": "1.3.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module swagger-test-templates](#apidoc.module.swagger-test-templates)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.</span>testGen (swagger, config)](#apidoc.element.swagger-test-templates.testGen)
1.  object <span class="apidocSignatureSpan">swagger-test-templates.</span>helpers

#### [module swagger-test-templates.helpers](#apidoc.module.swagger-test-templates.helpers)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>ifCond (v1, v2, options)](#apidoc.element.swagger-test-templates.helpers.ifCond)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>is (lvalue, rvalue, options)](#apidoc.element.swagger-test-templates.helpers.is)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>length (description)](#apidoc.element.swagger-test-templates.helpers.length)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>pathify (path, pathParams)](#apidoc.element.swagger-test-templates.helpers.pathify)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>printJSON (data)](#apidoc.element.swagger-test-templates.helpers.printJSON)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>setLen (descriptionLength)](#apidoc.element.swagger-test-templates.helpers.setLen)
1.  [function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>validateResponse (type, noSchema, options)](#apidoc.element.swagger-test-templates.helpers.validateResponse)



# <a name="apidoc.module.swagger-test-templates"></a>[module swagger-test-templates](#apidoc.module.swagger-test-templates)

#### <a name="apidoc.element.swagger-test-templates.testGen"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.</span>testGen (swagger, config)](#apidoc.element.swagger-test-templates.testGen)
- description and source-code
```javascript
function testGen(swagger, config) {
  var paths = swagger.paths;
  var targets = config.pathName;
  var result = [];
  var output = [];
  var i = 0;
  var source;
  var filename;
  var schemaTemp;
  var environment;
  var ndx = 0;

  config.templatesPath = (config.templatesPath) ? config.templatesPath : path.join(__dirname, 'templates');

  swagger = deref(swagger);
  source = fs.readFileSync(path.join(config.templatesPath, '/schema.handlebars'), 'utf8');
  schemaTemp = handlebars.compile(source, {noEscape: true});
  handlebars.registerPartial('schema-partial', schemaTemp);
  source = fs.readFileSync(path.join(config.templatesPath, '/environment.handlebars'), 'utf8');
  environment = handlebars.compile(source, {noEscape: true});
  helpers.setLen(80);

  if (config.maxLen && !isNaN(config.maxLen)) {
    helpers.setLen(config.maxLen);
  }

  if (!targets || targets.length === 0) {
    // builds tests for all paths in API
    _.forEach(paths, function(apipath, pathName) {
      result.push(testGenPath(swagger, pathName, config));
    });
  } else {
    // loops over specified paths from config
    _.forEach(targets, function(target) {
      result.push(testGenPath(swagger, target, config));
    });
  }

  // no specified paths to build, so build all of them
  if (!targets || targets.length === 0) {
    _.forEach(result, function(results) {
      output.push({
        name: '-test.js',
        test: results
      });
    });

    // build file names with paths
    _.forEach(paths, function(apipath, pathName) {
      // for output file name, replace / with -, and truncate the first /
      // eg: /hello/world -> hello-world
      filename = sanitize((pathName.replace(/\//g, '-').substring(1))
        + output[i].name);
      // for base path file name, change it to base-path
      if (pathName === '/') {
        filename = 'base-path' + output[i].name;
      }
      output[i++].name = filename;
    });
  } else {
    // loops over specified paths
    _.forEach(targets, function(target) {
      // for output file name, replace / with -, and truncate the first /
      // eg: /hello/world -> hello-world
      filename = sanitize((target.replace(/\//g, '-').substring(1))
        + '-test.js');
      // for base path file name, change it to base-path
      if (target === '/') {
        filename = 'base-path' + '-test.js';
      }
      output.push({
        name: filename,
        test: result[ndx++]
      });
    });
  }

  if (swagger.securityDefinitions) {
    var keys = Object.keys(swagger.securityDefinitions);

    keys.forEach(function(element, index, array) {
      array[index] = _.snakeCase(element).toUpperCase();
    });
    var data = {envVars: keys};
    var envText = environment(data);

    output.push({
      name: '.env',
      test: envText
    });
  }
  return output;
}
```
- example usage
```shell
...
  maxLen: 80,
  pathParams: {
    "id": "0123"
  }
};

// Generates an array of JavaScript test files following specified configuration
stt.testGen(swagger, config);
'''

## API

'swagger-test-templates' module exports a function with following arguments and return values:

### Arguments
...
```



# <a name="apidoc.module.swagger-test-templates.helpers"></a>[module swagger-test-templates.helpers](#apidoc.module.swagger-test-templates.helpers)

#### <a name="apidoc.element.swagger-test-templates.helpers.ifCond"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>ifCond (v1, v2, options)](#apidoc.element.swagger-test-templates.helpers.ifCond)
- description and source-code
```javascript
function ifCond(v1, v2, options) {
  if (arguments.length < 3) {
    throw new Error('Handlebars Helper \'ifCond\' needs 2 parameters');
  }
  if (v1.length > 0 || v2) {
    return options.fn(this);
  }
  return options.inverse(this);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.swagger-test-templates.helpers.is"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>is (lvalue, rvalue, options)](#apidoc.element.swagger-test-templates.helpers.is)
- description and source-code
```javascript
function is(lvalue, rvalue, options) {
  if (arguments.length < 3) {
    throw new Error('Handlebars Helper \'is\' needs 2 parameters');
  }

  if (lvalue !== rvalue) {
    return options.inverse(this);
  } else {
    return options.fn(this);
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.swagger-test-templates.helpers.length"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>length (description)](#apidoc.element.swagger-test-templates.helpers.length)
- description and source-code
```javascript
function length(description) {
  if (arguments.length < 2) {
    throw new Error('Handlebar Helper \'length\'' +
    ' needs 1 parameter');
  }

  if ((typeof description) !== 'string') {
    throw new TypeError('Handlebars Helper \'length\'' +
      'requires path to be a string');
  }

  if (len === -1) { // toggle off truncation, this is a noop
    return description;
  }

  return strObj(description).truncate(len - 50).s;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.swagger-test-templates.helpers.pathify"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>pathify (path, pathParams)](#apidoc.element.swagger-test-templates.helpers.pathify)
- description and source-code
```javascript
function pathify(path, pathParams) {
  var r;

  if (arguments.length < 3) {
    throw new Error('Handlebars Helper \'pathify\'' +
      ' needs 2 parameters');
  }

  if ((typeof path) !== 'string') {
    throw new TypeError('Handlebars Helper \'pathify\'' +
      'requires path to be a string');
  }

  if ((typeof pathParams) !== 'object') {
    throw new TypeError('Handlebars Helper \'pathify\'' +
      'requires pathParams to be an object');
  }

  if (Object.keys(pathParams).length > 0) {
    var re = new RegExp(/(?:\{+)(.*?(?=\}))(?:\}+)/g);
    var re2;
    var matches = [];
    var m = re.exec(path);
    var i;

    while (m) {
      matches.push(m[1]);
      m = re.exec(path);
    }

    for (i = 0; i < matches.length; i++) {
      var match = matches[i];

      re2 = new RegExp('(\\{+)' + match + '(?=\\})(\\}+)');

      if (typeof (pathParams[match]) !== 'undefined' && pathParams[match] !== null) {
        path = path.replace(re2, pathParams[match]);
      } else {
        path = path.replace(re2, '{' + match + ' PARAM GOES HERE}');
      }
    }
    return path;
  }

  r = new RegExp(/(?:\{+)(.*?(?=\}))(?:\}+)/g);
  return path.replace(r, '{$1 PARAM GOES HERE}');
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.swagger-test-templates.helpers.printJSON"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>printJSON (data)](#apidoc.element.swagger-test-templates.helpers.printJSON)
- description and source-code
```javascript
function printJSON(data) {
  if (arguments.length < 2) {
    throw new Error('Handlebar Helper \'printJSON\'' +
    ' needs at least 1 parameter');
  }

  if (data !== null) {
    if ((typeof data) === 'string') {
      return '\'' + data + '\'';
    } else if ((typeof data) === 'object') {
      return '{\n'+prettyPrintJson(data, '        ')+'\n      }';
    } else {
      return data;
    }
  } else {
    return null;
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.swagger-test-templates.helpers.setLen"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>setLen (descriptionLength)](#apidoc.element.swagger-test-templates.helpers.setLen)
- description and source-code
```javascript
function setLen(descriptionLength) {
  len = descriptionLength
}
```
- example usage
```shell
...

swagger = deref(swagger);
source = fs.readFileSync(path.join(config.templatesPath, '/schema.handlebars'), 'utf8');
schemaTemp = handlebars.compile(source, {noEscape: true});
handlebars.registerPartial('schema-partial', schemaTemp);
source = fs.readFileSync(path.join(config.templatesPath, '/environment.handlebars'), 'utf8');
environment = handlebars.compile(source, {noEscape: true});
helpers.setLen(80);

if (config.maxLen && !isNaN(config.maxLen)) {
  helpers.setLen(config.maxLen);
}

if (!targets || targets.length === 0) {
  // builds tests for all paths in API
...
```

#### <a name="apidoc.element.swagger-test-templates.helpers.validateResponse"></a>[function <span class="apidocSignatureSpan">swagger-test-templates.helpers.</span>validateResponse (type, noSchema, options)](#apidoc.element.swagger-test-templates.helpers.validateResponse)
- description and source-code
```javascript
function validateResponse(type, noSchema, options) {
  if (arguments.length < 3) {
    throw new Error('Handlebars Helper \'validateResponse\'' +
      'needs 2 parameters');
  }

  if (!noSchema && type === TYPE_JSON) {
    return options.fn(this);
  } else {
    return options.inverse(this);
  }
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
