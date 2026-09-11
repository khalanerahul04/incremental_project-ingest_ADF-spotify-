## incremental_project-ingest_ADF-spotify-

### lookup activity 
 to fetch last watermark or last updated date from cdc file by give file path JSON
<img width="1467" height="775" alt="image" src="https://github.com/user-attachments/assets/06eb15d1-d84e-4ea2-9386-70dd4f7af6b8" />
<img width="573" height="348" alt="image" src="https://github.com/user-attachments/assets/dfee4c21-4ccc-474b-aa8e-8a20e7dca4ce" />
<img width="541" height="313" alt="image" src="https://github.com/user-attachments/assets/9a59ab01-ac7c-4c13-a16f-5dcc03f8d842" />


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

input       <img width="556" height="370" alt="image" src="https://github.com/user-attachments/assets/a940a004-80cf-4736-b880-08746d2bab5e" />
 

### sink dataset 
"""  
@concat(pipeline().parameters.table,'_',variables('current_timestamp'))    

"""
<img width="1371" height="605" alt="image" src="https://github.com/user-attachments/assets/8125a18c-9d0e-47fb-ab52-1e2448b5e99c" />

### if condition 
### if new records read by copy data by expression for new data "dataRead >0 "
<img width="736" height="414" alt="image" src="https://github.com/user-attachments/assets/821458c3-ad8b-4c51-b536-e3876938910b" />
### if true = pipeline working next to script activity to fetch max value (key point of incremental load) cdc_col (e.g. updated_at) other wise false if dataRead is 0 use delete activity for unnecessary file 
<img width="1752" height="828" alt="image" src="https://github.com/user-attachments/assets/cf0b6052-0e1a-47b8-93a9-009fc9d9a29a" />
<img width="1843" height="834" alt="image" src="https://github.com/user-attachments/assets/d19d5c8e-bb3c-4045-906a-10b962adb7f9" />

### 2nd copy activity for updated new incremental or updated cdc col value to the cdc file (in source data set use one json empty file with new column function for overwrite the o/p of script activity) <img width="1318" height="678" alt="image" src="https://github.com/user-attachments/assets/31296d5a-3e12-4485-9d9e-882d5d1aedd1" />
<img width="1826" height="820" alt="image" src="https://github.com/user-attachments/assets/fc945688-2f57-4e01-931e-37be48eefde2" />

### Sink dataset for cdc file .
<img width="1310" height="659" alt="image" src="https://github.com/user-attachments/assets/e9c646d9-46c5-47e9-97ec-c5cd46309e78" />

### Delete actvitity within if condition
if dataRead is 0 use delete activity for unnecessary file 
<img width="1830" height="837" alt="image" src="https://github.com/user-attachments/assets/5024a045-3116-4cf3-85b1-b3f1beb82044" />


### pipeline run 1st time input and output by each activity 
### o/p set variables
<img width="575" height="267" alt="image" src="https://github.com/user-attachments/assets/13c5e9d1-cfc2-4845-9f58-88aea3d9cc1f" />

### lookup activity for last cdc 
input <img width="649" height="305" alt="image" src="https://github.com/user-attachments/assets/1c608a50-3d55-4d5f-9a78-fed94d77f8cc" />
output
<img width="746" height="400" alt="image" src="https://github.com/user-attachments/assets/a8148060-8bf3-4ea9-b0d1-a87b62d906e1" />
### copy activity sql to stog.a/c
<img width="542" height="249" alt="image" src="https://github.com/user-attachments/assets/03fb40d7-3b54-4913-aedd-4ee8c4019ad0" />

### script activity for max value from sql db table 
<img width="651" height="366" alt="image" src="https://github.com/user-attachments/assets/91204b77-4888-477b-9a83-dfbb3fe19899" />

<img width="667" height="320" alt="image" src="https://github.com/user-attachments/assets/bb8e5f2b-832b-45de-af1a-6bad600f0fa6" />

### update cdc 
<img width="745" height="340" alt="image" src="https://github.com/user-attachments/assets/e94a95cb-86a1-4a8e-8640-79cb70570ab5" />

<img width="668" height="353" alt="image" src="https://github.com/user-attachments/assets/aa39c648-22e1-46cf-ada7-9e799ac2f84f" />
 












<img width="1515" height="646" alt="image" src="https://github.com/user-attachments/assets/40009f64-2953-4be8-bc5b-bcbad454a226" />




