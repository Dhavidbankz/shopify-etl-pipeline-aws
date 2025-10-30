# shopify-etl-pipeline-aws
ETL pipeline that extracts Shopify product data, loads it into S3, catalogs with AWS Glue, queries via Athena and visualize with quicksight
Shopify ETL Pipeline on AWS
Overview

This project implements a full end-to-end ETL (Extract, Transform, Load) pipeline for Shopify product data using AWS services. The pipeline extracts product data from a Shopify store, transforms it into a queryable format, stores it in Amazon S3, catalogs it using AWS Glue, and enables fast analysis with Amazon Athena.

The project demonstrates cloud-based data engineering, automation, and handling of structured and semi-structured JSON data.

Architecture
Shopify API --> AWS Lambda --> S3 (partitioned by date) --> AWS Glue Catalog --> Amazon Athena--> Quicksight


Workflow:

Lambda function fetches product data from Shopify.

Data is stored in Amazon S3 as NDJSON (newline-delimited JSON) files, partitioned by date.

AWS Glue Crawler updates the data catalog automatically.

Athena is used to query and analyze product data.

Technologies Used

AWS Lambda – Serverless compute for data extraction.

Amazon S3 – Scalable storage for raw product data.

AWS Glue – Catalogs data and creates tables for querying.

Amazon Athena – Serverless query engine for analyzing the data.

Python – Language for Lambda scripts and data processing.

JSON / NDJSON – Data format for structured product information.

Features

Incremental product data fetching from Shopify API.

Partitioned storage in S3 for efficient querying.

Automated Glue catalog updates for new data.

Query product information with Athena using SQL.

Supports multiple product variants and options.

Sample JSON Data
{
  "id": 7049855008866,
  "title": "Bath Duck V",
  "vendor": "training-store-syncspider.myshopify.com",
  "product_type": "duck",
  "created_at": "2024-04-09T14:54:54-04:00",
  "variants": [
    {"id": 40944952639586, "title": "Pink", "price": "27.00", "inventory_quantity": 10},
    {"id": 40944952737890, "title": "Yellow", "price": "27.00", "inventory_quantity": 10}
  ],
  "date": "2025-10-29"
}

Example Athena Query
-- Fetch all products with inventory > 0
SELECT
    id,
    title,
    vendor,
    product_type,
    variants
FROM shopifyetlproduct.products
WHERE date = '2025-10-29'
AND JSON_EXTRACT(variants, '$[*].inventory_quantity') > 0;

Setup / How to Run

Note: This assumes you have AWS credentials with permissions for Lambda, S3, Glue, and Athena.

Deploy the Lambda function with the Shopify API credentials.

Configure your S3 bucket and folder structure:

s3://your-bucket-name/shopify/products/date=YYYY-MM-DD/


Run AWS Glue Crawler to detect new files and update the catalog.

Query your data using Amazon Athena.

Author

David Okenwa

LinkedIn: https://www.linkedin.com/in/david-o-067600218/

GitHub: https://github.com/Dhavidbankz

Optional Enhancements

Add automated scheduling with CloudWatch Events to run Lambda daily.

Integrate QuickSight or Tableau for visual analytics.

Handle Shopify API rate limits and incremental updates efficiently.
