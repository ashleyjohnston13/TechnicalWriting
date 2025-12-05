# CRM–Analytics Sync Incident – Root Cause Analysis

**Role:** Technical support / analyst investigation  
**Context:** Internal SaaS product used by customer-facing teams  
**Date:** April 2025  

---

## Overview

A customer reached out because new and updated CRM contacts were not showing up in their analytics dashboard. The CRM and analytics tool normally sync every 15 minutes, but the customer noticed that several hours had gone by with no updates appearing.

I handled the investigation, worked with engineering when needed, and documented the issue, root cause, fix, and follow-up actions.

---

## Symptoms

- New CRM contacts were missing from the analytics dashboard.  
- Changes to existing contacts (email, lifecycle stage, status, etc.) weren’t reflected.  
- Automated segments in the analytics tool were empty or missing expected contacts.  
- The sync job appeared to run normally, but the number of processed records wasn’t changing.

---

## Impact

- The customer couldn’t run accurate weekly reports.  
- Automated marketing campaigns had to be paused due to outdated data.  
- Leadership requested updates because the delay affected time-sensitive decision-making.  
- The incident was treated as **high priority** since multiple teams relied on the missing data.

---

## Environment

- **CRM:** Salesforce (example system) with custom lifecycle fields  
- **Analytics Tool:** Internal reporting platform using an ETL-style sync  
- **Sync Method:** API pull every 15 minutes  
- **Reporting User:** Marketing Operations Manager  
- **Record Volume:** ~50,000 contacts, ~300 changes/day  

---

## Reproduction Steps

To confirm the issue myself:

1. Added a new contact in the CRM with a unique email.  
2. Verified it displayed correctly in CRM reports.  
3. Waited one sync window (15 minutes) and checked the analytics dashboard.  
4. The new contact did **not** appear.  
5. Updated an existing CRM contact.  
6. Again, the analytics dashboard didn’t reflect the update.  

This showed the problem was consistent and affected both new and edited records.

---

## Investigation

I walked through logs, recent changes, and system behavior to narrow down where the sync was failing.

### 1. Sync Job History  
- The job was running on time and showed no errors.  
- The “processed records” count hadn’t changed for hours.  
  - **Meaning:** The job was running but not picking up any new data.

### 2. API Logs  
- API calls from the analytics system returned **200 OK**, but with zero records.  
  - **Meaning:** The CRM API wasn’t identifying any updated or new contacts.

### 3. CRM Record Timestamps  
- Checked timestamps (`LastModifiedDate`) on multiple contacts.  
- Noticed that some records didn’t show updated timestamps even when fields were edited.  
  - **Red flag:** The sync depends on timestamp changes to know what to pull.

### 4. Recent CRM Configuration Changes  
- Reviewed internal change notes and saw that an admin recently updated automation rules.  
- One script designed to clean up unused fields was unintentionally overwriting the system-managed `LastModifiedDate` field.  
  - **This explained almost everything:** If the timestamp doesn’t change, the sync sees nothing to pull.

### 5. Reproducing the Behavior  
- Edited a test record while watching logs.  
- CRM UI showed the update, but the API returned the old timestamp.  
- This confirmed the behavior wasn’t isolated or random.

### 6. Engineering Review  
- Shared findings with engineering.  
- They confirmed the automation script wasn’t meant to touch the system field.  
- It had been deployed earlier that day without a full regression test.

---

## Root Cause

The CRM automation script unintentionally overwrote the system-managed `LastModifiedDate` field. Because the analytics system only syncs records that appear “new or changed,” it didn’t detect any of the recent updates. As a result:

- No records met the sync criteria  
- The API returned empty payloads  
- The analytics dashboard remained out of date  

---

## Resolution

1. Engineering rolled back the automation script.  
2. Confirmed that new CRM updates showed correct timestamps.  
3. Triggered a full re-sync to process all pending updates.  
4. Verified that:
   - New contacts appeared in analytics  
   - Updated fields synced correctly  
   - Segments populated as expected  

---

## Validation

To ensure the issue was fully resolved:

- Created and updated several test contacts  
- Checked timestamps via the CRM API  
- Ran a targeted sync and confirmed the data moved end-to-end  
- Verified that reports and segments updated normally  
- The customer confirmed everything looked correct on their dashboard  

---

## Prevention and Follow-Up

- Added a regression test to flag any changes to system-managed fields.  
- Updated documentation for CRM admins about handling system fields safely.  
- Added alerts for sync jobs that process zero records.  
- Updated the runbook with steps for checking timestamp-related sync issues.  

---

## Documentation Deliverables

As part of closing the incident, I created or updated:

- A troubleshooting guide for sync failures  
- Best practices for CRM automations  
- Runbook updates covering timestamp validation  
- A post-incident summary for internal teams  

---
