```bash
cp -r apa(tab) /usr/local
ln -s /usr/local/apa(tab)/bin/startup.sh starttomcat
ln -s /usr/local/apa(tab)/bin/shutdown.sh stoptomcat
cd /usr/local/apa(tab)/conf
vi server.xml

69 port 8080-> 80
```

```bash
# ftp설치
```



```sql
CREATE TABLE shopclick(
	date STRING,
    fn STRING,
    id STRING,
    item STRING,
    price INT,
    age INT,
    gender STRING
)
PARTITIONED BY (logdate STRING)
ROW FORMAT DELIMITED
	FIELDS TERMINATED BY ','
	LINES TERMINATED BY '\n'
	STORED AS TEXTFILE;
```



```bash
vi hive.sh

date=`date`
echo $date
partitionName="${date:0:4}-${date:6:2}-${date:10:2}"
echo $partitionName
fileName="data.log.$partitionName"
echo $fileName

echo "Load the Data ?"
read yn


if [ $yn == "y" ]
then
echo "Start Load the Data ..."

if [ -f /root/log/$fileName ]
then
hive << EOF
LOAD DATA LOCAL INPATH '/root/log/$fileName' OVERWRITE INTO LOGINFO PARTITION (cDate="$partitionName")
EOF
echo "OK"
echo "OK"
else
echo "File Not Found"
echo "Exit Now..."
fi

else
echo "Exit Now..."

fi
exit 0
```



```java
String urlstr = 
"http://70.12.113.220/test/car.jsp?";
urlstr += "id=id33&lat=127.89&lng=37.2";

URL url = new URL(urlstr);
HttpURLConnection con = 
(HttpURLConnection)url.openConnection();
con.setReadTimeout(5000);
con.setRequestMethod("POST");
con.disconnect();
```

