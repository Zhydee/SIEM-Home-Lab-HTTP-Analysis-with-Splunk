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

## 1. Top 10 Source IPs Generating Web Traffic
### 🎯 Goal

Identify the top 10 source IP addresses (`id.orig_h`) that generated the most HTTP requests. This helps you:
- Detect possible attackers or scanners  
- Identify noisy or misbehaving clients  
- Spot abnormal traffic generators  

---
![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471758116-b4da05d8-3146-48e1-bc4f-707d6e60fe3c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3NTAwODEsIm5iZiI6MTc1Mzc0OTc4MSwicGF0aCI6Ii82NzU4Nzk4NS80NzE3NTgxMTYtYjRkYTA1ZDgtMzE0Ni00OGUxLWJjNGYtNzA3ZDZlNjBmZTNjLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI5VDAwNDMwMVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI5YWIwMmMzNDFhMDg3NjBjZDgwYWIwOTk5ZDFmNTRmMjI0YWFlMjAzYjc3ZjI5MjZiZTVjYTU3ZWQ5MzU2ZmImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.ij-wXi6hjxOaM69eCXgWnn_hpbrrHK2R1W8khyB-bsw)

<p align="center"><b>Figure 7:</b> Result of Top 10 Source IPs Generated.</p>

### 📊 Top 10 Source IPs Generating HTTP Traffic

| IP Address  | Request Count |
|-------------|----------------|
| 10.0.0.28   | 76             |
| 10.0.0.31   | 73             |
| 10.0.0.42   | 73             |
| 10.0.0.27   | 72             |
| 10.0.0.40   | 70             |
| 10.0.0.45   | 70             |
| 10.0.0.14   | 69             |
| 10.0.0.11   | 67             |
| 10.0.0.13   | 65             |
| 10.0.0.25   | 65             |

### ✅ What I Observed and Concluded

All top 10 IPs in the HTTP dataset are internal IP addresses (RFC1918 range), and their request counts are fairly close — ranging between 65 and 76 requests each. This strongly suggests:

- The traffic is **evenly distributed**, possibly from **simulated or scripted clients** in a lab environment.  
- There is **no single IP overwhelming the system**, which reduces the likelihood of DoS-style abuse.  
- These IPs may represent different **simulated users, scanning agents, or test nodes** interacting with the server in a controlled or test dataset.

> In a real-world environment, I should pay close attention to any IP with **significantly higher traffic**, especially when combined with **high 4xx/5xx error rates** or access to **suspicious URIs**.  
> However, in this dataset, the pattern appears **deliberately balanced**, likely for **training or testing purposes**.


## 2. Count the number of server errors (5xx) observed
### 🎯 Goal

Count how many times the server returned an error (status codes **500–599**), which helps detect:

- Application crashes  
- Resource exhaustion (e.g., out of memory, CPU overload)  
- Misconfigured services  
- Possible exploitation attempts (e.g., forced 500 errors via malformed inputs)

---
![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471760265-1e544cbe-91fb-42ae-9530-b7d6a4a4038f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3NTA4MjcsIm5iZiI6MTc1Mzc1MDUyNywicGF0aCI6Ii82NzU4Nzk4NS80NzE3NjAyNjUtMWU1NDRjYmUtOTFmYi00MmFlLTk1MzAtYjdkNmE0YTQwMzhmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI5VDAwNTUyN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU1ZjI0MDczMmZlMGE2NmY3YzBjYWU4ODI1OTU0NTQ0ZDM5NGM4OTNhYmNhY2M5MTMyYzNkMzVkNmFiMzJjNDYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.UQhQkjhwEJ84a5vlQBJADEVxD4ts2a0pHUtFNN7G-VM)

<p align="center"><b>Figure 8:</b> Result of Server Error Responses (5xx)</p>

### 📊 Result of Total HTTP status codes in the 5xx range

| Metric         | Value |
|----------------|-------|
| Server Errors  | 285   |

### 🧠 What It Means

| Status Code | Meaning                 | Possible Cause                             |
|-------------|-------------------------|--------------------------------------------|
| 500         | Internal Server Error   | Unhandled exception, backend crash         |
| 502         | Bad Gateway             | Invalid response from upstream server      |
| 503         | Service Unavailable     | Server overloaded or under maintenance     |
| 504         | Gateway Timeout         | Upstream timeout                           |

### ✅ What I Observed and Concluded

The analysis revealed a total of **285 HTTP 5xx responses**, which accounts for approximately **9.5%** of all HTTP activity in the dataset. This is a significant portion, suggesting it could potentially have issues on the **server side**.

Server-side errors (such as `500 Internal Server Error` and `503 Service Unavailable`) generally indicate that the **web application failed to process valid client requests**. These errors could be caused by:

- Misconfigured server components  
- Backend crashes or unhandled exceptions  
- Overloaded or unavailable services  
- Or in lab environments, intentional simulation of faults or stress conditions  

Given the presence of previously identified **suspicious URIs** (`/etc/passwd`, `/shell.php`, etc.), it's also likely that **some of these 5xx errors were triggered by malformed or malicious input**.

In a real-world scenario, this volume of server errors would be a **red flag**, prompting deeper investigation into web server logs, backend application errors, and potential exploitation attempts.


## 3. Identify User-Agents associated with possible scripted attacks
### 🎯 Goal

Spot automated tools or scripted attacks based on the `user_agent` string in HTTP requests. Many attack tools use unique or non-browser user agents that can help identify malicious activity.

![Top Source IPs](https://private-user-images.githubusercontent.com/67587985/471761499-0289d26f-6506-4002-b33a-b8e3711c2b32.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTM3NTEzMTQsIm5iZiI6MTc1Mzc1MTAxNCwicGF0aCI6Ii82NzU4Nzk4NS80NzE3NjE0OTktMDI4OWQyNmYtNjUwNi00MDAyLWIzM2EtYjhlMzcxMWMyYjMyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA3MjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNzI5VDAxMDMzNFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ0NGFlYzYwN2QxNjgyMTdhYThlY2ZjNWNmOTcyYTU2MGI3MTczODBiYWJiMjY0MjFmMTcwM2Y3YjQ4YjRlMWMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.2ip-FNRevsAXsJqU4KDRmfk0xy4D-ukoIOITRYnISLE)

<p align="center"><b>Figure 9:</b> Result of Malicious User Agents</p>

## 📊 Result: Suspicious User-Agent Activity

| User-Agent                | Count |
|--------------------------|-------|
| sqlmap/1.5.1             | 79    |
| python-requests/2.25.1   | 78    |
| botnet-checker/1.0       | 70    |
| curl/7.68.0              | 69    |

### ✅ What I Observed and Concluded

The HTTP log analysis reveals that at least 296 requests were made using non-browser user agents commonly associated with automated attacks or malicious scanning tools. The presence of tools like `sqlmap`, `curl`, and `python-requests` highlights:

- Reconnaissance and probing activity from automated tools  
- Potential exploitation attempts, especially SQL injection (`sqlmap`)  
- Traffic consistent with simulated attack scenarios or real-world attacker behavior  

In a real-world environment, these user-agents would be considered strong Indicators of Compromise (IOCs) and should trigger:

- Security alerts  
- IP blocks  
- Further investigation into the affected endpoints

## 🛡️ Real-World HTTP Log Analysis: What Pro Analysts Look For

| 🔍 **Focus Area** | 🧠 **Why It's Important** | 🧪 **Sample Splunk Query** |
|------------------|---------------------------|----------------------------|
| **Sensitive URIs Accessed** | Detects probing of admin or critical files | `index=<your_index> sourcetype=<your_sourcetype> uri="/admin" OR uri="/wp-admin" OR uri="/etc/passwd" OR uri="/shell.php" \| stats count by uri, id.orig_h` |
| **Brute Force Attempts** | Identifies repeated login failures from same host | `index=<your_index> sourcetype=<your_sourcetype> action="login" status="failed" \| stats count by id.orig_h, user` |
| **Malicious User-Agents** | Flags toolkits like `sqlmap`, `curl`, etc. | `index=<your_index> sourcetype=<your_sourcetype> \| top limit=10 user_agent` |
| **Unusual HTTP Methods** | `PUT`, `DELETE`, or `CONNECT` may indicate attacks | `index=<your_index> sourcetype=<your_sourcetype> \| stats count by method \| where method IN ("PUT", "DELETE", "CONNECT")` |
| **High 4xx/5xx Error Rates** | Detects scanning, broken endpoints, or attacks | `index=<your_index> sourcetype=<your_sourcetype> status_code>=400 \| stats count by status_code` |
| **Spike in Requests/IP** | Unusual traffic from a host = DDoS or scanner | `index=<your_index> sourcetype=<your_sourcetype> \| timechart span=1m count by id.orig_h` |
| **Large File Downloads** | Could indicate data exfiltration attempts | `index=<your_index> sourcetype=<your_sourcetype> resp_body_len>500000 \| table _time id.orig_h uri resp_body_len \| sort -resp_body_len` |
| **GeoIP Lookup** | Identifies access from foreign or malicious regions | `index=<your_index> sourcetype=<your_sourcetype> \| iplocation id.orig_h \| stats count by Country` |
| **Repeated URI Requests** | High repetition may signal automation or attack scripts | `index=<your_index> sourcetype=<your_sourcetype> \| stats count by id.orig_h, uri \| sort -count` |

## 🧠 Conclusion
This project demonstrates how Splunk can be effectively used to analyze HTTP logs for identifying web traffic patterns, anomalies, and signs of malicious behavior. Through field extraction, search queries, and visualizations, we were able to:

- Identify common and uncommon HTTP methods, including possible automated activity through abnormal usage of `PUT`, `DELETE`, `OPTIONS`, and `CONNECT`
- Analyze the most accessed URIs, revealing signs of scanning, brute-force, and exploitation attempts (e.g., `/admin`, `/etc/passwd`, `/wp-admin`)
- Evaluate response codes to detect a significant number of 4xx and 5xx errors, highlighting potential misconfigurations or attack-related errors
- Detect suspicious user-agent strings like `sqlmap`, `curl`, and `python-requests`, which are commonly tied to automated tools and scanners
- Examine source IP addresses, showing evenly distributed activity from internal IPs — likely indicating a controlled lab or simulation environment
- Identify large file transfers and server-side errors that would warrant investigation in a real-world setup

## 📎 References
(https://github.com/0xrajneesh/Splunk-Projects-For-Beginners/blob/main/project%233-analyzing-http-logs-using-splunk-siem.md)
