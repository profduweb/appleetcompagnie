# n8n YouTube Random Webhook Workflow

This workflow is fully built from n8n nodes. It picks one random video from a
YouTube channel RSS feed once per day and sends it to an IFTTT Webhooks applet.
The applet can then publish to Threads, Mastodon, Bluesky, Instagram, or any
other service you connect in IFTTT.

1. Schedule Trigger once per day
2. HTTP Request to the YouTube RSS feed
3. Code node to choose a random video
4. HTTP Request to the publishing webhook

The webhook receives:

```json
{
  "value1": "Video title",
  "value2": "https://www.youtube.com/watch?v=VIDEO_ID",
  "value3": "youtube"
}
```

## Files

- `youtube-random-to-webhook-6h.workflow.json`: n8n workflow export

## IFTTT Applet

Create an IFTTT applet like this:

1. If This: `Webhooks` -> `Receive a web request`
2. Event name: choose your own event name, for example `youtube_random_post`
3. Then That: choose the publishing action you want
4. Map the ingredients:
   - `Value1`: post text or title
   - `Value2`: YouTube URL
   - `Value3`: optional source label, currently `youtube`

Your IFTTT webhook URL has this form:

```text
https://maker.ifttt.com/trigger/YOUR_EVENT_NAME/with/key/YOUR_IFTTT_WEBHOOK_KEY
```

Do not commit your real IFTTT key to GitHub.

## n8n Installation

1. Open n8n.
2. Import `youtube-random-to-webhook-6h.workflow.json`.
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

- The workflow is inactive by default.
- The schedule is once per day.
- The YouTube URL sent in `value2` uses the full `youtube.com/watch?v=...`
  format, which is more compatible with link preview systems than `youtu.be`.
- If a destination platform does not render a thumbnail, the limitation is
  usually in that platform or IFTTT's preview generation, not in n8n.
