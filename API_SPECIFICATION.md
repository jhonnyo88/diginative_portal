# DigiNativa Portal: API-Specifikation v1.0

Detta dokument definierar det tekniska kontraktet mellan `diginative-portal` (klienten) och DigiNativas backend-system. All utveckling i portalen som rör kommunikation med backend måste strikt följa denna specifikation.

## Bas-URL

`https://api.diginativa.se/v1`

## Autentisering

För MVP kommer en statisk API-nyckel att användas, skickad som en `Authorization: Bearer <API_KEY>`-header.

---

### Endpoint: Skapa en ny beställning

Skapar ett nytt spel-genereringsjobb i systemet.

* **URL:** `/orders`
* **Metod:** `POST`
* **Headers:**
    * `Content-Type: application/json`
    * `Authorization: Bearer <API_KEY>`

#### Request Body

```json
{
  "customer_contact": {
    "name": "string",
    "email": "string (format: email)",
    "organization": "string"
  },
  "strategy_document": {
    "filename": "string",
    "content_base64": "string (base64-kodad PDF)"
  },
  "personalization": {
    "theme_colors": {
      "primary": "string (hex-kod, e.g., '#005AA7')",
      "secondary": "string (hex-kod, e.g., '#FFFDE4')"
    },
    "persona_id": "string (e.g., 'anna-kommunal')"
  }
}

Success Response (202 Accepted)
Indikerar att beställningen har tagits emot och köats för bearbetning.

{
  "order_id": "string (UUID, e.g., 'f47ac10b-58cc-4372-a567-0e02b2c3d479')",
  "status": "QUEUED",
  "message": "Din beställning har tagits emot och kommer att bearbetas.",
  "status_check_url": "/orders/f47ac10b-58cc-4372-a567-0e02b2c3d479/status"
}

Error Response (400 Bad Request)
Indikerar ett valideringsfel i den skickade datan.

{
  "error": "VALIDATION_FAILED",
  "message": "Ett eller flera fält är ogiltiga.",
  "details": [
    { "field": "customer_contact.email", "issue": "Must be a valid email format." },
    { "field": "personalization.theme_colors.primary", "issue": "Must be a valid hex color code." }
  ]
}

Endpoint: Hämta orderstatus
Används av portalen för att pollen efter statusuppdateringar på en beställning.

URL: /orders/{order_id}/status

Metod: GET

Headers:

Authorization: Bearer <API_KEY>

Success Response (200 OK)
{
  "order_id": "string",
  "status": "string (e.g., QUEUED, PROCESSING, FAILED, COMPLETED)",
  "last_updated": "string (ISO 8601 datetime)",
  "details": {
    "current_step": "string (e.g., 'Analysing document', 'Generating game assets')",
    "game_url": "string (URL, endast vid status COMPLETED)"
  }
}
