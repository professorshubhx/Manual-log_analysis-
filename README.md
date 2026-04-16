# Manual-log-analysis-
This project highlights essential skills such as log filtering, event correlation, and manual threat analysis without relying on advanced SIEM tools. It serves as a foundational step toward understanding security monitoring and incident detection in Windows environments, preparing learners for more advanced cybersecurity tools and techniques.
# Log Analysis Basics: Windows Security Logs

---

## Objective

The objective of this lab is to introduce Windows Security Logs and demonstrate how to analyze authentication-related events. This includes identifying failed login attempts, successful logins, and credential access activities that may indicate suspicious behavior.

---

## Lab Setup

### Requirements

* System: Windows 10 / Windows 11
* RAM: Minimum 4 GB
* Tool: Windows Event Viewer
* Access: Administrator privileges
* Test Account: testuser

---

## What are Windows Security Logs?

Windows Security Logs record all security-related events on the system. These logs are critical for monitoring and detecting suspicious activities.

Key events include:

* Successful and Failed Login Attempts: Track user authentication
* Account Lockouts: Triggered after multiple failed attempts
* User Account Changes: Creation or modification of accounts
* Privilege Assignments: Administrative access events

These logs are essential for identifying brute force attacks, unauthorized access, and post-login activities.

---

## Understanding Event IDs in Security Logs

Some common Event IDs used in this lab include:

* Event ID 4625: Failed Login Attempt
* Event ID 4624: Successful Login
* Event ID 5379: Credential Access

These events help in identifying authentication patterns and suspicious behavior.

---

## Lab Task: Explore and Analyze Windows Security Logs

### Step 1: Open Event Viewer

* Press Win + R
* Type: eventvwr
* Navigate to: Windows Logs → Security

---

### Step 2: Filter Security Logs

* Click "Filter Current Log"
* Enter Event IDs:
  4625, 4624, 5379

---

### Step 3: Simulate Failed Login Attempts

Use incorrect credentials multiple times.

Example command:

net use \127.0.0.1\IPC$ /user:testuser test321

Repeat this multiple times to generate failed login logs.

### -In this image we use PowerShell command net use \127.0.0.1\IPC$ /user:testuser test321 to try access of Local host testuser
<img width="1920" height="950" alt="Annotation 2026-04-16 180735" src="https://github.com/user-attachments/assets/a37e8502-806e-431b-b3f3-3db41782faed" />


---

### Step 4: Simulate Successful Login

Use correct credentials:

net use \127.0.0.1\IPC$ /user:testuser correctpass

### -Successful Login after many unsuccessful try 
<img width="1920" height="950" alt="sucess" src="https://github.com/user-attachments/assets/798f02e0-fec2-4cd8-ace6-d263a470e3fc" />


### Step 5: Analyze Logs

Observe the generated logs in Event Viewer.

#### Failed Login (Event ID: 4625)

* Account Name: testuser
* Logon Type: 3 (Network)
* Failure Reason: Bad password
* Status Code: 0xC000006D
* Sub Status: 0xC000006A

<img width="1920" height="950" alt="faild login" src="https://github.com/user-attachments/assets/86614547-7b00-47a4-aefb-8af9a6f7dbaf" />


Observation:
Multiple failed attempts within a short time indicate possible brute force behavior.

---

#### Successful Login (Event ID: 4624)

* Account Name: testuser
* Domain: SHUBH
* Logon Type: 3 (Network)
* Authentication Package: NTLM

Observation:
Login was successful after multiple failed attempts.

---

#### Credential Access (Event ID: 5379)

* Account Name: admin
* Action: Credential Manager credentials accessed

Observation:
Indicates access to stored credentials, which may require further investigation.

---

## Attack Timeline

| Time     | Event ID | Description                    |
| -------- | -------- | ------------------------------ |
| 03:58 PM | 4625     | Multiple failed login attempts |
| 06:06 PM | 4624     | Successful login               |
| 06:08 PM | 5379     | Credential access              |

---

## Analysis

The logs show repeated failed login attempts followed by a successful login. This pattern is commonly associated with brute force attacks. The credential access event suggests possible post-authentication activity.

---

## Conclusion

The system experienced multiple failed authentication attempts, followed by a successful login. This indicates a potential brute force attack that resulted in successful access.

---

## Recommendations

* Enable account lockout policies
* Monitor failed login attempts regularly
* Implement multi-factor authentication
* Review credential access logs for suspicious activity

---

## Project Outcome

This lab demonstrates the ability to manually analyze Windows Security logs, detect suspicious authentication patterns, and perform basic SOC-level investigation.

