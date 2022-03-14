# iamfast

:construction: EXPERIMENTAL :construction:

> IAM policy generation from application code

## Installation

```
npm i -g iamfast
```

## Usage

```
iamfast yourfile.js
```

Execute `iamfast` with the first argument being the file or directory (currently slow!) to be scanned.

### Optional Flags

`--output <outputtype>`: Sets the type of policy to output, currently supporting `json` (default), `yaml` and `sam` (experimental)

## Example

```
> cat tests/js/test1.js
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'us-east-1'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: 'CUSTOMER_LIST',
  Item: {
    'CUSTOMER_ID' : {N: '001'},
    'CUSTOMER_NAME' : {S: 'Richard Roe'}
  }
};

// Call DynamoDB to add the item to the table
ddb.putItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

```
> iamfast tests/js/test1.js
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "dynamodb:PutItem",
            "Resource": [
                "arn:aws:dynamodb:us-east-1:123456789012:table/CUSTOMER_LIST"
            ]
        }
    ]
}
```

## Test

> Tests are still a work in progress

To run tests:

```node
npm test
```
