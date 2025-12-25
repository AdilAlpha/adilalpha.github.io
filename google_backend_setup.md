# 📧 Free Unlimited Email Backend (Google Apps Script)

Follow these steps to enable your Contact Form to send emails using your own Gmail account for free (up to 100/day).

## Step 1: Create the Script
1. Go to **[script.google.com](https://script.google.com/)** and click **"New Project"**.
2. Name the project `Portfolio Contact Core`.
3. Delete any code in `Code.gs` and paste the following script:

```javascript
const ADMIN_EMAIL = "adilwebsite01@gmail.com"; // Where form submissions go
const SEND_RECEIPT = true; // Send confirmation to user?

function doPost(e) {
  var lock = LockService.getScriptLock();
  lock.tryLock(10000);

  try {
    var subject = e.parameter.subject;
    var message = e.parameter.message;
    var userEmail = e.parameter.email;
    var name = e.parameter.name;

    // --- A. Send to Admin (Lavender Dream Theme) ---
    var adminHTML = '<table width="100%" cellpadding="0" cellspacing="0" style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); padding: 40px 20px;">' +
      '<tr><td align="center">' +
        '<table width="500" cellpadding="0" cellspacing="0" style="background: #fff; border-radius: 12px; overflow: hidden;">' +
          '<tr><td style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); padding: 30px; text-align: center;">' +
            '<div style="color: #fff; font-size: 24px; font-weight: bold; text-shadow: 0 2px 4px rgba(0,0,0,0.1);">ADIL ANWAR</div>' +
          '</td></tr>' +
          '<tr><td style="padding: 40px;">' +
            '<h2 style="margin: 0 0 20px 0; color: #333;">New Contact Inquiry</h2>' +
            '<table style="width: 100%;">' +
              '<tr><td style="padding: 8px 0; color: #888;">From:</td><td style="font-weight: bold;">' + name + '</td></tr>' +
              '<tr><td style="padding: 8px 0; color: #888;">Email:</td><td style="font-weight: bold; color: #a18cd1;">' + userEmail + '</td></tr>' +
              '<tr><td style="padding: 8px 0; color: #888;">Subject:</td><td style="font-weight: bold;">' + subject + '</td></tr>' +
            '</table>' +
            '<div style="margin-top: 25px; padding: 20px; background: #f9f6fc; border-left: 4px solid #a18cd1; border-radius: 4px;">' +
              '<strong style="display: block; margin-bottom: 10px;">Message:</strong>' +
              '<p style="margin: 0; color: #555; font-size: 14px; line-height: 1.6;">' + message + '</p>' +
            '</div>' +
            '<div style="margin-top: 25px; text-align: center;">' +
              '<a href="mailto:' + userEmail + '" style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); color: #fff; padding: 12px 30px; text-decoration: none; border-radius: 25px; display: inline-block; font-weight: bold;">Reply Now</a>' +
            '</div>' +
          '</td></tr>' +
          '<tr><td style="background: #fafafa; padding: 15px; text-align: center; font-size: 12px; color: #888;">' +
            'adilalpha.github.io' +
          '</td></tr>' +
        '</table>' +
      '</td></tr>' +
    '</table>';

    MailApp.sendEmail({
      to: ADMIN_EMAIL,
      subject: "Portfolio Contact: " + subject,
      htmlBody: adminHTML
    });

    // --- B. Send Receipt to User (Lavender Dream Theme) ---
    if (SEND_RECEIPT && userEmail) {
      var userHTML = '<table width="100%" cellpadding="0" cellspacing="0" style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); padding: 40px 20px;">' +
        '<tr><td align="center">' +
          '<table width="500" cellpadding="0" cellspacing="0" style="background: #fff; border-radius: 12px; overflow: hidden;">' +
            '<tr><td style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); padding: 30px; text-align: center;">' +
              '<div style="color: #fff; font-size: 24px; font-weight: bold; text-shadow: 0 2px 4px rgba(0,0,0,0.1);">ADIL ANWAR</div>' +
            '</td></tr>' +
            '<tr><td style="padding: 40px; text-align: center;">' +
              '<h2 style="margin: 0 0 10px 0; color: #333;">Thanks for Visiting! ✨</h2>' +
              '<p style="color: #666; font-size: 16px; margin: 0 0 25px 0;">Hi <strong>' + name + '</strong>,</p>' +
              '<p style="color: #666; font-size: 15px; line-height: 1.6; margin: 0;">Your message has been received. I\'ll review your inquiry and will be contacting you real soon.</p>' +
              '<div style="margin: 30px 0; padding: 20px; background: #f9f6fc; border-radius: 8px;">' +
                '<div style="font-size: 13px; color: #888; margin-bottom: 5px;">Your inquiry:</div>' +
                '<div style="font-size: 14px; color: #a18cd1; font-weight: bold;">' + subject + '</div>' +
              '</div>' +
              '<a href="https://adilalpha.github.io" style="background: linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%); color: #fff; padding: 12px 25px; text-decoration: none; border-radius: 25px; display: inline-block; font-weight: bold;">Visit Portfolio</a>' +
            '</td></tr>' +
            '<tr><td style="background: #fafafa; padding: 15px; text-align: center; font-size: 12px; color: #888;">' +
              '&copy; 2024 Adil Anwar' +
            '</td></tr>' +
          '</table>' +
        '</td></tr>' +
      '</table>';

      MailApp.sendEmail({
        to: userEmail,
        name: "Adil Anwar",  // Display name
        replyTo: "adilwebsite01@gmail.com",  // Replies go here
        subject: "Thanks for contacting Adil Anwar",
        htmlBody: userHTML
      });
    }

    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'success', 'row': 'sent' }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (e) {
    return ContentService
      .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  } finally {
    lock.releaseLock();
  }
}
```

## Step 2: Deploy as Web App
1. Click the blue **Deploy** button (top right) -> **New deployment**.
2. Click the gear icon ⚙️ next to "Select type" and choose **Web app**.
3. **Description:** "Contact Form Backend"
4. **Execute as:** `Me (your@gmail.com)`
5. **Who has access:** `Anyone` (Important! This allows your website guests to use it).
6. Click **Deploy**.
7. **Authorization:** It will ask you to authorize. Click "Review Permissions" -> Choose Account -> **Advanced** (small text) -> **Go to (unsafe)** (since you wrote the code, it is safe).

## Step 3: Connect to Website
1. Copy the **Web App URL** (starts with `https://script.google.com/macros/s/...`).
2. Open your local file `assets/js/main.js`.
3. Scroll to the bottom function `sendEmail`.
4. Replace the placeholder URL with your real one:

```javascript
const scriptURL = 'https://script.google.com/macros/s/AKfycbx.../exec';
```

## Step 4: Test
1. Refresh your website (`index.html`).
2. Fill out the form and click Send.
3. Check your Gmail Sent folder/Inbox!
