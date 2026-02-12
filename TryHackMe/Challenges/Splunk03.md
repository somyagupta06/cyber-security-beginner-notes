# Splunk 3
## Q1. List out the IAM users that accessed an AWS service (successfully or unsuccessfully) in Frothly's AWS environment? Answer guidance: Comma separated without spaces, in alphabetical order. (Example: ajackson,mjones,tmiller)

- As from the guide we get to know the all the events in AWS are managed by cloudtrail so users that accessed AWS service also managed by sourcetype =  aws:cloudtrail.
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 33 03 PM" src="https://github.com/user-attachments/assets/7ca8997b-ea72-46b2-b54d-db865bde0960" />
- So in the users we got our desired IAM users

## Q2. What field would you use to alert that AWS API activity has occurred without MFA (multi-factor authentication)? Answer guidance: Provide the full JSON path. (Example: iceCream.flavors.traditional)

- Using the same Query we get the desired field
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 39 17 PM" src="https://github.com/user-attachments/assets/2d592fcd-0b57-4dd4-857a-1c53c2dad420" />

## Q3. What is the processor number used on the web servers? Answer guidance: Include any special characters/punctuation. (Example: The processor number for Intel Core i7-8650U is i7-8650U.)

- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 42 17 PM" src="https://github.com/user-attachments/assets/3a2a08b3-61f6-4425-b7bc-eaff084d6d5c" />

## Q4. Bud accidentally makes an S3 bucket publicly accessible. What is the event ID of the API call that enabled public access? Answer guidance: Include any special characters/punctuation.

- from the link i got the keyword for the bucket that is putbucketacl.
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 45 20 PM" src="https://github.com/user-attachments/assets/3d3146c4-8b89-46a4-b7ce-d64b756cc431" />
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 45 36 PM" src="https://github.com/user-attachments/assets/5af634c0-b3b2-41fa-802a-dfccfc0a143e" />

## Q5. What is Bud’s username?
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 49 12 PM" src="https://github.com/user-attachments/assets/b1783d77-b8d9-449d-a82c-d7bf69cd3f0c" />

## Q6. What is the name of the S3 bucket that was made publicly accessible?
- <img width="1470" height="956" alt="Screenshot 2026-02-10 at 6 49 40 PM" src="https://github.com/user-attachments/assets/af5cde89-2c42-43f3-808f-ff3977c2e483" />

# Task - 4 Cryptomining Events

## Q.1 A Frothly endpoint exhibits signs of coin mining activity. What is the name of the second process to reach 100 percent CPU processor utilization time from this activity on this endpoint? Answer guidance: Include any special characters/punctuation.

- <img width="1470" height="956" alt="Screenshot 2026-02-12 at 8 01 06 PM" src="https://github.com/user-attachments/assets/6007aeac-12c7-49aa-aca0-5ff2112951e6" />
- <img width="1470" height="956" alt="Screenshot 2026-02-12 at 8 01 27 PM" src="https://github.com/user-attachments/assets/b588c226-dd5f-47ac-9f9a-eb7cd4e11e4c" />


## Q.2 What is the short hostname of the only Frothly endpoint to actually mine Monero cryptocurrency? (Example: ahamilton instead of ahamilton.mycompany.com)
- <img width="1470" height="956" alt="Screenshot 2026-02-12 at 8 02 54 PM" src="https://github.com/user-attachments/assets/ccd71ebf-66f7-4458-805b-d3e00ddb69bc" />

## Q.3 Using Splunk's event order functions, what is the first seen signature ID of the coin miner threat according to Frothly's Symantec Endpoint Protection (SEP) data?

- we have got the source type to searched.
- After searching the Query
<img width="1470" height="956" alt="Screenshot 2026-02-12 at 7 51 35 PM" src="https://github.com/user-attachments/assets/ea46acfa-fcc5-4945-a129-ca999aaed7f0" />
<img width="1470" height="956" alt="Screenshot 2026-02-12 at 7 52 53 PM" src="https://github.com/user-attachments/assets/9fe55c41-73dc-4a70-aa22-36f0918fd9e0" />

## Q.4 What is the name of the attack?
- <img width="1470" height="956" alt="Screenshot 2026-02-12 at 7 54 24 PM" src="https://github.com/user-attachments/assets/e3e06b84-af2f-47f6-b655-46334ed9326d" />

## Q.5 According to Symantec's website, what is the severity of this specific coin miner threat?
- medium

## Q.6 What is the short hostname of the only Frothly endpoint to show evidence of defeating the cryptocurrency threat? (Example: ahamilton instead of ahamilton.mycompany.com)
- <img width="1470" height="956" alt="Screenshot 2026-02-12 at 7 58 11 PM" src="https://github.com/user-attachments/assets/20d2870d-3ce0-4a9c-96e1-4c093e715f02" />


