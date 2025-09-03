# Payment SDK Challenge — Simulated Marketplace with Mock Transactions

**Objective:** Build a lightweight **SDK/plugin** that exposes a simple payments flow inside a game engine scene/map: list 3 items, show balance, purchase → create mock transaction → poll until confirmation → update inventory & UI.

**Submission deadline:** **Thursday, September 11th, 2025 (09/11/2025)** — 5 working days from receipt.

---

## 1) Scope & Constraints

- The system shall be implemented as an **SDK/plugin** for **Unity (C#)** or **Unreal (C++)**, developer's choice. Deliver one engine only.
- The system shall run **entirely local** using a **mock REST API** (JSON files + artificial delays). **No real payment processor** required.
- The system shall separate concerns via services (e.g., `StoreService`, `WalletService`, `TransactionService`, `InventoryService`) and manage transaction states via a **state machine**.

---

## 2) System Requirements

| ID | Category | Requirement |
|----|----------|-------------|
| 1 | Store & Items | The system shall load store items from a local JSON file via a mock endpoint **`/store/items`** |
| 2 | Store & Items | The system shall expose exactly **three (3) items** with `id`, `name`, `description`, and `price` |
| 3 | Store & Items | The system shall render a store list in the UI with a **Buy** button per item |
| 4 | Wallet & Balance | The system shall load the user's balance from a local JSON file via mock endpoint **`/wallet/balance`** and display it prominently |
| 5 | Wallet & Balance | The system shall prevent purchases when balance is insufficient and provide UI feedback |
| 6 | Payments & Transactions | The system shall create a **mock purchase** via **`/store/purchase`**, returning a **transaction** object `{ "txId": string, "status": "pending" }` |
| 7 | Payments & Transactions | The system shall poll **`/tx/{id}`** until the transaction status transitions from **`pending`** to **`confirmed`** or **`failed`** (include artificial delay to simulate network/processor latency) |
| 8 | Payments & Transactions | The system shall update the UI to reflect transaction progress with at least these states: **Pending**, **Confirmed**, **Failed** |
| 9 | Payments & Transactions | The system shall, on **Confirmed**, deduct balance and append the purchased item to the **inventory**; on **Failed**, it shall leave state unchanged and show an error |
| 10 | Inventory | The system shall display an **Inventory** panel with items owned by the player |
| 11 | Inventory | The system shall update and persist inventory state for the current session (in-memory or local file OK). **Bonus** if persistence survives engine restarts |
| 12 | SDK API Surface | The system shall expose a minimal **SDK API** consumable by a game script/blueprint: `GetItems(): Item[]`, `GetBalance(): Balance`, `Purchase(itemId): TxHandle`, `OnTxUpdate(txId, callback(Status))` |
| 13 | SDK API Surface | The system shall provide an example **scene (Unity)** or **map (Unreal)** demonstrating the above end-to-end |
| 14 | Architecture | The system shall structure code in **modular services** (Store/Wallet/Transaction/Inventory) |
| 15 | Architecture | The system shall keep **UI separate** from business logic and data access |
| 16 | Documentation | The system shall include **clear documentation** (README) describing setup, API, and assumptions, in **English** |
| 17 | Error Handling | The system shall include **basic error handling** (network/read failures, invalid item) |
| 18 | Configuration | The system shall include **simple configuration** for artificial delay (e.g., 500–1500 ms) |
| 19 | UI Design | The system shall present a **single screen** with three regions: **Balance** (top/left), **Store** (items + Buy buttons), **Inventory** (owned items), and provide visual feedback for transaction statuses |
| 20 | Project Structure | The system shall adopt engine-friendly foldering: **Unity:** `Assets/KaizenStore/...`, **Unreal:** `Plugins/KaizenStore/...` |
| 21 | State Management | The system shall include a **transaction state machine** (`Pending → Confirmed|Failed`) |

### Reference JSON Examples

**Store Items (`/store/items`):**
```json
[
  {
    "id": "item001",
    "name": "Iron Sword",
    "description": "Reliable close-combat blade.",
    "price": 100
  },
  {
    "id": "item002",
    "name": "Healing Potion",
    "description": "Restores 50 HP.",
    "price": 50
  },
  {
    "id": "item003",
    "name": "Leather Armor",
    "description": "Basic defense boost.",
    "price": 150
  }
]
```

**Wallet Balance (`/wallet/balance`):**
```json
{ "address": "player-local", "currency": "GOLD", "balance": 200 }
```

**Transaction Flow:**
```json
// POST /store/purchase (request)
{"itemId":"item002","wallet":"player-local"}

// POST /store/purchase (response)
{"txId":"tx-7f3a","status":"pending","amount":50,"itemId":"item002"}

// GET /tx/tx-7f3a (progression, polled)
{"txId":"tx-7f3a","status":"pending"}     // then later...
{"txId":"tx-7f3a","status":"confirmed"}   // or {"status":"failed"}
```

---

## 3) Mock API Endpoints

- **GET `/store/items`** → `Item[]` (from local JSON)
- **GET `/wallet/balance`** → `{ address, currency, balance }`
- **POST `/store/purchase`** → `{ txId, status: "pending", amount, itemId }`
- **GET `/tx/{id}`** → `{ txId, status: "pending"|"confirmed"|"failed" }`

All endpoints are **local mocks** backed by JSON and **delays** to simulate real payment gateways.

---

## 4) Deliverables

- **SDK/Plugin** (Unity C# or Unreal C++)
- **Sample scene/map** exercising the flow
- **README** with setup, API usage, and assumptions
- **Short technical notes** explaining design decisions and trade-offs

---

## 5) Evaluation Criteria

- **Correctness:** Meets "The system shall…" requirements; flow works end-to-end
- **SDK Quality:** API clarity, engine-native patterns, separation of concerns
- **Code Quality:** Readability, error handling, minimal dependencies, performance sanity
- **UX:** Clear states (Pending/Confirmed/Failed), intuitive UI, no blocking UI thread
- **Documentation:** Enough for another dev to integrate quickly (payments focus)

---

## 6) Submission & Deadline

- **What to submit:** Git repo link (public or private invite), plus brief demo video 
- **Deadline:** **Thursday, September 11th, 2025 (09/11/2025)** — 5 working days from receipt 