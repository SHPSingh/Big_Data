create or replace temporary table demo_customer_info (
user_id string,
first_name string,
last_name string
);
Select * from demo_customer_info limit 10;
COPY INTO demo_customer_info
FROM @demo_aws_stage/people-10000.csv
FILE_FORMAT=(format_name=demo_format)
ON_ERROR='CONTINUE'; -- or ON_ERROR='SKIP_FILE' if you want to skip the entire file
CREATE OR REPLACE FILE FORMAT demo_format
TYPE = 'CSV'
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
SKIP_HEADER = 1
ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE; -- Add this line to ignore the 
column count mismatc-Scenario :1
COPY INTO demo_customer_info
from @demo_aws_stage/
file_format=(format_name=demo_format)
on_error='Skip_file'; --Skip the whole file
COPY INTO demo_customer_info
from @demo_aws_stage/
file_format=(format_name=demo_format)
on_error='Continue'; --Skip only the bad record and load the rest of the record
COPY INTO demo_customer_info
FROM @demo_aws_stage/
FILE_FORMAT=(format_name=demo_format)
ON_ERROR='ABORT_STATEMENT'; -- Change 'abort' to 'ABORT_STATEMENT'
--Scenario :2
COPY INTO demo_customer_info
from @demo_aws_stage/people-10000.csv
file_format=(format_name=demo_format)
 force=true;
List @demo_aws_stage/people-10000.csv;
SELECT count(*) from demo_customer_info;
--Scenario :3
COPY INTO demo_customer_info
from @demo_aws_stage/people-10000.csv
file_format=(format_name=demo_format)
 force=true purge=true;
Select * from demo_customer_info limit 10;
COPY INTO demo_customer_info
FROM @demo_aws_stage/people-10000.csv
FILE_FORMAT=(format_name=demo_format)
ON_ERROR='CONTINUE'; -- or ON_ERROR='SKIP_FILE' if you want to skip the entire file
CREATE OR REPLACE FILE FORMAT demo_format
TYPE = 'CSV'
FIELD_OPTIONALLY_ENCLOSED_BY = '"'
SKIP_HEADER = 1
ERROR_ON_COLUMN_COUNT_MISMATCH = FALSE; -- Add this line to ignore the 
column count mismatc
