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
⭕️ Objective: Find the total number of events logged in March 2022.
⭕️ Method:

In Kibana Discover tab, set time range to March 1–31, 2022.

Observe statistics/hits counter.
✅ Outcome: 1,482 events identified 
reddit.com
+15
mattheweaton.net
+15
harpocrat3s.com
+15
reddit.com
reddit.com
+9
medium.com
+9
harpocrat3s.com
+9

📸 Screenshot Space:

✏️ Task 2: Identify the suspicious source IP
⭕️ Objective: Determine which IP triggered the C2 beacon.
⭕️ Method:

Filter on source_ip field.

Note two unique IPs: one with ~99.6% traffic, the other ~0.4%.

Drill into lower-volume IP via user agent filter.
✅ Outcome: Suspicious IP: 192.166.65.54 
github.com
+8
mattheweaton.net
+8
mahmoudelfawair.medium.com
+8

📸 Screenshot Space:

✏️ Task 3: Identify the Windows binary used
⭕️ Objective: Spot the legitimate binary used to download C2 content.
⭕️ Method:

Inspect user_agent for 192.166.65.54.

Identify binary name in the logs.
✅ Outcome: bitsadmin 
jacob-taylor.gitbook.io
+7
mattheweaton.net
+7
mahmoudelfawair.medium.com
+7

📸 Screenshot Space:

✏️ Task 4: Recognize the C2 host
⭕️ Objective: Name the remote file-hosting site used for C2.
⭕️ Method:

Review host field in filtered logs.
✅ Outcome: pastebin.com 
medium.com
+7
medium.com
+7
jacob-taylor.gitbook.io
+7
medium.com
+5
mattheweaton.net
+5
mahmoudelfawair.medium.com
+5

📸 Screenshot Space:

✏️ Task 5: Reconstruct the full C2 URL
⭕️ Objective: Discover the exact URL for payload retrieval.
⭕️ Method:

Combine host and uri fields from the log.
✅ Outcome: pastebin.com/yTg0Ah6a 
mahmoudelfawair.medium.com
+1
medium.com
+1
jacob-taylor.gitbook.io
+5
mattheweaton.net
+5
mahmoudelfawair.medium.com
+5

📸 Screenshot Space:

✏️ Task 6: Identify the file name accessed
⭕️ Objective: Name the file downloaded and retrieve its contents.
⭕️ Method:

Visit reconstructed URL.
✅ Outcome: File downloaded: secret.txt containing the flag 
mahmoudelfawair.medium.com
+1
sowl1.github.io
+1
reddit.com
+1
reddit.com
+1

📸 Screenshot Space:

✏️ Task 7: Extract the flag code
⭕️ Objective: Retrieve the flag in the format THM{_____}.
⭕️ Method:

View content of secret.txt.
✅ Outcome: Flag revealed: THM{SECRET__CODE} 
reddit.com
enescayvarli.medium.com
+10
medium.com
+10
medium.com
+10

📸 Screenshot Space:

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

