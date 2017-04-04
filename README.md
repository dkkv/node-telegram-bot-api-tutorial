# node-telegram-bot-api-tutorial


This is a beginners guide for [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api) .


 - [Creating new bot with BotFather](#Creating+new+bot+with+BotFather)
 - [First message](#First+message)
 - [Commands](#Commands)
 - [Keyboards](#Keyboards)
 - [User](#User)
 - [Inline keyboards](#Inline+Keybords)
 - [parse_mode](#parse_mode)
 - [Location and Number](#Location+and+Number)
 - [Interacting with groups and channels](#grpups+and+channel+interaction)
 
 <a name="Creating+new+bot+with+BotFather"></a>
### Creating new bot with BotFather
To create a bot on Telegram messenger firstly you need to contact with @BotFather .So go ahead and search for @BotFather in your messenger.Once you got there you should ask BotFather to give you a token.  
You could do this by typing  "/newbot" and sending it to BotFather.  
Dear Father will ask you what you want to call you bot and you'll chose a name , and then you need to make a username for your bot , actually something that ends with the word 'bot' like: "my_test_bot".  
If you write a username which is available, BotFather will send you a token.  
Grab that token and keep somewhere safe.

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/BotFather.JPG" height="500" width="400">

Now that you bot is created maybe you want to set a description for that.
description are those messages showing in middle of the page usually  describing what this bot can do.

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/desc.JPG" height="500" width="500">

To set description for you bot in BotFather write "/setdescription" and send , then chose the bot you mean to change it's description and send description you want to be shown in you bot.

 <a name="First+message"></a>
### First message
Ok now you'r ready to go. Create a node project and install bot-api:

    npm install --save node-telegram-bot-api
    
   Create a file index.js (or any other name) and inside the file require node-telegram-bot-api:
   
```js
const TelegramBot = require('node-telegram-bot-api');
```
   Then you need to assign your token witch you got from BotFather:
   
```js
const token = 'YOUR_TELEGRAM_BOT_TOKEN';
```
   And now create a new bot :
   
```js
const bot = new TelegramBot(token, {polling: true});
```
  Lets try out our bot and do some real world things .We need to get messages that user sends us , to do so we would use following code:
  
```js
bot.on('message', (msg) => {
    
     //anything
     
});
```
Lets create simple greeting here. Here's big picture of our code :
```js
const TelegramBot = require('node-telegram-bot-api'); 
const token = 'YOUR_TELEGRAM_BOT_TOKEN';
const bot = new TelegramBot(token, {polling: true});
    
bot.on('message', (msg) => {
    
  //anything
     
});
```
  We were trying to greet and we'll do it here:
 
```js
bot.on('message', (msg) => {
    
var Hi = "hi";
if (msg.text.toLowerCase().indexOf(Hi) === 0) {
bot.sendMessage(msg.chat.id,"Hello dear user");
} 
    
});
```
Ok , now open up you command prompt and type:

    node index.js
  Go to your bot and hit on "/start" and then type "Hi" to it:
  
<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/first%20message.JPG" height="500" width="400">

  So now that you know how to send and receive messages in your bot you may want to put some salt on it:
  
```js
bot.on('message', (msg) => {

var Hi = "hi";
if (msg.text.toLowerCase().indexOf(Hi) === 0) {
bot.sendMessage(msg.chat.id,"Hello dear user");
} 
    
var bye = "bye";
if (msg.text.toLowerCase().includes(bye)) {
bot.sendMessage(msg.chat.id, "Hope to see you around again , Bye");
} 

});
```
This time we're using "includes" method so if user sends us anything containing "bye" word we'll send him back the message:

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/Bye.JPG" height="500" width="400">
And definitely you can use any other string method that you want.


 <a name="Commands"></a>
### Commands
That's really common to send user a message describing use of bot while he taps on "/start". (these are called [commands](https://core.telegram.org/bots#commands))
To do so :
```js
bot.onText(/\/start/, (msg) => {

bot.sendMessage(msg.chat.id, "Welcome");
    
});
```
  Lets create another command that will send a picture to user:
  
```js
bot.onText(/\/sendpic/, (msg) => {

bot.sendPhoto(msg.chat.id,"https://www.somesite.com/image.jpg" );
    
});
```
So now if you write "/sendpic" on your bot an image will be sent.
Sending audios is same and simple you can use "[sendAudio](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#TelegramBot+sendAudio)" method .

Now you might have seen some pictures containing caption with them like the following picture.

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/CaptionJPG.JPG" height="500" width="400">


Well , How to to create these?
Answer is really simple you can send a caption with option on photo like so :
```js
bot.onText(/\/sendpic/, (msg) => {

bot.sendPhoto(msg.chat.id,"https://www.somesite.com/image.jpg",{caption : "Here we go ! \nThis is just a caption "} );
    
});
```
  So now you know how to create captions and how to go to new line in your messages by typing \n .

 <a name="Keyboards"></a>
### Keyboards
Lets go a step further and start working with [keyboards](https://core.telegram.org/bots#keyboards).
keyboards are atcually the ones shown in this picture:

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/keyboard.jpg" height="500" width="400">


Keyboards are nothing but an easy way to send messages. It's like your not forcing users to write something down and send it to bot but instead your demonstrating them some options that they can tap on and a message will be sent after that.
So lets see how we can create Keyboards , we'll send Keyboards on "/start" message:
```js
bot.onText(/\/start/, (msg) => {
    
bot.sendMessage(msg.chat.id, "Welcome", {
"reply_markup": {
    "keyboard": [["Sample text", "Second sample"],   ["Keyboard"], ["I'm robot"]]
    }
});
    
});
```
So now if you run you will see:

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/Keyboards.JPG" height="500" width="400">

As I said in fact Keyboards are not nothing but automatic type and send for user. There is no difference if you write "I'm robot" and sending on your own or you click on Keyboard. Lets do something simple when that "I'm robot"  is received.So add this up to your previous on message:
```js
bot.on('message', (msg) => {
var Hi = "hi";
if (msg.text.toLowerCase().indexOf(Hi) === 0) {
    bot.sendMessage(msg.chat.id, "Hello dear user");
}
var bye = "bye";
if (msg.text.toLowerCase().includes(bye)) {
    bot.sendMessage(msg.chat.id, "Hope to see you around again , Bye");
}    
var robot = "I'm robot";
if (msg.text.indexOf(robot) === 0) {
    bot.sendMessage(msg.chat.id, "Yes I'm robot but not in that way!");
}
});
```
   So now if you go to your bot tap on start you see Keyboards and if you tap on I'm robot you'll see the message. Note that there is no difference if you type it or you send it by Keyboards.

 <a name="User"></a>
### User
node-telegram-bot-api does not have any method to get users information but in case if you want to, you can get information like so:
```js
var Hi = "hi";
if (msg.text.toLowerCase().indexOf(Hi) === 0) {
    bot.sendMessage(msg.from.id, "Hello  " + msg.from.first_name);
}
```

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/usersname.JPG" height="500" width="400">

And if you wanted to get user profile pictures you can use [getUserProfilePhotos](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#telegrambotgetuserprofilephotosuserid-options--promise) .

 <a name="Inline+Keybords"></a>
### Inline Keybords 


<a name="parse_mode"></a>
### parse_mode
If you want to send messages with some style there, here is how it goes. <code>parse_mode</code> defines how you want you message to be renedered.You can define it inside in your options when sending message.
Available option are HTML and Markdown.Lets see how it works in action:
```js
bot.on('message', (msg) => {

 var Hi = "hi";
 if (msg.text.toLowerCase().indexOf(Hi) === 0) {
   bot.sendMessage(msg.chat.id,"<b>bold</b> \n <i>italic</i> \n <em>italic with em</em> \n <a href=\"http://www.example.com/\">inline URL</a> \n <code>inline fixed-width code</code> \n <pre>pre-formatted fixed-width code block</pre>" ,{parse_mode : "HTML"});
   }
});
```
So you get idea where <code>parse_mode</code> is defined.Now if we run this:

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/parse_mode.JPG" height="500" width="400">

So that's how we do it with Html you can also write with Markdown.As mentioned in official api of Telegram only these tags are supported till now:

```
<b>bold</b>, <strong>bold</strong>
<i>italic</i>, <em>italic</em>
<a href="http://www.example.com/">inline URL</a>
<code>inline fixed-width code</code>
<pre>pre-formatted fixed-width code block</pre>
```
And for Markdown you can only use:
```
*bold text*
_italic text_
[text](http://www.example.com/)
`inline fixed-width code`
```
 <a name="Location+and+Number"></a>
### Location and Number
There are some methods that enable you to send users location.  
Here is an example:
```js
bot.on('message', (msg) => {
    var location = "location";
    if (msg.text.indexOf(location) === 0) {
        bot.sendLocation(msg.chat.id,44.97108, -104.27719);
        bot.sendMessage(msg.chat.id, "Here is the point");

    }
});

```

<img src="https://raw.githubusercontent.com/hosein2398/node-telegram-bot-api-tutorial/master/pics/Location.JPG" height="500" width="400">


And there is also [sendVenue](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#telegrambotsendvenuechatid-latitude-longitude-title-address-options--promise).  
To send a phone number you can use [sendContact](https://github.com/yagop/node-telegram-bot-api/blob/master/doc/api.md#TelegramBot+sendContact).

 <a name="grpups+and+channel+interaction"></a>
### Interacting with groups and channels
One interesting use of bots is in goupes and channels.You can use bots to manage groupes , receiving users messages and sending groupe messages. You can even send keyboards to members so that they can use the bot easier.  
For better understanding of how bots can be useful in goupes let create one and add it a group.  
So we want to create a bot that says "Have a nice day" when any member of group said something containing "Bye" keyword.
Before doing anything you have to know that a bot has no accessibility to users messages unless we add it as of a administrator.
And if don't do that bot will only has access to [Commands](https://github.com/hosein2398/node-telegram-bot-api-tutorial#commands) that any user sends to group.And all of this is because of the [Privacy Policy](telegram.com) that Telegram messenger follows which I think is right.
So lets get back to creating that bot , actualy we don't have to do anything diffrent than creating bot for a single user usage , everything is similiar.

More coming up soon.