# 🤖 AI-Powered Cloud Chatbot

An **AI-powered chatbot** that interacts with users using **AWS Lex, AWS Lambda, and DynamoDB**, providing dynamic responses based on a knowledge base stored in the cloud.

## 🚀 Features
- **Natural Language Processing (NLP)** using AWS Lex
- **Serverless Backend** with AWS Lambda
- **DynamoDB Integration** for storing chatbot responses
- **API Gateway** for web access
- **Optional React Frontend** for user interaction

---

## 🛠️ Tech Stack
- **AWS Lex** – Chatbot NLP processing
- **AWS Lambda** – Backend logic
- **AWS DynamoDB** – Database for storing responses
- **AWS API Gateway** – Exposing chatbot as an API
- **React.js (Optional)** – Web UI for chatbot

---

## 📌 Setup Guide

### **Step 1: Create AWS Lex Chatbot**
1. Go to **AWS Console → Amazon Lex**.
2. Create a new bot (e.g., `CloudChatBot`).
3. Define **Intents**, **Utterances**, and **Responses**.
4. Attach a Lambda function for dynamic responses.
5. Deploy and test the chatbot.

### **Step 2: Create AWS Lambda Function**
1. Go to **AWS Console → AWS Lambda → Create Function**.
2. Choose **Python 3.x** as the runtime.
3. Add the following code:

```python
import json
import boto3

dynamodb = boto3.resource("dynamodb")
table = dynamodb.Table("ChatbotResponses")

def lambda_handler(event, context):
    user_message = event["inputTranscript"]
    response = table.get_item(Key={"query": user_message})
    
    bot_reply = response["Item"].get("response", "Sorry, I don't have an answer for that.")
    
    return {
        "dialogAction": {
            "type": "Close",
            "fulfillmentState": "Fulfilled",
            "message": {"contentType": "PlainText", "content": bot_reply}
        }
    }
```

4. Deploy the function and copy its **ARN**.
5. Attach it to AWS Lex **intent fulfillment**.

### **Step 3: Set Up DynamoDB**
1. Go to **AWS DynamoDB → Create Table**.
2. Table Name: `ChatbotResponses`.
3. Primary Key: `query` (String).
4. Add sample data:

| query | response |
|---------------------------|------------------------------------------------|
| "What is cloud computing?" | "Cloud computing delivers computing services over the internet." |
| "How to create an EC2 instance?" | "Go to AWS EC2, select 'Launch Instance,' and configure settings." |

### **Step 4: Deploy API Gateway**
1. Go to **AWS API Gateway → Create API**.
2. Create a **POST method** and connect it to your **Lambda function**.
3. Deploy the API and copy the **API Endpoint URL**.

### **Step 5 (Optional): React Frontend**
1. Create a new React app:
   ```bash
   npx create-react-app chatbot-ui
   cd chatbot-ui
   npm start
   ```
2. Modify `App.js`:

```javascript
import React, { useState } from "react";

function App() {
  const [message, setMessage] = useState("");
  const [response, setResponse] = useState("");

  const sendMessage = async () => {
    const res = await fetch("YOUR_API_GATEWAY_URL", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ userInput: message }),
    });
    const data = await res.json();
    setResponse(data.reply);
  };

  return (
    <div>
      <h1>Cloud Chatbot</h1>
      <input value={message} onChange={(e) => setMessage(e.target.value)} />
      <button onClick={sendMessage}>Send</button>
      <p>Bot: {response}</p>
    </div>
  );
}

export default App;
```
3. Run the app and test the chatbot!

---

## 🎯 Final Outcome
✅ **A fully functional AI chatbot** deployed on AWS, accessible via API or web frontend, with dynamic responses stored in DynamoDB.

### 📢 Future Enhancements
- 🔹 Add **voice support** using Amazon Polly
- 🔹 Deploy as a **Telegram/WhatsApp bot**
- 🔹 Use **Google Dialogflow** as an alternative to AWS Lex

📌 **Contributions are welcome!** Feel free to fork and improve this project. 🚀
