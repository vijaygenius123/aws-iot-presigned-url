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
