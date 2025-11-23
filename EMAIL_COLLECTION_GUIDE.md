# Email Collection Guide for Doorothy Landing Page

## ✅ What's Fixed

Your email collection system is now **fully functional**! When users submit their email (either through the popup or footer), the system will:

1. ✓ Show visual feedback ("Submitting..." → "✓ Reserved!")
2. ✓ Store emails locally in the browser
3. ✓ Optionally send to Google Sheets (if configured)

---

## 📧 How to Access Collected Emails

### Option 1: Browser Console (Immediate Access)

1. Open your deployed Wix site
2. Press `F12` (or `Cmd+Option+I` on Mac) to open Developer Tools
3. Go to the "Console" tab
4. Type: `getCollectedEmails()`
5. Press Enter

You'll see a table with all collected emails including:
- Email address
- Source (popup or footer)
- Timestamp
- Page URL

### Option 2: Google Sheets Integration (Recommended for Production)

This automatically saves all emails to a Google Sheet in real-time.

#### Setup Steps:

1. **Create Google Apps Script**
   - Go to https://script.google.com
   - Click "New Project"
   - Delete any existing code
   - Paste this code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  
  // Add headers if sheet is empty
  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Timestamp', 'Email', 'Source', 'Page URL']);
  }
  
  sheet.appendRow([
    new Date(data.timestamp),
    data.email,
    data.source,
    data.page
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({status: 'success'}));
}
```

2. **Deploy as Web App**
   - Click "Deploy" → "New deployment"
   - Click the gear icon ⚙️ next to "Select type"
   - Choose "Web app"
   - Settings:
     - Execute as: **Me**
     - Who has access: **Anyone**
   - Click "Deploy"
   - **Copy the Web App URL** (it looks like: `https://script.google.com/macros/s/...`)

3. **Update Your Website Code**
   - Open `index.html`
   - Find line ~975: `const GOOGLE_SCRIPT_URL = '';`
   - Paste your Web App URL between the quotes:
     ```javascript
     const GOOGLE_SCRIPT_URL = 'https://script.google.com/macros/s/YOUR_URL_HERE';
     ```
   - Save and re-deploy to Wix

4. **Create the Spreadsheet**
   - In Google Apps Script, click "Extensions" → "Apps Script"
   - Or create a new Google Sheet and link it to your script

---

## 🧪 Testing

1. Visit your Wix site
2. Scroll down twice to trigger the popup
3. Enter a test email
4. Click "Reserve My Discount"
5. You should see:
   - Button changes to "Submitting..."
   - Then "✓ Reserved!" with green background
   - Popup closes after 2 seconds

Check if it worked:
- **Console method**: Open console, type `getCollectedEmails()`
- **Google Sheets**: Check your spreadsheet for the new row

---

## 📊 Data Structure

Each email submission includes:

| Field | Description | Example |
|-------|-------------|---------|
| `email` | User's email address | user@example.com |
| `source` | Where they submitted | "popup" or "footer" |
| `timestamp` | When they submitted | 2025-11-23T12:00:00.000Z |
| `page` | Page URL | https://yoursite.wixsite.com/doorothy |

---

## 🔒 Privacy & Data

- **localStorage**: Emails are stored in the user's browser only (not shared across devices)
- **Google Sheets**: Only you can access the spreadsheet (keep the URL private)
- **No personal tracking**: We only collect what users explicitly provide

---

## 🚀 Next Steps

### For Testing Phase:
- Use the console method (`getCollectedEmails()`) to verify emails are being collected

### For Production:
1. Set up Google Sheets integration (recommended)
2. Consider adding email marketing integration:
   - Mailchimp
   - ConvertKit
   - SendGrid
   - Or any email service provider with API

### Alternative: Wix Forms
If you prefer using Wix's built-in system:
1. Create a Wix Form in your Wix dashboard
2. Replace the custom email inputs with Wix Form elements
3. Access submissions through Wix's Contact Manager

---

## ❓ Troubleshooting

**Problem**: "getCollectedEmails() is not defined"
- **Solution**: Make sure you're on the live site (not editing mode)

**Problem**: Emails not appearing in Google Sheets
- **Solution**: 
  1. Check the Web App URL is correct in `index.html`
  2. Verify deployment settings (Execute as: Me, Access: Anyone)
  3. Check the Apps Script logs for errors

**Problem**: Button doesn't respond
- **Solution**: Check browser console (F12) for JavaScript errors

---

## 📞 Support

If you need help:
1. Check the browser console for error messages
2. Verify the Google Apps Script is deployed correctly
3. Test with a simple email first (e.g., test@test.com)

---

**Last Updated**: November 23, 2025
