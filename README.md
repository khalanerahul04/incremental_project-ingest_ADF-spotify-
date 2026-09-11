## incremental_project-ingest_ADF-spotify-

### lookup activity 
 to fetch last watermark or last updated date from cdc file by give file path JSON
<img width="1467" height="775" alt="image" src="https://github.com/user-attachments/assets/06eb15d1-d84e-4ea2-9386-70dd4f7af6b8" />

 ### set variable 
 1st create variable "current"for pipeline (this is for current time when data will be extracting and write in the destination folder this timestamp use set variable @utcnow().
 purpose : it will be saved at destination with file name by using output of this activity
 <img width="1234" height="598" alt="image" src="https://github.com/user-attachments/assets/66ac98f2-8292-4505-aad7-9cac4cd95ade" />

### copy activity 
data extract from sql data base dynamically
create data sql sets and linked service for source data set.
for sink dateset parquet with parameter for dynamically load data 
  ''' 
   SELECT * FROM @{pipeline().parameters.schema}.@{pipeline().parameters.table} WHERE @{pipeline().parameters.cdc_col}> '@{activity('last_cdc').output.value[0].cdc}' 
   
   '''

### source dataset 
<img width="1710" height="828" alt="image" src="https://github.com/user-attachments/assets/409c85ba-f49b-4f99-a57c-c8a9af8eba78" />

### sink dataset 
"""  
@concat(pipeline().parameters.table,'_',variables('current_timestamp'))    

"""
<img width="1371" height="605" alt="image" src="https://github.com/user-attachments/assets/8125a18c-9d0e-47fb-ab52-1e2448b5e99c" />

### if condition (if new records read by copy data, by expression for new data "dataRead >0 " if true = pipeline working next to script activity to fetch max value (key point of incremental load) cdc_col (e.g. updated_at) other wise false if dataRead is 0 use delete activity for unnecessary file 
<img width="1752" height="828" alt="image" src="https://github.com/user-attachments/assets/cf0b6052-0e1a-47b8-93a9-009fc9d9a29a" />
<img width="1843" height="834" alt="image" src="https://github.com/user-attachments/assets/d19d5c8e-bb3c-4045-906a-10b962adb7f9" />

### 2nd copy activity for updated new incremental or updated cdc col value to the cdc file (in source data set use one json empty file with new column function for overwrite the o/p of script activity) <img width="1318" height="678" alt="image" src="https://github.com/user-attachments/assets/31296d5a-3e12-4485-9d9e-882d5d1aedd1" />
<img width="1826" height="820" alt="image" src="https://github.com/user-attachments/assets/fc945688-2f57-4e01-931e-37be48eefde2" />

### Sink dataset for cdc file .
<img width="1310" height="659" alt="image" src="https://github.com/user-attachments/assets/e9c646d9-46c5-47e9-97ec-c5cd46309e78" />

### Delete actvitity within if condition
if dataRead is 0 use delete activity for unnecessary file 
<img width="1830" height="837" alt="image" src="https://github.com/user-attachments/assets/5024a045-3116-4cf3-85b1-b3f1beb82044" />











<img width="1515" height="646" alt="image" src="https://github.com/user-attachments/assets/40009f64-2953-4be8-bc5b-bcbad454a226" />




