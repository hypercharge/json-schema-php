# JSON Schema for PHP [![Build Status](https://secure.travis-ci.org/justinrainbow/json-schema.png)](http://travis-ci.org/justinrainbow/json-schema)

A PHP Implementation for validating `JSON` Structures against a given `Schema`.

See [json-schema](http://json-schema.org/) for more details.

## Installation

### Library

    $ git clone https://github.com/justinrainbow/json-schema.git

### Dependencies

#### [`Composer`](https://github.com/composer/composer) (*will use the Composer ClassLoader*)

    $ wget http://getcomposer.org/composer.phar
    $ php composer.phar install

## Usage

```php
<?php

// Get the schema and data as objects
$retriever = new JsonSchema\Uri\UriRetriever;
$schema = $retriever->retrieve('file://' . realpath('schema.json'));
$data = json_decode(file_get_contents('data.json'));

// If you use $ref or if you are unsure, resolve those references here
// This modifies the $schema object
$refResolver = new JsonSchema\RefResolver($retriever);
$refResolver->resolve($schema, 'file://' . __DIR__);

// Validate
$validator = new JsonSchema\Validator();
$validator->check($data, $schema);

if ($validator->isValid()) {
    echo "The supplied JSON validates against the schema.\n";
} else {
    echo "JSON does not validate. Violations:\n";
    foreach ($validator->getErrors() as $error) {
        echo sprintf("[%s] %s\n", $error['property'], $error['message']);
    }
}
```

## Running the tests

There are two test groups:

1. unit and integration tests provided by [json-schema](#)
1. the testcases provided by [json-schema-test-suite](https://github.com/json-schema/JSON-Schema-Test-Suite)

### Unit and Integration Tests

    $ phpunit

### json-schema-test-suite draft v3 Tests

    $ phpunit test/suite/SuiteTest.php

To run the remote tests in json-schema-test-suite see [README](tests/suite/server/README.md).

