# image-lambda

## AWS Lambda

- create a serverless function and triggered it by S3.
- when we add an image to S3 AWS, will run the Lambda function.
- images.json file will have all images properties that exists in S3.

## images.json link :

- [https://image-lambda-lab17.s3.amazonaws.com/images.json](https://image-lambda-lab17.s3.amazonaws.com/images.json)

## images.json content :

```
[{
    "name" : "caps.jpg",
    "size" : "1000"
},
{
    "name" : "test-image-1.jpg",
    "size" : "2520"
},
{
    "name" : "test-image-2.jpg",
    "size" : "3204"
}]
```

## Lambda function code :

```
    const AWS = require('aws-sdk');
    const s3 = new AWS.S3();
    // const images = require('./images.json')
exports.handler = async (event) => {
    // TODO implement
    console.log('event.Records[0].s3',event.Records[0].s3 )
    console.log('_event_', event)
    let images = []
    let metadata ={ name : event.Records[0].s3.object.key,
    size : event.Records[0].s3.object.size }

    let checkName = false
    for (var i = 0; i < images.length; i++) {
        if (images[i].name == metadata.name) {
            checkName = true
            images[i]=metadata
            break
        }
    }
    let key = event.Records[0].s3.object.key.replace(/\+/g, ' ')

    if (checkName) {
         console.log('images.json file', images)
        // return JSON.stringify(images);
    //     s3.getObject({
    //     Bucket: event.Records[0].s3.bucket.name,
    //     key: key,
    //   })

    } else {

          images.push(metadata)
          console.log('images.json file', images)
        //   console.log('JSON.stringify(images)', JSON.stringify(images))
            // return JSON.stringify(images);
    //       s3.getObject({
    //     Bucket: event.Records[0].s3.bucket.name,
    //     key: key,
    //   })
    }


    // const response = {
    //     statusCode: 200,
    //     body: JSON.stringify('Hello from Lambda!'),
    // };
    return event.Records[0].s3;
};

```
