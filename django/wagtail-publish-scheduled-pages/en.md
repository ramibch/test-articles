# How to publish scheduled pages on Wagtail


## Intro

Often times, editors need to set up some pages on draft mode in Wagtail and publish them later on at a certain time under Settings tab. As a developer you need to support that required option by running a periodic command that Wagtail provides.

That command is _python manage.py publish_scheduled_pages_ which it publishes, updates or unpublishes pages which were set up by an editor. The Wagtail docs recommend to run it every hour.

## Bash script

Now, you could run that command either manually, which I don´t really recommend because I want you to have freedom, or much better you can save a bash script on your server and then run it any time you like.

```bash
#!/bin/bash
source /home/myuser/myproject/venv/bin/activate
python /home/myuser/myproject/manage.py publish_scheduled_pages --settings=config.settings.production
```

The script activates the project virtual environment and then runs the Wagtail command _publish_scheduled_pages_.

## Crontab

If you are on a Linux machine, install crontab if you don't have it, and then run the next command on your terminal

```bash
crontab -e
```

Add the end of the file, write the following:

```bash
0 * * * * /home/myuser/myproject/management/publish_scheduled_pages.sh
```
