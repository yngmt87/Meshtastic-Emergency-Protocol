
# Meshtastic Disaster Operations Protocol

| Item | Content |
| :---: | ----- |
| Edition | Version 3.8 |
| Date Created | 2026/03/30 |
| Purpose | A protocol for operating a private Meshtastic mesh during disasters, including coordination and handoff to municipalities, fire services, medical institutions, and shelters |
| Intended Readers | People who intend to use Meshtastic during disasters as a supplementary communications method alongside ordinary information infrastructure |
| Position of This Text | This document distinguishes between “facts” based on laws and official materials and the “operational proposals” adopted by this protocol |
| Author | YNGMT |

**Assumptions of This Protocol**  
Meshtastic is not a substitute for public mass-alert systems.  
It is intended for closed-network use for supplementary communications among field sites, regions, and bases, for case management, limited situational sharing, and handoff.  
For operations in Japan, the on-site responsible person must make the final confirmation regarding each device’s technical conformity certification, construction design certification, frequency planning, and consistency with local government regional disaster-prevention plans.  
The codes used in this document should be prioritized on trained networks; however, plain-language transmission is permitted for untrained users, external responders, and emergencies.  
When handing information off to outside organizations, it must always be converted into plain language.  

## Table of Contents

0. First Pages to Read  
1. Position of This Document  
2. Public Basis and Compliance with Japanese Regulations  
3. Operational Philosophy and Command & Control  
4. Network Design and Standard Equipment Settings  
5. Message Grammar, Case Management, and Correction Rules  
6. Handoff to Municipalities, Fire Services, Medical Institutions, and Shelters  
7. Handling of Missing Persons, Safety Confirmation, and Personal Information  
8. Preparedness, Training, and Audit  
9. Disaster-Specific Operations  
10. Operations During Armed Attack, Conflict, Riot, Missile Attack, and Nuclear Attack  
11. Case Ledger / Message Management Software (in progress)  
12. Proposed Canned Message Implementation  
13. Lessons from the Great East Japan Earthquake and Radio Operations  
14. References  
15. Supplementary Material: Codebook (by Category)  
16. Supplementary Material: Example Training Drills  
17. Supplementary Material: Pre-Transmission Check Card  
18. Supplementary Material: Operational Evaluation Indicators  
19. Supplementary Material: Conceptual Diagrams  

## 0. First Pages to Read

### 0-1. See the Big Picture First

The first thing to remember is simple. Field stations send short factual reports. Communications control sets priorities. Liaison personnel convert information into plain language for external organizations. Record staff maintain case IDs and correction history. The first step in reducing confusion is not to mix up who is responsible for what.  
<img src="/img/mep_english_01.png"> 
**Figure A. Overall View of This Protocol**  
A diagram for understanding who sends what, and to where.

### 0-2. What to Prioritize Immediately After Disaster Onset

A common failure in disasters is trying to transmit too much information. Immediately after impact, do not send everything; prioritize only the information directly tied to saving lives. Understand by phase what can be transmitted at each stage.  
<img src="/img/mep_english_02.png">  
**Figure B. Priorities by Phase**  
A diagram showing what should be transmitted from immediate impact through prolonged operations.

### 0-3. How to Read the Standard Syntax

The short reports in this protocol may look difficult, but in practice they consist of the sequence: “priority,” “case ID,” “SEQ,” “status,” “action,” “body,” “confidence,” and “report time.” The key point is that the time inside the case ID is for identifying the incident, while the time at the end is the time of that specific report. They do not mean the same thing.  
<img src="/img/mep_english_03.png"> 

**Figure C. How to Read the Standard Syntax (with SEQ)**  
A diagram for reading the time in the case ID separately from the report order and report time of each update.

### 0-4. Think About Confidence First in Three Levels

If users try to judge confidence too finely from the beginning, beginners easily become uncertain. Therefore, this protocol first organizes confidence into three levels—CFM / PRB / UNC—and only later, if necessary, maps them to A1–E5. The crucial point is not to mix unconfirmed information with confirmed information.  
<img src="/img/mep_english_04.png">  
**Figure D. How to Determine Confidence**  
Start by judging with CFM / PRB / UNC.

## 1. Position of This Document

**[Fact]**  
The U.S. ICS/NIMS framework emphasizes common terminology, plain language, and standardized message handling. Meanwhile, Meshtastic official documentation clearly states that excessive use of position sharing, MQTT, and ROUTER/REPEATER can worsen bandwidth usage and confidentiality; that position sharing is transmitted on the PRIMARY channel; and that public MQTT can expose position data to the internet. [R1][R2][R3][R4]

**[Adopted in This Protocol]**  
Accordingly, this protocol is built on four pillars: “prefer codes internally,” “plain-language conversion is mandatory for external handoff,” “do not replace public warning systems,” and “enforce case management.” Meshtastic is limited to supplementary communications under blackout, congestion, or local isolation. Handoff to municipalities, fire services, medical institutions, and shelters is handled by liaison personnel.

**Summary of This Document**
Internal = transmit in short codes.
External = hand off in plain language.
Personal information = minimum necessary, purpose-limited, and always recorded.
Network settings = CLIENT-family roles, restricted position sharing, public MQTT prohibited.

## 2. Public Basis and Compliance with Japanese Regulations

### 2-1. The 920 MHz Band and ARIB STD-T108

**[Fact]**  
ARIB STD-T108 is the standard specification for radio equipment for telemetry, telecontrol, and data transmission in the 920 MHz band. It covers 920.5–923.5 MHz for land mobile stations and 915.9–929.7 MHz for specified low-power radio stations. The standard also includes a category of specified low-power stations that do not require carrier sense. [R5]

**[Adopted in This Protocol]**  
For Meshtastic operations in Japan, only radio modules, finished devices, and antenna configurations individually confirmed to be within the applicable conformity scope in Japan are eligible for use. Generic LoRa hardware or overseas finished products are not treated as “obviously usable in Japan” without verification.

### 2-2. Technical Conformity and Construction Design Certification

**[Fact]**  
Japan has systems for technical conformity certification and construction design certification for specified radio equipment. Their institutional purpose differs between certification of each finished unit and certification focused on design and quality control. For large numbers of identical models, construction design certification is common, and conformity markings on equipment must be confirmed. [R6][R7]

**[Adopted in This Protocol]**  
At minimum, the operational ledger must record “model name / certification number / antenna configuration / firmware version / owner / installation location / inspection date.” On-site modification, external antenna replacement, or output setting changes are prohibited without separate approval because they may exceed the certified scope.

### 2-3. Personal Information, Disaster-Time Sharing, and Missing-Person Searches

**[Fact]**  
Japan’s Personal Information Protection Commission has stated that in emergencies such as major disasters, third-party provision of personal data is permissible when necessary to protect life, body, or property and when obtaining the subject’s consent is difficult. It has also clarified in its Q&A that rosters of persons requiring evacuation assistance and individualized evacuation plans may be provided without consent to the extent necessary during disasters. In addition, the 2025 revised Q&A states that provision among administrative bodies for missing-person searches should be appropriately judged by local governments in light of the content of the retained personal information, the purpose of use, and the necessity. [R8][R9][R10]

**[Adopted in This Protocol]**  
“What may be shared” and “what may be transmitted without limit” are different matters. On general Meshtastic operating channels, highly identifying information such as names, dates of birth, detailed addresses, diagnoses, care levels, or disability details is prohibited in principle. When necessary, switch to a private side-channel, paper ledgers, or face-to-face communication.

### 2-4. Shelter and Medical Coordination

**[Fact]**  
In December 2024, Japan’s Cabinet Office published revised shelter-related guideline materials covering shelter operations, living environment, toilet availability, and support for home evacuees and vehicle evacuees. Revisions to the Ministry of Health, Labour and Welfare’s DMAT operational guidelines indicate that prefectures should prepare operational plans and agreements in peacetime and carry out information collection, communication, coordination, and personnel/material support during disasters, and that DMAT-designated medical institutions should work on training and drills. [R11][R12]

**[Adopted in This Protocol]**  
The Meshtastic network is defined not as a “replacement administrative line” for shelter coordination, medical transport, or resource sharing, but as a “supplementary line that keeps things connected even intermittently.” It does not overwrite existing systems of shelters, medical institutions, or municipalities; it is used for handoff, flash reporting, case-number sharing, and situational snapshots when needed.

## 3. Operational Philosophy and Command & Control

### 3-1. Four Pillars

**Bandwidth protection:** Short codes and minimum necessary transmission are the rule.  
**Case management:** Every case gets a case ID, and unresolved cases remain tracked.  
**Correctability:** False reports, hearsay, and revisions are controlled through COR.  
**Responsibility for handoff:** When communicating externally, send plain language with time, location, and counts aligned.

### 3-2. Roles

| Abbrev. | Role Name | Responsibility |
| :---: | :---: | ----- |
| NETCTL | Communications Control | Phase transition, silence orders, prioritization, channel maintenance |
| OPS | Field Operations | Rescue, medical response, evacuation guidance, progress management of field cases |
| LOG | Logistics / Rear Support | Water, food, power, fuel, blankets, hygiene supplies, vehicles |
| Liaison | Handoff | Connects with municipalities, fire services, police, medical institutions, shelters, councils of social welfare, and volunteers |
| SCRIBE | Recordkeeping | Ledger, chronology, COR reflection, preservation of handoff logs |
| RFL | Restoring Family Links / Safety & Missing Persons | Name information, inquiries, family contact, personal information control |
| GATE | Gateway Operations | Case aggregation, group operations, individual inquiries, audit, map display, control of external supplementary lines |

### 3-3. Phases

| Phase | Main Focus | Transmission Discipline |
| :---: | :---: | ----- |
| 0 PREPARE | Peacetime | Confirm settings, maintain rosters, train, update canned messages |
| 1 IMPACT | Immediately after impact | Prioritize [[SOS]] and [[EMG]]. No chatter or MISS rosters |
| 2 LIFE-SAFETY | Life-saving focus | Prioritize entrapment, fire, transport, road blockage, and evacuation guidance |
| 3 STABILIZE | Emergency stabilization | Materials, shelters, infrastructure, limited MISS activation |
| 4 SUSTAIN | Prolonged operations | Periodic reporting, power-saving operation, handoff, external agreement operations |

| Basic Rules for Transmission Restrictions |
| ----- |
| In Phase 1, prohibit long explanations, discussion, speculation, casual chat, missing-person lists, and circulation of detailed addresses. |
| Even in Phase 2, rescue requests not based on EYE (direct observation) are handled in principle as PRB / UNC and must not be mixed with immediate rescue cases. |
| RFL can be activated from Phase 3 onward, but on general channels use case numbers as the focus, and separate personally identifying information into private channels or paper ledgers. |

### 3-4. Initial Confidence Filter

Before applying an A1–E5 assessment, the field should first sort information with a three-level initial filter. This is pre-processing to avoid hesitation under time pressure.  
**CFM:** Information directly seen by oneself, or backed by an official announcement.  
**PRB:** Information with matching multiple testimonies, or highly plausible information from reliable related parties.  
**UNC:** Single hearsay reports, social media, or unsupported rumors. Do not use as a basis for immediate decisions.  
Initial reports may use CFM / PRB / UNC, and can later be converted to A1–E5 when handing off externally or when finalizing the ledger.

## 4. Network Design and Standard Equipment Settings

### 4-1. Basic Meshtastic Settings

**[Fact]**  
Meshtastic official guidance generally recommends CLIENT / CLIENT_MUTE / CLIENT_BASE, and warns that casual use of ROUTER / REPEATER increases collisions, lowers delivery rates, and reduces effective range. Position sharing is transmitted on the PRIMARY channel, public MQTT may expose location to the internet, and MAX HOPS of 3 is usually recommended. [R1][R2][R3][R4]

| Item | Recommended Value | Notes |
| :---: | :---: | ----- |
| Role | CLIENT in principle | Consider CLIENT_MUTE in dense portable-node environments; only base stations should consider CLIENT_BASE |
| MAX HOPS | 3 | Fixed in principle. Consider exceptions only for fringe nodes |
| Position sharing | OFF on PRIMARY | Only when necessary, on a private secondary channel + coarse precision |
| MQTT | OFF | public MQTT / map reporting prohibited |
| NodeInfo | Keep default | Do not shorten unnecessarily |
| Telemetry | Only when necessary | During disasters, suppress constant transmission of device/env/health telemetry |
| Favorite / mute | Use actively | Locally suppress noisy nodes |

### 4-2. Standard Channel Design

**[Supplementary Rule for Node Names]**  
Nodes operating in disaster areas should add a base-location prefix at the beginning of the long node name in case GPS is OFF or broken and current position cannot be shared. For example, a Sendai base would use “SENDAI_” or the short form “SNDI_”.

**[Operational Intent]**  
This lets receivers quickly infer from which region a transmission originated even when position sharing is unavailable. In a wide-area mesh, a base-location prefix alone already makes search, transcription, and case routing easier.

| Channel | Purpose | Discipline |
| :---: | :---: | ----- |
| Ch0 | Interop / Monitor | Wide-area monitoring and awareness of known meshes. No sensitive information. Position sharing OFF |
| Ch1 | OPS-ENC | Main channel for field rescue and medical operations |
| Ch2 | LOG-ENC | Materials, transport, power, fuel |
| Ch3 | RFL-ENC | Safety confirmation, missing persons, inquiries. Limited participants |
| Ch4 | Liaison-ENC | Coordination among municipalities, shelters, and base stations |
| Ch5 | ADMIN-ENC | Configuration changes, tests, maintenance |
| Ch6 | FIELD-BACKUP | Backup |
| Ch7 | EVENT / LOCAL | Auxiliary use according to local circumstances |

### 4-3. Priority of Installation Locations

High points easily connected to municipal disaster headquarters  
Disaster base hospitals, secondary emergency centers, DMAT reception points  
Shelter headquarters, welfare shelters, material staging areas  
Strategic points requiring monitoring of rivers, coasts, and mountainous areas  
Private-sector cooperating bases capable of road opening, relay, and power generation

### 4-4. Physical Topology (Relay Placement)

More dedicated relay nodes is not always better. Place the minimum necessary number at high points with line of sight within the area to prevent radio loops and congestion.  
As a rule, place only one or two units at the highest-elevation points in the area, such as ridgelines, rooftops, or upper floors with good visibility.  
Avoid excessive use of ROUTER / REPEATER, and separate the roles of permanent base stations and reserve stations.  
Prepare solar panels, external power, and backup batteries so that the network can be sustained even through prolonged outages.  
Set Smart ExtCtrl, GPS update frequency, and telemetry intervals to the minimum necessary to suppress unnecessary transmissions and power consumption.

### 4-5. Operation of Devices Without Keyboards or with Extremely Small Displays

For devices on which text entry is difficult, ensure that minimum responses can be sent through button operations and canned-message macros.  
Single short press: send the shortest response such as QSL or ACK.  
Long press: send a pre-registered [[SOS]] initial report.  
Double press: send a progress notification such as WIP or ARRV.  
Attach an operation card to each terminal and confirm in training that it can be operated without accidental transmissions.

## 5. Message Grammar, Case Management, and Correction Rules

### 5-1. Standard Syntax

To prevent the update order of the same case from becoming unclear, this protocol uses SEQ (sequence number) as a syntax element. SEQ is written as S01, S02, S03, and so on, indicating which report number it is for that case. This allows receivers, transcribers, and external liaison staff to follow the order of reports within the same case ID in exactly the same way.  
The time at the end of the syntax is not redundant. The time inside the case ID is the filing/occurrence time used to identify the case, whereas the time at the end is the report time indicating when the information in that line was observed or transmitted. Therefore, to track the course of the same case—for example, an incident filed at 14:30, an initial report at 14:38, an access-route update at 14:51, and a closure report at 15:10—both SEQ and the report time are necessary.

| Syntax Example | Meaning |
| ----- | ----- |
| [Priority] [Case ID] [SEQ] [Status] [Action] : [Content] [Confidence] [Report Time] | The basic form that aligns priority, case number, report number, progress, action, content, confidence, and report time on one line. |
| Example: [[SOS]] MAI-Q-20260329-1430 S01 NEW REQ: COL TRP 2PAX 1RED 1YLW. B2 1438J | First report of an earthquake case in Maizuru. Two people are trapped in a collapsed building, including one red and one yellow casualty, and this is a report as of 14:38. |
| Example: [[SIT]] KOB-F-20260329-1510 S02 WIP RPT: ROD BLK BY FLD. C3 1515J | Second report of a flood case in Kobe. Roads remain blocked by flooding; assessment C3; updated at 15:15. |
| Example: [[ADM]] RFL-MAI-20260329-001 S03 NEW RPT: MISS M70 LAST SEC-B 1410J. C3 1519J | Third report of a missing-person / family reconnection case in Maizuru. A male in his 70s was last confirmed in Sector B; assessment C3; shared at 15:19. |

### 5-2. Separate Status from Action

| Status Code | Meaning | When to Use |
| :---: | :---: | ----- |
| NEW | New case | First registration |
| ACK | Received | Received and recorded in the ledger |
| WIP | In progress | Assigned and actively being handled |
| HLD | On hold | Waiting for conditions or confirmation |
| COR | Correction | Correction of a previous report |
| CLO | Closed | Completed / cleared / terminated |

| Action Code | Meaning | When to Use |
| :---: | :---: | ----- |
| REQ | Request | Request for resources, dispatch, or inquiry |
| RPT | Report | Report of current situation, change, or observation |
| SEND | Sent / en route | Materials or personnel have departed |
| ARRV | Arrived | Reached the site |
| EVQ | Evacuate / evacuation | Evacuation carried out or guided |
| QSL | Understood / acknowledged | Received, understood, read back completed |
| CAL | Notify / request external agency | Notify an outside agency, request dispatch, or request liaison handoff |

### 5-3. Case ID

In this protocol, the case ID itself is used for incident identification in the form “AREA-CAT-YYYYMMDD-HHMM[-BRANCH].” AREA is 3–5 characters. CAT uses Q (earthquake), T (tsunami), P (heavy rain / linear rainband), Y (typhoon / windstorm / storm surge), F (flood / river overflow), L (landslide), I (fire), V (volcano), W (snow/cold), H (heat), M (medical), N (nuclear/radiation), C (chemical), B (biological), U (infrastructure), G (supplies), S (security / armed attack / conflict / riot), K (mountain rescue), O (marine rescue), and R (RFL/safety confirmation). BRANCH is a branch number used only when multiple independent cases arise in the same minute; it does not indicate which report number it is for the same case. Report sequence is expressed separately by SEQ.

### 5-4. Correction and Overwrite Rules

Mistyped or incorrect transmissions are not deleted; they are corrected with COR.  
Keep the same case ID as the previous report, and put FIX or CANCEL at the beginning of the body.  
SCRIBE keeps the old report in the ledger and links it to the corrected version.  
When changing information from “estimated” to “confirmed,” always update the confidence as well.

### 5-5. Countering False Information, Rumors, and Fraud

During disasters, well-intentioned hearsay easily turns into misinformation. If left unchecked, it can misdirect rescue resources or trigger unnecessary evacuations, so this protocol uses codes to negate such information logically.  
**FAK:** Clearly false information, malicious rumors, or information that spreads misrecognition.  
**NEG:** A negation indicating that the event in question did not exist upon investigation.  
**SCM:** Fraud, suspicious solicitation, or illegitimate collection disguised as aid or support.  
Example: `[[ALR]] FAK-INFO : TSUNAMI RUMOR IS FALSE. CFM.` / An alert stating that a tsunami rumor is false.  
Example: `[[SIT]] MAI-Q-20260329-1430 NEG : NO TRAPPED PAX. ALL EVQ. CFM.` / A negation stating that no one is trapped and everyone has evacuated.

### 5-6. Time-to-Live (TTL) for Information

Cases without updates become stale. To prevent backlog aging, set reconfirmation deadlines by stage.  
**Phase 1:** Emergency cases must be reconfirmed or receive a responsible-unit response within 15–30 minutes.  
**Phase 2:** NEW / WIP cases with no update for more than 2 hours after occurrence should be re-queried as HLD or “reconfirmation required.”  
**Phase 3 onward:** For supply, shelter, and infrastructure cases, include the next reconfirmation time in each scheduled reporting unit.  
Cases past TTL are not deleted; they are moved into a waiting-for-update state with a timestamp.

### 5-7. Distinguishing NIL / NEG / SAFE / CLO

This protocol does not confuse NIL, NEG, SAFE, and CLO. NIL means that at the time of the report there is no additional new information to write. NEG means that an event that was suspected to have occurred was found not to exist after confirmation. SAFE means that the safety of a person, group, shelter, facility, or other target has been confirmed. CLO means that processing of the case is complete and it can be moved to closed status in the ledger.  
Therefore, NIL is not a substitute for leaving a field blank; it is used for roll calls, scheduled checks, and reports with no supplemental information. NEG is used to cancel false reports, misrecognition, or hearsay. SAFE shows the status of the subject, but if transport, handoff, or re-inquiry remain, do not mark it CLO. CLO means the entire case is complete and is not synonymous with SAFE.  
Operationally, remember: “NIL = no additional information,” “NEG = the event did not exist upon confirmation,” “SAFE = safety confirmed,” and “CLO = case completed.” Preserve these distinctions when converting to plain language for external organizations.

| Code | Meaning | Use Case | Example | Meaning of Example |
| :---: | :---: | ----- | :---: | ----- |
| NIL | No additional information / no abnormality | Roll call, periodic check, report with no supplement | SAFE ALL NIL | Safety has been confirmed and there is no additional abnormal information at this time. |
| NEG | Upon confirmation, the event does not exist | Canceling false reports, misrecognition, hearsay | NEG NO FIRE | Upon confirmation, there was no fire. |
| SAFE | Safety confirmed | Identity confirmation, shelter confirmation, facility safety confirmation | SAFE EVQ-01 | Safety has been confirmed at Shelter 1. This does not necessarily mean the case is closed. |
| CLO | Case completed | Rescue completed, distribution completed, recovery completed | CLO NIL | The case is completed and no additional action is required. |

## 6. Handoff to Municipalities, Fire Services, Medical Institutions, and Shelters

### 6-1. Principles for External Output

**[Fact]**  
ICS/NIMS emphasizes common terminology, plain language, and standardized contact formats. ICS 213 (General Message), ICS 209 (Incident Status Summary), and similar forms standardize messages, situation summaries, and resource requests. [R13][R14]

**[Adopted in This Protocol]**  
Liaison personnel must not pass Meshtastic internal codes to external organizations directly. The external handoff template should place in plain language: “location,” “time,” “what is happening,” “number of people,” “whether danger is ongoing,” “required support,” and “source evaluation.”

| Type | Example |
| :---: | ----- |
| Internal | `[[EMG]] KOB-L-20260329-1510 NEW REQ: ROD BLK BY LND. C3 1515J` |
| External Plain Language | `2026/03/29 15:15, Kobe-area case KOB-L-20260329-1510. Suspected road blockage caused by landslide. Current assessment C3. Confirmation of whether road opening is possible and a road-closure decision are required.` |
| Internal | `[[SOS]] MAI-Q-20260329-1430 NEW REQ: COL TRP 2PAX 1RED 1YLW. B2 1438J` |
| External Plain Language | `2026/03/29 14:38, building collapse with two trapped persons in the Maizuru area. Triage is one red and one yellow. Assessment B2. A rescue team and medical transport capability are required.` |

### 6-2. Minimum Information by Recipient

| Handoff Destination | Minimum Information to Align |
| :---: | ----- |
| Municipality | Shelter opening status, road conditions, isolated communities, persons requiring support, shortages of supplies, home evacuees / vehicle evacuees |
| Fire service | Fire, entrapment, rescue requests, hazardous materials, inaccessible routes, water supply / hydrant status |
| Medical | Number of injured/sick, triage colors, need for transport, receiving destination, drugs / oxygen / power, infectious disease considerations |
| Shelter | Occupancy, vulnerable persons, food / water / hygiene, power generation, communications, toilets, public health information |
| Police | Public order, traffic restrictions, suspicious objects, possible unexploded ordnance, missing-person searches, cordon information |
| EMS | Severity, need for transport, receiving destination, route to scene, oxygen / defibrillation / emergency equipment |
| Coast Guard | Tsunami / storm surge / drifting / marine distress / coastal isolation / port damage / vessels requiring rescue |
| Self-Defense Forces | Isolated communities, wide-area transport, heavy machinery / boats / helicopter support, water supply, transport, road opening |
| Administrative agencies | Evacuation information, roads / rivers / ports, supplies, welfare, public health, public information, utility restoration |

When requesting notification or dispatch to an outside organization from inside Meshtastic, use  
`REQ CAL [agency code]`.  
Agency codes are `PJSDF (Self-Defense Forces)`, `PPOLICE (Police)`, `PFIRE (Fire service)`, `PEMS (Emergency medical services)`, `PCG (Japan Coast Guard)`, and `PADMIN (other administrative agencies)`. For example, `REQ CAL PPOLICE` means a request to notify police, and `REQ CAL PFIRE` means a request to notify the fire service.

### 6-3. Standard Handoff Wording

When handing internal codes to external organizations, fix the plain-language format so the receiver can act on it immediately. As a rule, include “time,” “location,” “what happened,” “how many people,” “what is needed,” and “hazards.”  
In plain-language conversion, do not output the internal SEQ as “S03”; instead, write it in natural language such as “third report” or “fourth report.” When handing off externally, clearly state both the case ID and the report number so that the update order of the same case can be followed.

**Standard for Municipalities / Administrative Agencies**  
`YYYY/MM/DD hh:mm, at [location], [disaster / damage]. Status of [isolation / shelters / lifelines / supply shortage]. [Needed support]. [Alternate routes / danger zones / unconfirmed matters].`

**Standard for Fire / EMS / Medical**  
`YYYY/MM/DD hh:mm, at [location], [fire / collapse / number of casualties]. [Breakdown of red, yellow, green, black]. [Whether access is possible / transport route / need for receiving-facility inquiry]. [Needed equipment].`

**Standard for Self-Defense Forces / Coast Guard / Wide-Area Support**  
`YYYY/MM/DD hh:mm, in [region], [isolation / wide-area transport / port or coastal damage]. [Number of people / duration / weather]. [Required transport means]. [Reception point / hazard information].`

## 7. Handling of Missing Persons, Safety Confirmation, and Personal Information

**[Fact]**  
The PPC recognizes exceptions for third-party provision during disasters and for provision of rosters of persons requiring evacuation assistance when necessary to protect life or body and when obtaining consent is difficult, but conditions this on “the necessary extent” and “appropriate judgment.” [R8][R9][R10]

**[Adopted in This Protocol]**  
On general channels, missing-person (MISS) information is limited to: (1) case number, (2) attributes, (3) last confirmed point, (4) time, (5) direction of movement, (6) urgency, and (7) confidence. Names, kana spelling, dates of birth, medical information, and family relationships are moved to the RFL private channel or an offline ledger.

| Category | Example | Handling |
| :---: | ----- | ----- |
| Allowed | M70 / F12 / INFANT / WHEEL / PREG / JP-LOW | Rough age band, attribute, vulnerability |
| Conditional | LAST SEC-B / LAST EVQ-01 / MOV N | Last confirmed location and direction |
| Prohibited | Name / address / diagnosis / care level / My Number | Do not transmit on general operating channels |

| Minimum Steps for RFL (Missing Persons / Family Reconnection) |
| :---: |
| The RFL officer receives only the case number on the general channel; details are collected via private channel or face-to-face. |
| If inquiries are made to government agencies, hospitals, or shelters, log the contacted organization, time, responsible person, and response content. |
| Minimize destinations to which personal information is disclosed, and prohibit secondary use outside the original purpose. |

## 8. Preparedness, Training, and Audit

### 8-1. Preparedness

Long-form node names should be standardized in advance to include a base-location prefix. Examples: `SENDAI_`, `SNDI_`, `MAIZURU_`, `KOBE_`. If GPS is available, prioritize position sharing; however, even when GPS is absent, broken, or blocked indoors, the base-location prefix should function as minimum geographic information.

| Item | Minimum Requirement |
| :---: | ----- |
| Equipment | Put model, certification number, firmware version, backup power, and antenna configuration into the ledger |
| Personnel | Set backup personnel for NETCTL / OPS / LOG / Liaison / SCRIBE / RFL |
| Forms | Print and distribute simplified local equivalents of ICS 205 / 209 / 213 / 214 |
| Contacts | Prepare contact lists for municipalities, fire headquarters, disaster base hospitals, shelter headquarters, and councils of social welfare |
| Training | Initial disaster response, entrapment, shelter supplies, missing-person inquiry, and handoff drills |

### 8-2. Training Drills

**Drill 1: Initial Control in the First 30 Minutes After an Earthquake**

**Scenario**  
Thirty minutes after impact, Phase 1. Aftershocks continue. Collapse, fire, and blackout are mixed together. Multiple nodes can transmit simultaneously, so congestion is likely.

**Tasks**  
Use only [[SOS]] and [[EMG]]. Stop chatter, long messages, unconfirmed information, and supply discussions. Assign case IDs and transmit location, number of people, danger level, and triage in the shortest possible form.

**Success Conditions**  
For five minutes, zero unauthorized transmissions. Every emergency case has a case ID, location, number of people, and danger level, and no ACK or WIP is missing.

**Drill 2: Handoff for Road Blockage + Shelter Overload**

**Scenario**  
After heavy rain, two main roads are impassable, shelters are over capacity, and water and hygiene supplies are short. The field uses coded operations, but the municipality requires plain-language receipt.

**Tasks**  
LOG and Liaison organize road conditions, alternate routes, remaining shelter capacity, supply shortages, and urgency, then expand coded reports into plain-language text for the municipality.

**Success Conditions**  
Within 10 minutes, produce a plain-language report of 200 characters or fewer with no missing required items and hand it off accurately to the municipality.

**Drill 3: Missing-Person Cross-Checking and Personal Information Protection**

**Scenario**  
Shelter rosters, hospital transport rosters, and family inquiries do not match. Families are repeatedly making name-based inquiries, creating pressure to write details on the general channel.

**Tasks**  
On the general channel, share only the case number, attributes, and last confirmed point. Separate names, addresses, and contact details into RFL and individual inquiries. Keep a history of cross-check work.

**Success Conditions**  
Zero transmission of personal information on the general channel. Cross-check targets, results, and pending cases can be uniquely tracked.

**Drill 4: Base-Station Failure and Switching to a Reserve Station**

**Scenario**  
The CLIENT_BASE stops, power at the base station is unstable, and the gateway station stops responding. A reserve station is on standby and immediate handover is required.

**Tasks**  
Activate backup power, switch the role to the reserve station, inherit group settings, scheduled broadcasts, audit records, and case ledgers, and notify field nodes that the switch is complete.

**Success Conditions**  
Control resumes within 15 minutes, with no missing case-ledger entries and no dual control. Field nodes recognize the new base station.

**Drill 5: DMAT Reception and Medical Transport Control**

**Scenario**  
Hospital function is degraded, and many casualties are arising from shelters and aid stations. DMAT arrival is expected, but receiving destinations and transport order are not yet organized.

**Tasks**  
Aggregate casualties by red / yellow / green / black, organize transport priority, receiving-facility inquiries, transport routes, and drug/oxygen demand. Re-triage if necessary.

**Success Conditions**  
Severe patients are organized by priority order; receiving-facility inquiry results and destinations are explicit; re-triage updates can be traced.

### 8-3. Pre-Transmission Check

The more urgent the situation, the more effective it is to fix the pre-send check items. Confirm the following six points before transmitting.

1. **Who is affected and what is happening:** Be specific about number of people, injuries, isolation, fire, entrapment, and so on.  
2. **Where:** Reinforce location in order of GPS, base-location prefix, facility name, area name, and last confirmed point.  
3. **What is needed:** Clearly state rescue, transport, notification, supplies, road opening, receiving-facility inquiry, etc.  
4. **What confidence level:** Attach A1–E5 or a simplified evaluation, and explicitly state if information is unconfirmed.  
5. **What must not be included:** Do not include unnecessary personal information, information that may induce attack, unconfirmed death counts, or chatter.  
6. **Where to send it:** Choose among a general channel, group, individual inquiry, or external handoff destination, and prevent over-transmission.

## 9. Disaster-Specific Operations

### 9-1. Earthquakes, Aftershocks, and Collapse

**[Operational Purpose]**  
Give top priority to collapse, entrapment, fire, re-search under aftershocks, hospital-function degradation, and wide-area blackouts. For coastal earthquakes with tsunami potential according to JMA, immediately shift to the tsunami chapter. [R15]

**Priority Matters**  
Immediate registration of COL/TRP / aggregation by RED/YLW/GRN/BLK / confirmation of hospitals, DMAT, and transport routes

**Transmission Restrictions**  
Chatter about damage scale / circulation of death counts before confirmation / unplanned re-entry instructions under aftershocks

**Liaison Destinations**  
Fire services / municipal disaster headquarters / disaster base hospitals / DMAT contact points

**Recommended Canned Messages**  
`[[SOS]] EQ COL TRP / [[EMG]] HOSP PARTIAL OUT / [[SIT]] ROAD PASSABLE?`

**Syntax Example**  
`[[SOS]] MAI-Q-20260329-1430 NEW REQ: COL TRP 2PAX 1RED 1YLW. B2 1438J`  
Meaning: In the Maizuru earthquake case, two people are trapped in a collapsed building and rescue is needed for one red and one yellow casualty.

**Field View**  
Earthquakes generate large volumes of information, but the first things needed are “how many,” “where,” and “how dangerous.” Before building names or photo descriptions, send the number of people, triage, and route obstruction.

### 9-2. Tsunami

**[Operational Purpose]**  
JMA requires immediate evacuation to high ground or evacuation buildings in coastal and river-side areas, and continued evacuation until warnings are lifted. Tsunami waves can repeat for long periods and become locally higher. [R15]

**Priority Matters**  
Immediate evacuation / leaving coasts, river mouths, and lowlands / confirmation of evacuation completion / recording last-seen points of isolated persons

**Transmission Restrictions**  
Remaining on the coast for observation / recommending “wait and see” / guidance to return before warning cancellation

**Liaison Destinations**  
Municipalities / fire services / Coast Guard and port managers / shelter headquarters

**Recommended Canned Messages**  
`[[ALR]] TSU EVAC NOW / [[SIT]] EVQ-01 FULL / [[EMG]] COAST ISO`

**Syntax Example**  
`[[ALR]] KES-T-20260329-1450 NEW EVQ: TSU EVAC NOW SEC-A/B/C. A1 1451J`  
Meaning: In the Kesennuma tsunami case, issue an A1-rated alert to evacuate Sectors A/B/C immediately.

**Field View**  
In a tsunami, it matters more whether anyone is still left behind than whether evacuation is “complete.” Do not guide return before official cancellation; prioritize confirmation of continued evacuation.

### 9-3. Heavy Rain and Linear Rainbands

**[Operational Purpose]**  
A linear rainband indicates a phase in which danger rises rapidly, requiring immediate appropriate evacuation behavior in dangerous areas. Meshtastic should remain only a supplementary line for quickly sharing underground-space danger, lowlands, cliff-adjacent zones, road inundation, and signs of isolation. [R32]

**Priority Matters**  
Securing high ground / avoiding underground spaces and underpasses / leaving cliffs and ravines / duration of ongoing rain / signs of becoming isolated

**Transmission Restrictions**  
Long narration of rain-cloud conditions / recommending patrols of rivers and irrigation channels / re-approach to dangerous zones / unconfirmed instructions to walk through hazards

**Liaison Destinations**  
Municipalities / fire services / shelters / river managers / road managers

**Recommended Canned Messages**  
`[[ALR]] RAIN BAND / [[EMG]] UG NO / [[SIT]] STAY HIGH`

**Syntax Example**  
`[[ALR]] FUK-P-20260329-1810 NEW RPT: RAIN BAND STAY HIGH UG NO. A2 1812J`  
Meaning: In the Fukuoka heavy-rain case, intense rain associated with a linear rainband is continuing; avoid underground spaces and prioritize reaching high ground.

**Field View**  
In heavy rain, it matters less whether it is raining right now than whether people will be unable to leave dangerous places in the next 10–30 minutes. Send the shortest possible instruction to avoid underground spaces, lowlands, and the base of cliffs.

### 9-4. Typhoon, Windstorm, and Storm Surge

**[Operational Purpose]**  
During typhoons, not only rain but also strong winds, storm surge, blackout, communications outage, and the degraded function of bridges, ports, and coastal lowlands can occur simultaneously. JMA indicates that evacuation should be completed before violent winds begin, and that water level in storm surges can rise sharply in a short time. [R33][R34]

**Priority Matters**  
Evacuation before severe winds / storm-surge inundation zones / blackout and water outage / ports, bridges, coastal roads / shelter capacity

**Transmission Restrictions**  
Recommending unnecessary outdoor movement after approach / sightseeing at the coast / unconfirmed bridge-passable information / long narration of wind/rain conditions

**Liaison Destinations**  
Municipalities / fire services / port managers / road managers / shelters

**Recommended Canned Messages**  
`[[ALR]] TYPHOON EVQ / [[ALR]] SURGE EVQ / [[SIT]] WIND STRG`

**Syntax Example**  
`[[ALR]] KOB-Y-20260329-0600 NEW RPT: TYPHOON SURGE EVQ SEC-A/B. A2 0605J`  
Meaning: In the Kobe typhoon case, evacuation should begin for Sectors A/B where storm-surge inundation is expected.

**Field View**  
For typhoons, not just rainfall but the rate at which winds intensify and the timing of storm surge matter. Once strong winds start, evacuation becomes dangerous, so send first “by when evacuation must be completed.”

### 9-5. Flood and River Overflow

**[Operational Purpose]**  
JMA designated-river flood forecasts and flood warnings are conveyed to municipalities as references for flood-fighting and evacuation behavior. Meshtastic should focus on reporting road inundation, isolation, overtopping, blackout, and shelter pressure as a supplementary line. [R16]

**Priority Matters**  
Road passability / rough water depth / isolated households / pumps, power generation, drinking water

**Transmission Restrictions**  
Simple “looks okay” messages / unconfirmed passable-route information / long late-night narration

**Liaison Destinations**  
Municipalities / fire services / shelters / river managers

**Recommended Canned Messages**  
`[[SIT]] FLD ROD BLK / [[EMG]] EVQ NEED BUS / [[ALR]] RIVER RISE`

**Syntax Example**  
`[[SIT]] KOB-F-20260329-2305 NEW RPT: FLD DEPTH 30-50CM ROD BLK. B3 2310J`  
Meaning: In the Kobe flood case, there is 30–50 cm of flooding and roads are blocked.

**Field View**  
In floods, if you align the four points of water depth, road blockage, isolation, and shelter pressure, municipalities can act more easily. Do not state passability as certain while still unconfirmed.

### 9-6. Landslides

**[Operational Purpose]**  
A landslide alert indicates that life-threatening landslides may occur at any time and supports mayors’ evacuation decisions and residents’ self-evacuation decisions; it corresponds to Warning Level 4. [R17]

**Priority Matters**  
Withdrawal from slope-adjacent areas / road blockage / isolated communities / suppression of night patrols

**Transmission Restrictions**  
Waiting below slopes / unnecessary re-checks in the rain / unconfirmed instructions to walk through

**Liaison Destinations**  
Municipalities / fire services / road managers / shelters

**Recommended Canned Messages**  
`[[ALR]] LND EVAC / [[SIT]] ROD BLK BY LND / [[EMG]] ISO HAMLET`

**Syntax Example**  
`[[EMG]] KOB-L-20260329-0310 NEW REQ: ROD BLK BY LND ISO 12HH. C3 0318J`  
Meaning: In the Kobe landslide case, roads are blocked by landslide and 12 households are isolated, so support is required.

**Field View**  
With landslides, re-approaching the scene itself is dangerous. Prioritize sharing the scale of isolation and lack of access over reconfirming the headcount.

### 9-7. Urban Fire, Fire Spread, and Wildland Fire

**[Operational Purpose]**  
For fires, wind direction, direction of spread, inaccessible routes, hazardous materials, water supply, and evacuation guidance are important. On Meshtastic, rather than long fire-scene narration, send route, blockage, retreat destination, and support requests concisely.

**Priority Matters**  
Fire compartment / direction of spread / evacuation zone / firefighting water sources and roads

**Transmission Restrictions**  
Long photo-based narration / certainty about hazardous materials without confirmation / curiosity-driven concentration at the scene

**Liaison Destinations**  
Fire services / municipalities / shelters / hospitals

**Recommended Canned Messages**  
`[[EMG]] FIRE MOV E / [[ALR]] EVAC ZONE-C / [[SIT]] HYD OK?`

**Syntax Example**  
`[[EMG]] OSA-I-20260329-1940 NEW RPT: FIRE MOV E WIND STRG EVQ Z-C. B2 1944J`  
Meaning: In the Osaka fire case, strong winds are pushing the fire eastward and evacuation guidance is needed for Zone C.

**Field View**  
In fire operations, it is more practical to convey the direction of spread, evacuation zones, blocked routes, and needed equipment than to narrate the fire scene in detail.

### 9-8. Volcano

**[Operational Purpose]**  
JMA’s eruption alert level is an indicator showing the area requiring caution and the disaster-prevention response to be taken in five stages. Meshtastic handles ashfall, roads, visibility, respiratory symptoms, and evacuation routes as auxiliary information. [R18]

**Priority Matters**  
Ashfall and visibility / no approach toward the crater / evacuation routes / masks, water, vehicle protection

**Transmission Restrictions**  
Independent interpretation of alert levels / recommending approach to craters / social-media-style live commentary

**Liaison Destinations**  
Municipalities / fire services / medical institutions / shelters

**Recommended Canned Messages**  
`[[ALR]] VOL LV UP / [[SIT]] ASH HVY / [[EMG]] EVQ ROUTE X`

**Syntax Example**  
`[[ALR]] KAG-V-20260329-0810 NEW RPT: ASH HVY VIS LOW EVQ RT-B. B2 0814J`  
Meaning: In the Kagoshima volcano case, ashfall is heavy and visibility is reduced, requiring confirmation of Evacuation Route B.

**Field View**  
With volcanoes, ashfall volume and reduced visibility directly affect traffic, respiratory health, and evacuation routes. Prioritize the effects on daily life and movement over narrating the eruption itself.

### 9-9. Heavy Snow and Cold Waves

**[Operational Purpose]**  
In snow disasters, prioritize isolation, blackout, fuel, dialysis, home oxygen, heating, and snow-clearing routes. Keep vehicle evacuation and sheltering at home in mind.

**Priority Matters**  
Snow-clearing feasibility / fuel / heating / medically dependent persons / isolation

**Transmission Restrictions**  
Redundant snowfall narration / unconfirmed passable routes / stay-put instructions without knowing available heating means

**Liaison Destinations**  
Municipalities / road managers / medical institutions / welfare shelters

**Recommended Canned Messages**  
`[[SIT]] SNW ISO / [[EMG]] FUEL LOW / [[EMG]] O2 NEED PWR`

**Syntax Example**  
`[[EMG]] NII-W-20260329-0630 NEW REQ: ISO 4HH FUEL LOW PWR OUT. B2 0638J`  
Meaning: In the Niigata snow/cold case, four households are isolated and both fuel shortage and blackout are occurring, so support is needed.

**Field View**  
Snow/cold disasters often worsen quietly. Understand isolation, fuel, electricity, dialysis, and home oxygen together rather than treating them as separate cases.

### 9-10. Combined Extreme Heat, Blackout, and Communications Failure

**[Operational Purpose]**  
When blackout and heat overlap, heatstroke, water supply, cooling, medical-device power, and communications failure progress simultaneously. This is treated as its own chapter because it can cause fatalities even without a conspicuous disaster.

**Priority Matters**  
Water supply / power generation / cooling centers / home medical care / communications restoration

**Transmission Restrictions**  
Downplaying with “no immediate danger to life” / overconfidence in private generators / recommending transport without confirmed hospital acceptance

**Liaison Destinations**  
Municipalities / medical institutions / shelters / power and telecommunications provider contacts

**Recommended Canned Messages**  
`[[EMG]] HEAT NEED COOL / [[SIT]] CELL OUT / [[EMG]] PWR FOR O2`

**Syntax Example**  
`[[EMG]] TKY-H-20260329-1230 NEW REQ: HEAT 6PAX O2 1 PWR NEED. B2 1235J`  
Meaning: In the Tokyo heat-combination case, there are six people at heat risk and one home-oxygen user, and power support is needed.

**Field View**  
In heat + blackout situations, clearly state which of cooling, water, power, and communications is unavailable. Not only symptom severity but also duration and the presence or absence of support means are important.

### 9-11. Infectious Disease Outbreak and Medical System Strain

**[Operational Purpose]**  
Meshtastic is not suitable for transmitting individual clinical information. Its use is limited to supplementary sharing of facility status, PPE, exposure of unspecified large groups, and transport / reception coordination.

**Priority Matters**  
Bed/outpatient saturation / PPE / zoning / receiving-facility inquiry

**Transmission Restrictions**  
Circulation of individual patient records / sending individual diagnoses on general channels / guidance to unconfirmed receiving facilities

**Liaison Destinations**  
Public health centers and municipalities / hospitals / shelters / welfare facilities

**Recommended Canned Messages**  
`[[SIT]] HOSP SAT / [[EMG]] PPE LOW / [[ADM]] FEVER SHELTER SEP`

**Syntax Example**  
`[[SIT]] KOB-M-20260329-0940 NEW RPT: HOSP SAT PPE LOW TRIAGE EXT. B2 0946J`  
Meaning: In the Kobe medical strain case, hospital reception is saturated, protective gear is short, and expanded external triage is needed.

**Field View**  
In outbreaks or medical strain, do not send individual medical information; concisely share facility function, isolation, PPE, and reception capacity.

### 9-12. Nuclear Disaster (Nuclear Facility Accident, etc.)

**[Operational Purpose]**  
In a nuclear disaster, instead of fine-grained debate about radiation dose, align and share shelter-in-place, sealing, stopping ventilation, evacuation instructions, facility name, and direction. Meshtastic is used to synchronize protective actions; it does not replace expert judgment on measurement. [R27][R28]

**Priority Matters**  
Shelter indoors / sealing / stopping ventilation / instructed zones / direction / facility name / traffic restrictions

**Transmission Restrictions**  
Independent interpretation of dose values / guidance to unconfirmed evacuation routes / recommending unnecessary outdoor movement / rumor-style spread

**Liaison Destinations**  
Municipalities / nuclear disaster officials / fire services / hospitals / shelters

**Recommended Canned Messages**  
`[[ALR]] RAD SEAL NOW / [[EMG]] HVAC OFF / [[SIT]] EVQ INDOOR STAY`

**Syntax Example**  
`[[ALR]] FKS-N-20260329-0910 NEW RPT: RAD ALR SEAL. A2 0912J`  
Meaning: In the Fukushima nuclear case, a nuclear emergency is suspected and shelter-in-place with sealing should begin.

**Field View**  
In nuclear disasters, before explaining technical terms, synchronize the four actions: go indoors, seal, stop ventilation, and wait for official instructions.

### 9-13. Chemical Disaster and Chemical Weapons Incident

**[Operational Purpose]**  
In chemical incidents, rather than identifying the substance immediately, align odor, irritant symptoms, wind direction, indoor/outdoor status, and need for decontamination. In the initial phase, describe it as “suspected chemical incident” and synchronize protective behavior. [R29][R30]

**Priority Matters**  
Odor / eye irritation and breathing difficulty / wind direction / move upwind or crosswind / cover mouth and nose / decontamination route

**Transmission Restrictions**  
Speculative identification of chemical name / re-approach to contaminated zones / bringing undecontaminated persons into shelters / curiosity-driven approach

**Liaison Destinations**  
Fire services / municipalities / EMS / hospitals / police

**Recommended Canned Messages**  
`[[ALR]] CHEM MASK ON / [[EMG]] DECON NEED / [[SIT]] ODR STRONG`

**Syntax Example**  
`[[ALR]] OSA-C-20260329-1012 NEW RPT: CHEM ODR MASK. B2 1014J`  
Meaning: In the Osaka chemical case, a chemical incident with unusual odor is suspected and mouth/nose protection has been started.

**Field View**  
In chemical incidents, the key is not proving the cause first, but being able to decide quickly whether it is safe to remain there, whether to move upwind, and whether decontamination is required.

### 9-14. Biological Disaster and Biological Weapons Incident

**[Operational Purpose]**  
In biological incidents, rather than writing long symptom descriptions, briefly align cluster nature, need for isolation, shortage of PPE, and need for receiving-facility coordination. Meshtastic is limited to support for facility operations rather than transmission of individual patient data. [R29][R30]

**Priority Matters**  
Cluster fever / cough and respiratory symptoms / isolation route / PPE / receiving capacity / exposure history

**Transmission Restrictions**  
Sending individual diagnoses on general channels / identifying the pathogen without confirmation / distributing patient lists / guiding to unconfirmed receiving facilities

**Liaison Destinations**  
Public health centers / municipalities / hospitals / shelters / welfare facilities

**Recommended Canned Messages**  
`[[ALR]] BIO ISO NOW / [[EMG]] PPE LOW / [[SIT]] HOSP SAT`

**Syntax Example**  
`[[ALR]] KYO-B-20260329-1030 NEW RPT: BIO FEV CLU ISO. B2 1034J`  
Meaning: In the Kyoto biological case, there is a fever cluster and isolation has begun under suspicion of a biological incident.

**Field View**  
In biological incidents, it is not necessary to identify the disease first. Prioritize whether isolation is needed, whether PPE is sufficient, and whether there is somewhere to receive patients.

### 9-15. Mountain Rescue / Mountain Distress

**[Operational Purpose]**  
Mountain distress is treated as a small-scale, point-like, highly mobile case type. Meshtastic is used as a local mesh among field teams, ridgeline relays, and trailhead base stations, not as a substitute for wide-area public networks, but to share position, medical condition, and accessibility.

**[Fact]**  
In mountain rescue practice, VHF/LMR/amateur radio are used for communications among field teams, command posts, and helicopters. Meshtastic official materials also explicitly mention use in search and rescue operations.

**Priority Matters**  
Trailhead / route / last confirmed point / altitude band / number of people / severity / ability to move / worsening weather / daylight remaining / helicopter feasibility

**Transmission Restrictions**  
Speculative route identification / unnecessary long live commentary / personal information on general channels / definite wording for unconfirmed positions

**Liaison Destinations**  
Fire services / municipalities / mountain rescue organizations / hospitals / police

**Recommended Canned Messages**  
`[[SOS]] LOST 1PAX / [[EMG]] FALL 1RED / [[SIT]] LAST RIDG ALT1800 / [[ADM]] LZ PRB`

**Syntax Example**  
`[[SOS]] HAK-K-20260420-1510 S01 NEW REQ: LOST 2PAX LAST RIDG-3 ALT1800. PRB 1514J`  
Meaning: In the Hokkaido mountain-distress case, two people are likely lost; they were last confirmed near Ridge 3 in the 1,800 m altitude band.

**Field View**  
In mountain rescue, prioritize the five points of location, number of people, ability to move, weather, and approaching nightfall. Even without a map, trailhead, ridge, gully, and altitude band are enough for rescuers to decide the initial search direction.

### 9-16. Marine Distress

**[Operational Purpose]**  
Meshtastic is not treated as a primary distress channel in marine incidents. The primary system is marine VHF, DSC, EPIRB, and GMDSS; Meshtastic is limited to supplementary communication among ports, coastal areas, shore-based headquarters, support vessels, and land search teams.

**[Fact]**  
The USCG officially guides DSC Distress and VHF Channel 16, NOAA positions EPIRB as a maritime distress beacon, and amateur-radio nets have been reported as a crucial link in at-sea rescues.

**Priority Matters**  
Vessel type / POB / whether anyone is overboard / flooding, capsize, drift, grounding / loss of propulsion / life jackets, life raft, DSC, EPIRB status / latitude-longitude or relative position / wind and sea conditions

**Transmission Restrictions**  
Speculative position statements / assuming offshore distress can be handled by Meshtastic alone / unnecessary chatter / operations that ignore official distress channels

**Liaison Destinations**  
Japan Coast Guard / municipalities / fire services / EMS / port managers

**Recommended Canned Messages**  
`[[SOS]] TAKW ADRF 3POB / [[EMG]] PFD ALL DSC DONE / [[ADM]] CAL PCG / [[SIT]] EPIR ON`

**Syntax Example**  
`[[SOS]] KOB-O-20260708-0635 S01 NEW REQ: TAKW ADRF 3PAX 1CHILD. CFM 0638J`  
Meaning: In the Kobe marine-distress case, a small vessel with three persons aboard, including one child, is flooding and drifting.

**Field View**  
In marine distress, prioritize the six points of location, number of people, flooding, man overboard, lifesaving actions, and status of official distress calls. Do not confuse Meshtastic as nearshore supplemental communications with the primary distress channel.

## 10. Operations During Armed Attack, Conflict, Riot, Missile Attack, and Nuclear Attack

**[Fact]**  
The Government of Japan states that when a ballistic missile may be incoming to Japan, urgent information will be transmitted via J-Alert and other means, and people should evacuate to nearby sturdy buildings or underground; indoors, move away from windows and into a room without windows if possible; and if impact occurs nearby, take action to avoid blast, fragments, and hazardous materials. [R20][R21][R22]

**[Fact]**  
The ICRC states that the use of explosive weapons in populated areas tends to cause long-term damage to civilians and to systems essential to survival such as power, water, healthcare, and communications. [R23]

**[Adopted in This Protocol]**  
During armed attack or conflict, Meshtastic is limited to: (1) receiving warnings, (2) transmitting retreat and shelter-in-place instructions, (3) short reports of explosions, fires, collapse, blackout, water outage, and medical demand, (4) reporting discovery of UXO and other dangerous objects, and (5) handoff to municipalities, fire services, and medical institutions. It is not used to narrate the positions or movements of attackers, security forces, or relief forces.

### 10-1. Priorities

The highest priority is protective evacuation behavior; communications are limited to short reports that do not interfere with evacuation.  
Position sharing should be minimized, and public channels or internet publication should be avoided.  
Create cases in the order of explosion, fire, collapse, medical demand, and UXO, and send even shorter messages than in peacetime.

### 10-2. Phases

| Phase | Main Focus | Communications Discipline |
| :---: | :---: | ----- |
| Phase A | ALERT | Receive J-Alert, begin withdrawal; only [[ALR]] and [[ADM]] |
| Phase B | IMPACT | Immediately after impact / explosion / fighting. Prioritize [[SOS]] / [[EMG]]. No live narration or chatter |
| Phase C | STABILIZE | Organize fire, collapse, blackout, aid, and sheltering. Handoff to public institutions |
| Phase D | RISK RESIDUAL | Manage UXO, closures, movement restrictions, intermittent attacks, and nighttime blackout |

### 10-3. During a Ballistic Missile Warning

If outdoors, evacuate to a nearby sturdy building or underground. If no building is available, take cover behind something or lie on the ground and protect your head.  
If indoors, move away from windows and, if possible, into a room without windows.  
After a nearby impact, if there is smoke, unusual odor, or suspected hazardous materials, cover your mouth and nose and leave the area; if indoors, stop ventilation, close windows, and seal the room.

### 10-4. Nuclear Weapon Attack

A nuclear weapon attack is treated as a situation with two elements layered together: damage from blast, thermal radiation, and overpressure due to a missile or other explosion, and subsequent contamination by radioactive fallout. Therefore, this protocol treats it separately from an ordinary nuclear disaster such as a nuclear facility accident.  
At the initial stage, prioritize protecting oneself from flash, blast, collapse, and fire. After that, to avoid radioactive fallout, prioritize moving underground or indoors, closing windows, stopping ventilation, checking whether decontamination is needed, and waiting for official instructions. Do not recommend casual outdoor movement or wide-area evacuation via unconfirmed routes.  
In short reports, briefly share whether there was an explosion, fire, collapse, number of injured, start of sheltering indoors, suspected fallout, and decontamination need. Combine MSL, BLAST, SHEL, RAD, and DECON as necessary. When handing off to outside organizations, convert the case ID, report number, location, time, number of casualties, and status of protective actions into plain language.

### 10-5. During Urban Combat, Gunfire, Explosions, and Destruction

Briefly report gunfire, explosion sounds, secondary fires, flying glass, and whether collapse has occurred. Long live narration of the scene is prohibited.  
For routes, send only “passable / blocked / dangerous area.” Do not transmit the detailed positions of field units, security perimeters, or checkpoints on general channels.  
At night and during blackout, the risks of using lights, moving outdoors, and mass concentration increase. Send only necessary times, places, and headcounts.

### 10-6. During Riots and Civil Disturbance

During riots, fires, looting, stone-throwing, barricades, crowd inflow, and route changes due to police response can shift rapidly. On Meshtastic, avoid long scene narration while briefly reporting route usability, whether there is fire, whether there are injuries, and in which direction the dangerous zone is expanding.  
On general channels, do not circulate detailed crowd headcount estimates, identify ringleaders, repost photos, or transmit unconfirmed social-media information. What matters is “where is dangerous,” “which routes are unusable,” “whether there is fire or injury,” and “whether external notification is needed.”  
Use the following body codes: RIT (riot outbreak), MOB (crowd concentration), LOOT (looting), ARSN (arson), STON (stone-throwing), and BARR (barricade/obstacle). When handing off to outside organizations, convert location, dangerous acts, rough crowd size, presence of fire or injuries, and no-approach zones into plain language.

### 10-7. UXO and Suspicious Explosive Objects

Do not touch them, do not approach them, and do not move them. On Meshtastic, share only the danger zone briefly, such as “UXO FOUND” or “STAY OUT.”  
On public channels, location should be limited to a rough sector or block; precise location is handled on the handoff side for municipalities, police, fire services, Self-Defense Forces, and others.

### 10-8. Transmission Restrictions

Do not narrate the exact deployment or movement of the Self-Defense Forces, police, fire services, EMS, or shelters.  
Do not transmit videos, photos, or reposts from social media within Meshtastic message bodies.  
Do not state as fact unconfirmed information about enemy/friendly forces, battle results, casualty counts, or suspects.  
Do not instruct anyone to recover or touch suspected UXO.

### 10-9. Recommended Short Codes

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | :---: | :---: | ----- | ----- |
| MSL | Missile alert / possible impact | J-Alert or possible incoming strike | `[[ALR]] TKY-S-20260329-1015 NEW ALR: MSL ALERT UG NOW. A1 1015J` | Tokyo armed-attack case. A missile warning has been received; begin underground evacuation immediately. |
| BLAST | Explosion / blast wave | Initial report of explosion, fire, collapse | `[[EMG]] OSA-S-20260329-1022 NEW RPT: BLAST FIRE COL 5PAX. B2 1024J` | Osaka armed-attack case. An explosion caused fire and collapse and affected at least five people. |
| SHEL | Shelter indoors / underground | Sheltering, staying indoors | `[[ADM]] KOB-S-20260329-1030 ACK EVQ: SHEL UG STAY. A1 1031J` | Kobe case. Underground sheltering is being continued. |
| UXO | Unexploded ordnance / suspicious explosive object | Hazard notification when discovered | `[[SIT]] KOB-S-20260329-1105 NEW RPT: UXO FOUND SEC-D STAY OUT. B2 1107J` | A UXO-like object was found in Sector D and entry is prohibited. |
| CURF | Movement restriction | Curfew, closure, checkpoint | `[[ADM]] TKY-S-20260329-1900 NEW RPT: CURF HOLD CHKP N. A1 1902J` | Hold position because of a northern checkpoint/closure. |
| RIT | Riot outbreak | Civil disturbance; prioritize safety | `[[ALR]] SND-S-20260329-1840 NEW RPT: RIT MOB SEC-C AVOID. B2 1842J` | Avoid approach because riots and crowd concentration are occurring in Sector C. |
| MOB | Crowd concentration | Crowd build-up / inflow | `[[SIT]] SND-S-20260329-1843 S02 RPT: MOB GATE-W GROW. C3 1845J` | Second report: crowd concentration near West Gate is growing. |
| LOOT | Looting | Looting of shops, warehouses, etc. | `[[EMG]] SND-S-20260329-1850 NEW CAL: LOOT SHOP-A POL. B2 1851J` | Looting is occurring at Shop A and police notification is required. |
| ARSN | Arson | Intentional fire / spread risk | `[[EMG]] SND-S-20260329-1854 NEW RPT: ARSN FIRE BLDG-2. B2 1856J` | A fire caused by arson is occurring in Building 2. |
| STON | Stone-throwing | Hazard from thrown objects | `[[ALR]] SND-S-20260329-1858 NEW RPT: STON ROAD-N AVOID. C3 1900J` | Stone-throwing on the north road requires route avoidance. |
| BARR | Barricade / obstacle | Road closure / improvised obstacle | `[[SIT]] SND-S-20260329-1902 NEW RPT: BARR INT-A BLK. B2 1904J` | A barricade has been built at Intersection A and it is impassable. |

## 11. Case Ledger / Short-Message Management Software
(In Progress)

## 12. Proposed Canned Message Implementation

**[Fact]**  
Meshtastic’s canned-message module can send pre-defined messages from a single device; message groups are pipe-separated (`|`), and the total byte count is limited to 200 bytes. [R19]

**[Adopted in This Protocol]**  
Canned messages are not a “full-text transmission device.” They are limited to initial reports, acknowledgments, and standardized requests. Switch presets by phase and role.

| Preset | String (can be set as-is) | Approx. Bytes |
| :---: | ----- | :---: |
| Pack-A Initial Response | \[\[SOS\]\] NEED MED</br>\[\[EMG\]\] FIRE MOV</br>\[\[ALR\]\] EVAC NOW</br>\[\[SIT\]\] ROD BLK</br>\[\[ADM\]\] RC ALL | 92 |
| Pack-B Supplies | \[\[EMG\]\] WAT NEED</br>\[\[EMG\]\] FOO NEED</br>\[\[EMG\]\] PWR NEED</br>\[\[SIT\]\] ISO</br>\[\[ADM\]\] LOG QSL | 91 |
| Pack-C Medical | \[\[SOS\]\] 1RED</br>\[\[EMG\]\] HOSP SAT</br>\[\[EMG\]\] O2 NEED</br>\[\[SIT\]\] 3YLW</br>\[\[ADM\]\] MED QSL | 92 |
| Pack-D RFL | \[\[ADM\]\] MISS NEW</br>\[\[ADM\]\] SAFE CONF</br>\[\[ADM\]\] RFL CH3</br>\[\[SIT\]\] EVQ FULL</br>\[\[ADM\]\] QSL | 92 |
| Pack-E Armed Attack | \[\[ALR\]\] MSL ALERT</br>\[\[ADM\]\] SHEL UG NOW</br>\[\[EMG\]\] BLAST FIRE</br>\[\[SIT\]\] UXO FOUND</br>\[\[ADM\]\] CURF HOLD | 100 |

| Rules for Canned Message Implementation |
| ----- |
| All information for a case cannot be sent in one shot. Use canned messages only as the starting point for an initial report, after which the receiver adds a case ID and proceeds to full transmission. |
| Packs for untrained users may include short plain-language phrases, but must not exceed the 200-byte limit. |
| Bell characters can become noise at night or in shelters, so enable them only on base stations. |

## 13. Lessons from the Great East Japan Earthquake and Radio Operations

**[Fact]**  
During the Great East Japan Earthquake, JARL collected information, procured and deployed transceivers for emergency communications support in affected areas, and carried out and supported emergency communications. JARL also maintains basic policies, implementation procedures, and an emergency communications manual in peacetime. [R24][R25][R26]

**[Reflected in This Protocol]**  
This protocol repositions into the Meshtastic era the practical items discussed in conversation: “rescue requests from isolated areas,” “relays linking shelters and headquarters,” “equipment and technical support to municipalities,” “immediate sharing of lifeline information,” “use of multiple bands and multiple paths,” “power self-sufficiency,” and “position identification through coordinates.”

### 13-1. Lessons Adopted

Transmission of rescue requests and safety information is a priority task in disasters.  
It is important to link shelters, headquarters, hospitals, and material depots with short case numbers.  
Lifeline information directly affects resident behavior, so short reports on water supply, fuel, roads, and business operations are highly valuable.  
The layered communications model of HF/VHF/UHF should be reinterpreted today as Meshtastic / cellular / satellite / municipal disaster-radio multi-layering.  
Power plans that work during blackouts, as well as simple maps, grid codes, and offline materials, are important.

### 13-2. Implementation in This Protocol

| Lesson | Implementation in This Protocol | Notes |
| :---: | ----- | ----- |
| Early external relay of SOS | Prioritize [[SOS]] and case IDs; Liaison converts them into plain language for fire services and municipalities | Codes first in the initial phase; plain language externally |
| Shelter-focused communications | Fix EVQ codes and shelter numbers | Fix node names by shelter as well |
| Immediate sharing of lifeline information | Register PWR/WAT/ROD/OUT in canned messages | Daily-life information is released from Phase 3 |
| Wide-area relay | CLIENT_BASE + private channels + external handoff role | Avoid excessive ROUTER/REPEATER use |
| Position identification methods | Area codes, 100 m grids, landmarks | Backup when GPS is unavailable |

## 14. References

[R1] Meshtastic, Configuration Tips. https://meshtastic.org/docs/configuration/tips/  
[R2] Meshtastic, Device Configuration. https://meshtastic.org/docs/configuration/radio/device/  
[R3] Meshtastic, Position Configuration. https://meshtastic.org/docs/configuration/radio/position/  
[R4] Meshtastic, MQTT Module Configuration. https://meshtastic.org/docs/configuration/module/mqtt/  
[R5] ARIB, Overview of Standard Specification (STD-T108). https://www.arib.or.jp/kikaku/kikaku_tushin/desc/std-t108.html  
[R6] TELEC, Inquiries Regarding Technical Conformity Certification and Construction Design Certification. https://www.telec.or.jp/faq/  
[R7] JATE, About the Conformity Certification System for Specified Radio Equipment. https://www.jate.or.jp/jp/radio/about-r.html  
[R8] Personal Information Protection Commission, FAQ Q7-21 Sharing Personal Information in Emergencies Such as Large-Scale Disasters. https://www.ppc.go.jp/all_faq_index/faq1-q7-21/  
[R9] Personal Information Protection Commission, APPI Q&A (Roster of Persons Requiring Evacuation Assistance / Individual Evacuation Plans). https://www.ppc.go.jp/personalinfo/faq/APPI_QA/  
[R10] Personal Information Protection Commission, APPI Q&A (Administrative Bodies Section), old/new comparison, 2025/12/12. https://www.ppc.go.jp/files/pdf/251212_koutekibumon_qa_shinkyu.pdf  
[R11] Cabinet Office Disaster Management, Shelter Living-Environment Measures (Guideline materials revised in December 2024). https://www.bousai.go.jp/taisaku/hinanjo/index.html  
[R12] Ministry of Health, Labour and Welfare, Partial Revision of the Japan DMAT Operational Guidelines (2024/03/29). https://www.mhlw.go.jp/content/10800000/001272085.pdf  
[R13] FEMA, IS-700.b An Introduction to the National Incident Management System. https://training.fema.gov/emiweb/is/is700b/student%20manual/smis0700b.pdf  
[R14] FEMA, ICS Forms Descriptions / ICS 209. https://training.fema.gov/emiweb/is/icsresource/assets/ics%20forms%20descriptions.pdf ; https://training.fema.gov/emiweb/is/icsresource/assets/ics%20forms/ics%20form%20209%2C%20incident%20status%20summary%20%28v3%29.pdf  
[R15] Japan Meteorological Agency, About Tsunami Warnings/Advisories, Tsunami Information, and Tsunami Forecasts. https://www.jma.go.jp/jma/kishou/know/jishin/joho/tsunamiinfo.html  
[R16] Japan Meteorological Agency, Designated River Flood Forecasts. https://www.jma.go.jp/jma/kishou/know/bosai/flood.html  
[R17] Japan Meteorological Agency, Landslide Warning Information / Landslide KikiKuru. https://www.jma.go.jp/jma/kishou/know/bosai/doshakeikai.html  
[R18] Japan Meteorological Agency, Explanation of Volcanic Alert Levels. https://www.jma.go.jp/jma/kishou/know/kazan/level_toha/level_toha.html  
[R19] Meshtastic, Canned Message Module Configuration. https://meshtastic.org/docs/configuration/module/canned-message/  
[R20] Cabinet Secretariat Civil Protection Portal, What to Do When a Ballistic Missile Is Incoming. https://www.kokuminhogo.go.jp/kokuminaction/  
[R21] Cabinet Secretariat Civil Protection Portal, Information Transmission by J-Alert (Nationwide Instant Alert System). https://www.kokuminhogo.go.jp/kokuminaction/jalert.html  
[R22] Government Public Relations Online, J-Alert Warns of Incoming Ballistic Missiles—What Actions Protect You? https://www.gov-online.go.jp/article/202412/entry-6847.html  
[R23] ICRC, Explosive Weapons in Populated Areas. https://www.icrc.org/en/law-and-policy/explosive-weapons-populated-areas  
[R24] JARL, JARL Activities During the Great East Japan Earthquake. https://www.jarl.org/Eastern_Japan_earthquake.htm  
[R25] JARL, Amateur Radio and Emergency Communications. https://www.jarl.org/Japanese/2_Joho/2-4_Hijou/Hijou.htm  
[R26] JARL, Emergency Communications Manual for Amateur Stations. https://www.jarl.org/Japanese/2_Joho/2-4_Hijou/emergency-communication-manual.pdf  
[R27] Nuclear Regulation Authority, Concepts of Protective Measures Based on the Nuclear Emergency Preparedness Guidelines. https://www.nra.go.jp/activity/bousai/measure/bogosoti.html  
[R28] Cabinet Office, Sheltering Indoors for Residents Living Roughly 5–30 km from Nuclear Power Plants. https://www8.cao.go.jp/genshiryoku_bousai/shiryou/okunaitaihi.html  
[R29] Cabinet Secretariat, Basic Guidelines on Civil Protection (including NBC attacks). https://www.kokuminhogo.go.jp/pdf/291219shishin.pdf  
[R30] Ministry of Health, Labour and Welfare, Health Crisis Management for Domestic Terrorism. https://www.mhlw.go.jp/content/20210428-01.pdf  
[R31] GitHub rkolbi/GuardianBridge README. https://github.com/rkolbi/GuardianBridge  
[R32] Japan Meteorological Agency, Information Related to Linear Rainbands. https://www.jma.go.jp/jma/kishou/know/bosai/kishojoho_senjoukousuitai.html  
[R33] Japan Meteorological Agency, Storm Surge Associated with Typhoons. https://www.jma.go.jp/jma/kishou/know/typhoon/4-1.html  
[R34] Japan Meteorological Agency, Disaster Caused by Storm Surge. https://www.jma.go.jp/jma/kishou/know/ame_chuui/ame_chuui_p6.html  
[R42] NOAA SARSAT, Emergency 406 Beacons / EPIRB. https://www.sarsat.noaa.gov/emergency-406-beacons/  
[R41] USCG Navigation Center, Radio Information for Boaters. https://www.navcen.uscg.gov/radio-information-for-boaters  
[R40] USCG Navigation Center, DSC Distress. https://www.navcen.uscg.gov/dsc-distress  
[R39] ARRL, Amateur Radio Nets Crucial Link in Maritime Rescues. https://www.arrl.org/news/view/amateur-radio-nets-crucial-link-in-maritime-rescues  
[R38] ARRL, 1997 Field Day report: Mesilla Valley Search and Rescue requested communications support during search for overdue hiker. https://www.arrl.org/files/file/ContestResults/97/fd.pdf  
[R37] RAF Mountain Rescue, Chapter 9 Mountain Rescue Communications: VHF portable radios are used between search parties, command and control, and SAR helicopters. https://rafmountainrescue.com/wp-content/uploads/2012/08/Chapter-9-MOUNTAIN-RESCUE-COMMUNICATIONS.pdf  
[R36] NMSAR, COMMUNICATIONS for SAR Field Teams: For search and rescue, LMRS and HAM are usually used/needed. https://www.nmsarc.org/uploads/6/3/5/6/63569937/comms_training_nmsar-fieldteams20230519.pdf  
[R35] Meshtastic Blog, Today, Meshtastic is used in Search and Rescue operations, off-grid communication, disaster recovery, and even grid-down scenarios. https://meshtastic.org/blog/page/2/

## 15. Codebook (by Category)

This section consolidates all codes used by this protocol by category. Codes, meanings, usage, example messages, and the meaning of those examples are gathered in one place for use as the master copy for field-distribution cards, wall ledgers, and training materials.

### 15-1. Priority and Management

**Note:** SEQ is not a standalone code in the codebook; it is a syntax element used within the standard syntax. Increase it as S01, S02, S03, etc. to indicate which report number it is within the same case. Combined with the report time at the end, it clarifies update order and update time.

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| [[SOS]] | Life-threatening / highest priority | Life-saving, entrapment, drowning, sudden deterioration | `[[SOS]] MAI-Q-20260329-1430 NEW REQ: COL TRP 2PAX 1RED.` | In the Maizuru earthquake case, two people are inside a collapsed building and one is red-triage and needs immediate rescue. |
| [[EMG]] | Emergency | Fire, severe injury, ongoing disaster, explosion | `[[EMG]] OSA-I-20260329-1940 NEW RPT: FIRE MOV E.` | In the Osaka fire case, the fire is moving east. |
| [[ALR]] | Alert / advance warning | Approaching danger, tsunami, chemical/nuclear/missile alert | `[[ALR]] FKS-U-20260329-0910 NEW RPT: RAD ALR SEAL.` | A nuclear/radiological emergency is suspected and shelter-in-place should begin. |
| [[SIT]] | Situation report | Current status, damage overview, observation | `[[SIT]] KOB-L-20260329-1500 NEW RPT: ROD BLK BY LND.` | Situation report that a road is blocked by landslide. |
| [[ADM]] | Administration / control | Roll call, control, channel management, training | `[[ADM]] NET ROLL CALL 0900.` | Administrative notice that a roll call will be held at 09:00. |
| NEW | New | At case occurrence | `NEW REQ` | New request. |
| WIP | In progress | Dispatch, arrangements, or investigation are moving | `WIP ETA20` | In progress; expected arrival in 20 minutes. |
| HLD | On hold | Suspended due to lack of resources, danger, weather | `HLD WX` | On hold because of weather. |
| CLO | Closed | Case complete | `CLO NIL` | Case completed; no further action required. |
| COR | Correction | Correction of false or mistyped report | `COR LOC B2` | Correct the location to B2. |
| FAK | False information / rumor | Alert for clearly false or malicious rumor | `FAK-INFO` | A case that should be handled as false information. |
| NEG | Negation / cancellation | Notice that the event did not exist on investigation | `NEG NO FIRE` | Negation stating that there was no fire. |
| SCM | Fraud / suspicious solicitation | Alert for aid fraud, suspicious recruiting, improper collection | `SCM FAKE DONATION` | Alert for fake donations or fraudulent solicitation. |
| ACK | Receipt confirmation | Received / accepted | `ACK #MAI-M001` | The Maizuru case has been received. |
| QSL | Acknowledged / comms established | Short acknowledgment response | `QSL ETA30` | Understood; expected arrival in 30 minutes. |
| NIL | No additional information / no abnormality | Status report with no change | `NIL ALL OK` | No abnormality; everyone safe. |
| CAL | Notify / request dispatch | Notification to outside agency, dispatch request, handoff request | `REQ CAL PFIRE` | Fire-service notification / dispatch request is needed. |
| PJSDF | Self-Defense Forces | Request destination for isolated communities, wide-area transport, heavy machinery/boats/helicopters | `REQ CAL PJSDF` | Notification / support request to the Self-Defense Forces. |
| PPOLICE | Police | Request destination for public order, traffic control, suspicious items, searches | `REQ CAL PPOLICE` | Notification / dispatch request to police. |
| PFIRE | Fire service | Request destination for firefighting, rescue, hazardous materials, entrapment response | `REQ CAL PFIRE` | Notification / dispatch request to the fire service. |
| PEMS | Emergency medical services | Request destination for transport and severe-case response | `REQ CAL PEMS` | Notification / dispatch request to EMS. |
| PRESC | Rescue team | Request destination for rescue, hazardous materials, entrapment response | `REQ CAL PRESC` | Notification / dispatch request to a rescue team. |
| PCG | Japan Coast Guard | Request destination for marine distress, drifting, coastal rescue, port incidents | `REQ CAL PCG` | Notification / support request to the Japan Coast Guard. |
| PADMIN | Administrative agency | Official contact for municipalities, prefectures, national agencies, etc. | `REQ CAL PADMIN` | Notification / contact request to an administrative agency. |

### 15-2. Confidence Assessment

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| A1 | Highly reliable source / content certain | Direct witness + multiple corroborations | `A1` | Direct witness plus multiple confirmations; confidence sufficient for immediate action. |
| A2/B2 | Strong but single-source or consistent with official information | Single direct witness or official consistency | `B2` | Strong confidence based on a single witness or official consistency. |
| B3/C3 | Moderate | Multiple hearsay reports, broadly consistent | `C3` | Multiple hearsay reports are consistent, but further confirmation is desirable. |
| D4/E4 | Low / unconfirmed | Rumor, fragmentary information, unconfirmed | `D4` | Unconfirmed; remain alert but do not treat as fact. |
| E5 | Suspected falsehood | Major contradictions, malice, misinformation | `E5` | High possibility of false or misleading information. |
| CFM | Initial filter: confirmed | Direct observation / official announcement | `CFM` | Initial assessment that may be treated as confirmed in the field. |
| PRB | Initial filter: probable | Multiple matching testimonies / high plausibility | `PRB` | High-confidence initial assessment, though further confirmation is desirable. |
| UNC | Initial filter: unconfirmed | Single hearsay / social media / no corroboration | `UNC` | Unconfirmed and not to be used as the basis for immediate decisions. |

### 15-3. Disaster Categories

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| Q | Earthquake | Earthquake, aftershock, collapse | `MAI-Q-20260329-1430` | Earthquake case in Maizuru. |
| T | Tsunami | Tsunami, tsunami evacuation, immediate coastal withdrawal | `KES-T-20260329-1450` | Tsunami case in Kesennuma. |
| P | Heavy rain / linear rainband | Extreme rain, rapid short-term escalation, underground-space danger | `FUK-P-20260329-1810` | Heavy-rain case in Fukuoka. |
| Y | Typhoon / windstorm / storm surge | Typhoon, strong wind, storm surge, coastal-lowland evacuation | `KOB-Y-20260329-0600` | Typhoon case in Kobe. |
| F | Flood / river overflow | Inundation, flood, overtopping, river overflow | `KOB-F-20260329-2305` | Flood case in Kobe. |
| L | Landslide | Landslide, slope failure | `H01-L-20260329-0715` | Landslide case near Shelter 1. |
| I | Fire / explosion | Fire, spread, explosion | `OSA-I-20260329-1940` | Fire/explosion case in Osaka. |
| V | Volcano | Eruption, ashfall, reduced visibility | `KAG-V-20260329-0810` | Volcano case in Kagoshima. |
| W | Snow / cold | Heavy snow, cold wave, freezing, isolation | `NII-W-20260329-0630` | Snow/cold case in Niigata. |
| H | Heat | Extreme heat, combined blackout, cooling failure | `TKY-H-20260329-1230` | Heat case in Tokyo. |
| M | Medical | Illness/injury, fever cluster, medical request | `KOB-M-20260329-0940` | Medical case in Kobe. |
| N | Nuclear / radiation | Nuclear disaster, radiological protection, shelter-in-place | `FKS-N-20260329-0910` | Nuclear case in Fukushima. |
| C | Chemical | Chemical incident, suspected chemical agent, decontamination | `OSA-C-20260329-1012` | Chemical case in Osaka. |
| B | Biological | Biological incident, isolation, fever cluster | `KYO-B-20260329-1030` | Biological case in Kyoto. |
| K | Mountain distress | Mountain accident, missing hiker, animal hazard | `HAK-K-20260420-1510` | Mountain-distress case in Hokkaido. |
| O | Marine distress | Capsize, flooding, drifting | `KOB-O-20260708-0635` | Marine-distress case in Kobe. |
| U | Infrastructure | Blackout, water outage, communications, road-function failure | `TKY-U-20260329-1010` | Infrastructure case in Tokyo. |
| G | Supplies | Food, water, hygiene, maternal-child supplies | `H03-G-20260329-1225` | Supply case for Shelter 3. |
| S | Security / armed attack / conflict / riot | Deteriorating security, armed attack, conflict, riot, civil disturbance | `SND-S-20260329-1840` | Security/conflict/riot case in Sendai. |
| R | RFL / safety confirmation | Missing persons, safety inquiries, re-cross-checking | `RFL-MAI-20260329-001` | RFL / safety-confirmation case in the Maizuru area. |

### 15-4. Events, Damage, and Protective Actions

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | :---- | :---- | :---- | :---- |
| COL | Collapse | Building collapse, structural damage | `COL TRP 2PAX` | Two people are trapped in a collapse. |
| TRP | Trapped / entrapment | Used together with rescue request | `TRP 1RED` | One person is trapped and red-triage. |
| ISO | Isolated | Isolated due to road disruption, etc. | `ISO VLG` | A village is isolated. |
| BLK | Blocked | Roads or routes blocked | `ROD BLK` | The road is blocked. |
| SINK | Inundation | Above-floor / below-floor flooding, road flooding | `SINK 2F` | Flooding has reached the second floor. |
| DRK | Blackout | Power interruption | `PWR DRK` | There is a blackout. |
| DRY | Water outage | Water supply interruption | `WTR DRY` | Water is out. |
| OUT | Communications outage | Cellular, IP, or radio outage | `NET OUT` | The communications network is down. |
| BOOM | Blast sound / large explosion sound | Initial report of impact or explosion | `BOOM HEARD NW` | A large explosion sound was heard to the northwest. |
| RAD | Radioactive material / suspected nuclear incident | Rising dose or suspected release | `RAD ALR SEAL` | Suspect a nuclear/radiological emergency; shelter and seal. |
| CHEM | Chemical incident / suspected chemical agent | Unusual odor, dispersal, irritant symptoms | `CHEM ODR MASK` | Suspect a chemical incident; begin respiratory protection. |
| BIO | Biological incident / suspected biological agent | Fever cluster, unusual infection | `BIO FEV CLU ISO` | Begin isolation in response to a fever cluster. |
| SEAL | Shelter and seal indoors | Close windows, stop ventilation | `SEAL NOW` | Seal immediately. |
| DECON | Decontamination | Contamination removal, clothing removal, washing | `DECON PT1` | Patient 1 requires decontamination. |
| MASK | Respiratory protection | Mask, covering mouth/nose | `MASK ON` | Begin mouth/nose protection. |
| ODR | Unusual odor | Chemical smell, gas smell | `ODR STRONG` | There is a strong unusual odor. |
| SHEL | Indoor shelter location | Underground, sturdy building | `MOVE SHEL B1` | Move to Basement Level 1. |
| MSL | Missile warning / possible impact | J-Alert, possible incoming strike, impact danger | `MSL ALERT` | A missile warning is in effect. |
| BLAST | Explosion / blast wave | Initial report of explosion, blast, secondary fire | `BLAST FIRE` | A fire has also occurred following an explosion. |
| UXO | Unexploded ordnance / suspicious explosive object | No-approach, zoning, reporting | `UXO FOUND` | A UXO-like object has been found. |
| CURF | Movement restriction / curfew | Checkpoint, closure, stay-off-roads | `CURF HOLD` | Hold position due to movement restrictions. |
| SURGE | Storm surge | Storm-surge inundation, coastal-lowland danger | `SURGE EVQ` | Storm-surge evacuation is required. |
| RIT | Riot outbreak | Initial report of civil disturbance | `RIT MOB` | A riot has started and crowds are gathering. |
| MOB | Crowd concentration | Crowd concentration / inflow | `MOB GATE-W` | A crowd is concentrating near the West Gate. |
| LOOT | Looting | Looting of shop, warehouse, etc. | `LOOT SHOP-A` | Looting is occurring at Shop A. |
| ARSN | Arson | Deliberate fire-setting | `ARSN FIRE` | There is a fire due to arson. |
| STON | Stone-throwing | Hazard from thrown objects | `STON ROAD-N` | There is stone-throwing danger on the north road. |
| BARR | Barricade / obstacle | Road closure / obstacle creation | `BARR INT-A` | A barricade has been built at Intersection A. |

### 15-5. Medical and Triage

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| T-RED | Immediate / highest priority | Immediate lifesaving treatment / transport | `1RED` | There is one red-triage patient. |
| T-YLW | Serious | Delay permissible but treatment required | `2YLW` | Two yellow-triage patients. |
| T-GRN | Minor | Ambulatory, first aid | `5GRN` | Five green-triage patients. |
| T-BLK | Dead / expectant | Includes non-resuscitatable cases | `1BLK` | One black-triage patient. |
| OXY | Oxygen required | Share patients requiring oxygen | `OXY 2PAX` | Two patients require oxygen. |
| DIAL | Dialysis required | Share patients needing continued dialysis / receiving inquiry | `DIAL 1PAX <12H` | One patient needs continued dialysis within 12 hours. |
| MED | Medical | General medical resources | `REQ: MED 3SET` | Three medical kits are needed. |
| PPE | Personal protective equipment | Gloves, gowns, protective clothing, etc. | `REQ: PPE 30` | Thirty PPE sets are needed. |
| N95 | High-performance protective mask | Droplet / aerosol protection | `REQ: N95 100` | One hundred N95-equivalent masks are needed. |

### 15-6. Supplies and Logistics

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| WAT | Drinking water | Water for drinking / cooking | `REQ: WAT 200L` | 200 liters of drinking water are needed. |
| FOO | Food | Staple food, side dishes, preserved food | `REQ: FOO 100R` | 100 meals are needed. |
| BAT | Batteries / stored power | Dry batteries, mobile power | `REQ: BAT AA80` | 80 AA batteries are needed. |
| GAS | Fuel | Gasoline, LPG, etc. | `REQ: GAS 40L` | 40 liters of fuel are needed. |
| KID | Pediatric support supplies | Comprehensive child-support supplies | `REQ: KID 20SET` | 20 child-support sets are needed. |
| PED | Pediatric medical supplies | Pediatric drugs, fluids, etc. | `REQ: PED 10SET` | 10 sets of pediatric medical supplies are needed. |
| NEO | Neonatal supplies | Comprehensive neonatal support supplies | `REQ: NEO 5SET` | 5 neonatal-support sets are needed. |
| MILK | Infant milk | Liquid milk / powdered milk | `REQ: MILK 40` | 40 units of infant milk are needed. |
| FORM | Formula / infant nutrition | Detailed specification for formula, etc. | `REQ: FORM 20CAN` | 20 cans of formula are needed. |
| BTL | Baby bottle | Bottles, nipples, sterilization supplies | `REQ: BTL 30` | 30 baby bottles are needed. |
| DIAP | Diapers | Infant / child diapers | `REQ: DIAP S/M/L` | Diapers in each size are needed. |
| WIPE | Wipes / baby wipes | For hygiene management | `REQ: WIPE 50PK` | 50 packs of wipes are needed. |
| FEM | Women’s support supplies | Daily-life support supplies for women | `REQ: FEM 30SET` | 30 sets of women’s support supplies are needed. |
| SAN | Sanitary products | Menstrual hygiene products | `REQ: SAN DAY/NGT` | Daytime and nighttime sanitary products are needed. |
| PREG | Supplies for pregnant/postpartum women | Pregnancy / postpartum care supplies | `REQ: PREG 8SET` | 8 sets of supplies for pregnant/postpartum women are needed. |
| MAT | Mother-and-child support supplies | Combined mother-child support set | `REQ: MAT 6SET` | 6 mother-and-child support sets are needed. |
| BRA | Underwear / nursing underwear | Women’s underwear / nursing bras | `REQ: BRA M/L` | Women’s underwear and nursing underwear are needed. |
| PAD | Postpartum / incontinence pads | Women’s hygiene / postpartum / caregiving use | `REQ: PAD 10PK` | 10 packs of postpartum or incontinence pads are needed. |
| SEND | En route | Transport has begun | `SEND ETA20` | In transit; expected arrival in 20 minutes. |
| ARRV | Arrived | Already arrived | `ARRV EVQ-01` | Arrived at Shelter 1. |
| NEED | Needed | Used for shortage / request | `WAT NEED` | Water is needed. |

### 15-7. Missing Persons and Safety Confirmation

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| MISS | Missing person | On general operations, attribute only | `MISS M70 LAST SEC-B` | A male in his 70s was last confirmed in Sector B. |
| SAFE | Safety confirmed | Identity confirmed / shelter confirmation | `SAFE EVQ-01` | Safety confirmed at Shelter 1. |
| FND | Found | Found / whereabouts confirmed | `FND HSP-02` | Found at Hospital 2. |
| RCO | Re-cross-check | Reconfirmation / roster cross-check | `RCO LIST-03` | Re-cross-check Roster 3. |
| EYE | Witnessed | Direct eyewitness information | `EYE 1410J` | Direct eyewitness observation at 14:10. |
| HRD | Heard / hearsay | Hearsay information | `HRD ONLY` | Information is hearsay only. |

### 15-8. Mountain Distress

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| K | Mountain distress | Case category | `HAK-K-20260420-1510` | Mountain-distress case in Hokkaido. |
| LOST | Lost / missing on route | Type of distress | `LOST 2PAX` | Two people are lost or missing. |
| FALL | Fall | Cause of injury | `1PAX FALL` | One person has fallen. |
| CLIF | Cliff area | Terrain hazard | `CLIF WEST` | There is cliff terrain to the west. |
| RIDG | Ridge | Terrain location | `LAST RIDG-3` | Ridge 3 is the last confirmed point. |
| GULLY | Gully / ravine | Terrain location | `GULLY NORTH` | Gully terrain to the north. |
| WHTO | Whiteout | Weather hazard | `MOVE NIL WHTO` | Unable to move because of whiteout. |
| AVAL | Avalanche | Snow hazard | `AVAL RISK` | Avalanche risk exists. |
| HYPOTH | Hypothermia | Medical status | `1RED HYPOTH` | One severe patient with hypothermia. |
| ALT | Altitude sickness | Medical status | `ALT PRB` | Suspected altitude sickness. |
| TRAIL | Mountain trail | Supplemental location | `TRAIL EAST` | East-side trail. |
| LZ | Helicopter landing zone | Rescue support | `LZ PRB` | Candidate helicopter landing zone likely available. |
| TH | Trailhead | Base point | `TH SOUTH` | South trailhead. |

### 15-9. Marine Distress

| Code | Meaning | Usage | Example | Meaning of Example |
| :---: | ----- | ----- | ----- | ----- |
| O | Marine distress | Case category | `KOB-O-20260708-0635` | Marine-distress case in Kobe. |
| POB | Persons on board | Headcount | `3POB` | Three persons on board. |
| MOB | Man overboard | Distress type | `MOB 1` | One person overboard. |
| TAKW | Taking water / flooding | Hull status | `TAKW` | Taking on water. |
| ADRF | Adrift | Position status | `ADRF` | Adrift. |
| CAPS | Capsize | Hull status | `CAPS PRB` | Suspected capsize. |
| GND | Grounded | Vessel position status | `GND` | Grounded. |
| ENGF | Engine failure | Loss of propulsion | `ENGF` | Engine failure. |
| SAILF | Sailing failure / unable to sail | Loss of propulsion | `SAILF` | Unable to sail. |
| RAFT | Life raft | Lifesaving measure | `RAFT DEPLOY` | Life raft deployed. |
| PFD | Life jacket worn | Lifesaving measure | `PFD ALL` | Everyone is wearing life jackets. |
| DSC | DSC distress sent | Distress call | `DSC DONE` | DSC has been transmitted. |
| EPIR | EPIRB active | Distress call | `EPIR ON` | EPIRB active. |
| FLAR | Flare observed / launched | Visual aid | `FLAR RED` | Red flare confirmed. |
| HULL | Hull damage | Vessel status | `HULL DMG` | Hull damage present. |
| BILG | Bilge worsening | Flooding supplement | `BILG FAST` | Flooding is progressing rapidly. |

## 16. Supplementary Material: Example Training Drills

Example drills are intended for peacetime training, coordination training with municipalities, fire services, medical institutions, and shelters, and NBC / nuclear-response training. Each drill is organized by four items—“drill name,” “scenario,” “tasks,” and “success conditions”—so that even short drills can be evaluated.

| Drill Name | Scenario | Tasks | Success Conditions |
| ----- | ----- | ----- | ----- |
| Initial control in first 30 minutes after earthquake | 30 minutes after impact, Phase 1. Aftershocks continue. Collapse, fire, blackout, and communication congestion occur at the same time, and multiple nodes are trying to send emergency reports. | Use only [[SOS]] and [[EMG]], and stop chatter, long messages, unconfirmed information, and supply discussions. Assign case IDs and send location, number of people, danger level, and triage as quickly as possible. | For 5 minutes, zero unauthorized transmissions. Every emergency case has a case ID, location, number of people, and danger level, and no ACK or WIP is missing. |
| Handoff for road disruption + shelter overload | After heavy rain, two main roads are impassable, shelters are over capacity, and water, toilets, and hygiene supplies are lacking. The field uses coded operations, but the municipality receives only plain language. | LOG and Liaison organize road conditions, alternate routes, remaining shelter capacity, supply shortages, and urgency, and expand coded reports into plain-language text for municipal handoff. | Within 10 minutes, create a plain-language notification of 200 characters or fewer with no missing required items, usable by the municipality for immediate decision-making. |
| Missing-person cross-check and personal-information protection | Shelter rosters, hospital transport rosters, and family inquiries do not match, and name-based family inquiries are arriving one after another. It is a situation in which one may be tempted to write details on the general channel. | On the general channel, share only the case number, attributes, and last confirmed point. Split names, addresses, and contact details into RFL and individual inquiries. Record the cross-check process as well. | Zero transmission of personal information on the general channel. Cross-check targets, results, pending cases, and next points of inquiry can be uniquely tracked. |
| Base-station failure and switch to reserve | CLIENT_BASE has stopped, power at the base station is unstable, and the gateway station is no longer responding. A reserve station is standing by and immediate handover is required. | Start backup power, switch roles to the reserve station, inherit group settings, scheduled distribution, audit records, and the case ledger, and notify field nodes of the switch. | Control resumes within 15 minutes. No missing case-ledger entries. No dual control. Field nodes recognize the new base station. |
| DMAT reception and medical transport control | Hospital function is degraded, and many casualties are arising from shelters and aid stations. DMAT is expected to arrive, but receiving destinations, transport order, and oxygen/drug demand have not yet been organized. | Aggregate patients by red/yellow/green/black, organize transport priorities, receiving-facility inquiries, transport routes, and demand for drugs and oxygen. Re-triage if necessary. | Severe patients are organized in priority order, and receiving-facility inquiry results and destinations are explicit. Re-triage results and update times can be tracked. |
| Initial collapse rescue | Immediately after an earthquake, two buildings collapsed, three persons trapped, communications congested | Send [[SOS]], assign case IDs, report red-triage status, secure access route | Within 5 minutes, case ID, number of people, triage, and location are aligned |
| Immediate tsunami evacuation | Tsunami warning issued; elderly people present at two coastal shelters | Send [[ALR]], move to vertical evacuation/high ground, conduct roll call | Within 10 minutes, evacuation completion rate and number of people not yet evacuated are shared |
| Flood / overtopping | River level rising, overtopping begins, roads inundated | Report road blockage and shelter capacity | Route usability, alternate routes, and shelter occupancy are organized |
| Landslide | A slope failure isolates a community | Turn isolation, injuries, and supply routes into short reports | Number of isolated persons and needed supplies are uniquely understood |
| Urban fire | Fire spread is expanding; wind direction changing | Share direction of spread, shortage of firefighting equipment, and retreat direction | Direction of spread and danger zones are conveyed without misunderstanding |
| Heavy-snow isolation | Roads cut by snow; dialysis patients present | Separate medical priority from snow-removal request | Medical cases and road cases are handled without mixing |
| Extreme heat blackout | Wide-area blackout, cooling failure, many heatstroke cases | Separate cases for power generation, cooling, water, and transport | Priority of high-risk heat cases is visible |
| Missing-person cross-check | Shelter roster and hospital transport roster do not match | Use MISS/FND/RCO to cross-check | Cross-check completed without sending personal information on general channels |
| Missile warning | J-Alert issued; some teams are outdoors | Apply transmission restrictions, shelter indoors/underground, report blast sounds | Unnecessary long transmissions stop and protective actions are synchronized |
| UXO / suspicious object | Suspicious object beside a building, people moving nearby | Isolate area, prohibit approach, notify authorities, do not request images | Zero approachers, zone set, secondary disaster prevented |
| Nuclear shelter-in-place | Facility accident assumed; shelter-in-place order issued | Use RAD / SEAL / HVAC OFF in short reports | Sealing, stopping ventilation, and indoor sheltering are synchronized |
| Initial chemical incident response | Unusual odor at station, many with eye irritation and coughing | Use CHEM / ODR / MASK / DECON | Wind direction and decontamination need are shared within 3 minutes |
| Initial biological incident response | Cluster fever and cough at shelter | Use BIO / ISO / PPE / N95 | Isolation routes and PPE shortages are explicitly stated |
| Pediatric supply support | 20 infants and young children at shelter | Request with KID / NEO / MILK / DIAP / WIPE | Quantities of child and neonatal supplies are made concrete |
| Women’s support supplies | Many female evacuees; sanitary products short | Use FEM / SAN / PREG / MAT / BRA / PAD | Supplies for women and pregnant/postpartum evacuees do not get buried among general supplies |
| Municipality handoff drill | Information from private mesh is handed to disaster headquarters | Convert code → plain language, attach confidence, send corrections | Municipality-oriented plain language of 200 characters or fewer is created accurately |
| Heavy rain / linear rainband | Nighttime; linear rainband information issued; people remain in underground malls and lowland residential areas | Turn securing high ground, avoiding underground space, and leaving cliff/ravine areas into short reports | Within 10 minutes, danger zones, retreat directions, and whether anyone remains unevacuated are shared |
| Typhoon / storm surge | Typhoon approaching, violent-wind warning equivalent, storm-surge inundation expected in coastal lowlands | Organize SURGE / evacuation completion deadline / bridge passability / expected blackout | Before severe winds begin, evacuation deadline and danger zones are communicated without misunderstanding |
| Multiple case-number responses | Three SOS cases and two supply requests occur simultaneously | Assign simple case numbers and return ACK / WIP / ARRV with the number attached | Everyone can track which response corresponds to which case |
| Gateway-station operation | Base station performs audit, group transmission, and individual safety inquiries simultaneously | Switch permanent / temporary groups, perform audit recording, individual inquiries, and external handoff | Configuration changes and field operations are not confused, and audit records are complete |

## 17. Supplementary Material: Pre-Transmission Check Card

A simple on-site card to confirm before sending. Even short reports should avoid the minimum critical omissions.

| Check Point | What to Confirm | Example of Transmission Not Allowed |
| :---: | ----- | ----- |
| Location | Does it include at least one of GPS, base-location prefix, facility name, area name, or last confirmed point? | Sending only “Help!” with no location. |
| Number of people / urgency | Does it include number of people, red/yellow/green/black triage, number isolated, or danger level? | Saying only “many” or “dangerous” with no specificity. |
| Required support | Is the needed support clearly stated, such as rescue, transport, notification, supplies, receiving-facility inquiry, or road opening? | The receiving side cannot judge what is needed. |
| Confidence / time | Are confirmation time and confidence evaluation included? | Resending old information with no timestamp. |
| Personal information | Are name, address, contact details, or clinical information being sent on a general channel? | Sending name or date of birth for a MISS case on the general channel. |
| Destination | Is it appropriate for general channel, group, individual, or handoff destination? | Sending what should be an individual inquiry to the whole channel. |

## 18. Supplementary Material: Operational Evaluation Indicators

Minimum indicators for evaluating not only traffic volume but also the quality of judgment in both training and real operations.

| Evaluation Item | Evaluation Focus | Pass Standard | Fail Condition |
| :---: | ----- | ----- | ----- |
| Initial response to emergency cases | Does the initial report include case ID, location, number of people, and danger level? | Within 3 minutes, at least 3 of the 4 elements are shared. | The initial report becomes chatter and the case cannot be formalized. |
| Handoff quality | Can codes be accurately expanded into plain language? | No required item missing within 200 characters. | The receiver cannot act without making a follow-up inquiry. |
| Personal information protection | Is unnecessary identifying information absent from the general channel? | Zero exposure of personal information. | Names, addresses, contact details, or detailed medical history are sent generally. |
| Base-station switching | Are case ledger and audit maintained during switch to reserve? | Control resumes within 15 minutes, with no dual control. | After switching, cases go missing or simultaneous control occurs. |
| Medical transport control | Are red/yellow/green/black, receiving-facility inquiries, and destinations organized? | Severe-case priority order and destinations are explicit. | Severe cases are buried among general supply cases. |
| Scheduled distribution control | Do scheduled transmissions avoid creating congestion? | Sent at the planned time, with the planned wording and planned destination. | Long scheduled transmissions are sent indiscriminately at arbitrary times. |
| Audit records | Are corrections, closures, and handoff updates recorded? | Zero missing records for high-impact operations. | It becomes impossible to trace who changed what. |

## 19. Supplementary Material:Conceptual Diagrams

### 19-1. Information Handoff Flow

Shows the flow in which internal short reports are converted through control and liaison into plain language for outside organizations, and responses return to the closed network with a case number.

<img src="/img/mep_english_05.png"> 
**Figure 1. Information Handoff Flow**</br>
Internal code → control/liaison → external plain-language conversion → return of response

### 19-2. Case Lifecycle

A case starts as NEW and follows the basic path of ACK, WIP, ARRV, and CLO. Holds, corrections, negations, and false-information handling remain in the history while preserving the case ID.

<img src="/img/mep_english_06.png"> 
**Figure 2. Case Lifecycle**</br>
The basic route of a case and the branches for hold, correction, negation, and false-information handling

### 19-3. Recommended Topology and Role Allocation

Shows the role allocation and physical placement concepts for field nodes, elevated relays, base stations, and external organizations. Relays are kept to the minimum necessary, and base stations handle external liaison and audit.

<img src="/img/mep_english_07.png">  
**Figure 3. Recommended Topology and Role Allocation**</br>
Relationships among field stations, base stations, gateway stations, and outside organizations, and the concept of physical placement
