# Echobot

A very simple chatbot.

![chat window](images/chat-with-bot.png)

This simple chatbot is pure CrmScript - it just echoes what the user typed back at them, until the user says "human" or "quit".

The `human` command transfers the session to the queue, where it will be picked up by a fleshbag.

The `quit` command ends the conversation.

If you configure a pre-chat form or FAQ lookup, those are done after the chatbot is finished, not before.

## Installation

1. Create a CrmScript folder named 'Echo'.
2. Place the 3 CrmScripts into the folder.
3. The presence of a script named  `...bot register...` signals the existence of a chatbot in the folder.
4. Go to the Chat admin and open a chat topic.
5. Go to the Chatbot tab and enable the chatbot.
6. Choose the "Echo" folder from the list, and name the chatbot.
7. Save the chat topic.

First create the scripts
![Crm scripts folder](images/crmscripts.png)

Then configure a chat channel to use the scripts folder as a bot:
![chat channel admin](images/channel-admin.png)

Now open a chat window for the chat topic.
You should be greeted by Echobot using the name you gave in step 6.

![chatbot welcome](images/chatbot-welcome.png)

## What Happens

### Bot Registration

When the chat channel (aka topic) is configured and saved, the `...bot register...` script is called.
The CrmScript folder is scanned for additional scripts, and the ones with recognized names are noted.
(So adding scripts after the bot is registered will not be noticed until you disable the bot on the channel.)

The echobot does not do anything, but you can use this to set up or clean up things if you need to.

### Session Created

When a customer clicks the chat button, a new chat session is created.
The chatbot script called `...bot session created...` is called. 
This happens before the FAQ or pre-chat forms are shown.

The echobot posts a welcome message, which changes the sessions status to 6 (user sent last message).
This skips the pre-chat form and FAQ states.

![chat states](images/states.png)

### Message Received

When the user posts a message to the channel, and the bot is active, then the script called
`...bot message received...` is called.

The echobot looks at the message contents from the event data.

If the customer wrote 'human' - we reset the session in order to hand it off.  The session starts back at the beginning, but without the bot active.
This means that the pre-chat form and FAQ are shown after the bot hands off the session, which may not be what you want.
To allow the pre-chat form and FAQ to be processed first, you need more logic. See [echobot2](../echobot2/) for details.

If the customer wrote 'quit' - we close the session.

If neither, we just echo the message back to the customer.