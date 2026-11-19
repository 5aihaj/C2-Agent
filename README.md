# C2-Agent
Command and Control (C2) phase detection agent, my contribution to a team-built agentic AI cyber kill chain defence framework (UTS capstone).
# C2 Phase Detection Agent

This repository contains the Command and Control (C2) phase detection agent I developed as part of AAKCD (Agentic AI Based Cyber Kill Chain Defence Framework), a team capstone project completed at the University of Technology Sydney in 2026.

AAKCD is a multi-agent framework where each agent is responsible for detecting a different stage of the cyber kill chain. My role was to design and build the agent that detects activity in the Command and Control phase, where an attacker sets up communication between a compromised system and their own infrastructure.

## About this repository

This repo only includes my individual contribution. The other agents in the framework, the shared orchestration components and any work done by my teammates are not included here. Team member details have also been left out for privacy.

As a result, this code shows how the C2 agent works on its own and may not run as part of the full framework without the other components.

## What the agent does

- **Parses network logs:** Reads Zeek `conn.log` files and groups connections by source, destination and port.
- **Detects malware beaconing:** Malware often "checks in" with its server on a fixed timer, so the gaps between connections barely change. The agent measures how regular those gaps are using the coefficient of variation. Low variation points to a beacon, while normal browsing is much more irregular. This replaced an earlier count-based approach that flagged ordinary browsing as suspicious.
- **Explains its findings with AI:** Each detection is sent to the Google Gemini API for a plain-language explanation of what was found and why it matters. If the API is down or out of quota, the agent still produces its alerts and marks the AI status as unavailable instead of failing.
- **Outputs structured alerts:** Results are written as JSON alerts in the framework's shared alert format, mapped to the MITRE ATT&CK Command and Control tactic (TA0011), so other parts of the framework can use them.

## Testing

The agent has a pytest test suite using synthetic Zeek log fixtures, including:

- A clean beacon that should be detected
- Normal browsing traffic that should not be flagged (a false positive check)
- A beacon with heavy random timing (jitter), which documents a known limitation: beacons with enough randomness can avoid detection at the default thresholds

## Tech stack

- Python
- Zeek (network log source)
- Google Gemini API (AI enrichment)
- pytest (testing)
- Git and GitHub
- Kali Linux (lab environment)

## Acknowledgements

Developed as part of the Cybersecurity Capstone Studio at UTS. Thanks to my project team and product owner for their collaboration on the wider framework.
