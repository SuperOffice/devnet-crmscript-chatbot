# Dialogflow chatbot

This is a more complex example using Google Dialogflow. You need to configure DialogFlow and put the
appropriate configuration values into the `chatbotConfig.crm` include script,

The chatbot uses blob table to store access and refresh tokens used to access the Google DialogFlow service.

## Installation

1. Create a CrmScript folder named 'Dad Joke bot'.
2. Place the 7 CrmScripts into the folder. Some are include scripts, so ensure their include names are set.
3. The presence of a script named  `...bot register...` signals the existence of a chatbot in the folder.
4. Go to the Chat admin and open a chat topic.
5. Go to the Chatbot tab and enable the chatbot.
6. Choose the "Dad Joke bot" folder from the list, and name the chatbot.
7. Save the chat topic.

Now open a chat window for the chat topic.
You should be greeted by the bot using the name you gave in step 6.

![chatbot cjat](images/chat.png)

## What Happens

### Bot Registration

When the chat channel (aka topic) is configured and saved, the `...bot register...` script is called.
The CrmScript folder is scanned for additional scripts, and the ones with recognized names are noted.
(So adding scripts after the bot is registered will not be noticed until you disable the bot on the channel.)

### Session Created

When a customer clicks the chat button, a new chat session is created.
If the chat channel the session is in has a chatbot enabled, then the chatbot script called `...bot session created...` is called. 

The dadjokebot posts a message of instructions, then gets a new joke from the api, and posts that too.


### Message Received

When the user posts a message to the channel, and the bot is active, then the script called
`...bot message received...` is called.
The script needs to analyze the message and either

* post one or more reply messages.
* transfer the chat session to the queue for humans to handle.
* change the session status to end the session.

The dadjokebot will get a new joke and post it to the chat if the user's message does not match the "human"
or "quit" commands.  

---  


![Dialogflow](https://upload.wikimedia.org/wikipedia/en/c/c7/Dialogflow_logo.svg)  
  

## Getting started with DialogFlow 

*** 

This is a short description on how to setup DialogFlow for your SuperOffice installation.  
This is not to be considered as best practice, but rather as a quick start to quickly get up and running.  

There are a couple of steps that needs to be taken in order to get get this to work:

- [ ] Create your DialogFlow agent.
- [ ] Generate client ID and client secret.
- [ ] Generate access and refresh tokens needed for the integration.  
- [ ] Add DialogFlow specific data into SuperOffice scripts.  

### *Extras:*
- Create a new intent in DialogFlow.  

***  

### Create your DialogFlow agent

> *More information regarding DialogFlow can be found in* *[DialogFlow documentation](https://cloud.google.com/dialogflow/docs)* 

- Head over to [DialogFlow console](https://dialogflow.cloud.google.com/)
- Create a [new agent](https://cloud.google.com/dialogflow/es/docs/agents-overview) 

 
We now have our DialogFlow agent up and running.  
- [x] Create your DialogFlow agent.  
---
### Get your client ID and secret
- Head over to [Google cloud console](https://console.cloud.google.com/)
- From the cloud console, navigate to *___APIs & services___* - *___OAuth consent screen___*.
  - For the sake of this demo, we will choose *User Type - External*
  - Fill in the necessary details.
  - Enter your email under *Test users*.
  - Finish the OAuth consent screen configuration.
- Next, go to the ___Credentials___ menu, and create a new *___OAuth client ID___*.
  - *Application type* - ___Web application___.
  - Under *Authorized redirect URIs* enter: ___https://developers.google.com/oauthplayground___
  - Note your Client ID and secret.  
  
  
With these ID's we will now obtain our access and refresh tokens. These values will be entered to the SuperOffice integration.
- [x] Get your client ID and secret  

---  

### Get your access and refresh tokens

- Head over to [Google Oauthplayground](https://developers.google.com/oauthplayground/)
- Select the cog wheel at the top right corner,click "*Use your own OAuth credentials*" and enter your client ID and secret.
- Under *Select & authorize APIs*, input your own scope: https://www.googleapis.com/auth/dialogflow
  - Authorize APIs
  - Give access to app.
  - Exchange your authorization code for tokens.  
- Store your refresh token on a secure location.  
  
Using this refresh token, we can now proceed and finalize the configuration in SuperOffice.  
- [x] Get your client ID and secret  
---  
### Enter your DialogFlow specific data into SuperOffice

In the chatbotConfig.crm, enter the values obtained from the previous steps.

```
//Project id in DialogFlow console
String project_id  = 'x';

//What is your client ID?
String client_id = 'X';

//What is your client secret?
String client_secret = 'x';
```

The tokens are stored in the BinaryObject table. To insert your token, enter your refresh token into InsertTokens.crm:

```
String access_token = "X";
String refresh_token ="X";
```  
- [x] Enter your DialogFlow specific data into SuperOffice  
---  
### Extras:

#### Create a new intent in DialogFlow:
- Head to intents, choose "___Create intent___".
  - Give it a name 
  > Intent names that starts with agentHandover_ will be seen as a Handover intent in SuperOffice. If such intent is returned, the session will be handed over to a human.
  - Add some [trainings phrases](https://cloud.google.com/dialogflow/es/docs/intents-training-phrases). This is phrases a user typically would use for asking a question.
  - Add a Text response. This is the text that will be presented in the chat.

 ### Other responses   
    
- If you add a second Text response box - this message will be sent as a second message to SuperOffice.
- To send buttons in your reply, add a *Custom Payload* response to your intent. Send a JSON "options", which should contain an array of the buttons you want to present:

```
{
  "options": [
    "Button 1",
    "Button 2"
  ]
}
````

