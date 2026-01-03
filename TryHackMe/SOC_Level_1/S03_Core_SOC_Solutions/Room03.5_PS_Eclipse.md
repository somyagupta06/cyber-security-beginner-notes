# PS Eclipse - Challenge 

### 1. A suspicious binary was downloaded to the endpoint. What was the name of the binary?
- What i do ?
  - First i check all the logs using the query. 
  ```
  index = *
  ```
  - Second given in the scenario the compromised device is of user Keegan.
  - Given in the question it is asking for a binary at the endpoint. So I ran the query
  ```
  index = * ".exe" User = "keegan"
  ```
  - In the targetfilename logs i found the file
    ```
    C:\Windows\Temp\OUTSTANDING_GUTTER.exe
    ```
- What actually should i implement ?
  - I must first check dest. port and dest. ip
  - There i have found an ip with most traffic and dest. port , than on using the query
  ```
  * Destinationport = "3.17.7.232" Destinationport = "443"
  ```
  - I will be able to find the executable binary with a proof .

- Proof
   - We can't just guess a file name that it is suspicious , we have to collect evidence.
   - We check the traffic from port 80 and in the image we got the powershell command running.
   - Now we have to see why powershell is used.
   - So now we used the query
     ```
     * powershell
     ```
   - here in the Command line logs we can see a base64 encoded command had runned.
     <img width="1470" height="605" alt="Screenshot 2026-01-03 at 12 10 28 PM" src="https://github.com/user-attachments/assets/389768b8-1255-4d38-9140-a360f405fcbf" />
   - On decoding it using cyberchef
     <img width="1470" height="820" alt="Screenshot 2026-01-03 at 12 11 10 PM" src="https://github.com/user-attachments/assets/a99f251a-f229-46c5-814b-0550da6398c6" />
   - Now we got the answers of many questions

### 2. What is the address the binary was downloaded from? Add http:// to your answer & defang the URL.

 - hxxp[://]886e-181-215-214-32[.]ngrok[.]io
 - we had defang it using cyberchef
   
### 3. What Windows executable was used to download the suspicious binary? Enter full path.
 - on running the query
  ```
     * powershell
  ```
 - Checking the ParentImage Logs we got the path of the powershell

### 4. What command was executed to configure the suspicious binary to run with elevated privileges?
 - Check the decoded base64 cyberchef text and you will be able to find the command that run the binary
   ```
   C:\Windows\Temp\OUTSTANDING_GUTTER.exe;SCHTASKS /Create /TN "OUTSTANDING_GUTTER.exe" /TR "C:\Windows\Temp\COUTSTANDING_GUTTER.exe" /SC ONEVENT /EC Application /MO *[System/EventID=777] /RU "SYSTEM" /f
   ```
### 5. What permissions will the suspicious binary run as? What was the command to run the binary with elevated privileges? (Format: User + ; + CommandLine)
- Check the decoded string
  ```
  NT AUTHORITY\SYSTEM;"C:\Windows\system32\schtasks.exe" /Run /TN OUTSTANDING_GUTTER.exe
  ```
### 6. The suspicious binary connected to a remote server. What address did it connect to? Add http:// to your answer & defang the URL.
- Check the query name logs
  ```
  9030-181-215-214-32.ngrok.io
  ```
- It look likes the part of the C2 server
### 7. A PowerShell script was downloaded to the same location as the suspicious binary. What was the name of the file?
- we use the query
  ```
  * .ps1
  ```

- The script have to be in the same directory as the binary
### 8. The malicious script was flagged as malicious. What do you think was the actual name of the malicious script?
- We check the md5  hash of the script.ps1 on virustotal
### 9. A ransomware note was saved to disk, which can serve as an IOC. What is the full path to which the ransom note was saved?
- on running the query
```
* .txt
```
### 10. The script saved an image file to disk to replace the user's desktop wallpaper, which can also serve as an IOC. What is the full path of the image?
- We use the query
  ```
  * . jpg
  ```
  
