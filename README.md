<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://ai.google.dev/static/site-assets/images/share-ais-513315318.png" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/1b9ea2e8-4cc6-4486-b2bb-0c7da36f7cb0

## Run Locally

**Prerequisites:**  Node.js


1. Install dependencies:
   `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app:
   `npm run dev`

## Razorpay and WhatsApp Confirmations

Live payment confirmation requires Razorpay credentials. Copy `.env.example` to `.env` and set `RAZORPAY_KEY_ID`, `RAZORPAY_KEY_SECRET`, and `RAZORPAY_WEBHOOK_SECRET`. In the Razorpay dashboard, configure the public HTTPS webhook URL `/api/webhooks/razorpay` and subscribe to `payment.captured` and `payment.failed`. A booking is confirmed only by a valid, amount-matched `payment.captured` event; the browser callback alone does not confirm payment.

WhatsApp confirmations require Meta WhatsApp Cloud API credentials, a publicly reachable HTTPS `APP_URL`, and an approved utility template. Set the `WHATSAPP_*` values from `.env.example`; the template body must use ten placeholders in the documented order. Configure the Meta webhook URL `/api/webhooks/whatsapp`, set its verification token and app secret, and subscribe to message status updates. Customers must enter a phone number in international format and explicitly opt in.

Keep `.env` private and never use `VITE_` prefixes for provider secrets. Without provider credentials, messages are recorded as not configured and are not sent.

## Gmail SMTP Setup

1. Enable 2-Step Verification for the Google account that will send booking email.
2. Create an App Password in the Google Account Security settings. Use the generated App Password, not the account password.
3. Copy `.env.example` to `.env`. Set `SMTP_USER` and `SMTP_FROM` to the sending Gmail address, and set `SMTP_PASS` to the App Password. Keep `SMTP_HOST=smtp.gmail.com`, `SMTP_PORT=587`, and `SMTP_SECURE=false`; the backend requires STARTTLS.
4. Restart the backend with `npm run dev`. SMTP credentials are read only by the server and are never included in the frontend bundle.

After the payment webhook confirms a booking, the server sends the confirmation email with the generated QR ticket PDF attached. The Admin email log reports SMTP acceptance, pending configuration, or the actual send error. A retry sends another copy for the same booking; it does not create a booking or payment.
