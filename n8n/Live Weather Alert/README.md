# 🌤️ Live Weather Alert

An n8n workflow that receives a city name via webhook, fetches live weather data, classifies it by severity (Hot / Cold / Stormy / Normal), and returns a formatted alert message. A simple HTML front-end form is included for triggering the webhook manually.

## How It Works

1. **Webhook** — Accepts a `POST` request at `/weather-alert` with a JSON body containing a `city` field.
2. **City Not Empty?** — Validates that a city was provided.
   - If missing → returns a `400` error (`"City is Missing"`).
   - If present → continues to fetch weather data.
3. **Get Weather Information** — Calls the [WeatherAPI](https://www.weatherapi.com/) `current.json` endpoint for the given city.
4. **Format Alert** — A Code node parses the response and builds a human-readable alert message, determining a `severity` level based on temperature and condition:
   - `Hot` — temperature above 35°C
   - `Cold` — temperature at or below 0°C
   - `Stormy` — condition text is "Thunderstorm"
   - `Normal` — anything else
5. **If Severe?** — Branches based on whether the severity is Hot, Cold, or Stormy.
6. **Respond to Webhook** — Returns a JSON response with the status code, severity, and formatted message (severe and normal paths currently return the same payload shape).

## Files

| File | Description |
|---|---|
| `Live_Weather_Alert.json` | n8n workflow export — import this directly into n8n. |
| `Weather_Form.html` | Standalone HTML form used to send a test payload to the webhook. |

## Setup

### 1. Import the Workflow
1. Open your n8n instance.
2. Go to **Workflows → Import from File**.
3. Select `Live_Weather_Alert.json`.

### 2. Configure the Weather API Credential
The **Get Weather Information** node uses Bearer Auth credentials named `Weather API`. You'll need to:
1. Sign up for a free API key at [weatherapi.com](https://www.weatherapi.com/).
2. In n8n, create/update the `Weather API` credential with your key.
3. Alternatively, replace the `key` query parameter value (`YOUR_API_KEY`) directly in the node if you prefer not to use the credential system.

### 3. Activate the Workflow
Toggle the workflow to **Active** in n8n so the webhook listens for incoming requests.

### 4. Use the HTML Form
1. Open `Weather_Form.html` in a browser.
2. Update the `WEBHOOK_URL` constant to match your n8n instance (defaults to `http://localhost:5678/webhook/weather-alert`).
3. Enter a city name and click **Send Alert**.

## Example Request

```json
POST /webhook/weather-alert
Content-Type: application/json

{
  "city": "Dubai"
}
```

## Example Response

```json
{
  "StatusCode": 200,
  "Severity": "Hot",
  "message": "🔥 *Weather Alert — Dubai*\nCondition: Sunny\nTemperature: 38°C (feels like 41°C)\nHumidity: 30% | Wind: 15 km/h\nSeverity: *Hot*"
}
```

## Notes & Possible Improvements
- The `Send Severe Alert` and `Respond to Webhook` nodes currently return identical payloads — this is a good place to add real alerting (e.g., WhatsApp, Slack, or email notifications) for severe weather cases.
- Consider adding input sanitization/validation on the city field beyond an empty check.
- The webhook URL in the HTML form is hardcoded for local development; update it for production use.

## Tech Stack
- [n8n](https://n8n.io/) — workflow automation
- [WeatherAPI](https://www.weatherapi.com/) — weather data provider
- Vanilla HTML/CSS/JavaScript — front-end trigger form

---

Author: Khaled Salah
