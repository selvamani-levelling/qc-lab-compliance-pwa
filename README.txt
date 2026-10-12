QC LAB COMPLIANCE — INSTALLABLE PWA

WHAT THIS PACKAGE DOES
- Adds an installable PWA shell for Android and desktop.
- Opens the existing Google Apps Script dashboard inside the app.
- Keeps Google Sheets as the source of truth; this package does not copy or migrate records.
- Does not replace, modify, or create reminder triggers. Existing Apps Script email reminders continue to run from the backend as configured.
- The service worker caches only the app shell, never Google Apps Script pages or QC records.

IMPORTANT BACKEND REQUIREMENT
The deployed Apps Script page must allow framing for the embedded dashboard. In Code.gs, doGet() should return the HTML output with:
  return HtmlService.createHtmlOutputFromFile('Index')
    .setTitle('QC Lab Compliance Dashboard')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);

If the current deployment does not already include ALLOWALL, add it to doGet(), save, deploy > Manage deployments > Edit > select New version > Deploy, and then reload the PWA. Be aware that allowing framing permits embedding; only use a trusted PWA host and keep access controls appropriate.
If you prefer not to allow embedding, the "Open full screen" button opens the existing dashboard directly, but it may leave the standalone app window.

PUBLISHING ON GITHUB PAGES
1. Create a GitHub repository named qc-lab-compliance-pwa (or use an existing repo).
2. Upload index.html, manifest.webmanifest, sw.js, and the icons/ folder to the repository root.
3. In repository Settings > Pages, deploy from the main branch and root folder.
4. Wait until GitHub Pages shows the live HTTPS URL.
5. Open that URL in Chrome on Android or desktop.

INSTALL
Android (Chrome):
- Open the HTTPS GitHub Pages URL.
- Tap the browser menu (⋮) > Install app or Add to Home screen > Install.
Desktop (Chrome/Edge):
- Open the HTTPS GitHub Pages URL.
- Use the install icon in the address bar, or browser menu > Install QC Lab Compliance.

DATA AND REMINDERS
- All document data remains in the existing Google Sheet used by Apps Script.
- Reminder emails remain handled by the Apps Script daily trigger and QC In-charge recipient setting.
- Do not create a second spreadsheet or run setupDashboard() against a new spreadsheet.
- PWA shell itself cannot show live records offline. Internet access and the correct Google account/access permissions are required.

TROUBLESHOOTING
- If the embedded view is blank, open the direct dashboard link from the app. If direct view works but embedded view is blank, check the doGet() ALLOWALL requirement and redeploy.
- If Google asks you to sign in, use the account that has access to the Apps Script deployment.
- If reminders stop, check Apps Script Triggers and the Settings recipient; PWA installation does not control reminders.
