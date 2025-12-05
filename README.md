# Documentation

This project provides a reusable ABBYY Vantage Connector that brings Vantage document intelligence into Camunda workflows.  
Vantage handles document understanding.Camunda handles orchestration.  

The connector brings both together into one seamless automated journey.

The design is modular. Each ABBYY Vantage operation is implemented as a separate subprocess so you can configure once and reuse everywhere.

## Supported Operations

The connector supports the seven official ABBYY Vantage operations:

1. create_transaction  
2. add_files_to_transaction  
3. start_transaction  
4. start_processing_of_the_files_in_the_transaction  
5. get_transaction_information  
6. download_the_source_file_of_the_transaction  
7. download_the_result_files  

## Artifacts Included

- Element-templates for easy configuration  
- Support processes implementing each Vantage API call  
- Demo process showing the end-to-end orchestration  

## Required Connector Secrets

This connector requires the following secrets to be present:

| Secret name | Description | Example |
|-------------|-------------|---------|
| ABBYY_VANTAGE_CLIENT_ID | Client ID of your ABBYY Vantage tenant | |
| ABBYY_VANTAGE_CLIENT_SECRET | Client Secret of your ABBYY Vantage tenant | |
| ABBYY_VANTAGE_BASE_URL | Base URL of your Vantage environment | https://vantage-us.abbyy.com |

## Installation

1. Set all required connector secrets  
2. Deploy all support processes (`/ABBYY connector/support processes`)  
3. Publish all element templates (`ABBYY connector/element-templates`)

## How to Use the Artifacts

The demo process shows a complete document-processing flow built using the reusable ABBYY Vantage subprocesses.  
Below is a simple explanation of how the workflow runs from start to finish:

1. **User uploads documents in a form**  
   The process begins when files are collected through the start form.

2. **Create and launch a Vantage transaction**  
   The subprocess `ABBYY_Vantage_Launch_Transaction_Process` is called.  
   It creates a new transaction in ABBYY Vantage and uploads all provided documents.  
   It returns the `transactionId` to Camunda.

3. **Poll Vantage for processing status**  
   The subprocess `ABBYY_Vantage_Check_Transaction_Status` is used in a loop.  
   It repeatedly checks whether Vantage has finished processing the documents.  
   The loop continues until the final status is reached or the maximum number of polls is exceeded.

4. **Download processed result files**  
   When Vantage finishes processing, the subprocess `ABBYY_Vantage_Download_Results` is called.  
   It downloads all output files (JSON, PDF, Excel, etc.) and makes them available as workflow variables.

5. **Continue your business workflow**  
   The results can be emailed, stored in a DMS, pushed to another system, or used in any follow-up automation step.

These subprocesses abstract away all API complexity so the workflow designer only needs to connect them together using Call Activities.  
This makes the solution easy to reuse in any document-based workflow.