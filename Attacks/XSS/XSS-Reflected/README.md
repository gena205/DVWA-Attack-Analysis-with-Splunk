 Sure, here's the entire project in plain text:

# Presentation: Analyzing XSS (Reflected) Attacks on DVWA Using Splunk

## Introduction
**Objective:** Conduct XSS (Reflected) attacks on DVWA, analyze logs in Splunk, and create alerts and a dashboard for monitoring.  
**Tools:** DVWA (Damn Vulnerable Web Application),Splunk Forwarder and XAMPP on Windows 10, Splunk Server On Kali linux, and Kali for attacks.

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
  ![DVWA Security Level](https://github.com/gena205/DVWA-Attack-Analysis-with-Splunk/blob/main/Screenshots/xss/xss_reflected/alert1.png)
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS1.2.png)

- Payload 2: `<img src="x" onerror="alert('XSS2');">`
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS2.1.png)
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS2.2.png)

- Payload 3: `"><script>alert('XSS3');</script>`
  ![DVWA Security Level](./Screenshots/xss/xss_reflected/XSS3.1.png)
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS3.2.png)

- Payload 4: `"><img src=x onerror=alert('XSS4');>`
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS4.1.png)
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS4.2.png)

- Payload 5: `<body onload=alert('XSS5');>`
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS5.1.png)
  ![DVWA Security Level](Screenshots/xss/xss_reflected/XSS5.2.png)


## Step 3: Capturing Logs with Splunk Forwarder
1. **Set up Splunk Forwarder on Windows 10:**
   - Ensure Splunk Forwarder is configured to capture Apache logs.
   - Apache logs are typically located at `C:\xampp\apache\logs\access.log`.

2. **Verify log reception on Splunk Server:**
   - Go to the Splunk Server interface and ensure logs are being received.
   - Run a search query: index=main source="C:\\xampp\\apache\\logs\\access.log" to check if logs are being received. 
   ![DVWA Security Level](Screenshots/dvwa-security-level.png)

## Step 4: Analyzing Logs in Splunk
1. **Create a new search in Splunk:**
   - Navigate to the Splunk Server interface.
   - Run the following search query: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload"`
   ![DVWA Security Level](Screenshots\xss\xss_reflected\splunk_logs1.png)
   ![DVWA Security Level](Screenshots\xss\xss_reflected\splunk_logs2.png)

## Step 5: Creating Alerts in Splunk
1. **Create a new alert:**
   - In the Splunk interface, save the search as an alert (`Save As -> Alert`).
   ![DVWA Security Level](Screenshots\xss\xss_reflected\alert1.png)

2. **Configure the alert:**
   - Title: XSS Attack Detection
   - Description: Alert for detecting XSS (Reflected) attacks in DVWA
   - Alert Type: Scheduled
   - Run: Every 5 minutes
   - Cron Expression: `*/5 * * * *`
   - Trigger Condition: If number of results > 3
   - Trigger Actions: Send email (specify the email address for notifications)
   ![DVWA Security Level](Screenshots\xss\xss_reflected\alert2.png)

## Step 6: Creating a Dashboard in Splunk
1. **Create a new dashboard:**
   - Go to the Dashboards section in the Splunk interface.
   - Click `Create New Dashboard`.
   - Enter a dashboard name, such as DVWA XSS Attack Dashboard.
   ![DVWA Security Level](Screenshots\xss\xss_reflected\Dashboard1.png)
   ![DVWA Security Level](Screenshots\xss\xss_reflected\Dashboard2.png)

2. **Add visualizations to the dashboard:**
   - **Events:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload"`
     - Content Title: XSS Attack Events
   ![DVWA Security Level](Screenshots\xss\xss_reflected\events1.png)
   ![DVWA Security Level](Screenshots\xss\xss_reflected\Events2.png)

   - **Line Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | timechart count by payload`
     - Content Title: XSS Attack Attempts Over Time
   ![DVWA Security Level](Screenshots\xss\xss_reflected\line.png)

   - **Bar Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | stats count by payload`
     - Content Title: XSS Payloads Frequency
   ![DVWA Security Level](Screenshots\xss\xss_reflected\barchart.png)

   - **Pie Chart:**
     - Search: `index=main source="C:\\xampp\\apache\\logs\\access.log" "<script>" OR "alert" OR "onerror" OR "img src" OR "document.cookie" OR "body onload" | stats count by payload`
     - Content Title: Distribution of XSS Payloads
   ![DVWA Security Level](Screenshots\xss\xss_reflected\piechart.png)

3. **Set the Time Range in the Dashboard:**
   - **Edit the time range of a panel:**
     - In edit mode, select the panel and click the pencil icon.
     - In the Time Range section, choose the desired time range (e.g., Last 24 hours).
     - Click Apply to save the changes.
     - Save the dashboard by clicking Save.
   ![DVWA Security Level]()

   - **Add a global time filter (optional):**
     - In edit mode, click Add Input and select Time.
     - Configure the time filter to apply to all panels in the dashboard.
   ![DVWA Security Level](Screenshots\xss\xss_reflected\timerange.png)

## Conclusion
**Results:** You now have a configured Splunk dashboard for monitoring and analyzing XSS (Reflected) attack attempts on DVWA.  
**Benefits:** A user-friendly visual interface for quickly identifying and analyzing threats, with the ability to receive automatic notifications of new attacks.

## Questions and Discussion
- What other attacks can be added for monitoring?
- How can the efficiency of the dashboard and alerts be improved?
- What other tools can be used for security analysis?

By following these steps, you can effectively set up monitoring and analysis of XSS (Reflected) attacks on DVWA using Splunk.
