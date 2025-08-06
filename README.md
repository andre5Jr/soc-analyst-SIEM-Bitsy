📝 Project Title:

TryHackMe – ItsyBitsy: Investigating Potential C2 Communication

🎯 Objective:

Investigate an IDS alert for possible command-and-control (C2) activity using HTTP connection logs in Kibana ELK Stack to identify the malicious payload and extract the flag.

🛠️ Tools Used:

Elasticsearch & Logstash

Kibana (Discover & Visualizations)

HTTP traffic logs

Pastebin (as suspected C2 server)

❌ Skills Demonstrated:

SIEM log filtering and analysis

Querying network connection logs

Interpreting user agents and identifying malicious tools

C2 investigation using public file hosting detection

Flag retrieval and secure investigation practices

1. Project Overview
This project documents an incident investigation in a simulated SOC environment. ItsyBitsy challenges the analyst to explore a potential C2 beacon via HTTP logs ingested into Kibana’s connection_logs index. The goal: uncover suspicious network activity, identify the C2 channel, and extract the payload flag.

2. Task Breakdown

✏️ Task 1: Determine March 2022 log volume
⭕️ Objective: How many events were returned for the month of March 2022?

⭕️ Method:

In Kibana Discover tab, set time range to March 1–31, 2022.

Observe statistics/hits counter.

🔱 Answer: 1482

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T1-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T1-2.png) 

✏️ Task 2: What is the IP associated with the suspected user in the logs?
⭕️ Objective: Determine which IP triggered the C2 beacon.

⭕️ Method:

Filter on source_ip field.

Note two unique IPs: one with ~99.6% traffic, the other ~0.4%.

Drill into lower-volume IP via user agent filter.

🔱 Answer: 192.166.65.54

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T2-1.png) 

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T2-2.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T2-3.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T2-4.png)   

✏️ Task 3: The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?
⭕️ Objective: Spot the legitimate binary used to download C2 content.

⭕️ Method:

Inspect user_agent for 192.166.65.54.

🔱 Answer: bitsadmin

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T3-1.png)   

✏️ Task 4: The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?
⭕️ Objective: Name the remote file-hosting site used for C2.

⭕️ Method: 

Review host field in filtered logs.

🔱 Answer: pastebin.com

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T4-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T4-2.png)   

✏️ Task 5: What is the full URL of the C2 to which the infected host is connected?
⭕️ Objective: Discover the exact URL for payload retrieval.

⭕️ Method:

Combine host and uri fields from the log.

🔱 Answer: pastebin.com/yTg0Ah6a

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T5-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T5-2.png)   

✏️ Task 6: A file was accessed on the filesharing site. What is the name of the file accessed?
⭕️ Objective: Name the file downloaded and retrieve its contents.

⭕️ Method:

Visit reconstructed URL.

🔱 Answer: secret.txt

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T6-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T6-2.png) 

✏️ Task 7: The file contains a secret code with the format THM{_____}.
⭕️ Objective: Retrieve the flag in the format THM{_____}.

⭕️ Method:

View content of secret.txt.

🔱 Answer: THM{SECRET__CODE}

📸 Screenshot Space:

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T7-1.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T7-2.png)   

![image alt](https://github.com/andre5Jr/soc-analyst-SIEM-Bitsy/blob/2e5bfb229fb33ef7c0e93005550e37817fe2c701/T7-3.png) 

3. Analysis and Reflection

💡 Challenges Faced:

Filtering large datasets to isolate anomalies.

Differentiating between high-volume legitimate traffic and low-volume malicious activity.

Recognizing bitsadmin as a legitimate Windows tool that can be weaponized.

💡 Lessons Learned:

SIEM filters (time, IP, UA, host) are crucial to triage.

Malicious actors often misuse legitimate tools—monitor metadata too.

Public paste sites are common C2 channels; logs reveal key indicators.

💡 Relevance to SOC Analyst Roles:

Practice in real-world log triage and malicious behaviour identification.

Understanding of C2 patterns and how to detect abuse of built-in tools.

Experience with flag retrieval enhances alert response workflows.

💡 Relevance to Penetration Testing / Red Teaming:

Highlights how simple HTTP-based C2 channels operate.

Shows how benign tools like bitsadmin can be abused in scenario crafting.

Valuable for modelling threat actor tradecraft in red team plans.

4. Conclusion

💡 Summary:
Isolated suspicious events, found low-traffic but high-risk IP, identified bitsadmin as the malicious binary, mapped C2 to Pastebin URL, and extracted the flag from secret.txt.

💡 Skills Gained:

Advanced ELK-based log analysis

Malicious traffic detection

Secure payload extraction

SIEM investigation documentation

💡 Next Steps:

Integrate detection rules for misuse of Windows tooling (e.g., bitsadmin) in SIEM.

Expand investigations to DNS, proxy, and EDR logs for richer detection context.

Simulate C2 scenarios to test alert effectiveness and response protocols.

