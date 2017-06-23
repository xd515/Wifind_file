This folder is about how to pull data from database, and shows the process of visualize the data collected through mobile.

**Log in**

```bash
ssh netid@gw.cusp.nyu.edu
ssh compute
ssh wifind@wifind.cusp.nyu.edu
mysql -uroot -p
```

```mysql
show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| capstonemysql      |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

```mysql
use capstonemysql;
show tables;
+----------------------------+
| Tables_in_capstonemysql    |
+----------------------------+
| auth_group                 |
| auth_group_permissions     |
| auth_permission            |
| auth_user                  |
| auth_user_groups           |
| auth_user_user_permissions |
| django_content_type        |
| django_migrations          |
| django_site                |
| gpsq                       |
| ingestion_scan             |
| test_unique_locations      |
| wifi_scan                  |
| wifi_scan_orig             |
| wifi_scan_temp             |
+----------------------------+
```

```mysql
describe wifi_scan;
+---------------+---------------+------+-----+---------+----------------+
| Field         | Type          | Null | Key | Default | Extra          |
+---------------+---------------+------+-----+---------+----------------+
| idx           | bigint(20)    | NO   | PRI | NULL    | auto_increment |
| lat           | double        | NO   |     | NULL    |                |
| lng           | double        | NO   |     | NULL    |                |
| acc           | float         | NO   |     | NULL    |                |
| altitude      | double        | NO   |     | NULL    |                |
| time          | decimal(15,0) | NO   |     | NULL    |                |
| device_mac    | varchar(20)   | NO   |     | NULL    |                |
| app_version   | varchar(10)   | NO   |     | NULL    |                |
| droid_version | varchar(10)   | NO   |     | NULL    |                |
| device_model  | varchar(50)   | NO   |     | NULL    |                |
| ssid          | varchar(100)  | YES  | MUL | NULL    |                |
| bssid         | varchar(20)   | YES  |     | NULL    |                |
| caps          | varchar(100)  | YES  |     | NULL    |                |
| level         | float         | YES  |     | NULL    |                |
| freq          | float         | YES  |     | NULL    |                |
+---------------+---------------+------+-----+---------+----------------+
```

```mysql
exit;
# Pull the data from the database.

mysql -uroot -p capstonemysql -e "select * from wifi_scan limit 30000" -B | sed "s/'/\'/;s/\t/\",\"/g;s/^/\"/;s/$/\"/;s/\n//g" > dxm10000.csv

scp wifind@wifind.cusp.nyu.edu:t10000.csv ./
```

```wifind
mv dxm10000.csv Wifind_file/
cd Wifind_file
ls
sudo git add dxm10000.csv; sudo git commit -m "dxm10000.csv"; sudo git push
```


