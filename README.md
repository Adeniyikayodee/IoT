# KayodeAWS_CLOUD - The IoT demo

> A simple Vue.js project demonstrating how AWS IoT can publish messages back to frontend in realtime, bringing messaging to serverless applications.

## Prerequisities

You will need:
- an AWS account
- a vanilla Cognito User Pool
- a IoT Core Device. More details are available in this article: https://itnext.io/using-iot-to-send-messages-back-to-your-serverless-front-end-fb335a099576

## Build Setup

``` bash
# install dependencies
npm install

# CreatePool on AWS-Cognito
AWS Cognito and click Create a User Pool, Copy Pool ID

# CreateAthing on AWS-IOT
Click ‘Manage’ (the ‘Things’ sub-category is auto-selected), click ‘Create Things’ and ‘Create a single thing’

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

