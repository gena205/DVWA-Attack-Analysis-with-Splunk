 Sure, here's the entire project in plain text:

# Presentation: Analyzing XSS (Reflected) Attacks on DVWA Using Splunk

## Introduction
**Objective:** Conduct XSS (Reflected) attacks on DVWA, analyze logs in Splunk, and create alerts and a dashboard for monitoring.  
**Tools:** DVWA (Damn Vulnerable Web Application), Splunk Server, Splunk Forwarder, XAMPP on Windows 10.

## Step 1: Setting Up DVWA
1. **Launch DVWA:**
   - Start XAMPP on Windows 10.
   - Open a browser and navigate to `http://<Windows 10 IP Address>/DVWA`.

2. **Log into DVWA:**
   - Use the default credentials (admin/password).

3. **Set Security Level:**
   - Set DVWA security level to "Low".

## Step 2: Conducting XSS (Reflected) Attacks
1. **Navigate to the "XSS (Reflected)" section in DVWA.**

2. **Execute the following XSS payloads:**
   - Payload 1: `<script>alert('XSS1');</script>`
   - Payload 2: `<img src="x" onerror="alert('XSS2');">`
   - Payload 3: `"><script>alert('XSS3');</script>`
   - Payload 4: `"><img src=x onerror=alert('XSS4');>`
   - Payload 5: `<body onload=alert('XSS5')>`

## Step 3: Capturing Logs with Splunk Forwarder
1. **Set up Splunk Forwarder on Windows 10:**
   - Ensure Splunk Forwarder is configured to capture Apache logs.
   - Apache logs are typically located at `C:\xampp\apache\logs\access.log`.

2. **Verify log reception on Splunk Server:**
   - Go to the Splunk Server interface and ensure logs are being received.

## Step 4: Analyzing Logs in Splunk
1. **Create a new search in Splunk:**
   - Navigate to the Splunk Server interface.
   - Run the following search query: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload"`

## Step 5: Creating Alerts in Splunk
1. **Create a new alert:**
   - In the Splunk interface, save the search as an alert (`Save As -> Alert`).

2. **Configure the alert:**
   - Title: XSS Attack Detection
   - Description: Alert for detecting XSS (Reflected) attacks in DVWA
   - Alert Type: Scheduled
   - Run: Every 5 minutes
   - Cron Expression: `*/5 * * * *`
   - Trigger Condition: If number of results > 3
   - Trigger Actions: Send email (specify the email address for notifications)

## Step 6: Creating a Dashboard in Splunk
1. **Create a new dashboard:**
   - Go to the Dashboards section in the Splunk interface.
   - Click `Create New Dashboard`.
   - Enter a dashboard name, such as DVWA XSS Attack Dashboard.

2. **Add visualizations to the dashboard:**
   - **Events:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload"`
     - Content Title: XSS Attack Events

   - **Line Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | timechart count by payload`
     - Content Title: XSS Attack Attempts Over Time

   - **Bar Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | stats count by payload`
     - Content Title: XSS Payloads Frequency

   - **Pie Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | stats count by payload`
     - Content Title: Distribution of XSS Payloads

3. **Set the Time Range in the Dashboard:**
   - **Edit the time range of a panel:**
     - In edit mode, select the panel and click the pencil icon.
     - In the Time Range section, choose the desired time range (e.g., Last 24 hours).
     - Click Apply to save the changes.
     - Save the dashboard by clicking Save.

   - **Add a global time filter (optional):**
     - In edit mode, click Add Input and select Time.
     - Configure the time filter to apply to all panels in the dashboard.

## Conclusion
**Results:** You now have a configured Splunk dashboard for monitoring and analyzing XSS (Reflected) attack attempts on DVWA.  
**Benefits:** A user-friendly visual interface for quickly identifying and analyzing threats, with the ability to receive automatic notifications of new attacks.

## Questions and Discussion
- What other attacks can be added for monitoring?
- How can the efficiency of the dashboard and alerts be improved?
- What other tools can be used for security analysis?

By following these steps, you can effectively set up monitoring and analysis of XSS (Reflected) attacks on DVWA using Splunk.
