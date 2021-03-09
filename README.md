# devnet-crmscript-chatbot
Chatbot examples - showing how to build a chatbot, and integrate with cloud based services.

## [Echo bot](echobot/)

This simple chatbot is pure CrmScript - it just echoes what the user typed back at them, until the user says "human" or "quit".

The `human` command transfers the session to the queue, where it will be picked up by a fleshbag.

The `quit` command ends the conversation.

## [Dad Joke bot](dad%20joke%20bot/)

A simple chatbot using an external service to get its responses.

Random jokes from https://icanhazdadjoke.com/ until you can't take any more.

## [Botkit bot](botkit%20bot/)

A more complex echobot using the [BotKit framework](https://botkit.ai/)
hosted on [our own server](https://crm-sod.superoffice.com/botkit/) using NodeJS.

## Dialogflow bot

TODO

## Azure LUIS Service

TODO

Chatbot that uses an Azure Language Understanding service to parse and categorize messages from the customer.
The chatbot then has to perform the actions/intents recognized by the language service.

This bot understands two intents:

* joke intent 
* requests intent

You need to set up a LUIS workspace in Azure with these two intents.
