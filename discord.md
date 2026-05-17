# Configure Discord

## Configure Developer Portal

### Create Bot Application

https://discord.com/developers/home

1. Create a new application > Create blank app.
2. Give it a name.
3. Click "Save changes".

### Get Application ID

Go to developer portal, select your application:

1. Go to "General Information"
2. Copy "Application ID"

### Get Bot Token

Go to developer portal, select your application:

1. Go to "Bot"
2. Reset Token (click "Reset Token" button)
3. Copy Token

### Bot Permissions

Go to developer portal, select your application > Bot > Authorization Flow:

- Public Bot > Enable
- Requires OAuth2 Code Grant > Disable
- Privileged Gateway Intents > Presence Intent > Enable
- Privileged Gateway Intents > Server Members Intent > Enable
- Privileged Gateway Intents > Message Content Intent > Enable

Bot Permissions > Administrator

Save Changes

### Configure OAuth2

Remarks: Create a server first `Create My Own` > `For a club or community`, select it during OAuth2 verification.

OAuth2 > URL Generator:
- Scopes: bot
- Scopes: applications.commands
- Bot Permissions > Administrator
- Integration Type > Guild Install
- Copy and open the Generated URL in browser to install the bot in your server

Select the newly created server in the `Add to Server` selection menu.

## Configure Discord Client

### Enable Developer Mode

Log in to Discord app or page, go to User Settings > Advanced > Developer Mode and enable it.

### Get User ID

Right click on your user avatar in Discord and select "Copy ID".

### Get Server ID

1. Create a server, and add your bot to it.

2. Right click on your server name in Discord and select "Copy ID".

### Get Channel ID

1. Create a text channel in your server.
2. Right click on your channel name in Discord and select "Copy ID".

### Configure Channel Permissions

Channel Permissions > Advanced Permissions, enable:

- View Channel
- Send Messages
- Send Messages in Threads
- Embed Links
- Attach Files
- Mention @everyone, @here, and All Roles
- Use Application Commands
- Manage Threads
- Create Public Threads
- Create Private Threads

# Export DISCORD_BOT_TOKEN

Edit `~/.bashrc` and add:

```bash
export DISCORD_BOT_TOKEN=xxx
```

# Verifiy Direct Message with Bot in Discord Client

This is to verify that the bot can receive messages and send messages.

# Working with Channels

Send at least one direct message to the bot before working with agents with channels.