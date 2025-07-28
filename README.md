# 🔐 SIEM Home Lab – HTTP Analysis with Splunk

This project demonstrates how Splunk can be used to ingest and analyze HTTP log files to detect suspicious activity, investigate web traffic patterns, identify server errors, and possible signs of scripted attacks. The dataset was obtained from an open-source repository and represents simulated HTTP interactions typically seen in web environments.


---
## Introduction
HTTP (Hypertext Transfer Protocol) log files contain valuable information about web server activity, including requests, responses, user agents, and more. Analyzing HTTP logs using Splunk SIEM enables security professionals to monitor web traffic, detect anomalies, and identify potential security threats.

## 📁 Data Source

- **Source file**: `http_logs.json`  
- **Sourcetype**: `_json`  
- **Event count**: 3000 events  
- **Format**: Structured JSON containing fields such as `ts`, `status_code`, `id.orig_h`, `id.resp_h`, `method`, `uri`, `user_agent`, `event_type`, and `resp_body_len`

![Data Uploaded to Splunk](https://private-user-images.githubusercontent.com/67587985/471569820-c42be56c-1dd8-446a-b17f-25255c52af6a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTU5MDcsIm5iZiI6MTc1MzcxNTYwNywicGF0aCI6Ii82NzU4Nzk4NS80NzE1Njk4MjAtYzQyYmU1NmMtMWRkOC00NDZhLWIxN2YtMjUyNTVjNTJhZjZhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1MTMyN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWE3Zjc1NWEwODQwNGVjNGRkYjQ0YmI3MWVjYmVmNzBmMDhiMzFlYjRmN2MwMzljYWFjZTVkM2Y4NDY4N2U3MjcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.gxIoOIuHsM4F1oi5W93B9riLaqGWi0Smf8sT3eWYT9c)

<p align="center"><b>Figure 1:</b> Data successfully uploaded into Splunk.</p>

## ✅ 1. Relevant Fields Extracted by Splunk

![Data Uploaded to Splunk](https://private-user-images.githubusercontent.com/67587985/471572219-23e0f53d-63c5-461b-93da-605038d5a088.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTYxNzQsIm5iZiI6MTc1MzcxNTg3NCwicGF0aCI6Ii82NzU4Nzk4NS80NzE1NzIyMTktMjNlMGY1M2QtNjNjNS00NjFiLTkzZGEtNjA1MDM4ZDVhMDg4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1MTc1NFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTFhNGQ0ZjU3YTA5ODVhOWIyYjk1ODI4MDlkOTJjNjg1MTk3YzNmZmFjMDYyNjk2NTQxM2Q3ZDc4ZmE0MzE0NTEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Pq4gRzziJ1bw6IHmFFxScw4NMx4S602HNXTgnIRVVSw)

<p align="center"><b>Figure 2:</b> Search for http event with source file name.</p>

![Data Uploaded to Splunk](https://private-user-images.githubusercontent.com/67587985/471572430-8b430df3-9e18-46ad-b660-3199d845fae4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTYyMDEsIm5iZiI6MTc1MzcxNTkwMSwicGF0aCI6Ii82NzU4Nzk4NS80NzE1NzI0MzAtOGI0MzBkZjMtOWUxOC00NmFkLWI2NjAtMzE5OWQ4NDVmYWU0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1MTgyMVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTk2MjY3ZjlmMDYyNjJmNWI1YTA5NzIwZGU2ZDMzNDFkM2U1OTFiZmMwODY2M2YzY2E4OGNhNTdmNTUyOGFmYzgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.2yOmL4lE3e7SH7pTGcbKyxX6zpO3M3tALEIEU706AC0)

<b>Figure 3:</b> Interesting field extracted by Splunk.

### ➤ What was done:
Splunk automatically extracted all important fields from the JSON structure, including:
### ➤ Useful Extracted Fields that I Found Useful are:

| Field         | Description                                                                 |
|---------------|------------------------------------------------------------------------------|
| `event_type`  | Helps categorize traffic (e.g., `standard`, `client error`, `server error`) |
| `id.orig_h`   | **Source IP address** — client making the HTTP request                      |
| `id.resp_h`   | **Destination IP address** — the server receiving the request               |
| `method`      | HTTP method used (GET, POST, etc.)                                          |
| `status_code` | HTTP response status (200, 404, 500, etc.)                                  |
| `user_agent`  | Browser or client tool making the request                                   |
| `uri`         | The endpoint or resource requested                                          |



## 📊 2. Analyze HTTP Activity Patterns

## 1. Distribution of request methods (GET, POST, etc.)

### 🎯 Goal

Identify what types of HTTP methods are used and how often — this helps me understand the behavior of clients interacting with the web server


![Data Uploaded to Splunk](https://private-user-images.githubusercontent.com/67587985/471574580-17232209-35cd-4d2c-8ead-a91a9a6984d2.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTY0NzUsIm5iZiI6MTc1MzcxNjE3NSwicGF0aCI6Ii82NzU4Nzk4NS80NzE1NzQ1ODAtMTcyMzIyMDktMzVjZC00ZDJjLThlYWQtYTkxYTlhNjk4NGQyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1MjI1NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTZlMTIwZDU0ZTExYWFhYzhkZDg3MjRmY2U1YTRlZWQxN2NlZGViNGNlYjk2ODAyN2MxNDI3OWE5MWM3MmRkMjYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.plNqw_dOL8poIxtDRS7hlGSQ9JidHS7kSwyt78JbVFQ)

<p align="center"><b>Figure 4:</b> Result of HTTP Methods</p>

### ✅ HTTP Method Distribution Result

| Method  | Count |
|---------|-------|
| GET     | 1346  |
| POST    | 1353  |
| OPTIONS | 77    |
| CONNECT | 77    |
| DELETE  | 74    |
| PUT     | 73    |

### ✅ What I Observed and Concluded

I analyzed the `method` field to understand the interaction patterns in the HTTP traffic. While the high volume of `GET` and `POST` requests indicates normal web activity such as page loads and form submissions, I noticed the presence of less common methods like `CONNECT`, `OPTIONS`, `DELETE`, and `PUT` — each occurring at nearly equal frequency (around 73–77 times).

These uncommon methods are typically rare in standard browsing behavior and may signal something unusual:

| Method   | Common Use Case                          | Suspicious When                                            |
|----------|-------------------------------------------|-------------------------------------------------------------|
| OPTIONS  | Browser preflight request (CORS)          | Appears too frequently, indicating automated scanning       |
| CONNECT  | Establishing tunnels (e.g., proxy usage)  | May imply proxy tunneling or active reconnaissance          |
| DELETE   | REST API operations                       | Rare in normal users; may point to probing or data removal  |
| PUT      | File uploads (REST APIs)                  | Could indicate upload attempts or backend interaction       |

The even distribution of these methods strongly suggests:
- The use of **automated tools or scripts**
- Possible **API fuzzing or scanning activity**
- A **simulated attack pattern**, especially if the data was generated in a lab environment

🧠 **Final Conclusion**:  
While the core web traffic behavior appears normal, the consistent and unusual presence of HTTP methods like `PUT`, `DELETE`, `OPTIONS`, and `CONNECT` is a red flag. This pattern is unlikely to occur in organic user behavior and more likely indicates scripted interactions, scanning tools, or a controlled attack simulation.


## 2. Identify top URLs or endpoints accessed by users.
### 🎯 Goal

Find out which endpoints (URIs) are most frequently accessed. This helps you:
- Understand common user behavior  
- Identify high-traffic pages or API endpoints  
- Spot unusual or suspicious activity (e.g., repeated access to login or admin pages)

---
![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471580665-4ad92425-a06e-40a6-8afc-11aa91c0b6a5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTczNjgsIm5iZiI6MTc1MzcxNzA2OCwicGF0aCI6Ii82NzU4Nzk4NS80NzE1ODA2NjUtNGFkOTI0MjUtYTA2ZS00MGE2LThhZmMtMTFhYTkxYzBiNmE1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1Mzc0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTFjMTFjM2IzYjVmNWQ5ZGZhZWU2OTJlMTM4NThlMzU3MTc2OWNmM2ZkNWE3ZmJhMWVhMTY2MDE0NDI0ZWZmZGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.52Qy0HHz5mQBcem20NfJBojIHwYeGVw1FifrRIIEVFU)

<p align="center"><b>Figure 5:</b> Result of HTTP The Top Source URLs or Endpoints</p>

### ✅ Summary of Top Accessed URIs

| URI           | Count | %      | Description / Insight                                        |
|---------------|-------|--------|---------------------------------------------------------------|
| /index.html   | 2830  | 94.3%  | Normal homepage traffic — this is your primary page          |
| /admin        | 38    | 1.27%  | Suspicious — trying to access admin panel                    |
| /etc/passwd   | 35    | 1.17%  | Highly suspicious — OS file path, likely an attack attempt   |
| /wp-admin     | 25    | 0.83%  | WordPress admin panel — brute force or enumeration           |
| /shell.php    | 25    | 0.83%  | Very suspicious — possible web shell upload attempt          |
| /config.php   | 25    | 0.83%  | Sensitive file — attackers may try to read config credentials|
| /phpmyadmin   | 22    | 0.73%  | Database admin panel — often targeted in scans               |


### ✅ What I Observed and Concluded

From the URI analysis, it reveals that while the majority of traffic (94.3%) was directed to a legitimate page (`/index.html`), there is clear evidence of malicious probing and exploitation attempts in the remaining requests.

Endpoints such as `/admin`, `/wp-admin`, `/phpmyadmin`, `/etc/passwd`, `/shell.php`, and `/config.php` are commonly targeted by attackers for:
- Admin panel access  
- CMS brute-force or enumeration  
- Sensitive file exposure  
- Web shell uploads  
- Path traversal or LFI (Local File Inclusion)  

These patterns strongly suggest that the environment was either:
- Under **automated scanner or bot activity**, or  
- Intentionally **seeded with malicious behavior** (e.g., for lab or simulation purposes)  

> In a production SIEM environment, such access patterns would be flagged for investigation and may indicate attempted exploitation or early stages of a web-based intrusion.


## 3.  Analyzing response codes to identify errors or successful requests.

### 🎯 Goal
Understand how your web server responded to requests — this helps detect:
- Successful user interactions (`200 OK`)
- Client errors (`404 Not Found`, `403 Forbidden`)
- Server errors (`500 Internal Server Error`, etc.)

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471584582-397d9445-a846-42c4-a507-d1fa5811975b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3MTc4NDUsIm5iZiI6MTc1MzcxNzU0NSwicGF0aCI6Ii82NzU4Nzk4NS80NzE1ODQ1ODItMzk3ZDk0NDUtYTg0Ni00MmM0LWE1MDctZDFmYTU4MTE5NzViLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI4VDE1NDU0NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWMwM2M4NTVlYWUyNzA4MTJjY2E1Y2JhYzg2YzYzNDk2MTk0NDAyYjYzMGM1ZTQwNGNmYzE3ZmNmNGRhNDgzMzcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.s_bSXOq17TYwmI8wdup1t6CpFpK3AiSgCsTO-PzCL6c)

<p align="center"><b>Figure 6:</b> Result of HTTP Response Code</p>

### 📊 Response Code Summary

| Status Code | Count | Meaning                                       |
|-------------|-------|-----------------------------------------------|
| 200         | 2256  | OK – Successful requests                      |
| 400         | 221   | Bad Request – Malformed requests (often attackers) |
| 404         | 238   | Not Found – Missing resource (probing / recon) |
| 500         | 140   | Internal Server Error – App/server failed     |
| 503         | 145   | Service Unavailable – Server temporarily overloaded/down |

### ✅ What I Observed and Concluded

The majority of traffic (2256 out of 3000) returned a `200 OK`, indicating normal and successful user interactions.

However, there is a notable volume of error responses (**744 events or ~24.8%**), which indicates abnormal or malicious activity:

- 🔸 **404 Not Found (238)**: Often a result of automated tools scanning for non-existent or vulnerable files (e.g., `/admin`, `/shell.php`, `/config.php`).
- 🔸 **400 Bad Request (221)**: May indicate malformed HTTP requests, potentially tied to attack payloads or scanner probes.
- 🔸 **500 & 503 Errors (140 + 145)**: Suggest backend instability or that the server was overloaded or reacting poorly to certain requests — possibly due to forced errors during exploitation attempts.

These error patterns, especially in combination with the suspicious URIs previously identified, strongly suggest this log set includes **simulated attack behavior** or **real probing/scanning attempts**.

## 📊 3. Detect Anomalies
## 📌 Objective

The goal is to detect unusual behavior in SSH activity by focusing on the following indicators:

- Time-based event spikes  
- Failed login patterns  
- Suspicious IP behavior

## 1. Look for unusual patterns in SSH activity (e.g., sudden spikes in login attempts)

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471067109-dff7a77a-dbfb-48a9-a672-7e926a4abea7.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MjY1NDMsIm5iZiI6MTc1MzUyNjI0MywicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjcxMDktZGZmN2E3N2EtZGJmYi00OGE5LWE2NzItN2U5MjZhNGFiZWE3LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDEwMzcyM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJiNzlmOGEyZTVjNTBmMDcxMDNjMzJlYjg3NzVlNDQzYzJiYWMyNGJjMDc1NDJhNDVhNzQ1MzkyMGVlMjkzZjcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.RDBuI-e9iCilqdjvMQb6XlC3FpVTXrtJ2lB5fLLpd0Y)

<p align="center"><b>Figure 5:</b> Timechart output showing the count of SSH log events over a one-hour span.</p>

## 📊 Result of timechart

| Time | Count |
|-------------|--------|
| 2025-04-25 18:00       | 3000   |

## 📉 Observation
All SSH events in this dataset occurred within a single hour, producing a single visible bar on the timechart. This suggests the dataset is time-compressed or simulated.


## 🧪 Conclusion
Even with a limited time range, this method demonstrates how time-based visualizations can help detect:

- Abnormal spikes in SSH traffic

- Potential brute-force patterns

- Unusual login activity frequency

## 2. Analyze failed login attempts

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471067675-976737f1-c302-4524-a0e2-a11475f8553c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MjcxMzIsIm5iZiI6MTc1MzUyNjgzMiwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjc2NzUtOTc2NzM3ZjEtYzMwMi00NTI0LWEwZTItYTExNDc1Zjg1NTNjLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDEwNDcxMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWRlOWQ1Yjk4ZGZlODE1NjgyMjA0Y2E2YzIyNjQ0MWVmYzRjMmNjZTRhZTJhZjM2MjkxOWRiN2E4OWFjOWI3NjYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.4NchKiW6x9Yt_OeHlXZOVrrQCe3HXrVtNOCroQu_-wE)

<p align="center"><b>Figure 6:</b> Result of failed SSH login attempts</p>

## 📊 Result of HTTP Status Codes

| Time | failed_logins |
|-------------|--------|
| 2025-04-25 18:00       | 744   |

## 📉 What I Observed

All 744 failed login attempts were logged within a single hour window (`2025-04-25 18:00`).  
This was visualized as a single bar on the Splunk timechart.

## 🧠 Why This Still Matters

Even though the dataset is time-compressed and doesn't span multiple hours or days, this approach still demonstrates:

- The value of time-based visualizations  
- How sudden spikes can reveal security threats  
- That real-world SSH logs would benefit from this technique

If this were production data, seeing hundreds of failed logins in a short time window would be a strong indicator of a brute-force attack or bot activity.

## 🧪 Conclusion

Using the condition `status_code >= 400`, I isolated failed login attempts and visualized their frequency over time.  
Although the dataset only covers a one-hour period (744 failed events), this time-based view is valuable in real-world scenarios to detect spikes in login failures.

A high number of failures in a short span is often associated with brute-force password guessing, scripted reconnaissance, or malformed request testing.

## 3. Investigate SSH sessions from unusual or suspicious source IP addresses
![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471068751-97cec61b-0830-4f31-b21b-983fc8c07374.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1Mjc5MzcsIm5iZiI6MTc1MzUyNzYzNywicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjg3NTEtOTdjZWM2MWItMDgzMC00ZjMxLWIyMWItOTgzZmM4YzA3Mzc0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDExMDAzN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTM5MWZmNWIxMmY3NTZjOTZlYzBhYmNhMGU5MjYxYzI2ZmMzNzFiZDQxMGQ0MTBhZGE5MzU0YzczMTRiM2NmMTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.zXU0RpeS8rQAeTh6ezw5Ni0E4SUhmt0NPVP5SEzGgXo)

<p align="center"><b>Figure 7:</b> Event type for 10.0.0.28</p>

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471068774-d7c3d06f-c51a-4661-8053-15515f705715.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1Mjc5NTAsIm5iZiI6MTc1MzUyNzY1MCwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjg3NzQtZDdjM2QwNmYtYzUxYS00NjYxLTgwNTMtMTU1MTVmNzA1NzE1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDExMDA1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTE3ZDBjMzllZTFkNmNmNjU5MDBmMjlmMTMyNDU5Yzc3YjJkMDEwNDg4Yzk4ZGNiZTFlNmI3OTFiN2VkMzMxMTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Oc9WCHL2L-x6qPjqDDVU0MBY8Z0iTU0L0hQ9FQHdNXU)

<p align="center"><b>Figure 8:</b> Method for 10.0.0.28</p>

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471069143-ec5ce1c6-38c9-4187-9ca2-6b125e168dd5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MjgxMTgsIm5iZiI6MTc1MzUyNzgxOCwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjkxNDMtZWM1Y2UxYzYtMzhjOS00MTg3LTljYTItNmIxMjVlMTY4ZGQ1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDExMDMzOFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJjNzI3YWM4MmIyYTIzNGViMjBiZmM4OWM1OGJjMjIzODkzMzM1ODA1NDgyODMxZTczM2MwNjgzMmNhZDk1NWEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.iegHbXEQXMtqBNiGWMy7DEVGPNuU5HQVou3izccybh8)

<p align="center"><b>Figure 9:</b> Event type for 10.0.0.42</p>

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471069157-cc44312c-5e7b-4c49-a5e8-409df8c93291.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MjgxMjUsIm5iZiI6MTc1MzUyNzgyNSwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNjkxNTctY2M0NDMxMmMtNWU3Yi00YzQ5LWE1ZTgtNDA5ZGY4YzkzMjkxLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDExMDM0NVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTEwZDcxYzFhYWE3OTQxYjQwYjE4MDQxMzlkOWJjZGRjMGNlNjUwNzU3Njg4MmI0MTk5YzhiZjg5ZjU4ZDAxMTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.E2cUZwYNfie3hOOJlDK-KVy8ie1poBg42-5p4_xykPg)

<p align="center"><b>Figure 10:</b> Method for 10.0.0.42</p>

## 📊 Per-IP Activity Analysis

This section dives into two of the top source IP addresses based on SSH activity volume, highlighting their behavior across key fields.

---

### 🔸 IP: `10.0.0.28`

| **Field**     | **Top Values**                                                                                   | **Insights**                                                |
|---------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `event_type`  | Standard (37%), Client Error (21%), Server Error (13%), Suspicious Agent(9.2%),                  | Shows both normal and suspicious interactions                |
| `method`      | POST (50%), GET (43%), PUT/DELETE (minor)                                                        | High POST = possible payload uploads or login attempts       |

---

### 🔸 IP: `10.0.0.42`

| **Field**     | **Top Values**                                                                                   | **Insights**                                                |
|---------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `event_type`  | Standard (41%), Client Error(19%), Suspicious Agent(10%)                                         | Server Errors and suspicious patterns suggest aggressive or invalid use |
| `method`      | GET (52%), POST (42%), DELETE/PUT (minor)                                                        | GET-dominant = reconnaissance or probing                     |

## ✅ What I Observed and Concluded

I analyzed the behavior of the top two source IPs (`10.0.0.28` and `10.0.0.42`) to determine the nature of their interactions with the system.

The IP `10.0.0.28` demonstrated behavior consistent with **exploitation or payload delivery attempts**, showing a high percentage of **POST** requests along with several **PUT** and **DELETE** methods. Additionally, the presence of event types like **"Suspicious Agent"** and **"Suspicious URI"** supports the hypothesis of malicious activity intended to compromise the system.

In contrast, IP `10.0.0.42` exhibited a pattern dominated by **GET** requests and a notable number of **Server Error** and **Unexpected Method** responses. This aligns more with **reconnaissance or probing behavior**, possibly using automated tools to scan for exploitable paths or services.

These observed patterns are valuable in real-world SOC environments to distinguish **active exploit attempts** from **reconnaissance bots**, allowing teams to **prioritize response** and improve detection strategies.

## 🎯 Purpose of Investigating Top Source IPs in SSH Logs

Investigating the top source IPs is a critical part of SSH log analysis, as it provides insight into how different actors interact with the system and reveals patterns that may indicate malicious behavior.

By analyzing the top IPs, I aimed to understand the behavior profile of each source — whether they exhibited signs of aggressive scanning, exploitation attempts, or more passive interaction. Certain combinations of `event_type` and `method` fields help identify these patterns more clearly, especially when linked to failed login attempts or unusual method usage.

This analysis also supports attacker classification. For example, repeated requests using common brute-force signatures can point to automated tools, while more targeted or varied activity may suggest more advanced or hands-on attackers.

Overall, source IP analysis helps distinguish between random noise, automated recon, and deliberate exploitation, which is vital for effective threat detection and response.

## 📊 4. Monitor User Behavior
## 1. Failed Attempts by Session ID (uid)

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471074714-41b589f9-848a-4f23-af02-098c4baf0da1.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MzA5NjksIm5iZiI6MTc1MzUzMDY2OSwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNzQ3MTQtNDFiNTg5ZjktODQ4YS00ZjIzLWFmMDItMDk4YzRiYWYwZGExLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDExNTEwOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBiM2I4YWJmNGI1MThjYTEyYmExNjhlOTZlNjViNDJkZjMxNTI5Y2UwYzhlYTlmMDM4OTJkYmY5M2RlOTEzOGImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.9sHRQqeUWGi_f9mUEe62_fJGBcZrd4FlNcln-dLz8as)

<p align="center"><b>Figure 11:</b> Result of identifying failed login attempts by session</p>

## 📊 Result of HTTP Status Codes

| Seesion uid | failed_attempts |
|-------------|--------|
| HT1002482   | 1      |
| HT1010816   | 1      |
| HT1021105   | 1      |
....
Based on the data, I noticed that each failed login attempt was linked to a different session ID (`uid`), meaning every session only had one failure.

There were a total of **744 failed login events**, and all of them came from **744 unique sessions**. This pattern suggests the use of automated tools that rotate session IDs to avoid detection or rate-limiting, which is something attackers often do to stay under the radar.

## ✅ What I Observed and Concluded

To monitor user behavior, I analyzed **failed login attempts using the `uid` (session ID)** field, since the dataset didn’t contain any actual user account data.

The results showed that all **744 failed login attempts** were each tied to a **unique session ID**, with no repetition. This strongly suggests the use of **automated attack scripts** that intentionally generate a **new session for every attempt** — a known tactic to evade **rate-limiting**, **brute-force protections**, and detection by tools that rely on repeated attempts from the same source.

Even though this behavior doesn’t show repeated failures within a single session, it clearly points to a **distributed attack pattern**, which is much harder to detect unless advanced **cross-session correlation** or **source IP tracking** is in place.

This type of pattern is critical for SOC teams to understand, as it demonstrates how attackers adapt to basic detection thresholds by rotating identifiers across their attempts.

## 🎯 Purpose of Analyzing Failed Logins by `uid` (Session ID)

Even though the dataset lacks a `user` field, analyzing failed login attempts based on the `uid` (session ID) still provides meaningful insights into attacker behavior at the session level.

By focusing on `uid`, I was able to detect sessions with repeated failed login attempts, which is a strong indicator of brute-force activity or credential spraying tools. When a single session repeatedly fails to authenticate, it often points to automated tools attempting password guesses across multiple accounts.

This method also gives SOC analysts session-level visibility. Even without knowing the actual username, tracking activity by session reveals how attackers move through the system, how persistent they are, and what methods they use.

Additionally, analyzing by `uid` helps reduce noise in the logs by narrowing focus to particularly “loud” sessions — those that generate large volumes of failed attempts. This makes it easier to trigger alerts or apply thresholds, enabling faster response to likely threats.

## 2. Analyze Session Duration

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471076849-3c35d28a-dfde-4989-8cf2-fcb81c5dc5ab.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM1MzIwNDEsIm5iZiI6MTc1MzUzMTc0MSwicGF0aCI6Ii82NzU4Nzk4NS80NzEwNzY4NDktM2MzNWQyOGEtZGZkZS00OTg5LThjZjItZmNiODFjNWRjNWFiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjYlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI2VDEyMDkwMVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTRkZDk1ZjcwMjc2NTRlZmIyZGM1YWFhY2MzMWJjODBiOGU1NjZiZmFiYjI0ZDlmYzU1NWMzMzM0YzA2OWQ4YjEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.5Ko69zDL1r0aOViafXoVYbBynSbvfPOe5kq1m-wfnPg)

<p align="center"><b>Figure 11:</b> Result of identifying failed login attempts by session</p>

## 📊 Result of HTTP Status Codes

| Seesion uid | failed_duration |
|-------------|--------|
| HT1002482   | 0.00000      |
| HT1010816   | 0.00000      |
| HT1021105   | 0.00000      |
....
## 🧠 Why Session Durations Are 0

While attempting to calculate session durations using the `ts` (timestamp) field and `uid` (session ID), I found that each session only had a single log entry or all entries shared the exact same timestamp. As a result, the calculation of `max(ts) - min(ts)` per session always resulted in zero.

This is likely because the dataset is simulated — it comes from a public repository and seems to be designed primarily for basic detection or pattern analysis rather than modeling full session behavior. The absence of multiple time-staggered events per session means duration-based analysis isn’t meaningful in this case.

## ✅ What I Observed and Concluded

I attempted to analyze session durations by calculating the time difference between the first and last event (`ts`) for each session (`uid`). However, the results showed that every session had either a single timestamp or multiple events with identical timestamps. This caused all calculated durations to be `0`.

This observation suggests that the dataset logs only one key event per session, rather than tracking full session lifecycles. It's likely a limitation of the simulated dataset — which appears to be designed for basic detection and pattern recognition rather than detailed session-based analysis. As such, session duration insights were not possible in this scenario.


## 🧾 5. Splunk Report

This section showcases my ability to create and export reports directly from Splunk based on SSH log data analysis. These reports highlight key metrics such as request methods, status codes, and top source IPs—even when using synthetic or simulated datasets.

### 📝 Report Title: `SSH Activity Summary Report`
- **Export Format**: `.pdf`
- **Purpose**: Demonstrate proficiency in transforming search results into structured, shareable executive reports using Splunk's reporting functionality.
- **Note**: These reports focus on presenting clear metrics visually rather than providing exhaustive analysis.

### 📄 Reports Included
| Report Title | Description |
|--------------|-------------|
| [SSH_Request_Methods_Distribution-2025-07-26.pdf](SSH_Request_Methods_Distribution-2025-07-26.pdf) | Summarizes the frequency of different HTTP/SSH methods (GET, POST, PUT, DELETE, etc.) used in the log data. |
| [HTTP_Status_Code_Breakdown_in_SSH_Logs-2025-07-26.pdf](HTTP_Status_Code_Breakdown_in_SSH_Logs-2025-07-26.pdf) | Displays the count and distribution of HTTP status codes (e.g., 200, 404, 503), useful for spotting failed or malicious interactions. |
| [SSH_Top_Source_IPs-2025-07-26.pdf](SSH_Top_Source_IPs-2025-07-26.pdf) | Lists the most frequent IP addresses initiating SSH sessions or requests, helping to identify potential attackers or scanning sources. |

📌 _These reports serve as evidence of my technical skill in creating and formatting Splunk reports, suitable for team briefings, stakeholder updates, or audits._


## 🧩 6. Splunk Dashboard

### 🎯 Dashboard Title: `SSH Log Behavior Overview`

**Panels included**:
- **Top 10 Source IPs**: Horizontal bar chart showing top IPs by activity  
- **HTTP Status Code Breakdown**: Pie chart showing 200, 404, 400, 500, etc.  
- **Request Method Distribution**: Pie chart showing GET, POST, PUT, DELETE  

🔍 **Description**:  
> This dashboard provides a visual overview of SSH log behavior, including the most active source IPs (potential attackers), the types of HTTP request methods observed, and the distribution of HTTP response status codes. It helps analysts quickly identify unusual patterns in SSH traffic.

📄 **Dashboard PDF**:  
- [ssh_log_behavior_overview-2025-07-26.pdf](ssh_log_behavior_overview-2025-07-26.pdf)


## 🧠 Conclusion

This project demonstrated key Splunk capabilities:
- Field extraction and search optimization  
- Anomaly detection using time-based visualizations  
- Attack surface profiling based on source IPs  
- Behavioral pattern analysis using session metadata  
- Creation of interactive dashboards and exportable reports  

Although the dataset was simulated and time-constrained, the methods and skills applied reflect real-world SIEM use cases and blue-team analysis workflows.

💡 **Key takeaways from this home lab**:
- Gained experience in identifying critical patterns in SSH logs  
- Learned what to analyze to understand potential attacker behavior  
- Practiced extracting valuable insights from SSH logs using Splunk’s search language and visualization tools
