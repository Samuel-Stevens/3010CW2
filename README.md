
# COMP3010 CW2 Report
Youtube Presentation Link: https://www.youtube.com/watch?v=74DODfoYMDs

# Introduction

Security Operations Centres(SOCs) play an important role in the monitoring, detection, and response to security incidents in enterprise environments, allowing quick and efficient response to security alerts and incidents. Modern SOCs rely on the use of Security Information and Event Management(SIEM) platforms to help provide a centralised view on security information such as managing alerts produced by platforms such as Splunk and support incident investigation. 
  

The Splunk Boss Of The SOC(BOTSv3) dataset simulates enterprise and cloud-based attack scenarios that realistically reflect SOC workload, BOTSv3 gives the opportunity to apply SOC methodologies to a dataset that is controlled yet remains realistic. 
  

The aim of the investigation is to analyse the BOTSv3 dataset using Splunk to identify suspicious and malicious activity relevant to real world SOC operations. This report then focuses on answering a set of 200-level BOTSv3 questions through the use of structured Splunk analysis. 


The scope of the investigation is limited to the BOTSv3 dataset and the log sources it contains such as AWS, The investigation is run under the assumption that the dataset is whole and contains accurate records of events from the simulated environment. 

# SOC Roles & Incident Handling Reflection
In SOC operations tend to make use of a tiered structure to help manage various tasks such as alert triage, investigation, and escalation, Tier 1 analysts are responsible for continuous monitoring and initial alert triage/validation, Tier 2 is responsible for performing in-depth investigation of incidents and event correlation while tier 3 deals with threat hunting and detection engineering. 

The work done for the BOTSv3 investigation align closely with the work of a Tier 2 analyst due to its nature as a more in-depth investigation than a Tier 1 analyst would perform, using log analysis to determine who was the cause of the incident for the purpose of attribution and accountability and identifying misconfiguration of assets such as S3 buckets. 

This investigation aligns with standard incident handling practices such as the NIST lifecycle, this investigation focuses on the Detection and Analysis section, looking into the users of the system, looking for API calls that caused the incident and identifying the MFA field that could be used to produce alert which can then be escalated to tier 3 for use in future detection and prevention, this shows the importance of proper logging for proper incident detection and response.

# Installation and Data Preparation
 For this investigation, I chose to run Splunk through a ubuntu VM using VMware Pro, after installing Linux I moved to downloading Splunk to my machine using the tgz wget link on the site I downloaded it to my VM. 
 <img width="1099" height="512" alt="image" src="https://github.com/user-attachments/assets/2173691b-adb0-4f97-b89e-81575ccc0c5e" />
After that I proceded to extract splunk to /opt.
<img width="857" height="416" alt="image" src="https://github.com/user-attachments/assets/056a6b30-de29-43fd-9e0b-57581bc6f065" />
After confirming that splunk was successfully downloaded and in the right area, i moved onto downloading the BOTSv3 dataset from the official GitHub page and once downloaded, extracting it then copying it to /opt/splunk/etc/apps/
<img width="1508" height="928" alt="image" src="https://github.com/user-attachments/assets/69968bc1-fb0f-4cb1-bd4f-0d5f3b2a9e77" />
<img width="1496" height="929" alt="image" src="https://github.com/user-attachments/assets/e2ea8e24-7828-4e06-8fd5-7b721ac54af5" />
Now that i had splunk installed and the dataset downloaded in the correct place i set up the Splunk account i would be using, and then once the localhost started working, i searched the BOTSv3 data set with the following SPL query to ensure the correct number of records were present, to ensure that i had a proper installation of splunk and the dataset
<img width="1511" height="939" alt="image" src="https://github.com/user-attachments/assets/803f7fa0-125a-4dd4-baad-bbfe95249206" />
<img width="1529" height="944" alt="image" src="https://github.com/user-attachments/assets/4e94b4e3-23ec-436d-b40f-e5762be7b7f0" />
The reason i chose Splunk as my SIEM platform of choice for this investigation is that it is a very commonly used SIEM platform used by enterprises across the globe, it also works across a large number of source types, allowing analysts to perform event correlation across a large number of source types to properly understand an incident and find its causes. 

Splunk also allows for the creation of alerts using different fields like the MFAUsed field. The choice to use linux for this investigation also fits well into SOC infrastructure as it is commonly used in real world SOC operations as it can be highly tailored to specific security tasks and is required for some of the more specialized tools used with SOC operations.

# Guided Answers to BOTSv3 Questions
Q1: IAM users accessing AWS services

There were a total of 6 Users within the dataset 2 being Admin accounts namely, web_admin and frothly_admin so they arent counted as the users leaving Bstoll, Btun, splunk_access, and mkraeus, these were found by looking at the values of the UserName field that the dataset contains.
<img width="1502" height="925" alt="image" src="https://github.com/user-attachments/assets/a2459198-10de-4c06-9e44-c0d862a5c4a8" />
This answer relates to SOC operations in that by having an easy method of tracking user names in splunk, it makes it easier to identify identity misuse as all actions are linked to an account.

Q2:Detecting AWS API activity without MFA

Within the dataset, there is a field that can be used to identify API activity that doesn't have MFA labelled "userIdentity.sessionContext.attributes.mfaAuthenticated", this field can be used to setup alerts that would trigger when this field is false. This answer is relevant in SOCs as it can be used to help alert to high-risk authentication behaviour, this field can be used in a splunk alert to alert analysts of this behaviour and the possible false positives can be reduced by excluding console logs
<img width="777" height="485" alt="image" src="https://github.com/user-attachments/assets/77d966a7-fc2c-42f9-b547-0bccb6b8501c" />

<img width="784" height="484" alt="image" src="https://github.com/user-attachments/assets/bb108777-59cf-4f21-b8bd-840b263cc183" />

Q3:Processor Number on Web Servers

The processor number for the web servers is E5-2676, the source type used to find this information is hardware source type. This answer is relevant for SOCs as it is a part of asset awareness and baseline monitoring, asset awareness is important especially as it allows for better decision making by identifying vulnerabilities that could affect overall security.
<img width="778" height="481" alt="image" src="https://github.com/user-attachments/assets/f9548bbb-a1ef-47d1-b801-15b678bef2bf" />

Q4: Event ID of call enabling public S3 access

The event ID of the API call that enabled public access to the S3 bucket is "ab45689d-69cd-41e7-8705-5350402cf7ac", to find this I made use of an SPL query that looked through the cloudtrail logs for events that contained API Calls. This answer is relevant to SOCs as it identifies the misconfiguration that lead to data exposure, making use of CloudTrail to provide forensic traceability and with the event IDs it allows, the event ID is a key factor in the investigation as it provides key information.

<img width="776" height="520" alt="image" src="https://github.com/user-attachments/assets/bd01c44a-6f0b-4dfa-9cf6-f41abdc4d717" />

Q5:Username of user who made the S3 Bucket public

The name of the user who made the Bucket public is bstoll, this was found by looking further into the event that made the bucket public and contained the name of the user who did it, this answer is highly relevant to SOCs as it is extremely important to identify who caused the exposure for the purposes of attribution and accountability, and also supports escalation and remediation of the exposure.
<img width="776" height="520" alt="image" src="https://github.com/user-attachments/assets/bd01c44a-6f0b-4dfa-9cf6-f41abdc4d717" />

Q6:Name of the S3 Bucket that was Made Public

The name of the bucket that was made public is "frothlywebcode", this was found by looking into the event where the bucket was made public as it specifies the name as well, this answer is relevant to SOCs as scoping out the affected bucket is very important for containment and rectifying the issue.
<img width="776" height="520" alt="image" src="https://github.com/user-attachments/assets/bd01c44a-6f0b-4dfa-9cf6-f41abdc4d717" />

Q7:Name of uploaded text file
The name of the text file that was uploaded is "OPEN_BUCKET_PLEASE_FIX.txt", I found this by running a keyword search across the AWS sources for ".txt" as is is very commonly used due to its near universal compatability so it made it a likely candidate for its file type/extension, this answer is relevant to SOCs as it shows evidence of data interaction during the exposure and it shows it was successfull using the http status code for confirmation.
<img width="782" height="472" alt="image" src="https://github.com/user-attachments/assets/6cec06fc-19fb-4ff0-9c7b-7863a9eaf02c" />

Q8:FQDN with different Windows OS
the FQDN that has a different Windows OS is "BSTOLL-L.froth.ly", in order to find this I first started with finding out what endpoint was running the different operating, the query used can be seen in the first screenshot along with the results, BSTOLL-L being the only different one. 

I then moved on to the rest of the domain name and started searching through the host field in AWS, this lead me to "splunk.froth.ly" which i identfied froth.ly as being the rest of the domain leading me to the answer of "BSTOLL-L.froth.ly". 

This answer is relevant to SOCs as non standard configurations lead to a wider range of vulnerabilities that need to be accounted for, so outlier detection helps alert SOCs to what needs to be taken into account to keep the system secure.

<img width="778" height="454" alt="image" src="https://github.com/user-attachments/assets/8238fdb8-8ac0-469b-a800-801632a3291b" />
<img width="779" height="460" alt="image" src="https://github.com/user-attachments/assets/7e54dd5b-685e-4680-9c5e-bc5a39269cd3" />

# Conclusion
To summarise the findings of the investigation, there were 4 IAM users within the BOTSv3 dataset, there were AWS API calls made without MFA, as well as a misconfigured S3 Bucket called frothlywebcode was made public by IAM user BSTOLL, and a .txt file was uploaded to bucket while exposed called "OPEN_BUCKET_PLEASE_FIX.txt" showing data interaction while it was publically accessable. 

A key lesson i learnt from this investigation is the impact that comprehensive logging and visibility can have on an investigation making it significantly easier to find key information and root causes of incidents. 

Another key lesson is that effective use of SIEM tooling can significantly enhance incident detection and investigation capabilites of a SOC, such as the use of alerts making it significantly easier to detect suspicious behaviour such as the lack of MFA usage found in this investigation. 

With the threats detected in this shows a strong SOC strategy but it has areas that can improve as all strategies do, mainly focusing on detection and response. My first improvement would be implementing targeted alerts with an example of where this would work being the MFAUsed field from Q2, it could be set to trigger an alert when that registers as false outside of console logins to reduce false positives.  

A second improvement that could be made after this is strengthening MFA requirements after this making it mandatory for accessing the AWS configs.  

And a third and final improvement is the use of automated detection rules for S3 bucket misconfiguration such as making it public, creating alerts for this allows analysts to respond much quicker to possible exposures. 

