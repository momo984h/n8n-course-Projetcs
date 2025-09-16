# Messenger API Audio Message Command

## Send Audio Message via Facebook Messenger API

```bash
curl -X POST \
  -F 'recipient={"id":"PSID"}' \
  -F 'message={"attachment":{"type":"audio","payload":{}}}' \
  -F 'filedata=@/path/audio.mp3;type=audio/mpeg' \
  "https://graph.facebook.com/v23.0/me/messages?access_token=PAGE_ACCESS_TOKEN"
```

### Parameters:
- **PSID**: Page-Scoped ID of the recipient
- **type**: The attachment type (audio in this case)
- **filedata**: Path to your audio file
- **PAGE_ACCESS_TOKEN**: Your Facebook Page Access Token

### Example Usage:
Replace `PSID` with the actual recipient ID and `PAGE_ACCESS_TOKEN` with your valid access token.
