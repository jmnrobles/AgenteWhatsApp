# 🤖 Agente IA WhatsApp FAQ — n8n

Workflow de **n8n** que responde automáticamente mensajes de WhatsApp consultando una FAQ en Google Sheets y generando respuestas con IA.

---

## ¿Qué hace?

1. Recibe un mensaje de WhatsApp
2. Consulta una hoja de Google Sheets con preguntas y soluciones
3. Busca la mejor coincidencia por palabras clave
4. Genera una respuesta formateada con GPT-3.5
5. Envía la respuesta por WhatsApp
6. Si hay imágenes asociadas a la respuesta, las envía también

---

## Flujo

```
WhatsApp Trigger
      ↓
Google Sheets (FAQ)
      ↓
JavaScript (matching por palabras clave)
      ↓
AI Agent — GPT-3.5
      ↓
Send texto → Send imagen 1 → Send imagen 2
```


## Requisitos

- [n8n](https://n8n.io) (self-hosted o cloud)
- Cuenta de **WhatsApp Business API** (Meta)
- Cuenta de **OpenAI**
- **Google Sheets** con la estructura indicada abajo

---

## Estructura de la hoja de Google Sheets

La hoja debe tener exactamente estas columnas:

| Problemas | Soluciones | Imágenes |
|---|---|---|
| no puedo iniciar sesión | Para restablecer tu contraseña sigue estos pasos... | https://url-imagen1.com |
| error al pagar | Comprueba que los datos de la tarjeta... | https://url-imagen1.com https://url-imagen2.com |

- **Problemas**: palabras clave o descripción del problema
- **Soluciones**: respuesta completa que enviará el bot
- **Imágenes**: URLs de imágenes separadas por espacio o coma (opcional)

---

## Instalación

### 1. Importa el workflow

En n8n: **Workflows → Import from file** → selecciona `workflow.json`

### 2. Configura las credenciales

Necesitarás crear estas credenciales en n8n:

| Credencial | Dónde obtenerla |
|---|---|
| WhatsApp OAuth | [Meta for Developers](https://developers.facebook.com) |
| WhatsApp API | [Meta for Developers](https://developers.facebook.com) |
| OpenAI API | [platform.openai.com](https://platform.openai.com/api-keys) |
| Google Sheets OAuth | [Google Cloud Console](https://console.cloud.google.com) |

### 3. Ajusta los parámetros en el workflow

Busca y reemplaza estos valores en los nodos:

| Placeholder | Descripción |
|---|---|
| `YOUR_WHATSAPP_PHONE_NUMBER_ID` | ID del número de WhatsApp Business |
| `YOUR_GOOGLE_SHEET_ID` | ID de tu hoja de Google Sheets |
| `HELPDESK_ES` | Teléfono de soporte en español |
| `HELPDESK_FR` | Teléfono de soporte en francés |
| `HELPDESK_PT` | Teléfono de soporte en portugués |

### 4. Activa el workflow

Activa el toggle en la esquina superior derecha de n8n.

---

## Detección de idioma

El agente detecta el idioma automáticamente según el prefijo del número:

| Prefijo | Idioma |
|---|---|
| 34 | Español |
| 33 | Francés |
| 351 | Portugués |

---

## Personalización del agente IA

El prompt del agente está en el nodo **AI Agent → System Message**. Puedes modificar:

- El nombre del asistente
- Los números de helpdesk
- Los idiomas soportados
- El tono y formato de las respuestas

---

## Notas

- El workflow viene con `"active": false` — actívalo manualmente tras configurarlo
- Las credenciales **no están incluidas** en el JSON por seguridad, debes crearlas en tu instancia de n8n
- El modelo por defecto es `gpt-3.5-turbo`, puedes cambiarlo a `gpt-4o` en el nodo **OpenAI Chat Model**
