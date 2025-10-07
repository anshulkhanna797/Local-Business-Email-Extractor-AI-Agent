# Local-Business-Email-Extractor-AI-Agent
This project is an AI-powered automation workflow built using n8n that extracts emails of local business leads from Google Maps. The AI agent allows the user to input a search query, for example, “interior designer Bangalore”, and automatically performs the following tasks:

Searches Google Maps listings for the given query.

Extracts the business websites from the search results.

Crawls each website to find publicly available email addresses.

Compiles all emails into a structured list for further use.

This workflow is designed to save time for sales, marketing, or outreach teams by automating the process of finding contact information for local businesses. It leverages AI to intelligently process data and improve accuracy while maintaining automation entirely within the n8n platform.

Features:

Fully automated email lead generation from local businesses

Supports custom search queries

Structured output of websites and email addresses

Easy-to-use AI agent workflow in n8n

Tech Stack / Tools Used:

n8n (Workflow Automation Platform)

Google Maps (for local business search)

AI agent (for intelligent data extraction and email scraping)

Step-by-Step Guide to Build the AI Agent in n8n

Here’s a detailed guide to set up the workflow you described:

Step 1: Trigger Node

Node: Webhook or Manual Trigger

Purpose: Start the workflow when the user inputs a query.

Input: User enters the search query, e.g., “interior designer Bangalore”.

Settings:

Name the node: User Search Input

Set HTTP Method to POST if using Webhook (optional).

Expect input field: search_query

Step 2: Google Maps Search

Node: HTTP Request

Purpose: Get Google Maps listings based on the query.

Settings:

Method: GET

URL: Use Google Maps API (or Places API) with query parameter search_query.

Headers: Include API key if using Google API.

Output: JSON containing business listings (name, website, address, etc.)

Step 3: Extract Websites

Node: Set / Function

Purpose: Extract the website field from the JSON response.

Function Example:

return items.map(item => {
  return { json: { website: item.json.website } };
});

Step 4: Fetch Emails from Websites

Node: HTTP Request

Purpose: Visit each website URL to get HTML content.

Settings:

Method: GET

URL: Use the website from previous step ({{$json["website"]}})

Response Format: Text / HTML

Step 5: Extract Email Addresses

Node: Function

Purpose: Parse the HTML and extract emails using regex.

Example Code:

const html = items[0].json.htmlContent; // HTML from previous node
const emails = html.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-z]{2,}/g) || [];
return emails.map(email => ({ json: { email } }));

Step 6: Store / Output Results

Node: Google Sheets / Airtable / CSV File

Purpose: Save the extracted emails in a structured format.

Settings:

Spreadsheet / Table name

Map the email and website fields to columns

Optional Step 7: AI Agent Enhancement

Node: OpenAI (or AI agent node in n8n)

Purpose: Clean or validate extracted emails.

Input: Raw email list

Prompt Example:

Validate and filter the list of emails. Remove duplicates or invalid entries.


✅ Workflow Summary:
Webhook → HTTP Request (Google Maps) → Function (Extract Websites) → HTTP Request (Get HTML) → Function (Extract Emails) → Google Sheets / Airtable

This setup ensures the AI agent fully automates email lead generation from Google Maps with minimal manual intervention.
