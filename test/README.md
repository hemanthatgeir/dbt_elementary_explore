Refer to docs for how to configure anomalies and monitor them from docs.elementary-data.com

Intention of this branch is to explore row_count

Expectation is that elementary will detect row counts in each 24 hr interval based on the provided timestamp column 'ts' and if the count deviates more than threshhold value it fails the monitors as an anomaly

NOTE: If use has the setup dbt project artifacts for other branches/features. user may want to point to a fresh schema in profiles.yml or clear the current schema so that exploring metadata created by dbt, elementary will be easy

cooking up required data:
1. CREATE TABLE sch3.dt1 (id int, NAME varchar, TS timestamp)
2. INSERT INTO sch3.DT1 VALUES (1, 'hemanth', '2022-04-01 00:00:00.000')
3. It is better to create records having ts values starting from 15 days before current date since elementary uses current date and uses historical counts of 14 days by default to calculate deviation score which is used to identify anomaly. So adjust the date values accordingly
4. To get more records rerun the following command as many times as required for example till 1024 records are produced.
	1. insert into sch3.dt1 select id, name, '2022-04-01 01:01:01'  from sch3.dt1 where ts = '2022-04-01 01:01:01'
5. To create more records for other days modify above command and run once the following command by modifying the date in select but not in where
	1. insert into sch3.dt1 select id, name, '2022-04-02 01:01:01'  from sch3.dt1 where ts = '2022-04-01 01:01:01'
6. skip inserting for say the 15th day so that it will be recognized as anomaly and create data for all days till current day

steps for demo:
1. create data table as explained above using which this project creates elementary model 'dt1_v'
2. make sure schema.yml is correctly configured with elementary test 'row_count' on this model
3. execute dbt run and dbt test commands, elementary should identify anomaly for skipped day
