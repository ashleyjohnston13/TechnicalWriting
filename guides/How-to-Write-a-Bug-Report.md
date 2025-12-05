# How to Write a Clear Bug Report

A clear bug report helps engineering and support teams understand the issue quickly so they can reproduce it, diagnose it, and fix it. A well-written report saves time, prevents confusion, and increases the chances that the issue gets resolved correctly the first time.

This guide outlines how I structure bug reports and the details I include to make them as useful as possible.

---

## 1. Title

Write a short, descriptive title that explains the problem at a glance.

**Example:**  
"Updated CRM records are not appearing in the analytics dashboard"

---

## 2. Summary

Give a brief overview of what is happening.  
Focus on what the user expected to happen vs. what actually happened.

**Example:**  
"When a CRM contact is edited, the changes do not sync to the analytics system. The sync job runs successfully but does not process any updated records."

---

## 3. Steps to Reproduce

List the exact steps someone can follow to recreate the issue.  
This is one of the most important parts of any bug report.

**Example:**
1. Create a new contact in the CRM  
2. Edit the email field  
3. Wait one sync interval  
4. Check the analytics dashboard  
5. Updated contact does not appear  

Good reproduction steps remove guesswork and help engineering confirm the issue.

---

## 4. Expected Result

Explain what should have happened.

**Example:**  
"The analytics dashboard should show the updated contact after the sync job runs."

---

## 5. Actual Result

Explain what actually happened.

**Example:**  
"No updated records appear in the analytics dashboard, even though the sync job reports a successful run."

---

## 6. Scope and Frequency

If possible, clarify whether the issue:

- Affects all users or specific users  
- Happens every time or only sometimes  
- Impacts only new records, updated records, or both  
- Started recently or has been ongoing  

This helps diagnose whether the issue is systemic or isolated.

---

## 7. Environment Details

Include any relevant information about where the issue occurred:

- System or platform  
- Browser or device  
- Environment (production, staging, internal tool)  
- User roles or permissions  

**Example:**  
"Occurs in production environment for all Marketing Ops users."

---

## 8. Relevant Logs, Screenshots, or Error Messages

Attach anything that supports the bug report:

- API responses  
- Error messages  
- Console logs  
- Timestamp details  
- Screenshots of incorrect behavior  

Even small details can help engineering narrow down the cause.

---

## 9. Additional Notes

Use this section for anything extra that might be useful:

- When the issue was first noticed  
- Recent deployments or configuration changes  
- Whether similar issues happened in the past  

---

## Example Bug Report (Complete)

**Title:**  
Updated CRM records are not appearing in the analytics dashboard

**Summary:**  
When a CRM contact is edited, the changes do not sync to the analytics system. The sync job runs successfully but does not detect or process updated records.

**Steps to Reproduce:**
1. Edit any existing CRM contact  
2. Save the changes  
3. Wait one sync interval  
4. Check the analytics dashboard  

**Expected Result:**  
Updated contact should appear with the new field values.

**Actual Result:**  
Dashboard still shows the old values. Sync job reports zero processed records.

**Environment:**  
Production – all Marketing Ops users

**Notes:**  
Issue began after recent CRM automation changes.

---

A well-written bug report makes it easier for engineering to understand the problem and fix it quickly. Clear communication upfront saves significant troubleshooting time later.
