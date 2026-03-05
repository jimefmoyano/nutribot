# 🥗 Bot de Registro de Alimentación y Actividad

Bot de Telegram para registro personal de comidas y actividad física, conectado a Google Sheets y visualizado en Looker Studio.

## El problema

Quería registrar lo que comía y mis caminatas de forma rápida, desde el celular, sin abrir planillas ni apps de nutrición. El flujo tenía que ser simple: mandarle un mensaje a un bot y que todo quedara guardado solo.

## La solución

Un bot de Telegram con flujo conversacional que guía el registro paso a paso. Según el tipo de entrada (comida, caminata, otra actividad), el bot hace las preguntas correctas y guarda todo automáticamente.

## Flujo del sistema

```
Usuario en Telegram
      ↓
Bot (webhook via Google Apps Script)
      ↓
¿Qué tipo de registro?
  ├── Comida → descripción + hora + foto
  │              ↓
  │         Foto → Google Drive
  │         Datos → Google Sheets
  │
  ├── Caminata → pasos + tiempo + km
  │               ↓
  │          Google Sheets
  │
  └── Otra actividad → nombre + rutina
                         ↓
                    Google Sheets
                         ↓
                   Looker Studio (dashboard)
```

## Stack

| Herramienta | Uso |
|---|---|
| Telegram Bot API | Interfaz conversacional |
| Google Apps Script | Backend / webhook |
| Google Drive | Almacenamiento de fotos |
| Google Sheets | Base de datos |
| Looker Studio | Dashboard y visualización |

## Estructura del código

```
telegram-food-bot/
├── Code.gs          # Script principal (doPost, lógica de estados)
├── README.md
└── screenshots/     # Imágenes del flujo y dashboard
```

## Cómo funciona el flujo de estados

El bot mantiene el estado de cada conversación usando `PropertiesService` de Google Apps Script. Cada usuario tiene su propio estado almacenado por `chatId`.

```
step 0 → bienvenida
step 1 → tipo de registro (comida / caminata / actividad)
step 2-4 → flujo de comida (descripción → hora → foto)
step 10-12 → flujo de caminata (pasos → tiempo → km)
step 20-21 → flujo de actividad (nombre → rutina)
```

Comandos especiales: `reiniciar` o `cancelar` en cualquier momento resetean el estado.

## Setup (para replicar)

1. Crear un bot en Telegram via [@BotFather](https://t.me/BotFather)
2. Crear un Google Sheet y una carpeta en Drive
3. Abrir [Google Apps Script](https://script.google.com) y pegar el código
4. Reemplazar las variables de configuración:

```javascript
const TELEGRAM_TOKEN = 'TU_TOKEN_AQUI';
const SPREADSHEET_ID = 'ID_DE_TU_SHEET';
const FOLDER_ID = 'ID_DE_TU_CARPETA_EN_DRIVE';
```

5. Deployar como Web App (ejecutar como: yo, acceso: cualquiera)
6. Registrar el webhook:
```
https://api.telegram.org/botTU_TOKEN/setWebhook?url=URL_DE_TU_DEPLOY
```

## Screenshots

<img width="332" height="375" alt="image" src="https://github.com/user-attachments/assets/a51942d5-ca57-4556-94b9-4719adb102e9" />
<img width="531" height="1280" alt="image" src="https://github.com/user-attachments/assets/a54662c5-a9eb-4a42-8437-e9dae3d45150" />




*Proyecto personal · Google Apps Script + Telegram API*
