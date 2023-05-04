Download Link: https://assignmentchef.com/product/solved-csc555-mining-big-data-assignment-3
<br>
<ul>

 <li>MapReduce:</li>

</ul>




<ol>

 <li>Describe how to implement the following query in MapReduce</li>

</ol>

SELECT SUM(lo_revenue)

FROM lineorder, dwdate

WHERE lo_orderdate = d_datekey

AND d_yearmonth = ‘Jan1994’

AND lo_discount BETWEEN 5 AND 7;




<ol>

 <li>SELECT d_month, COUNT(DISTINCT d_sellingseason)</li>

</ol>

FROM dwdate

GROUP BY d_month

ORDER BY COUNT(DISTINCT d_sellingseason)




<ul>

 <li>Consider a Hadoop job that processes an input data file of size equal to 120 disk blocks (120 different blocks, you can assume that HDFS replication factor is set to 1). The mapper in this job requires 1 minute to read and process a single block of data. Reducer requires 1 second (<u>not</u> 1 minute) to produce an answer for one key worth of values and there are a total of 4000 <strong>distinct</strong> keys (mappers generate a lot more key-value pairs, but there 4000 unique keys). Assume that each node has a reducer and that the keys are distributed evenly.</li>

</ul>




<ol>

 <li>How long will it take to complete the job if you only had one Hadoop worker node? For the sake of simplicity, assume that that one mapper and one reducer are created on every node.</li>

</ol>




<ol>

 <li>10 Hadoop worker nodes?</li>

</ol>




<ol>

 <li>30 Hadoop worker nodes?</li>

</ol>




<ol>

 <li>50 Hadoop worker nodes?</li>

</ol>




<ol>

 <li>Why (or why not) would the introduction of the combiner affect the runtime of this job?</li>

</ol>




<ol>

 <li>How would changing the replication factor affect your answers for a-d?</li>

</ol>




You can ignore the network transfer costs as well as the possibility of node failure.




<ul>

 <li>

  <ol start="3">

   <li>Suppose you have a 6-node cluster with replication factor of 3. Describe what MapReduce has to do after it determines that a node has crashed while a job was being processed. For simplicity, assume that the failed node is not replaced and your cluster is reduced to 5 nodes. Specifically:</li>

  </ol></li>

</ul>




<ol>

 <li>What does HDFS (the storage layer/NameNode) have to do in response to node failure in this case?</li>

</ol>




<ol>

 <li>What does MapReduce execution engine have to do to respond to the node failure? Assume that there was a job in progress because otherwise MapReduce does not need to do anything to address a failure.</li>

</ol>




<ol>

 <li>Where does the Mapper store output key-value pairs before they are sent to Reducers?</li>

</ol>




<ol>

 <li>Why can’t Reducers begin processing before Mapper phase is complete?</li>

</ol>




<ul>

 <li>Using the SSBM schema (<a href="http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/SSBM1/SSBM_schema_hive.sql">http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/SSBM1/SSBM_schema_hive.sql</a>) load the Part table into Hive (data available at http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/SSBM1/part.tbl)</li>

</ul>

<strong><u> </u></strong>

<strong><u>NOTE</u></strong>: The schema above is made for Hive, but by default Hive assumes ‘t’ separated content. You will need to modify your CREATE TABLE statement to account for ‘|’ delimiter in the data or this won’t work.




Use Hive user defined function (i.e., SELECT TRANSFORM with weekday mapper is available here: <a href="http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/weekday_mapper.py">http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/weekday_mapper.py</a>) to perform the following transformation on Part table (creating a new transformed table): for 2<sup>nd</sup> and 7<sup>th</sup> columns, split it into individual columns. That is, a value in 2<sup>nd</sup> column, ‘blush maroon’ now becomes 2<sup>nd</sup> and 3<sup>rd</sup> column with ‘blush’ and ‘maroon’ respectively. Similarly, the 7<sup>th</sup> column will be transformed from ‘STANDARD BURNISHED NICKEL’ into three columns with values ‘STANDARD’, ‘BURNISHED’, and ‘NICKEL‘.

Please be sure that you create the entire new table, not just the transformed column. You will <strong>add a total of 3 new columns</strong> to the original part table as a result.

Remember that your transform python code (split/join) should always use tab (‘t’) between fields even if the source data is |-separated.




<ul>

 <li>Download and install Pig:</li>

</ul>

cd

wget http://rasinsrv07.cstcis.cti.depaul.edu/CSC555/pig-0.15.0.tar.gz

gunzip pig-0.15.0.tar.gz

tar xvf pig-0.15.0.tar




set the environment variables (this can also be placed in ~/.bashrc to make it permanent)

export PIG_HOME=/home/ec2-user/pig-0.15.0

export PATH=$PATH:$PIG_HOME/bin




Use the same vehicles file. Copy the vehicles.csv file to the HDFS if it is not already there.




Now run pig (and use the pig home variable we set earlier):

cd $PIG_HOME

bin/pig

<u> </u>

Create the same table as what we used in Hive, assuming that vehicles.csv is in the <u>home directory on HDFS</u>:




<strong>VehicleData = LOAD ‘/user/ec2-user/vehicles.csv’ USING PigStorage(‘,’) </strong>

<strong>AS (barrels08:FLOAT, barrelsA08:FLOAT, charge120:FLOAT, charge240:FLOAT, city08:FLOAT);</strong>

<u> </u>

You can see the table description by

<strong>DESCRIBE VehicleData;</strong>

Verify that your data has loaded by running:

<strong>                </strong>

<strong>VehicleG = GROUP VehicleData ALL;</strong>

<strong>Count = FOREACH VehicleG GENERATE COUNT(VehicleData);</strong>

<strong>                DUMP Count;</strong>




How many rows did you get? (if you get an error here, it is likely because vehicles.csv is not in HDFS)




Create the same ThreeColExtract file that you have in the previous assignment, by placing barrels08, city08 and charge120 into a new file using PigStorage .You want the STORE command to record output in HDFS. (discussed in p457, Pig Chapter, “Data Processing Operator section)

NOTE: You can use this to get one column:




OneCol = FOREACH VehicleData GENERATE barrels08;




Verify that the new file has been created and report the size of the newly created file.

(you can use <strong>quit</strong> to exit the grunt shell)





