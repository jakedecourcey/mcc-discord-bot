## Usage
/stats [gamertag: xxx]
/performance [gamertag: xxx]

If you do not specify a gamertag, it will search for your Discord username.

## Installation
If you are a discord admin, simply click this link and accept the terms, then add the bot to the appropriate channels. Commands will work immediately.

https://discord.com/api/oauth2/authorize?client_id=876326120826470411&permissions=2048&scope=applications.commands%20bot

## Architecture
Discord bot >> AWS API Gateway >> AWS Lambda >> AWS Lambda >> Discord Webhook

This bot requires almost no permissions - only to send messages in its channels.

The bot generates a webhook containing the command, any extra options, and all the basic information about the user, etc. The API Gateway receives this information, validates it, and triggers a Lambda function.

Discord requires a response within 3000ms, so the first Lambda function merely returns an ACK back to the API Gateway, telling Discord to wait, then passes the payload to another function. The second functions calls that stats API and performs all necessary logic, then calls Discord's API using a token to replace the "please wait" message with the real output. 

The stats are pulled from https://autocode.com/lib/halo/mcc/

Even without accounting for the AWS free tier, this bot costs less than $5 per million requests.

## TODO
Squad performance
Unit tests
Terraform everything maybe
