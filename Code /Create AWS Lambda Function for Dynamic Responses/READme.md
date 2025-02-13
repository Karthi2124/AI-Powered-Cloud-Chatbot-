import json
import boto3

dynamodb = boto3.resource("dynamodb")
table = dynamodb.Table("ChatbotResponses")

def lambda_handler(event, context):
    user_message = event["inputTranscript"]
    
  Query the DynamoDB table for the response
    response = table.get_item(Key={"query": user_message})
    
  if "Item" in response:
        bot_reply = response["Item"]["response"]
    else:
        bot_reply = "Sorry, I don't have an answer for that."

   return {
        "dialogAction": {
            "type": "Close",
            "fulfillmentState": "Fulfilled",
            "message": {"contentType": "PlainText", "content": bot_reply}
        }
    }
