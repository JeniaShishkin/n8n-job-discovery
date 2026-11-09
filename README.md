# LinkedIn Job Discovery & Relevance Automation (n8n)

This workflow automates the process of finding LinkedIn jobs that match a candidate’s background, tailoring their resume for the best opportunities, and tracking the results in Google Sheets.

## Overview

The workflow is built in n8n and is designed to:

1. Start from a manually triggered execution.
2. Build a LinkedIn job search URL for a target role and location.
3. Pull the candidate’s profile materials from Google Docs, including:
   - projects and skills
   - current resume
4. Use the Apify LinkedIn Jobs Scraper to fetch job listings from LinkedIn.
5. Exclude senior-level titles such as senior, staff, lead, principal, manager, and architect roles.
6. Use OpenAI to evaluate whether each job is relevant to the candidate.
7. Filter to only the job postings that appear to be a strong fit.
8. Create a customized resume version for each matching job.
9. Copy a Google Docs resume template and replace placeholders with AI-generated content.
10. Save a record of the matched role in Google Sheets with links to the job posting, resume, and application URL.

## Workflow steps

### 1. Manual trigger
The workflow begins when the user clicks “Execute workflow” in n8n.

### 2. Job search setup
A Set node defines the LinkedIn search URL used for job discovery, targeted at software engineering roles and excluding senior-level titles.

### 3. Load candidate profile data
The workflow retrieves:
- project and skill information from a Google Doc
- the base resume from another Google Doc

This provides the context used by the AI to assess relevance and tailor the CV.

### 4. Scrape LinkedIn jobs
An Apify actor is used to run a LinkedIn job search and return relevant job data such as:
- title
- company name
- job description
- apply URL
- posting date
- link

### 5. Filter out senior roles
The workflow removes jobs whose titles match senior-level patterns, such as:
- senior
- staff
- lead
- principal
- manager
- architect

### 6. Check job relevance
OpenAI reviews each job description against the candidate’s background, skills, and resume. It returns a verdict indicating whether the role is a likely fit.

### 7. Keep only relevant jobs
A filter node keeps only jobs where the AI verdict is true.

### 8. Tailor the resume
For each relevant opportunity, OpenAI rewrites the resume content to better match the job description while preserving the original structure and template.

### 9. Copy and update the resume template
The workflow:
- loads the resume template from Google Docs
- duplicates it in Google Drive
- replaces placeholder tokens with the AI-generated resume fields
- sends a Google Docs batch update request to populate the new resume document

### 10. Register matched jobs
The final matched job data is appended to a Google Sheet tracking:
- job post link
- job title
- posted date
- company name
- description
- resume URL
- application URL

## Integrations used

- n8n
- Apify LinkedIn scraper actor
- OpenAI
- Google Docs
- Google Drive
- Google Sheets

## Typical use case

This automation is useful for a software candidate who wants to:
- discover suitable job openings
- quickly assess whether they are a good fit
- generate a customized resume for each matching role
- keep a structured tracker of opportunities and resume versions

## Prerequisites

Before using the workflow, make sure the following are configured in n8n:

- Apify API credentials
- OpenAI credentials
- Google Docs OAuth credentials
- Google Drive OAuth credentials
- Google Sheets OAuth credentials
- Access to the Google Docs and Google Sheet files referenced in the workflow

## Notes

This workflow is intended for targeted job discovery and applicant tracking. It depends on valid credentials, access to LinkedIn data via Apify, and the correct Google Drive/Docs files for the resume template and project profile data.
