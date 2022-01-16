<template>
  <div></div>
</template>

<script>
export default {
  name: 'IoT',
  created () {
    const AWS = require('aws-sdk')
    const AWSIoTData = require('aws-iot-device-sdk')
    console.log('IoT component created')
    let that = this

    // VARIABLES
    const currentlySubscribedTopic = 'kayodeIoT'
    const AWSConfiguration = {
      poolId: 'us-west-2_O7qBGttQz', // 'CognitoIdentityPoolId'
      host: 'a1hh9tz62gsvfw-ats.iot.us-west-2.amazonaws.com', // 'AwsIoTEndpoint', e.g. 'prefix.iot.us-east-1.amazonaws.com'
      region: 'us-west-2' // 'YourAwsRegion', e.g. 'us-east-1'
    }

    const clientId = 'kayodeIoT' + (Math.floor((Math.random() * 100000000) + 1))
    AWS.config.region = AWSConfiguration.region

    AWS.config.credentials = new AWS.CognitoIdentityCredentials({
      IdentityPoolId: AWSConfiguration.poolId
    })

    const mqttClient = AWSIoTData.device({
      region: AWS.config.region,
      host: AWSConfiguration.host,
      clientId: clientId,
      protocol: 'wss',
      maximumReconnectTimeMs: 8000,
      debug: false,
      accessKeyId: '',
      secretKey: '',
      sessionToken: ''
    })

    const cognitoIdentity = new AWS.CognitoIdentity()
    const getCreds = () => {
      AWS.config.credentials.get((err, data) => {
        if (!err) {
          console.log('retrieved identity: ' + AWS.config.credentials.identityId)
          const params = {
            IdentityId: AWS.config.credentials.identityId
          }
          cognitoIdentity.getCredentialsForIdentity(params, (err, data) => {
            console.log('CREDS:', data)
            if (!err) {
              mqttClient.updateWebSocketCredentials(data.Credentials.AccessKeyId,
                data.Credentials.SecretKey,
                data.Credentials.SessionToken)
            } else {
              console.log('error retrieving credentials: ' + err)
            }
          })
        } else {
          console.log('error retrieving identity:' + err)
        }
      })
    }

    mqttClient.on('connect', () => {
      console.log('mqttClient connected')
      mqttClient.subscribe(currentlySubscribedTopic)
    })

    mqttClient.on('error', (err) => {
      console.log('mqttClient error:', err)
      getCreds()
    })

    mqttClient.on('message', (topic, payload) => {
      const msg = JSON.parse(payload.toString())
      console.log('mqttClient message: ', msg)
      // Send the message back to parent component
      that.$root.$emit('send', msg)
    })
  }
}
</script>
