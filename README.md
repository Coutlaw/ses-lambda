# SES-Lambda

This is a simple handler for an AWS Lambda function to handle email events and send them using SES. The events are expected to pass through an API gateway in AWS which can be triggered with an HTTPS requests.

# Usage
Steps to prepare the handler for AWS Lambda
1. The handler needs to be built on a linux OS with the following command

	`GOOS=linux go build -o handler`

2. Once built, the handler.go needs to be compressed in order to be uploaded to aws lambda. Use the following command 

	`zip handler.zip handler` 

Once you have a compressed file you can navigate to [AWS Lambda](https://aws.amazon.com/lambda/) to upload your zip file and test.

The lambda requires 3 **Environment Variables** to run
1. `TO_EMAIL` is the destination for your emails
2. `SENDER` is the source for sent emails 

	_both above emails need to be verified if you are in an AWS sandbox_
3. `SUBJECT` is a default subject line for your emails

Your lamda will need an IAM role with permissions to read/write with SES, and CloudFormation access.


# Event structure
Incoming event body:
```
{
	"email": "incomingEmail@email.com",
	"name": "Johnny Sender",
	"message": "I'm sending you an email"
}
```

Response:
```
// Success
{
  "type": "success",
  "message": "Message is sent"
}

// Error
{
  "type": "error",
  "message": "error message body"
}
```

# Logging
If your lambda has a role that allows communication with cloud formation then you will get event logs for incoming events and outgoing message ID's

## License

This project is licensed under the GNU GENERAL PUBLIC LICENSE License - see the [LICENSE](LICENSE) file for details
