# AgenticFinancialWorkflow
A small financial analysis Python workflow.

Team & Roles:

Prompt Engineer: Justin Xie

Designs and refines prompts for PLAN, categorization, KPIs, summary, and reflection.

Data Engineer: Jason Zheng

Manages the CSV file, checks for data quality issues, and validates JSON outputs.

Financial Analyst: Justin & Jason

Interprets the outputs (plan, categories, KPIs) and connects them to FinTech/SMB use cases.

Dataset Description:
number of rows: 26
For columns, there are the "date", "merchant", "amount", and "description" columns.
This is a CSV of the example person's personal finance.

PLAN prompt:

You are an intelligent financial analysis agent.

You will be given a CSV dataset representing this month's personal financial activity. 
Each row contains: date, merchant, amount, and description.

Your task is to create a structured 5-step analysis plan using the agentic workflow:
Plan → Act → Observe → Summarize → Reflect.

Each step should clearly describe how the agent will process, review, and reason about 
financial transactions, emphasizing spending patterns, income detection, and meaningful insights.

Return ONLY valid JSON in the following format:

{
  "plan_steps": [
    "Step 1 ...",
    "Step 2 ...",
    "Step 3 ...",
    "Step 4 ...",
    "Step 5 ..."
  ]
}

# This is done on a console on AWS Bedrock playground mode, specifically Chat/Text Playground with Llama 3.1 8B Instruct model. The output is saved into outputs/plan.json.  

Categorization Prompt (Draft):

You are a financial transaction categorization agent.

You will receive a list of transactions, each containing:
- date
- merchant
- amount
- description

Your task is to assign each transaction to exactly ONE of the following categories:
• Shopping
• Dining
• Utilities
• Income
• Other

Base the category on the merchant name and description.  
Be consistent and avoid guessing beyond reasonable interpretation.

Return your output ONLY as valid JSON in this structure:

{
  "categorized": [
    {
      "date": "",
      "merchant": "",
      "amount": 0,
      "category": ""
    }
  ]
}

After using this categorization prompt, the categorized JSON was saved into 'outputs/categorized.json'.
This file was later corrected because the first model output was incomplete.

Step 1 - Plan
Generated a 5-step agentic workflow describing how the system would Act, Observe, Summarize, and Reflect.  

Step 2 - Act
Ran the categorization prompt on the transaction list.  
The model originally cut off the output, so categorization was regenerated and validated manually.

Step 3 - Observe
Computed KPIs:  
- total_spend  
- total_income  
- top_merchants  
- average_expense  
Saved into `outputs/kpis.json`.  
Incorrect KPIs from the partial dataset were corrected using the final categorized file.

Step 4 - Summarize
Created a professional ≤100-word financial summary using KPIs + categories.  
Saved as `outputs/summary.txt`.

Step 5 - Reflect
Identified categorization issues, KPI errors, and weaknesses in the workflow.  
Reflection saved in `outputs/reflection.txt`.

Reflection Summary
- The LLM initially produced a **truncated categorized.json**, which caused wrong KPI values.  
- Total spend, top merchants, and average expense were incorrect until the full dataset was regenerated.  
- Some categorizations were ambiguous (e.g., Uber as Other, Spotify as Utilities).  
- Improvements include enforcing strict transaction count matching, adding stronger schema rules, and preventing early cutoffs.  
- Manual verification helped confirm accurate KPIs, demonstrating the importance of validation in agentic loops.

JSON Troubleshooting Notes
- In the first run, Bedrock stopped generating mid-JSON, producing incomplete output.  
- This caused missing transactions and incorrect KPI values.  
- A stronger prompt was used requiring the model to include **ALL transactions**.  
- Regeneration produced a complete and valid categorized.json.  
- After corrections, KPIs were recalculated and saved properly.

This project demonstrates the full Agentic AI loop:

- Plan: Define the entire analysis strategy.  
- Act: Categorize financial transactions.  
- Observe:** Compute KPIs to measure financial behavior.  
- **Summarize:** Produce a human-readable summary of financial health.  
- **Reflect:** Identify errors and propose improvements.

The workflow shows how LLM reasoning can support financial analysis, but also highlights the importance of validation, schema enforcement, and iterative improvement.


