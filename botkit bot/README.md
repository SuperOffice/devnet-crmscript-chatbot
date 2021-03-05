# BotKit Framework bot

The [BotKit framework](https://botkit.ai/) is on open-source framework for bots. It provides support for
conversations, regular expressions, hooks for language understanding services, and lots more.

Glitch is free, open programming platform we can use to host a simple demo bot.
[Glitch botkit](https://glitch.com/~botkit-botframework)

Our botkit remix lives here: https://mint-strong-chinchilla.glitch.me/
There is a simple web chat there you can use to test the bot.

It has no state, and just uses some simple matching rules. It is a glorified echo bot.

* "sample" -> I heard a sample message.
* "foo" -> I heard "foo" via a function test
* "123" -> I heard a number using a regular expression.
* "ABC" -> I HEARD ALL CAPS!
* wildcard -> Echo wildcard

We post a request to `https://mint-strong-chinchilla.glitch.me/api/messages` 

```json
{   "type":"message",
    "text":"see",
    "user":"28ef498a-936d-9e6a-8117-bb7a20d43e9c",
    "channel":"webhook" }
```

and get back one or more messages:

```json
[ {
    "type": "message",
    "text": "Echo: see"
  } ]
```

## Installation

1. Create a CrmScript folder named 'Botkit'.
2. Place the 3 CrmScripts into the folder.
3. The presence of a script named  `...bot register...` signals the existence of a chatbot in the folder.
4. Go to the Chat admin and open a chat topic.
5. Go to the Chatbot tab and enable the chatbot.
6. Choose the "Botkit" folder from the list, and name the chatbot.
7. Save the chat topic.

Now open a chat window for the chat topic.
You should be greeted by the Botkit using the name you gave in step 6.

![chatbot cjat](images/chat.png)

## What Happens

### Bot Registration

When the chat channel (aka topic) is configured and saved, the `...bot register...` script is called.
The CrmScript folder is scanned for additional scripts, and the ones with recognized names are noted.

### Session Created

When a customer clicks the chat button, a new chat session is created.
If the chat channel the session is in has a chatbot enabled, then the chatbot script called `...bot session created...` is called. 

The `bot session created` script takes the message from the user, calls the botkit API with it, and gets back a result.

posts a message of instructions, then gets a new joke from the api, and posts that too.


### Message Received

When the user posts a message to the channel, and the bot is active, then the script called
`...bot message received...` is called.
The script needs to analyze the message and either

* post one or more reply messages.
* transfer the chat session to the queue for humans to handle.
* change the session status to end the session.

The dadjokebot will get a new joke and post it to the chat if the user's message does not match the "human"
or "quit" commands.
