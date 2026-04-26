# n8n Social Publishing Workflows

This folder contains reusable n8n workflows for publishing YouTube videos through
an IFTTT Webhooks applet. The applet can then publish to Threads, Mastodon,
Bluesky, Instagram, or any other service connected in IFTTT.

All workflow exports are sanitized. Replace placeholders before activating them.

## Workflows

### 1. Random YouTube Video Once Per Day

File:

```text
youtube-random-to-webhook-daily.workflow.json
```

What it does:

1. Runs once per day.
2. Reads a YouTube channel RSS feed.
3. Chooses one public video at random.
4. Sends the title and link to an IFTTT webhook.

Webhook payload:

```json
{
  "value1": "Video title",
  "value2": "https://www.youtube.com/watch?v=VIDEO_ID",
  "value3": "youtube"
}
```

### 2. New Public YouTube Video Detector

File:

```text
youtube-public-release-to-webhook.workflow.json
```

What it does:

1. Runs every 10 minutes.
2. Reads a YouTube channel RSS feed.
3. Keeps a `seenVideoIds` list in n8n workflow static data.
4. Sends the title and link to an IFTTT webhook only when a newly public video
   appears in the public RSS feed.

Webhook payload:

```json
{
  "value1": "Video title",
  "value2": "https://www.youtube.com/watch?v=VIDEO_ID",
  "value3": "youtube_public_release"
}
```

The first manual execution initializes the seen-video list and intentionally
publishes nothing.

## IFTTT Applet

Create an IFTTT applet like this:

1. If This: `Webhooks` -> `Receive a web request`
2. Event name: choose your own event name, for example `youtube_publish`
3. Then That: choose the publishing action or multi-service flow you want
4. Map the ingredients:
   - `Value1`: post text or title
   - `Value2`: YouTube URL
   - `Value3`: optional source label

Your IFTTT webhook URL has this form:

```text
https://maker.ifttt.com/trigger/YOUR_EVENT_NAME/with/key/YOUR_IFTTT_WEBHOOK_KEY
```

Do not commit your real IFTTT key to GitHub.

## n8n Installation

1. Open n8n.
2. Import the workflow JSON file you want.
3. Open the `Lire le flux YouTube` node.
4. Replace the YouTube channel ID in the URL:

```text
https://www.youtube.com/feeds/videos.xml?channel_id=YOUR_YOUTUBE_CHANNEL_ID
```

5. Open the `Envoyer au webhook` node.
6. Replace the webhook URL with your real IFTTT URL:

```text
https://maker.ifttt.com/trigger/YOUR_EVENT_NAME/with/key/YOUR_IFTTT_WEBHOOK_KEY
```

7. Run the workflow manually once to test.
8. Activate the workflow.

## Notes

- Workflows are inactive by default.
- The YouTube URL sent in `value2` uses the full `youtube.com/watch?v=...`
  format, which is more compatible with link preview systems than `youtu.be`.
- If a destination platform does not render a thumbnail, the limitation is
  usually in that platform or IFTTT's preview generation, not in n8n.
