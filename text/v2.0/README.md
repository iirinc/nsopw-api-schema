# Uploading Offender Files  
The NSOPW database, which is set up to store the sex offender information that is uploaded from some registration jurisdictions, has been redesigned so the data is structured properly. A Secure File Transfer Protocol (SFTP) server is available for the jurisdictions that wish to continue to upload their sex offender information. The SFTP server login information will be provided via email.  
  
# File Structure for Upload  
Registration jurisdictions should divide the information into three files so the data is structured properly to ensure that it is imported into the NSOPW database correctly. The three files will need to contain the following columns:  
 
## File 1 – Offender Information  ([jurisdiction abbr]-offenders.csv) 
1. Offender ID  
2. Date of Birth  
3. Gender  
4. Offender Web site URL  
5. Offender Photo URL  
6. Is Absconder (True/False)  
 
## File 2 – Offender Name  ([jurisdiction abbr]-names.csv) 
1. Offender ID  
2. Prefix  
3. First Name  
4. Middle Name  
5. Last Name  
6. Suffix  
7. Comment  
8. Is Primary (True/False)  
 
## File 3 – Offender Addresses  ([jurisdiction abbr]-addresses.csv) 
1. Offender ID  
2. Location Name  
3. Location Type (R, E, or S)  
4. Street  
5. City  
6. State  
7. County  
8. Zip  
9. Latitude (Decimal Format)  
10. Longitude (Decimal Format)  
 
The Offender ID column from each file will be used to develop the relationship between the data so the proper information from File 2 and File 3 is associated with the correct offender in File 1. Since this data is normalized, File 1 information will have a one-to-many relationship with File 2 and File 3 information. This means that an offender will only be referenced one time in File 1, but that offender can be referenced many times in File 2 and File 3. For example, an offender will have only one name, date of birth, gender, absconder status, Web site URL, and picture URL, but that same offender may have many aliases and many addresses. 

