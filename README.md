<h1>Network Forensics - Wireshark</h1>

<h2>Scenario</h2>
<a href="https://cyberdefenders.org/blueteam-ctf-challenges/149/">https://cyberdefenders.org/blueteam-ctf-challenges/149/</a>
<br><br>An anomaly was discovered within our company's intranet as our Development team found an unusual file on one of our web servers. Suspecting potential malicious activity, the network team has prepared a pcap file with critical network traffic for analysis for the security team, and you have been tasked with analyzing the pcap.
<br><br>
Tools Used:<br>
- Wireshark

<h3>Identifying the city the attack originated from.</h3>
I began by pinpointing a TCP request from the source to the destination IP. Utilising the source IP, I performed a WHOIS lookup, revealing that the source IP originated from Tianjin.<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 13 35 51" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/6071f8f2-8f1e-496c-a5a6-397db4abc0f3">
<br><br>

![Screenshot 2024-02-04 at 13 36 11](https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/741a7a37-288e-4d4f-8cfe-94d9cf934b76)

<h3>Identifying the attackers user-agent.</h3>
I was able to identify the attackers user-agent by reviewing the HTTP packet details for a HTTP request with the GET method from the attacker, which was Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0.<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 13 48 59" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/98ec5066-6158-4d2b-9c1e-4d942e083417">

<h3>Identifying the vulnerabilities exploited by the attacker.</h3>
I initiated a filter (http.request.method == "POST") to identify the uploaded web shell. By following the HTTP stream, I identified the web shell filename as "image.jpg.php," likely used to bypass file upload rules.<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 14 27 44" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/8f7fb301-5516-4a3c-b338-18d01b250bc4"><br><br>

<img width="1470" alt="Screenshot 2024-02-04 at 14 27 57" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/103d9312-f414-49cf-9e49-e95abdd860f5">

<h3>Identifying the directory used by the site to store uploaded files.</h3>
Applying the filter (http.request.method == "GET"), I identified the web shell filename "image.jpg.php" located at the path "/reviews/uploads/".<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 14 33 42" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/c8995795-bd8a-419a-ac7d-a1eb959bb7c4">

<h3>Identifying the port used by the malicious web shell</h3>
Reviewing TCP packet details, I determined that the attacker used port 8080.<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 14 41 34" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/0dfe2441-76c4-4917-8e17-aa934fe5a061"><br><br>

<h3>Identifying the file the attacker was tryign to exfiltrate.</h3>
Following the same packet identified earlier and examining the TCP stream, I identified the file as "/etc/passwd."<br><br>
<img width="1470" alt="Screenshot 2024-02-04 at 14 48 48" src="https://github.com/Rahul0902/network-forensics-wireshark/assets/44233038/aa501687-c050-4cf1-9f8a-3896084ea9c9">
