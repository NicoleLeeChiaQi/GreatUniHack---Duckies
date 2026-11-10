# PassPot — Student Rotating Savings Committees

PassPot is a student-focused platform prototype for organizing rotating savings committees (ROSCA). Members contribute an agreed amount on a schedule, and each cycle one member receives the pooled contribution. The prototype demonstrates committee discovery/matching, private committee creation, invite codes and links, turn swaps, trust and risk concepts, and an n8n event-orchestration interface.

## Run the demo

1. Download or clone this project.
2. Open `index.html` in a modern browser, or serve this folder with a static web server.
3. Navigate the app and use the demo interactions.
4. To connect an n8n workflow, open **Operations & compliance**, enter your n8n Webhook URL, and save it.
5. Use the demo event controls or create a committee and submit WhatsApp invitee numbers to send an event to the configured webhook.

## n8n event contract

The browser sends JSON POST events to the configured webhook. WhatsApp invitation requests use:

- `event_type`: `invitation.whatsapp_send_requested`
- `data.committee_code`
- `data.committee_name`
- `data.invitation_url`
- `data.invitees`: array of `{ phone, channel, recipient_opt_in_confirmed }`
- `data.message_template`: `passpot_committee_invitation`
- `data.action`: `send_whatsapp_template_messages`

The webhook event is an orchestration request. Actual WhatsApp delivery requires a configured WhatsApp Business Cloud API or Twilio account, credentials in n8n, an approved WhatsApp message template, and recipient opt-in. The prototype does not itself send WhatsApp messages or verify provider delivery receipts.

## Demo and production limitations

- Committee data is stored in the browser's local storage. Invite links/codes are not yet backed by a shared database, so cross-device joining requires a deployed backend and persistent database.
- The app is a hackathon prototype; sample balances, trust indicators, committee activity, and risk outcomes are illustrative, not real financial records or credit decisions.
- No funds are held, transferred, or guaranteed by this frontend prototype.
- Do not place production secrets or WhatsApp API credentials in the browser. Keep provider credentials in n8n credentials or a secure backend.
- Configure webhook authentication, HTTPS, CORS, rate limits, consent capture, logging, and privacy controls before any real deployment.

## Suggested n8n flow

Webhook → validate `event_type` → Split Out `data.invitees` → WhatsApp Business Cloud API/Twilio send-template-message node → record provider response and delivery status.

For the webhook output, the event fields may be nested under `body` depending on the n8n Webhook node configuration. Inspect the execution JSON and use the matching field path in expressions.

Pitch and idea

Where the idea came from

The BNY challenge was to design an original financial tool for one specific user and one real problem. We chose students. I suggested a savings tool with a social side, such as a streak with a friend, so that someone else saving keeps you accountable. Bilal then proposed building on committees, the informal rotating savings circles common in South Asia, and we adapted that for students. I came up with the name Pass Pot after a long list of options.

The pitch

I was our main presenter. I pitched Pass Pot for eight minutes to one BNY judge, using signposting so they knew what was coming next, and Bilal led a two-minute pitch to another judge. When asked whether we would use it ourselves, we said yes: it is interest-free, and students don't have to ask their parents for money and then feel guilty about it.

Feedback

The BNY judge told us the idea was excellent but not that unique, because committee-style savings already exist, even if informally. Next time we would research existing solutions before settling on an idea.
