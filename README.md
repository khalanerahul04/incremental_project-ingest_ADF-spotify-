# incremental_project-ingest_ADF-spotify-

# lookup activity 
 to fetch last watermark or last updated date from cdc file by give file path JSON
<img width="1467" height="775" alt="image" src="https://github.com/user-attachments/assets/06eb15d1-d84e-4ea2-9386-70dd4f7af6b8" />

 # set variable 
 1st create variable "current"for pipeline (this is for current time when data will be extracting and write in the destination folder this timestamp use set variable @utcnow().
 purpose : it will be saved at destination with file name by using output of this activity
 <img width="1234" height="598" alt="image" src="https://github.com/user-attachments/assets/66ac98f2-8292-4505-aad7-9cac4cd95ade" />



