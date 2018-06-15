# NCDC-weather-dataset-Hadoop-MapReduce-Pig-Hive

The National Climatic Data Center (NCDC) is the world's largest active archive of weather data. I downloaded the NCDC data for year 1930 and loaded it in HDFS system. I implemented MapReduce program and Pig, Hove scripts to findd the Min, Max, avg temparature for diffrent stations.

Compiled the Java File: javac -classpath /home/student3/hadoop-common-2.6.1.jar:/home/student3/hadoop-mapreduce-client-core-2.6.1.jar:/home/student3/commons-cli-2.0.jar -d .  MaxTemperature.java MaxTemperatureMapper.java  MaxTemperatureReducer.java

Created the JAR file: jar -cvf hadoop-project.jar *class

Executed the jar file: hadoop jar hadoop-project.jar MaxTemperature  /home/student3/Project/ /home/student3/Project_output111

Copy the output file to local
hdfs dfs -copyToLocal /home/student3/Project_output111/part-r-00000

PIG Script

Pig -x local
grunt> records = LOAD '/home/student3/Project/Project_Output/output111.txt'
AS (year:chararray, temperature:int);
grunt> DUMP records;
grunt> grouped_records = GROUP records BY year;
grunt> DUMP grouped_records;
grunt> max_temp = FOREACH grouped_records GENERATE group,
>> MAX(filtered_records.temperature);
grunt> DUMP max_temp;
grunt> min_temp = FOREACH grouped_records GENERATE group,
>>  MIN(records.temperature);
grunt> DUMP min_temp;

Hive Script

Commands to create table in hive and to find average temperature

DROP TABLE IF EXISTS w_hd9467;

CREATE TABLE w_hd9467(year STRING, temperature INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ‘\t’; 

LOAD DATA LOCAL INPATH '/home/student3/Project/Project_Output/output1.txt'    
OVERWRITE INTO TABLE w_hd9467;

SELECT count(*) from w_hd9467;

SELECT * from w_hd9467 limit 5;

Query to find average temperature
SELECT year, AVG(temperature) FROM w_hd9467 GROUP BY year;
