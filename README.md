
# COMP3010 CW2 Report

# Introduction

Security Operations Centres(SOCs) play an important role in the monitoring, detection, and response to security incidents in enterprise environments, allowing quick and efficient response to security alerts and incidents. Modern SOCs rely on the use of Security Information and Event Management(SIEM) platforms to help provide a centrailsed view on security information such as managing alerts produced by platforms such as splunk and support incident investigation.

The Splunk Boss Of The SOC(BOTSv3) dataset simulates enterprise and cloud-based attack scenarios that realisticly reflect SOC workload, BOTSv3 gives the oppotunity to apply SOC methodologies to a dataset that is controlled yet remains realistic.

The aim of the investigation is to analyse the BOTSv3 dataset using Splunk to identify suspicious and malicious activity relevant to real world SOC operations. This report then focuses on answering a set of 200-level BOTSv3 questions through the use of structured splunk analysis.

The scope of the investigation is limited to the BOTSv3 dataset and the log sources it contains such as AWS, The investigation is run under the assumption that the dataset is whole and contains accurate records of events from the simulated environment.

# SOC Roles & Incident Handling Reflection
In SOC operations tend to make use of a tiered structure to help manage various tasks such as alert triage, investigation, and escalation, Tier 1 analyists are responsible for continuous monitoring and initial alert triage/validation, Tier 2 is responsible for peforming in-depth investigation of incidents and event correlation while tier 3 deals with threat hunting and detection engineering. The work done for the BOTSv3 investigation align closely with the work of a Tier 2 analyst due to its nature as a more in-depth investigation than a Tier 1 analyst would perform


