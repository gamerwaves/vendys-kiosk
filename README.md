# Vendy's Kiosk Plan

## 1. Components & Connections
- **PN532 NFC Reader** → reads the card ID when a card is tapped.
- **Raspberry Pi** → runs the program, talks to the PN532, shows the touchscreen UI, and sends commands to the vending machine relay.
- **Pi Touchscreen** → shows the user interface (buy products, check balance, settings).
- **MongoDB Database** → stores all user accounts, their points, and transaction history.
- **Vending Machine Relay/Interface** → receives a signal from the Pi to release the chosen product.

---

## 2. User Flow
1. **Idle Screen**: The Pi shows “Tap your card” and product options.
2. **User Taps Card**:
   - PN532 reads card ID → sends it to Pi program.
   - Pi program checks MongoDB for that card ID.
3. **If normal user**:
   - Screen shows name and balance.
   - User taps a product button.
   - Program checks if points are enough.
   - If yes:
     - Deduct points in MongoDB.
     - Log transaction.
     - Trigger vending machine relay to release item.
   - If no:
     - Show “Insufficient Points.”
4. **If master card (admin)**:
   - If tapped normally → acts like infinite points card.
   - If tapped after pressing “Settings” button → unlocks admin menu.

---

## 3. Admin Flow
1. In normal mode, tap “⚙ Settings” in top right.
2. Screen says: “Tap Admin Card.”
3. Tap master card:
   - If correct → show admin menu.
   - If not admin card → show “Access Denied.”
4. Admin Menu options:
   - **View Users** → list all cards with balances.
   - **View Transactions** → recent purchases with time, user, and item.
   - (Optional) Add/Remove Points, Register New Card, Change Prices.
5. Tap “Exit” → return to normal mode.

---

## 4. Data Connections
- **Card Tap → PN532 → Pi → MongoDB** → fetch user data.
- **Buy Product → Pi → MongoDB** → update points + save transaction.
- **Pi → Vending Relay** → send pulse to release product.
- **Admin Menu → Pi → MongoDB** → read/write user & transaction info.
