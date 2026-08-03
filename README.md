PO3 Discord Bot — Setup Guide

This bot does 3 things automatically:

Welcomes new members — posts in #welcome, DMs them, assigns the Student role
Creates a private 1-on-1 channel for every new member (only they + you can see it)
Auto-answers FAQs — replies when someone types a keyword like "payout" or "homework" (edit the FAQ dict in bot.py anytime to add more)
Step 1 — Create the bot on Discord's Developer Portal
Go to https://discord.com/developers/applications
Click New Application, name it (e.g. "PO3 Bot")
Go to the Bot tab → click Add Bot
Under Privileged Gateway Intents, turn ON:
SERVER MEMBERS INTENT
MESSAGE CONTENT INTENT
Click Reset Token → copy the token. Keep this secret — this is your BOT_TOKEN.
Step 2 — Invite the bot to your server
Go to the OAuth2 → URL Generator tab
Under Scopes, check bot
Under Bot Permissions, check: Manage Roles, Manage Channels, Send Messages, Read Message History, View Channels
Copy the generated URL, open it in your browser, and add the bot to your PO3 server
Step 3 — Get the IDs you need

Turn on Developer Mode in Discord (User Settings → Advanced → Developer Mode), then right-click to "Copy ID" on each of these:

GUILD_ID — right-click your server icon
WELCOME_CHANNEL_ID — right-click #welcome
STUDENT_ROLE_ID — Server Settings → Roles → right-click @Student (create this role first if you haven't)
ONE_ON_ONE_CATEGORY_ID — right-click the "PRIVATE — 1-ON-1" category (create this category first)
MENTOR_ROLE_ID (optional) — if you have a staff/mod role that should also see every 1-on-1 channel

Important: make sure the bot's role in Server Settings → Roles is positioned above the @Student role, or it won't be able to assign it.

Step 4 — Set your environment variables

You'll set these wherever you host the bot (see Step 5). They are:

BOT_TOKEN=your_bot_token_here
GUILD_ID=your_server_id
WELCOME_CHANNEL_ID=your_welcome_channel_id
STUDENT_ROLE_ID=your_student_role_id
ONE_ON_ONE_CATEGORY_ID=your_category_id
MENTOR_ROLE_ID=your_mentor_role_id   (optional)
Step 5 — Host it (simplest/cheapest options)

You need somewhere that keeps the bot running 24/7 — your own computer won't work unless it's always on.

Easiest paid option: Railway.app

Create a free account at https://railway.app
New Project → Deploy from GitHub repo (push this folder to a GitHub repo first) or use their CLI to deploy directly
Add the environment variables from Step 4 in Railway's dashboard (Variables tab)
Railway auto-detects requirements.txt and runs python bot.py
Cost: Railway's free trial credit covers a small bot for a while; after that it's usually a few dollars a month for something this size

Alternative: a cheap VPS (DigitalOcean, Oracle Cloud Free Tier)

Spin up the smallest Ubuntu instance (Oracle Cloud has a genuinely free tier)
SSH in, install Python 3.11+, pip install -r requirements.txt --break-system-packages
Set the environment variables (export BOT_TOKEN=... etc, or use a .env file with python-dotenv)
Run with python3 bot.py inside a screen or tmux session, or set it up as a systemd service so it restarts automatically

Not recommended long-term: Replit's free tier will put the bot to sleep — workarounds exist (UptimeRobot pinging it) but they're unreliable and against some hosts' terms. Fine for testing, not for a paying-client server.

Editing the FAQ later

Open bot.py, find the FAQ = { ... } dictionary near the top, and add or edit entries like:

python
"risk management": "Check #documents-and-templates for the risk management template 📂",

No restart needed beyond redeploying — most hosts (Railway included) redeploy automatically when you push a change.

Testing before going live

Run it locally first to make sure it connects (you'll still need real IDs and a test server):

bash
pip install -r requirements.txt
export BOT_TOKEN=...
export GUILD_ID=...
# etc for all required vars
python3 bot.py

You should see Logged in as PO3 Bot (id: ...) in the terminal — that confirms it's connected.
