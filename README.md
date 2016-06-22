# Zendesk Mentions to Slack bot

This webtask checks for user mentions in Zendesk and DMs the relevant Slack user with ticket info. The webtask is triggered by Zendesk when a mention is detected in a comment. The format of the mention should be `<@username>`

## How to setup the webtask

    wt create --secret BOT_TOKEN=<Slack bot token goes here> slackmention.js

## How to configure Zendesk

1. In Settings > Extensions, create a new HTTP target with the webtask's URL as URL, POST as the method and JSON as the content type.
2. In Settings > Triggers, create a new trigger with 'Ticket: Comment text...' as the condition, "Contains the following string" as the op, and `<@` as the string. The performed action should be 'Notifications: Notify target' set to the HTTP target created in previous step. Set the JSON body as follows:

    {
      "id": "{{ticket.id}}",
      "title": "{{ticket.title}}",
      "comment": "{{ticket.latest_comment}}",
      "author": "{{ticket.latest_comment.author.name}}",
      "tags": "{{ticket.tags}}"
    }
