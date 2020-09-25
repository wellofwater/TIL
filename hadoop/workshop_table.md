### CREATE TABLE #1

```sql
CREATE TABLE subway_people(
Day STRING,                  
SubwayLine STRING,         
SubwayStation STRING,      
GetOn INT,                 
GetOff INT,                
RegDate STRING)              
COMMENT 'TEST DATA'        
PARTITIONED BY (subwayDay STRING)  
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```



### DATA LOAD #1

```sql
LOAD DATA LOCAL INPATH '/root/seoul-subway-people.csv' OVERWRITE INTO TABLE subway_people PARTITION (subwayDay='20200814-20200921');
```





### CREATE TABLE #2

```bash
CREATE TABLE bus_time(
Day STRING,                  
BusNumber STRING,         
BusName STRING,      
StationID STRING,                 
ARSNumber INT,
BusStation STRING,
00On INT,
00Off INT,
01On INT,
01Off INT,
02On INT,
02Off INT,
03On INT,
03Off INT,
04On INT,
04Off INT,
05On INT,
05Off INT,
06On INT,
06Off INT,
07On INT,
07Off INT,
08On INT,
08Off INT,
09On INT,
09Off INT,
10On INT,
10Off INT,
11On INT,
11Off INT,
12On INT,
12Off INT,
13On INT,
13Off INT,
14On INT,
14Off INT,
15On INT,
15Off INT,
16On INT,
16Off INT,
17On INT,
17Off INT,
18On INT,
18Off INT,
19On INT,
19Off INT,
20On INT,
20Off INT,
21On INT,
21Off INT,
22On INT,
22Off INT,
23On INT,
23Off INT,
RegDate STRING)              
COMMENT 'TEST DATA'        
PARTITIONED BY (BusDay STRING)  
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```



### DATA LOAD #2

```sql
LOAD DATA LOCAL INPATH '/root/seoul-bus-time.csv' OVERWRITE INTO TABLE bus_time PARTITION (BusDay='202008');
```





### CREATE TABLE #3

```bash
CREATE TABLE covid19_patient(
num INT,                  
ConfDate STRING,         
ID STRING,      
Nation STRING,                 
Info STRING,
Area STRING,
Travel STRING,
Contact STRING,
Actions STRING,
State STRING,
Move STRING,
RegDate STRING,
Update STRING,
Exposure STRING)              
COMMENT 'TEST DATA'        
PARTITIONED BY (InputDate STRING)  
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
STORED AS TEXTFILE;
```



### DATA LOAD #3

```sql
LOAD DATA LOCAL INPATH '/root/covid19.csv' OVERWRITE INTO TABLE covid19_patient PARTITION (InputDate='20200925');
```

