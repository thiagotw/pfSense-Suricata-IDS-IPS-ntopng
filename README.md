# pfSense & Suricata IDS/IPS
> I built this lab to gain practical experience configuring and managing a firewall and IDS/IPS, specifically to understand how network threats are detected, logged, and prevented in real time. I began by experimenting with firewall rules in pfSense to understand how traffic is allowed or denied. Once I was comfortable with that, I moved on to configuring alerts and blocking suspicious traffic using Suricata. > > My goal was not only to become comfortable navigating alert logs, tuning rule sets, and simulating real-world traffic but also to develop a cost-effective security solution that could be implemented in a medium-sized company with limited resources for cybersecurity. By optimizing firewall rules and IDS/IPS configurations, I aimed to create a practical and affordable security setup that enhances network protection without requiring significant financial investment. 

## Tools & Technologies Used

- VirtualBox (Home Lab Environment)
  - Ubuntu (24.04.2)
  - Kali-linux
-Wireshark
- pfSense (Firewall)
- Suricata (IDS/IPS)
  - Emerging Threats Rule Set
  - ntopng collects and visualizes these alerts, displaying them in interactive dashboards and help to buildup the pdf reports.

## What This Lab Does
- Sets up pfSense as the primary firewall and router within a virtual or physical home lab environment.
- Installs and configures Suricata to monitor traffic on the WAN and LAN interface.
- Enables IDS and IPS modes to detect and automatically block suspicious or malicious activity.
- Integrates Emerging Threats rule set to detect common attacks.
- Captures, logs, and displays real-time alerts through the pfSense GUI.

## Screenshots (before Suricata, just using pfSense)

#### Using Kali to flood the target with hping3:
<img src="https://github.com/user-attachments/assets/6f136ebf-5bba-4e4d-9878-f2a9aaefad1d" width="600"/>

#### Wireshark showing hping3 traffic:
<img src="https://github.com/user-attachments/assets/513d5f80-fe0e-4c21-9063-e180b25b74f8" width="600"/>

#### Created new pfSense rule to block TCP traffic from Kali:
<img src="https://github.com/user-attachments/assets/b80dee63-1d64-4366-a23e-60e8cc1d3643" width="600"/>

#### "Block Kali DoS" rule successfully blocks the hping3 traffic:
<img src="https://github.com/user-attachments/assets/113e5009-08d0-49ef-b20d-2017c25d4af9" width="600"/>

## Screenshots (Now, using Suricata)
After setting up my Suricata IDS, alerts can be seen now.

#### Suricata alerts for hping3 traffic:
<img src="https://github.com/user-attachments/assets/19f559f4-7676-4a23-afa3-345e69fdce27" width="600"/>

#### Suricata alerts for nmap scan:
<img src="https://github.com/user-attachments/assets/daac497a-66b1-4bfe-8574-0adca26ea770" width="600"/>

Then, I configured my Suricata IPS to block malicious activity.

#### hping3 (Blocked by IPS):

#### ntopng Graphical interface for alerts

1. Install ntopng on pfSense
Go to System > Package Manager > Available Packages.

Once installed, go to Services > ntopng and start the service.

2. Enable syslog output in Suricata
Edit the Suricata configuration file (suricata.yaml) and enable syslog logging:
```bash
eve-log:
  enabled: yes
  filetype: syslog
  ip_version: 4
  interface: default
```

Restart Suricata to apply the changes:
systemctl restart suricata

3. Configure ntopng to receive Suricata logs
Edit the ntopng configuration file (ntopng.conf) and add the syslog input:

```bash
-i=syslog://127.0.0.1:5140
-i=eth1
```
Restart ntopng to apply the changes:

systemctl restart ntopng

4. Access ntopng’s Web Interface

```bash   
Open a browser and go to http://<pfSense-IP>:3000.
```
Navigate to Interfaces > Settings and select the syslog interface.

Now, Suricata alerts and firewall logs will be displayed in graphical format.

#### ntopng Dashboard:
<img src="[https://www.ntop.org/guides/ntopng/user_interface/network_interface/dashboard/dashboard.html](https://www.ntop.org/guides/ntopng/_images/web_gui_dashboard_community.png)" width="600"/>


Benefits of This Integration
✅ Real-time monitoring of IDS/IPS and firewall alerts.

✅ Interactive dashboards for threat analysis.

✅ Cost-effective security solution for medium-sized businesses with limited cybersecurity budgets.

## Lessons Learned
Throughout this experience, I learned many things. I gained practical knowledge in building and managing firewall rules, monitoring network traffic, and configuring intrusion detection and prevention systems. This helped me understand how security teams use these tools to protect networks against real-world threats.

Additionally, I focused on developing a cost-effective security solution that could be implemented in a medium-sized company with limited financial resources for cybersecurity. By optimizing firewall rules and IDS/IPS configurations, I learned how to enhance network protection without requiring significant investment in expensive security solutions. This experience gave me confidence in using tools like Suricata, pfSense, and Wireshark to create a practical and affordable security setup that can be deployed in resource-constrained environments.
