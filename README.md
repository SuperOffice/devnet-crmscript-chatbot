# devnet-crmscript-chatbot
Chatbot examples - showing how to build a chatbot, and integrate with cloud based services.

## [Echo bot](echobot/README.md)

This simple chatbot is pure CrmScript - it just echoes what the user typed back at them, until the user says "human" or "quit".

The `human` command transfers the session to the queue, where it will be picked up by a fleshbag.

The `quit` command ends the conversation.

## Dad Joke bot

A simple chatbot using an external service to get its responses.
