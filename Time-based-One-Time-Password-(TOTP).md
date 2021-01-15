The Time-based One-time Password algorithm (TOTP) is an extension of the HMAC-based One-time Password algorithm (HOTP) that generates a one-time password (OTP) by instead taking uniqueness from the current time.

We define a publisher or player as a subscriber. If time based token enabled, a subscriber should be created for the stream to able to publish or play. Each subscriber has an ID and a code. When a subscriber requests to publish or play a stream, he should provide his ID and time based token generated for his code. Otherwise server doesn't accept the publish or play request.

## Enabling and Setting
You can enable TOTP using Management Panel or in configuration file as
`settings.timeTokenSubscriberOnly=true`
You can also set TOTP period in seconds in configuration file as
`settings.timeTokenPeriod=60`

## Subscriber Operations
You should create subscribers and assign them a base 32 secret to each subscriber. A secret should be in length of multiple of 8 characters.
You can create, delete, list subscribers using REST API as follows:
### Create Subscriber
`curl -X POST -H "Accept: Application/json" -H "Content-Type: application/json" http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscribers -d '{"subscriberId":"publisherA", "b32Secret":"mysecret", "type":"publish"}'`

or

`curl -X POST -H "Accept: Application/json" -H "Content-Type: application/json" http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscribers -d '{"subscriberId":"playerB", "b32Secret":"mysecret", "type":"play"}'`

### Delete subscriber
`curl -X DELETE -H "Accept: Application/json" -H "Content-Type: application/json"
http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscribers/publisherA`

### Delete all subscribers
`curl -X DELETE -H "Accept: Application/json" -H "Content-Type: application/json" http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscribers`
### List All Subscribers
`curl -i -H "Accept: Application/json" -X GET
"http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscribers/list/0/5"`

## Publish Play Stream
A subscriber (publisher or player) should pass subscriber id and generated TOTP to publish or play.
### Publish URL
`http://localhost:5080/WebRTCAppEE/?subscriberId=publisherA&subscriberCode=​ 440456`

### Player URL
`http://localhost:5080/WebRTCAppEE/?subscriberId=playerB&subscriberCode=​ 438610`

## Subscriber Statistics
You can get the stats for each subscriber with the following REST method.

`curl -i -H "Accept: Application/json" -X GET "http://localhost:5080/WebRTCAppEE/rest/v2/broadcasts/stream1/subscriber-stats/list/0/5"`

