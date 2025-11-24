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




