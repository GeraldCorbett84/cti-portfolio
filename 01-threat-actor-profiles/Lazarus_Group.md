# Threat Actor Profile: Lazarus Group (Hidden Cobra)

**Origin/Attribution:** Lazarus Group is attributed to North Korea's Reconnaissance General Bureau (RGB), the country's primary foreign intelligence service. The group has been active since at least 2009 and is one of the few APTs with confirmed destructive, espionage, and financially motivated operations. Attribution has been made by the U.S. government, Kaspersky, Mandiant, and multiple private sector researchers.

**Motivation:** Dual-mission threat actor with both espionage and financial objectives. Financial operations are believed to directly fund North Korea's weapons programs. The group has stolen an estimated $3+ billion in cryptocurrency since 2017.

**Targeted Sectors:**
- Financial institutions and cryptocurrency exchanges
- Defense contractors (especially U.S. and South Korean)
- Healthcare and pharmaceutical companies
- Media and entertainment
- Government agencies (South Korea, U.S., Europe)

**Known TTPs (MITRE ATT&CK):**
- Watering hole attacks against specific industry portals (T1189)
- Trojanized software supply chain attacks (T1195.002)
- Extensive use of custom malware with anti-analysis features (T1497)
- Blockchain/DeFi smart contract exploitation (T1657)
- Social engineering via fake job recruiters on LinkedIn (T1566.003)
- Use of cryptocurrency mixing/tumbling services for money laundering (T1531)

**Signature Tools/Malware:**
- **BLINDINGCAN:** Remote access trojan used in defense contractor targeting
- **AppleJeus:** Trojanized cryptocurrency trading application
- **FASTCash:** Payment switch malware targeting banking SWIFT systems
- **WannaCry ransomware:** Deployed globally in 2017, attributed to Lazarus by NSA and UK NCSC
- **ELECTRICFISH:** Tunneling tool for data exfiltration

**Notable Campaigns:**
- **Sony Pictures Hack (2014):** Destructive attack using Destover malware; exfiltrated ~100TB of data in retaliation for the film "The Interview"
- **Bangladesh Bank Heist (2016):** Attempted theft of $1 billion from SWIFT; $81 million successfully stolen
- **WannaCry (2017):** Global ransomware outbreak affecting 200,000+ systems across 150 countries
- **Operation Dream Job (2020–ongoing):** Targeting defense/aerospace employees with fake LinkedIn recruitment to deploy BLINDINGCAN
- **Ronin Network Hack (2022):** $620 million stolen from Axie Infinity's Ronin blockchain bridge

---
*Sources: MITRE ATT&CK, Mandiant APT38 Report, Kaspersky Lazarus Group Analysis, U.S. CISA Advisory AA21-048A*
