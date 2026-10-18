# Setup (for the administrator)

About 30 to 45 minutes. You need n8n, a Google account, a Telegram account and a Gmail account.

## 1. Make the Google Sheet

One spreadsheet with two tabs. The column headings are in [`templates/`](../templates).

- **Inventory:** Product ID, Item name, Quantity, Unit, Reorder level, Low stock alerted, Updated at
- **Inventory Change Log:** Timestamp, Request ID, Product ID, Item name, Action, Previous quantity, Change quantity, New quantity, Status, Message, Request source

## 2. Make the Telegram bot

Message **@BotFather** in Telegram, send `/newbot`, and copy the token. Keep it private.

## 3. Import the workflows into n8n

1. Import `workflows/inventory-stock-update-engine.json` first, then `workflows/bulk-inventory-import-telegram.json`.
2. In the nodes, choose your own Google Sheets, Gmail and Telegram credentials. Use **one** Telegram credential on every Telegram node.
3. In the engine, set where low-stock emails go (the **Email Low-Stock Alert** node).
4. In the front door, open **Call Engine** and select the engine workflow you just imported.

## 4. Choose who is allowed

Open the **Route Message** node and edit the lines at the top:

```js
const ALLOWED_CHAT_IDS = ['REPLACE_WITH_MANAGER_CHAT_ID'];
const MANAGER_EMAIL = 'manager@example.com';
```

To find someone's chat ID, ask them to send `/myid` to the bot.

## 5. Test, then publish

1. Send `/myid`, `list` and `low stock`.
2. Send `samples/01_clean_import.csv` with the caption `dry run`, then again without it. Check the sheet.
3. Send the same file once more. Every row should be reported as a duplicate.
4. Delete the test products from the sheet.
5. Publish both workflows.

## If something goes wrong

| Problem | Likely fix |
|---|---|
| The bot never replies | The front door workflow isn't published |
| "Chat not found" | A Telegram node uses a different bot credential. Use the same one everywhere |
| "Not authorised" | The chat ID isn't in `ALLOWED_CHAT_IDS`, or you didn't publish again after editing |
| Error at **Call Engine** | Re-select the engine workflow and publish it |

## Running n8n on your own computer

Telegram only talks to secure (HTTPS) addresses, so a local n8n needs a tunnel such as ngrok, and n8n must be started with `WEBHOOK_URL` set to the tunnel address. The bot only works while your computer, n8n and the tunnel are running. For real use, put it on an always-on host.

## Security

Never put tokens, passwords or real stock exports in this folder. The workflow files here have had your identifiers replaced with placeholders.
