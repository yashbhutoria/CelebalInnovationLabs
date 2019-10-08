# BASIC COMMANDS

## We are working in two fs

### _1. ext4_

### _2. hdfs_

### _3. ext3 we logged in with user hadoop, default path is / home/hadoop_

### _4. hdfs default path is /user/hadoop_

```
cat>samyak
bigdata
java
ruby
python
^c

ls
```

- ### To view the default path in hadoop

```
hadoop fs -ls /
```

- ### create a dir in default path

```
hadoop fs -mkdir /bigdata
```

- ### To copy a file into default dir of hdfs

```
hadoop fs -put samyak /bigdata ---- linux to hdfs

hadoop fs -ls /bigdata ------ listing hadoop files
```

- ### to read the file---

```
hadoop fs -cat /bigdata/samyak
.

cat>for5.txt
sudhir
shathak
```

- ### to send the data from ext3 - hdfs

```
hadoop fs -mkdir /names
hadoop fs -put for5.txt /names/
```

- ### Check the default permission in hdfs

```
touch for5.txt
chmod 777 for5.txt
hadoop fs -put for5.txt /

hadoop fs -ls /for5.txt
```

## Output which is 644

```
-rw-r--r-- 2 hadoop supergroup 0 2015-08-01 17:58 /a1.txt
hadoop fs -chmod 777 /for5.txt
```

- ### To create a zero byte file in hdfs

```
hadoop fs -touchz /t1.txt
```

- ### To display the contents of a file

```
hadoop fs -cat /for5.txt
```

- ### Read a zip file in hdfs

```
cat>zip.sh
write command to unzip
command executed
ctrl+c

gzip zip.sh
hadoop fs -put zip.sh.gz /

hadoop fs -cat /zip.sh.gz -----error
hadoop fs -text /zip.sh.gz
```

- ### To copy a file within hdfs

```
cat>t1.java --------- ext4
hadoop fs -put t1.java / ---------- hdfs

hadoop fs -cp /t2.txt /cpy
```

### Both source and target are in hdfs

- ### Cut within hdfs with a new name

```
hadoop fs -mv /t2.txt /cpy/t3.txt
hadoop fs -ls /cpy
```

- ### To rename a file

```
hadoop fs -mv /zip.sh /a1.sh
```

- ### To display the filesize and absolute path

```
hadoop fs -du /a1.sh
```

- ### To delete a dir with it contents

```
hadoop fs -rmr /cpy
```

- ### To change the rep factor

```
hadoop fs -setrep -w 1 /a1.sh

hadoop fs -ls /a1.sh
```

- ### To view the help of any command

```
hadoop fs -help
```

- ### To change the permission in hdfs

```
hadoop fs -chmod 666 /a1.sh
hadoop fs -ls /a1.sh
```

- ### To view the statistics of a file

```
hadoop fs -stat %b,%o,%r /a1.sh

%b byte,%o block size,%r replication factor
```

> - ### [WebUI of hdfs](http://1.1.1.61:50070/dfshealth.jsp)

- ### To change the group of a file

```
hadoop fs -chgrp root /a1.sh
```

- ### To view the hdfs block size,ext3 and hdfs relation

```
dd if=/dev/zero of=dump1.txt bs=1024K count=68

ls -lh dump1.txt ---------to see permission of the linux file

hadoop fs -put dump1.txt /

hadoop fsck /dump1.txt -files -blocks -locations
```

## The data of the above file is actually stored in

```
ls /home/hadoop/hadoop_data/dfs/data/current

blk_6648759680878941733_1905
```

- ### To run the mapreduce program

```
nano book1.txt

anjan
anjan
amit
amit
amit
arpita
arpita
```

- ### Now copy the book1.txt file into HDFS

```
hadoop fs -put book1.txt /user/hadoop
```

- ### Run the mapreduce program for wordcount

```
hadoop jar /home/hadoop/hadoop/hadoop-examples-1.2.1.jar wordcount /user/hadoop/book1.txt output1

hadoop fs -ls output1
```

- ### To view the output of mapreduce

```
hadoop fs -cat output1/part-r-00000
```

- ### Run the mapreduce program for wordcount and grep

```
hadoop jar /home/hadoop/hadoop/hadoop-examples-1.2.1.jar grep /user/hadoop/book1.txt output1 'anjan'

hadoop fs -ls output3

hadoop fs -cat output3/part-00000
```
