# API and Data Sync Troubleshooting Guide

This guide outlines how I troubleshoot issues when data is not syncing correctly between two systems. These steps apply to CRM-to-analytics syncs, ETL pipelines, and any integration that relies on API calls or scheduled jobs.

The goal is to confirm the issue, isolate the cause, and determine whether the problem is in the source system, the API layer, the sync job, or the destination system.

---

## 1. Confirm the Issue

Before assuming something is broken, I start by checking:

- What data is missing  
- Whether the issue affects new records, updated records, or both  
- Whether other users are seeing the same problem  
- When the issue first started  
- Whether the affected records share any patterns  

This helps rule out one-off edge cases.

---

## 2. Check Recent Changes

Most sync issues tie back to something that changed recently. I look at:

- Admin changes in the CRM or source system  
- Automation or workflow updates  
- Field permission changes  
- API rate limits or credential updates  
- Release notes or deployments from engineering  

A lot of problems trace back to configuration updates that weren’t tested fully.

---

## 3. Review Sync Job History

Next I check whether the sync job itself is running as expected:

- Did the job run on schedule?  
- Did the job report errors?  
- How many records were processed?  
- Are counts increasing over time or stuck at zero?  

A “successful” job that processes zero records is often the biggest clue.

---

## 4. Inspect Data Timestamps

Most syncs only pull records that changed since the last run.  
I look at:

- CreatedDate  
- LastModifiedDate  
- Custom timestamp fields  
- Whether timestamps match what the user expects  

If timestamps don’t update correctly, the sync won’t recognize the record as “new” or “changed.”

---

## 5. Test the API Directly

I use test queries to confirm what the API is returning:

- Does the API return the missing records?  
- Are the fields populated correctly?  
- Are filters (such as “modified since X”) excluding the records?  
- Are errors hidden behind 200 OK responses?  

Sometimes the UI shows everything, but the API can’t see it because of permissions, field-level changes, or broken workflows.

---

## 6. Reproduce the Issue

To make sure the issue isn’t isolated, I:

- Create a new record  
- Update an existing record  
- Save and verify timestamps  
- Trigger the sync manually if possible  

If both new and updated records fail to sync, the problem is systemic.

---

## 7. Check the Destination System

Once data reaches the destination, I verify:

- Whether it was ingested  
- Whether reports rely on cached data  
- Whether filters hide recently added records  
- Whether data types match expected formats  

Sometimes the sync works fine, but the reporting tool is outdated or filtering incorrectly.

---

## 8. Identify the Root Cause

Common causes include:

- Automated workflows overwriting system fields  
- Incorrect timestamps  
- Missing permissions in the API  
- Sync pipeline filtering out records unintentionally  
- Schema changes that weren’t documented  
- Jobs stuck in “successful but idle” loops  

The goal is to narrow down where the data flow is breaking.

---

## 9. Resolve and Validate

Once the cause is found, I:

- Apply the fix (rollback, permissions update, schema correction, etc.)  
- Trigger a full or partial resync  
- Confirm new and updated records sync correctly  
- Walk through the entire flow again  
- Confirm the user sees the expected outcome  

Validation ensures the issue won’t return.

---

## 10. Document the Outcome

Finally, I document:

- What happened  
- What caused it  
- How it was fixed  
- Any follow-up steps  
- What monitoring or alerts should be added  

This helps prevent the same issue in the future and makes it easier for others to troubleshoot similar problems.

---

This guide reflects the workflow I follow when investigating sync or integration issues. It focuses on structured, repeatable steps that help isolate where the data flow is breaking so the issue can be resolved efficiently.
