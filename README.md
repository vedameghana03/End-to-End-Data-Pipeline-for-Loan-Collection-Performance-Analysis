# End-to-End-Data-Pipeline-for-Loan-Collection-Performance-Analysis
End-to-End Data Pipeline for Loan Collection Performance Analysis
Objective
The goal of this  is to simulate a real-world data pipeline for a daily call campaign in a loan collection process. The pipeline involves reading daily CSV files from multiple sources, performing data validation, transforming and enriching the data, and finally generating a performance summary report for each agent.
Step 1: Data Ingestion and Validation
• Loaded three CSV files using pandas:
    - call_logs.csv
    - agent_roster.csv
    - disposition_summary.csv
• Cleaned column names by stripping whitespace.
• Checked for data types and converted 'call_date' to datetime format (day-first).
• Verified that critical columns like 'agent_id', 'org_id', and 'call_date' are not missing.
• Confirmed there were no null and duplicate values in the datasets.
Step 2: Join Logic
• Ensured consistent data types across all dataframes for the joining keys: 'agent_id', 'org_id', and 'call_date'.
• Merged the datasets using LEFT JOINs:
    - call_logs + agent_roster: joined on ['agent_id', 'org_id'].
    - result + disposition_summary: joined on ['agent_id', 'org_id', 'call_date'].
• Added a 'presence' column to indicate if the agent logged in on that date (1 if login_time exists, else 0).
Step 3: Feature Engineering
• Grouped the merged dataset by the following columns to calculate per-agent per-day performance:
    ['agent_id', 'org_id', 'call_date', 'users_first_name', 'users_last_name', 'presence']
• Calculated key metrics:
    - Total Calls Made: Count of 'call_id' per group.
    - Unique Loans Contacted: Count of unique 'installment_id' per group.
    - Completed Calls: Count where 'status' = 'completed'.
    - Connect Rate: (Completed Calls / Total Calls) * 100, rounded to 2 decimal places.
    - Average Call Duration: Mean of 'duration' in minutes.
Step 4: Output
• Combined all computed metrics into a final summary dataframe.
• Exported the summary to 'agent_performance_summary.csv'.
• Also created a Slack-style summary message with the following format:

    Agent Summary for 2025-04-28
    Top performer: AgentFirst3 AgentLast3 (38.1% connect rate)
    Total Active Agents: 17
    Average Duration: 7.53 min
Tools & Libraries Used
• Python 
• pandas – for data loading, transformation, and aggregation
• numpy – for numerical operations
• datetime – for date handling and formatting
