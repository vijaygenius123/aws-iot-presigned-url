# Creating A Presigned AWS IoT Websocket URL

## Install Serverless

To install serverless use the command below

    npm install -g serverless

## Project Setup

1.  Create the project

        serverless create --template aws-nodejs --path <Project_Name>

2.  Run the command below to setup serverless-offline plugin. It will help test the system locally.


        npm install serverless-offline --save-dev

3.  Add the below section in the `serverless.yml` file generated below the service

          plugins:
          - serverless-offline

4.  Add the snippet below under the hello handler, to handle http call


        handler: handler.hello
        events:
          - http: GET /

5.  install `aws-iot-device-sdk` using npm install

        npm install aws-iot-device-sdk

6.  Create a function to generate the signed url with the code below. This will return signed url

          "use strict";

          var awsIot = require("aws-iot-device-sdk");

          module.exports.handler = async (event, context) => {
          const options = {
          host: process.env.IOT_ENDPOINT_HOST.toLowerCase(),
          region: process.env.IOT_AWS_REGION
          };

          const url = awsIot.device.prepareWebSocketUrl(
          options,
          process.env.IOT_ACCESS_KEY,
          process.env.IOT_SECRET_KEY
          );

          return {
          statusCode: 200,
          headers: {
          "Access-Control-Allow-Origin": "\*",
          "Access-Control-Allow-Credentials": true
          },
          body: JSON.stringify({ url: url })
          };
          };

7.  Add the snippet below to the functions section in `serverless.yml`


          iotPresignedUrl:
          handler: src/iotPresignedUrl.handler
          timeout: 30
          events:
            - http: OPTIONS /iot-presigned-url
            - http:
                method: GET
                path: /iot-presigned-url
          environment:
            IOT_AWS_REGION: "<Your AWS region>"
            IOT_ENDPOINT_HOST: "<Pick from AWS console IoT -> Settings -> Endpoint>"
            IOT_ACCESS_KEY: "<Access key ID from iot-connector>"
            IOT_SECRET_KEY: "<Secret access key from iot-connector>"

## References

1. https://docs.aws.amazon.com/iot/latest/developerguide/mqtt-ws.html
2. https://www.digitalminds.io/blog/tutorial-real-time-frontend-updates-with-react-serverless-and-websockets-on-aws-iot
