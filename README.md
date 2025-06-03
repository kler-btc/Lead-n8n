# AI Lead Qualification & CRM Entry Workflow for n8n

This repository contains an n8n workflow that automates lead qualification using OpenAI's GPT-4 model and stores the results in Airtable. Incoming leads are evaluated based on their industry relevance and job title, then automatically recorded in your Airtable base.

---

## Overview

The workflow performs the following steps:

1. **Webhook Node**: Receives incoming lead data via HTTP POST requests.
2. **Set Prompt Node**: Constructs a prompt string for OpenAI based on the lead data.
3. **OpenAI Node (Completion)**: Sends the prompt to OpenAI GPT-4 and receives a qualification response.
4. **Parse Response Node**: Extracts qualification status ("Qualified" or "Not Qualified") and the reason from the OpenAI response.
5. **Airtable Node**: Creates a new record in Airtable with the lead information and qualification result.

---

## Prerequisites

- [n8n](https://n8n.io/) instance setup
- OpenAI API key with access to GPT-4 or GPT-3.5 models
- Airtable account with:
  - Base ID
  - Table ID
  - API key with write access

---

## Setup Instructions

1. **Import Workflow**

   - Import the `AI Lead Qualification & CRM Entry` workflow JSON into your n8n instance.

2. **Configure Credentials**

   - Set up OpenAI credentials in n8n with your API key.
   - Set up Airtable credentials in n8n with your API key.

3. **Configure Webhook**

   - The workflow listens for POST requests on the webhook path: `/lead-qualification`
   - Send JSON lead data with the following fields to the webhook URL:

     ```json
     {
       "name": "Jane Doe",
       "email": "jane.doe@example.com",
       "company": "Example Corp",
       "job_title": "Marketing Manager",
       "industry": "Advertising"
     }
     ```

4. **OpenAI Node Settings**

   - Resource: `Completion`
   - Operation: `Create`
   - Model: `gpt-4` (or fallback `text-davinci-003`)
   - Prompt: `={{$json["prompt"]}}`
   - Temperature: `0.7`
   - Max Tokens: `100`

5. **Airtable Node Settings**

   - Operation: `Create`
   - Base ID: your Airtable base ID
   - Table ID: your Airtable table ID
   - Map fields accordingly (Name, Email, Company, Job Title, Industry, Qualification, Reason)

---

## Example Payload

```json
{
  "name": "Sarah Connor",
  "email": "sarah@skynet.ai",
  "company": "Skynet",
  "job_title": "AI Strategist",
  "industry": "Artificial Intelligence"
}
