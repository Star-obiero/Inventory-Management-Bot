# How to use the Inventory Bot

One page. Send these messages to the bot in Telegram.

## Change stock (one product per message)

| To do this | Type |
|---|---|
| Add stock | `add 20 bread` |
| Remove stock | `remove 5 milk` |
| Set an exact count | `set 30 bread` |
| Add a new product | `create Bottled Water 100 bottle 20` (name, quantity, unit, reorder level) |

- Type the product name exactly as it appears in your stock list. Capital letters don't matter.
- You can use the Product ID instead, for example `add 20 P001`.
- Put `dry run` first to preview without changing anything: `dry run remove 5 milk`.

## Ask questions

| Type | You get |
|---|---|
| `stock bread` | How much you have, and whether it is low |
| `low stock` | Everything at or below its reorder level |
| `list` | All products |
| `history` or `history bread` | The last 10 changes |

## Send a file (many changes at once)

1. Make a CSV or Excel file with these columns in the first row:

| Action | Product ID | Item name | Quantity | Unit | Reorder level | Request ID |
|---|---|---|---|---|---|---|
| `create`, `add`, `remove` or `set` | Optional | Product name | Number | e.g. kg | New products only | Optional |

2. In Telegram tap the paperclip and send it as a **File** (not a photo). Type `dry run` as the caption to test first.
3. You get a report: every row is OK or REJECTED, with the reason.

**Tip:** give each row its own Request ID (for example `DELIVERY-1001-1`). Then if you fix a file and send it again, rows that already went in are skipped instead of being counted twice.

## If a row is rejected

| Message | What to do |
|---|---|
| Product not found | Check the spelling with `list`, or use the Product ID |
| Not enough stock to remove | Check the count, or use `set` |
| Unit mismatch | Use the product's own unit (kg, litre) or leave the unit blank |
| This product already exists | Use `add` or `set` instead of `create` |
| Duplicate Request ID | Already processed. Use a new ID only for a genuinely new change |

## Low-stock warnings

When a product reaches its reorder level you get an email. For files, low products are listed once in the report instead of one email per row.

## Cheat sheet

`add 20 bread` · `remove 5 milk` · `set 30 bread` · `stock bread` · `low stock` · `list` · `history` · `dry run ...` · `help`
