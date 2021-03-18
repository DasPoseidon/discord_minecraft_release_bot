Dieser Fork ist eine deutsche Übersetzung.

MESSAGE_CONTENT in der Config-Datei wird aktuell1 ignoriert!

# discord_minecraft_release_bot
A Discord Bot which checks for whether a new Minecraft release or snapshot has been released and makes a post via Discord Webhook.

The script will extract the "snapshot" and "release" version information from the Mojang version manifest file used by the Minecraft launcher and compare it against a cached copy. If it detects a variation, a message will be posted to Discord.


# Configuration File

The source code includes an example configuration file specifying several options. Some are required for the bot to work and others are optional.

**Required:**

ID: The unique identifier for this bot instance. This is useful if running multiple instances of the bot for multiple discord servers.

WEBHOOK_URL: The URL of the webhook used for posting to the Discord server.

**Optional:**

TMP: Path to your temporary files location.

DISCORD_USERNAME: The username that the Bot will use in Discord when posting.

MESSAGE_CONTENT: This is the text string posted to the Discord server. If you want anything more than the basic message you need to ensure that it's in the appropriate format to be included in the JSON payload. For example including \n for newlines.

e.g.

```
MESSAGE_CONTENT="A new Minecraft $VERB is available! | Release: $MANIFEST_VERSION_RELEASE | Snapshot: $MANIFEST_VERSION_SNAPSHOT\n\nEnjoy yourselves!"
```

RELEASE_VERB: Override the formatting/wording of the word "release" to your liking.

SNAPSHOT_VERB: Override the formatting/wording of the word "snapshot" to your liking.

e.g.

```
RELEASE_VERB=__release__
```

# Installation Instructions
1. Create a configuration file and include the three variables for ID and WEBHOOK_URL as per the example.
2. Set up a Discord Webhook in your channel. See: https://support.discordapp.com/hc/en-us/articles/228383668-Intro-to-Webhooks
3. Add your webhook URL to the WEBHOOK_URL variable within the configuration file. Note, if you are using separate channels for release and snapshots, you can define these instead as SNAPSHOT_WEBHOOK_URL and RELEASE_WEBHOOK_URL respectively.
4. Add the script to cron to run at your convenience, but please use discretion as each run will fetch the version manifest.

**Cron Example:**

```
*/15 * * * *  ~/bots/discord_minecraft_release_bot.sh -c ~/bots/myinstance.conf
```

# Notes
- The default bot username is "Minecraft" but this can be changed by modifying the DISCORD_USERNAME variable.
- You can override the temporary location where the script should store its cache file by adding a TMP variable to the configuration file.
- The output message is also configurable if you want, just add this to your configuration file with the correct syntax:

```
MESSAGE_CONTENT="A new Minecraft $VERB is available! | Release: $MANIFEST_VERSION_RELEASE | Snapshot: $MANIFEST_VERSION_SNAPSHOT\n\nEnjoy yourselves!"
```

# First Run
Upon first run, the script will download the version manifest and cache it. It will not post anything to Discord until it detects a new change in the Mojang version manifest. This is to ensure that it does not spam your Discord server if the tmp files don't exist or are removed.

# Debugging
The easiest way to debug the script is to modify the cached Mojang manifest file directly in order to trigger the script to detect a change. Edit the file with your favourite editor and just modify the first release or snapshot value and change it to something else. Upon the next run, the script will detect a change in these values and post to Discord.

# Requirements
**Bash Version:**

curl: https://curl.haxx.se/

jq: https://stedolan.github.io/jq/

# Contributors

In an earlier version, a Python variation of this script was created and contributed by TheUnlocked. As it is unmaintained and does not follow the latest version changes, it has been removed from this repository but can be found at https://github.com/TheUnlocked/discord_minecraft_release_bot if there is still an interest in it.
