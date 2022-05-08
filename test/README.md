Refer to docs for how to configure anomalies and monitor them from docs.elementary-data.com

Intention of this branch is to explore schema_changes

Expectation is that elementary will detect column addition or deletion related to a dbt-elementary model as an anomaly

steps for demo:
1. assume a data table exists using which this project creates elementary model 'dt1_v'
2. make sure schema.yml is correctly configured with elementary test 'schema_changes' on this model
3. execute dbt run and dbt test commands
4. add a column to dt1
5. adjust dt1_v.sql according to this col. need to confirm this step
6. run the dbt run and dbt test commands. elementary should identify added col as anomaly
