# serveralerts2telegram
Bash scripts that send a telegram message if server load raise $LOAD_LIMIT or use of disk space raise $DISK_LIMIT

## Instalation

```bash
# Install dependencies
apt update
apt install iotop
# Clone repository
cd /root
git clone https://github.com/joker-x/serveralerts2telegram.git
cd serveralerts2telegram
# Copy binary
cp serveralerts2telegram /usr/local/sbin/
chmod u+x /usr/local/sbin/serveralert2telegram
# Create log directory
mkdir /var/log/loadmonitor
touch /var/log/loadmonitor/load.tsv
# Activate logrotate
cp loadmonitor.logrotate /etc/logrotate.d/loadmonitor
service logrotate restart
```

## Configuration

All options can be set with environment variables. Only two are required:

**TELEGRAM_BOT_TOKEN**

**TELEGRAM_CHAT_ID**

To obtain, you need to:

1. Get a bot token. To get one, contact the @BotFather bot and send the command /newbot. Follow the instructions.
2. Start a conversation with your bot or invite it into a group where you want it to send messages.
3. Find the chat ID for every chat you want to send messages to. Contact the @myidbot bot and send the /getid command to get your personal chat ID or invite it into a group and use the /getgroupid command to get the group chat ID. Group IDs start with a hyphen, supergroup IDs start with -100.
4. Alternatively, you can get the chat ID directly from the bot API. Send your bot a command in the chat you want to use, then check https://api.telegram.org/bot{YourBotToken}/getUpdates, eg. https://api.telegram.org/bot111122223:7OpFlFFRzRBbrUUmIjj5HF9Ox2pYJZy5/getUpdates

**LIMIT_LOAD**
Integer. If puntual server load is greater than LIMIT_LOAD send an alert. By default, the number of cores minus 2.

**LIMIT_DISK**
Integer. If percent of use of a partition is greater than LIMIT_DISK. By default, 90 and test all mounted partitions.

**MOUNT_POINTS**
String (path). Only test the partitions with contain MOUNT_POINTS


## Cron activation

Add this line to /etc/crontab to execute every 5 minutes:

```
*/5 * * * * root TELEGRAM_BOT_TOKEN= TELEGRAM_CHAT_ID= LIMIT_LOAD= LIMIT_DISK= MOUNT_POINTS= /usr/local/sbin/serveralerts2telegram >/dev/null 2>&1
```

## Update

```
cd /root/serveralerts2telegram
git pull origin main
cp serveralerts2telegram /usr/local/sbin/
```

