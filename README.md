# Go Holidays · Meta Dashboard — Setup & Maintenance Guide

This guide is for the social media team to manage the dashboard independently. No coding knowledge needed.

---

## How It Works (Quick Overview)

The dashboard has two parts:

1. **The dashboard file** (`index.html`) — hosted on GitHub Pages. This is what you open in your browser.
2. **The Google Apps Script proxy** — a small script that sits between the dashboard and Meta's API. It's what actually fetches your ad data.

When the proxy URL expires, the dashboard will tell you and ask for a new one. This guide walks you through fixing that.

---

## Part 1 — Getting a New Meta Access Token

You'll need this when the dashboard stops loading data and shows a "Proxy URL Expired" message, or when Meta tells you the token has expired.

### Step 1 — Open the Meta Developer App

1. Go to [developers.facebook.com](https://developers.facebook.com)
2. Log in with the Facebook account that has access to the Go Holidays ad account
3. Click **My Apps** in the top right
4. Select the Go Holidays app (or whichever app was used to generate the previous token)

> If you don't see an app, ask your manager — the app may be under a different account.

### Step 2 — Open the Graph API Explorer

1. In the left sidebar, click **Tools**
2. Click **Graph API Explorer**

### Step 3 — Generate a User Token

1. In the top right of the Explorer, click the **Generate Access Token** button
2. A Facebook login popup will appear — log in and click **Continue**
3. Make sure the following permissions are ticked:
   - `ads_read`
   - `read_insights`
   - `pages_read_engagement`
   - `pages_show_list`
4. Click **Continue**, then **Done**
5. You'll see a short-lived token appear in the **Access Token** field — copy it

### Step 4 — Extend the Token to 60 Days

The token from the Explorer only lasts 1–2 hours. You need to extend it.

1. Go to [developers.facebook.com/tools/explorer](https://developers.facebook.com/tools/explorer)
2. At the top, click the **info icon** next to your token
3. Click **Open in Access Token Debugger**
4. On the debugger page, click **Extend Access Token** at the bottom
5. Click **Extend Access Token** on the confirmation screen
6. Copy the new long token that appears — this one lasts **60 days**

> Save this token somewhere safe. You'll need it in Part 2.

---

## Part 2 — Updating the Google Apps Script

This is where you paste the new token so the dashboard can use it.

### Step 1 — Open the Script

1. Go to [script.google.com](https://script.google.com)
2. Log in with the Google account used to set up the dashboard
3. Find the project called **Go Holidays Meta Proxy** and open it

### Step 2 — Paste the New Token

1. You'll see a file open with code that looks like this at the top:

```
const META_TOKEN = 'EABAKU723...';
```

2. Select everything between the single quotes after `META_TOKEN =`
3. Delete it and paste your new 60-day token in its place
4. The line should now look like:

```
const META_TOKEN = 'YOUR_NEW_TOKEN_HERE';
```

5. Click **File → Save** (or press Ctrl+S / Cmd+S)

### Step 3 — Redeploy the Script

After saving, you must redeploy so the dashboard picks up the change.

1. Click **Deploy** in the top right
2. Click **Manage deployments**
3. Find your active deployment and click the **pencil (edit) icon**
4. Under **Version**, select **New version**
5. Click **Deploy**
6. Copy the new **Web App URL** that appears — it looks like:
   `https://script.google.com/macros/s/ABC123.../exec`

> The URL changes every time you redeploy. You'll need to paste it into the dashboard.

---

## Part 3 — Updating the Dashboard With the New URL

1. Open the dashboard in your browser (your GitHub Pages link)
2. If it's showing the "Proxy URL Expired" screen, paste the new URL into the input field and click **Connect & Load Data**
3. If the dashboard is already loaded, scroll to the top and use the refresh option

The dashboard will remember the new URL automatically for next time on that browser.

---

## Reminder Schedule

| Action | How Often |
|---|---|
| Extend the Meta access token | Every 60 days |
| Update the token in Google Apps Script | Every 60 days (same day) |
| Redeploy the script and update the dashboard URL | Every 60 days (same day) |

**Tip:** Set a reminder in your phone or calendar for 55 days after each update so you don't get caught off guard.

---

## Troubleshooting

**Dashboard shows "Proxy URL Expired" immediately after updating**
→ Make sure you selected **New version** when redeploying. If you kept the same version, the old cached script runs. Redeploy again with a new version.

**Token is valid but data isn't loading**
→ Check that the Facebook account used to generate the token still has access to the Go Holidays ad account and page.

**Graph API Explorer shows a permissions error**
→ The app may need to be re-approved for certain permissions. Contact your manager or the person who originally set up the Meta developer app.

**The dashboard URL on GitHub Pages stopped working**
→ GitHub Pages sometimes disables repos that are inactive. Go to the repository settings, find the Pages section, and re-enable it.

---

## Quick Reference

| Resource | Link |
|---|---|
| Meta Graph API Explorer | [developers.facebook.com/tools/explorer](https://developers.facebook.com/tools/explorer) |
| Meta Access Token Debugger | [developers.facebook.com/tools/debug/accesstoken](https://developers.facebook.com/tools/debug/accesstoken) |
| Google Apps Script | [script.google.com](https://script.google.com) |
| Go Holidays Dashboard | *(paste your GitHub Pages URL here)* |
