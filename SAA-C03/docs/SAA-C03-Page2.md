# AWS Certified Solutions Architect – Associate (SAA-C03) Vol 2

## 9.0 Solutions Architecure Discussions 
This section covers the topic of solutions architecture and their design 

### 9.1 Statless Web App: WhatIsTheTime.com - Solution Architecture 1
- Allows people **know what the current time**
- **No Database**
- Start small - **Accept downtime**

**Scenario 1 : Initially**
- Public EC2 instance
- Elastic IP - static public IP
- **Result**: Application is working alright.

**Scenario 2 - Increase in No Users**
- **Vertical Scaling** - Increased the instance size from t3.micro to M5

