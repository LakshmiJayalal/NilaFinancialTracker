# 💰 Family Expense Tracker

A minimalistic PWA that syncs with Google Sheets. Works on any browser, installable on Android home screens (Oppo, OnePlus, etc.), and hostable on GitHub Pages for free.

---

## Setup Guide (15 minutes)

### Step 1: Create the Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a new spreadsheet
2. Name it **"Family Expenses"** (or anything you like)
3. Note the **Sheet ID** from the URL:
   ```
   https://docs.google.com/spreadsheets/d/THIS_IS_YOUR_SHEET_ID/edit
   ```

### Step 2: Add the Apps Script Backend

1. In your Google Sheet, go to **Extensions → Apps Script**
2. Delete any existing code in `Code.gs`
3. Paste the entire contents of the `Code.gs` file from this project
4. Click **Save** (💾)
5. Run the **`setupSheets`** function:
   - Select `setupSheets` from the function dropdown at the top
   - Click **Run** (▶️)
   - Grant permissions when prompted (this is your own script accessing your own sheet)
   - This creates 4 sheets with headers + sample data: Members, Categories, Transactions, QuickAdds

### Step 3: Deploy the Apps Script as a Web App

1. In Apps Script, click **Deploy → New deployment**
2. Click the gear icon ⚙️ → select **Web app**
3. Set:
   - **Description**: Family Tracker API
   - **Execute as**: Me
   - **Who has access**: Anyone
4. Click **Deploy**
5. Copy the **Web App URL** (looks like `https://script.google.com/macros/s/xxxxx/exec`)
6. **Save this URL** — you'll need it in the app

> ⚠️ Every time you edit Code.gs, you must create a **New deployment** (not update) for changes to take effect.

### Step 4: Host on GitHub Pages

1. Create a new GitHub repository (e.g., `family-tracker`)
2. Upload these files to the repo root:
   - `index.html`
   - `manifest.json`
   - `sw.js`
3. Go to **Settings → Pages**
4. Under "Source", select **Deploy from a branch**
5. Choose **main** branch, **/ (root)** folder
6. Click **Save**
7. Your app will be live at `https://yourusername.github.io/family-tracker/`

### Step 5: First Login

1. Open your GitHub Pages URL in a browser
2. You'll see the config banner — paste your Apps Script Web App URL
3. The default member is **Admin** with PIN **1234**
4. Add your family members in Settings → Family Members

### Step 6: Install on Android Phones

**For Oppo / OnePlus / any Android:**
1. Open the GitHub Pages URL in **Chrome**
2. Tap the **⋮** menu (three dots)
3. Tap **"Add to Home screen"** or **"Install app"**
4. The app will appear on your home screen like a native app

**Alternative for Oppo (ColorOS):**
1. Open in Chrome → menu → "Add to Home screen"
2. If that doesn't work, use the browser's bookmark feature and add to home

---

## Editing Members & PINs

Go to your Google Sheet → **Members** tab to directly edit names, PINs, and colors.

| Name   | PIN  | Color   |
|--------|------|---------|
| Dad    | 5678 | #3b82f6 |
| Mom    | 4321 | #9333ea |
| Ravi   | 1111 | #16a34a |

Then tap **Sync** in the app's Settings to pull changes.

## Managing Categories

You can add/delete categories from the app's Settings screen, or edit the **Categories** sheet directly:

| Category    | Subcategory  | Icon |
|-------------|-------------|------|
| Food        | Groceries   | 🛒   |
| Food        | Dining Out  | 🍽️   |
| Transport   | Fuel        | ⛽   |

## SMS Parsing

The SMS parser understands common Indian bank formats:
- HDFC, SBI, ICICI, Axis, Kotak, etc.
- UPI transaction messages
- Credit/debit card alerts

**How to use:**
1. Go to the SMS tab
2. Copy-paste bank SMS from your messages app
3. The parser extracts amount, type (debit/credit), and merchant
4. Review, pick a category, and tap **Add**
5. Duplicates are detected by hash and flagged

**Tip:** Paste multiple SMS at once, separated by blank lines.

## Quick Add Buttons

Pre-configured one-tap expense buttons for things you buy regularly:
- Tea/Coffee ☕ ₹20
- Auto 🛺 ₹50
- Lunch 🍛 ₹150

Add your own in Quick Add → the **+** button, or edit the **QuickAdds** sheet directly.

---

## Architecture

```
[Phone/Browser] ←→ [GitHub Pages (static HTML)] ←→ [Google Apps Script API] ←→ [Google Sheet]
```

- **No server needed** — everything runs on Google's free infrastructure
- **Data stays in your Google Sheet** — full control, export anytime
- **PIN login** — lightweight family member separation (not bank-grade security)
- **PWA** — installable, works offline for cached data

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "Connection failed" | Check the Apps Script URL is correct and deployed |
| CORS errors | Make sure the deployment is set to "Anyone" access |
| Data not updating | Tap Sync in Settings, or redeploy Apps Script |
| Can't install on home screen | Use Chrome (not the default Oppo/OnePlus browser) |
| SMS not parsing | The parser handles most Indian bank formats; unusual formats may not work |
