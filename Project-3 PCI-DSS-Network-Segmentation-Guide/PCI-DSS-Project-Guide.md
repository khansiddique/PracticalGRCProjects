# PCI DSS Network Segmentation Project - Complete Implementation Guide

## Project Overview

This comprehensive guide provides step-by-step instructions for completing a PCI DSS Network Segmentation project. Network segmentation is a critical security control that isolates the Cardholder Data Environment (CDE) from other network components, reducing PCI DSS scope and enhancing security posture.

### Project Objectives

1. Define and document PCI DSS scope boundaries
2. Implement effective network segmentation controls
3. Validate segmentation effectiveness through testing
4. Reduce PCI DSS assessment scope
5. Enhance security by limiting potential attack paths
6. Create comprehensive documentation for auditors

## Section 1: Understanding PCI DSS Scope

### 1.1 What is PCI DSS Scope?

PCI DSS scope encompasses all system components that store, process, or transmit cardholder data (CHD), or could impact the security of the Cardholder Data Environment.

#### Cardholder Data Includes:
- Primary Account Number (PAN)
- Cardholder Name
- Expiration Date
- Service Code

#### Sensitive Authentication Data:
- Full magnetic stripe data
- CAV2/CVC2/CVV2/CID
- PINs and PIN blocks

### 1.2 Three Categories of Systems

**In-Scope Systems:**
- Systems that store, process, or transmit cardholder data
- Payment terminals and POS systems
- Payment gateways
- Databases containing CHD
- Applications processing payments

**Connected Systems:**
- Systems that connect to or can impact in-scope systems
- Network devices providing connectivity
- Security systems protecting the CDE
- Management systems with access to CDE

**Out-of-Scope Systems:**
- Systems completely isolated from the CDE
- No connection or access path to cardholder data
- Cannot impact CDE security

### 1.3 Scoping Methodology

**Step 1: Data Discovery**
- Identify all locations where cardholder data exists
- Map data flows throughout the organization
- Document all systems touching CHD

**Step 2: Asset Inventory**
- Create comprehensive inventory of all systems
- Document network connectivity
- Identify system owners and purposes

**Step 3: Data Flow Analysis**
- Map how cardholder data moves through systems
- Identify all processing points
- Document transmission methods and protocols

**Step 4: Boundary Definition**
- Define clear boundaries of the CDE
- Identify all connection points
- Document segmentation controls

## Section 2: Network Segmentation Fundamentals

### 2.1 What is Network Segmentation?

Network segmentation divides a computer network into smaller, isolated sections. In PCI DSS context, it separates the CDE from other network segments to:
- Reduce assessment scope
- Limit lateral movement in case of breach
- Focus security resources on critical systems
- Simplify compliance efforts

### 2.2 Benefits of Segmentation

**Security Benefits:**
- Contains potential breaches
- Reduces attack surface
- Limits lateral movement
- Improves incident detection

**Compliance Benefits:**
- Reduces number of in-scope systems
- Decreases audit complexity
- Lowers compliance costs
- Simplifies evidence collection

**Operational Benefits:**
- Improves network performance
- Enables targeted security controls
- Facilitates troubleshooting
- Allows flexible security policies

### 2.3 Segmentation Methods

**Physical Segmentation:**
- Completely separate hardware and network infrastructure
- No physical connection between segments
- Highest level of isolation
- Most expensive to implement

**Logical Segmentation:**
- VLANs (Virtual Local Area Networks)
- Firewalls with strict rule sets
- Network Access Control (NAC)
- Software-Defined Networking (SDN)

**Hybrid Approach:**
- Combination of physical and logical controls
- Most common in practice
- Balances security and cost

## Section 3: Implementing Network Segmentation

### 3.1 Planning Phase

**Step 1: Define Segmentation Strategy**
- Identify segmentation objectives
- Select appropriate segmentation methods
- Define segment boundaries
- Plan for future growth

**Step 2: Network Design**
- Create network architecture diagrams
- Define subnet structures
- Plan IP addressing scheme
- Design firewall rule sets

**Step 3: Security Controls Planning**
- Identify required security controls
- Plan firewall configurations
- Design access control policies
- Plan monitoring and logging

### 3.2 Implementation Phase

**Step 1: Infrastructure Setup**

**Physical Segmentation:**
1. Install dedicated network hardware
2. Create separate network zones
3. Implement physical access controls
4. Configure network equipment

**Logical Segmentation:**
1. Configure VLANs on switches
2. Set up firewall zones
3. Implement access control lists (ACLs)
4. Configure routing between segments

**Step 2: Firewall Configuration**

**Firewall Rule Principles:**
- Default deny all traffic
- Explicitly allow only necessary traffic
- Document all rules with business justification
- Review rules at least every 6 months

**Example Firewall Rules:**

```
# Default deny all
deny ip any any

# Allow web traffic to payment gateway
permit tcp 192.168.1.0/24 host 10.0.1.100 eq 443
  Description: Allow web servers to payment gateway
  Justification: Process credit card transactions
  Owner: Payment Team
  Review Date: 2026-08-01

# Allow database queries from application
permit tcp host 192.168.1.50 host 10.0.1.200 eq 3306
  Description: Allow app server to CDE database
  Justification: Retrieve transaction data
  Owner: IT Security
  Review Date: 2026-08-01

# Deny all other CDE access
deny ip any 10.0.1.0/24
```

**Step 3: Access Control Implementation**

**Network Access Controls:**
- Implement 802.1X authentication
- Configure MAC address filtering (as additional layer)
- Set up VPN for remote access
- Implement multi-factor authentication

**Administrative Access:**
- Separate admin network segment
- Jump boxes for CDE access
- Privileged access management
- All admin sessions logged

### 3.3 Validation Phase

**Configuration Verification:**
1. Review firewall rules for accuracy
2. Verify VLAN configurations
3. Test access controls
4. Validate routing tables

**Documentation Review:**
1. Network diagrams accurate and current
2. Firewall rules documented
3. Data flow diagrams complete
4. Change management procedures in place

## Section 4: Segmentation Testing

### 4.1 PCI DSS Requirement 11.4.5

**Requirement:**
"Segmentation controls are tested as follows:
- At least every six months and after any changes to segmentation controls/methods
- By an internal qualified resource or external third party
- To confirm that segmentation is effective"

### 4.2 Testing Methodology

**Penetration Testing Approach:**

**Phase 1: Planning**
- Define test scope
- Identify test systems
- Obtain necessary approvals
- Schedule testing window

**Phase 2: Reconnaissance**
- Network mapping
- Service discovery
- Vulnerability scanning
- Information gathering

**Phase 3: Testing**
- Attempt to access CDE from outside
- Test firewall rules
- Attempt to bypass controls
- Test all documented paths

**Phase 4: Reporting**
- Document findings
- Classify vulnerabilities
- Provide remediation guidance
- Create executive summary

### 4.3 Testing Scenarios

**Scenario 1: External to CDE Access**
```
Objective: Verify external networks cannot access CDE
Test Steps:
1. Position testing system on corporate network
2. Attempt to ping CDE systems
3. Attempt to connect to CDE services
4. Scan for open ports on CDE
5. Attempt to access CDE applications
Expected Result: All attempts should fail
```

**Scenario 2: Guest WiFi to CDE**
```
Objective: Verify guest network isolation from CDE
Test Steps:
1. Connect to guest WiFi
2. Attempt to access CDE IP ranges
3. Test for routing to CDE
4. Attempt DNS lookups of CDE systems
5. Try to access corporate resources
Expected Result: Complete isolation from CDE
```

**Scenario 3: Workstation to CDE**
```
Objective: Verify corporate workstations cannot directly access CDE
Test Steps:
1. Use standard employee workstation
2. Attempt to access CDE systems
3. Try to connect to CDE databases
4. Test for unauthorized pathways
Expected Result: Access denied except through approved methods
```

**Scenario 4: Compromised System Lateral Movement**
```
Objective: Verify segmentation limits lateral movement
Test Steps:
1. Simulate compromised system outside CDE
2. Attempt privilege escalation
3. Try to pivot to CDE network
4. Test for vulnerable pathways
Expected Result: Unable to reach CDE from compromised system
```

### 4.4 Testing Tools

**Network Scanning:**
- Nmap: Port scanning and service detection
- Masscan: Fast network scanning
- Netcat: Network connectivity testing

**Vulnerability Assessment:**
- Nessus: Comprehensive vulnerability scanning
- OpenVAS: Open-source vulnerability scanner
- Qualys: Cloud-based scanning

**Penetration Testing:**
- Metasploit: Exploitation framework
- Burp Suite: Web application testing
- Wireshark: Network traffic analysis

### 4.5 Remediation Process

**When Segmentation Failures Found:**

1. **Immediate Actions:**
   - Document the finding
   - Assess risk level
   - Implement temporary controls if needed
   - Notify stakeholders

2. **Remediation:**
   - Develop fix plan
   - Implement corrective measures
   - Update documentation
   - Review related controls

3. **Re-testing:**
   - Verify fix effectiveness
   - Test related scenarios
   - Update test documentation
   - Obtain sign-off

## Section 5: Documentation Requirements

### 5.1 Essential Documentation

**Network Diagrams:**
- Current state network architecture
- Logical network topology
- Physical network layout
- Data flow diagrams
- Segmentation boundaries clearly marked

**Firewall Documentation:**
- Complete rule sets
- Rule justifications
- Review dates and ownership
- Change history
- Testing results

**Scope Documentation:**
- System inventory
- In-scope system list
- Connected system list
- Out-of-scope system justification
- Scope validation evidence

**Testing Documentation:**
- Test plans and procedures
- Testing results and evidence
- Remediation records
- Re-test results
- Sign-off documentation

### 5.2 Scope Determination Template

```markdown
# PCI DSS Scope Determination

## Organization Information
- Organization Name: [Company Name]
- Assessment Date: [Date]
- Assessor: [Name]
- Review Period: [Date Range]

## Cardholder Data Environment (CDE)

### System Components Storing CHD
| System Name | Description | Location | CHD Stored | Data Type |
|-------------|-------------|----------|------------|-----------|
| [System 1] | [Description] | [Location] | [Yes/No] | [Type] |

### System Components Processing CHD
| System Name | Description | Location | Processing Type |
|-------------|-------------|----------|-----------------|
| [System 1] | [Description] | [Location] | [Type] |

### System Components Transmitting CHD
| System Name | Description | Location | Transmission Method |
|-------------|-------------|----------|---------------------|
| [System 1] | [Description] | [Location] | [Method] |

## Connected Systems

### Systems Connecting to CDE
| System Name | Description | Connection Type | Security Controls |
|-------------|-------------|-----------------|-------------------|
| [System 1] | [Description] | [Type] | [Controls] |

## Segmentation Controls

### Implemented Controls
| Control Type | Description | Location | Effectiveness |
|--------------|-------------|----------|---------------|
| [Firewall] | [Description] | [Location] | [Status] |

### Testing Results
| Test Date | Test Type | Result | Tester | Notes |
|-----------|-----------|--------|--------|-------|
| [Date] | [Type] | [Pass/Fail] | [Name] | [Notes] |

## Scope Reduction Strategy

### Current Scope
- Total Systems: [Number]
- In-Scope Systems: [Number]
- Connected Systems: [Number]

### Segmentation Impact
- Systems Removed from Scope: [Number]
- Scope Reduction Percentage: [Percentage]

## Sign-Off
- Prepared by: [Name, Date]
- Reviewed by: [Name, Date]
- Approved by: [Name, Date]
```

### 5.3 Data Flow Diagram Template

```markdown
# Data Flow Diagram - [Process Name]

## Process Description
[Describe the business process involving cardholder data]

## Systems Involved
1. [System 1] - [Description]
2. [System 2] - [Description]
3. [System 3] - [Description]

## Data Flow Steps

Step 1: [Process Step]
- Source: [System/User]
- Destination: [System]
- Data Type: [CHD elements]
- Protocol: [HTTP/HTTPS/etc]
- Security Controls: [Encryption, authentication, etc]

Step 2: [Process Step]
- Source: [System/User]
- Destination: [System]
- Data Type: [CHD elements]
- Protocol: [Protocol]
- Security Controls: [Controls]

## Security Controls Matrix

| Flow Step | Encryption | Authentication | Logging | Monitoring |
|-----------|------------|----------------|---------|------------|
| Step 1 | [Yes/No] | [Method] | [Yes/No] | [Yes/No] |
| Step 2 | [Yes/No] | [Method] | [Yes/No] | [Yes/No] |

## Diagram
[Include visual representation of data flow]
```

## Section 6: Ongoing Maintenance

### 6.1 Regular Activities

**Monthly:**
- Review firewall logs
- Check for unauthorized changes
- Verify monitoring is active
- Review access logs

**Quarterly:**
- Review and update network diagrams
- Validate scope boundaries
- Review firewall rules
- Test key segmentation controls

**Semi-Annually:**
- Conduct full segmentation testing
- Review all documentation
- Update risk assessments
- Validate control effectiveness

**Annually:**
- Complete PCI DSS assessment
- Review segmentation strategy
- Update policies and procedures
- Comprehensive security review

### 6.2 Change Management

**All Changes Require:**
1. Change request documentation
2. Security impact assessment
3. Testing before implementation
4. Updated documentation
5. Rollback plan
6. Post-implementation validation

**Segmentation-Impacting Changes:**
- New system deployments
- Network configuration changes
- Firewall rule modifications
- VLAN changes
- Routing updates

### 6.3 Monitoring and Alerting

**Key Monitoring Points:**
- Firewall rule hits and blocks
- Unauthorized access attempts
- Configuration changes
- Network traffic patterns
- Segmentation control failures

**Alert Triggers:**
- Access from unexpected sources
- Failed authentication attempts
- Firewall rule violations
- Configuration drift
- Unusual traffic patterns

## Section 7: Common Challenges and Solutions

### 7.1 Challenge: Legacy System Integration

**Problem:** Old systems cannot support modern security controls

**Solutions:**
- Implement compensating controls
- Use additional network segmentation
- Deploy security proxies
- Plan for system replacement
- Extra monitoring and logging

### 7.2 Challenge: Business Resistance

**Problem:** Segmentation impacts business operations

**Solutions:**
- Clearly communicate security benefits
- Provide business impact analysis
- Offer alternative workflows
- Implement gradual rollout
- Provide adequate training

### 7.3 Challenge: Cloud Environments

**Problem:** Traditional segmentation doesn't apply to cloud

**Solutions:**
- Use cloud-native security groups
- Implement micro-segmentation
- Deploy cloud firewalls
- Use identity-based access controls
- Leverage cloud provider tools

### 7.4 Challenge: Third-Party Connections

**Problem:** Vendors need access to in-scope systems

**Solutions:**
- Dedicated VPN connections
- Strict firewall rules
- Time-limited access
- Multi-factor authentication
- Comprehensive logging
- Regular access reviews

## Section 8: Best Practices

### 8.1 Design Principles

1. **Defense in Depth:** Multiple layers of security controls
2. **Least Privilege:** Minimal access necessary
3. **Zero Trust:** Verify everything, trust nothing
4. **Simplicity:** Simple designs are easier to secure and maintain
5. **Documentation:** Comprehensive and current documentation

### 8.2 Implementation Tips

- Start with clear scope definition
- Document everything from the beginning
- Test before production deployment
- Implement change management early
- Train staff on new procedures
- Plan for ongoing maintenance
- Build relationships with auditors

### 8.3 Testing Tips

- Test from multiple network locations
- Use both automated and manual testing
- Document all test activities
- Keep detailed evidence
- Remediate findings promptly
- Re-test after remediation
- Maintain testing schedule

## Section 9: Practical Implementation Example

### 9.1 Example Scenario

**Organization:** Mid-sized retail company
**Challenge:** E-commerce website shares infrastructure with corporate network
**Goal:** Implement segmentation to reduce PCI scope

### 9.2 Current State Assessment

**In-Scope Systems:**
- Web servers (2): 192.168.10.10-11
- Application servers (2): 192.168.10.20-21
- Database server (1): 192.168.10.30
- Payment gateway: 192.168.10.40

**Connected Systems:**
- Corporate network: 192.168.1.0/24
- Guest WiFi: 192.168.100.0/24
- Management network: 192.168.200.0/24

### 9.3 Segmentation Design

**New Network Structure:**

**CDE Segment:** 10.0.1.0/24
- Payment processing systems only
- Strict firewall controls
- No direct internet access
- Dedicated monitoring

**DMZ Segment:** 10.0.2.0/24
- Web servers
- Application servers
- Limited CDE access
- Internet-facing

**Corporate Segment:** 192.168.1.0/24
- Employee workstations
- No CDE access
- Standard security controls

**Management Segment:** 10.0.3.0/24
- IT administration
- Jump boxes for CDE access
- Enhanced logging

### 9.4 Firewall Rules

```
# CDE Inbound Rules
deny ip any 10.0.1.0/24  # Default deny

# Allow application servers to CDE
permit tcp 10.0.2.20-21 10.0.1.0/24 eq 443
  Description: App servers to payment gateway
  Justification: Process transactions
  Review: 2026-08-01

# Allow management access via jump box
permit tcp host 10.0.3.10 10.0.1.0/24 eq 22
  Description: SSH from jump box
  Justification: Administrative access
  Review: 2026-08-01

# CDE Outbound Rules
deny ip 10.0.1.0/24 any  # Default deny

# Allow payment gateway to processor
permit tcp host 10.0.1.40 host 203.0.113.50 eq 443
  Description: Payment processor connection
  Justification: Submit transactions
  Review: 2026-08-01
```

### 9.5 Testing Plan

**Test 1:** Corporate to CDE
- Source: 192.168.1.50 (workstation)
- Target: 10.0.1.0/24
- Expected: Denied

**Test 2:** DMZ to CDE
- Source: 10.0.2.20 (app server)
- Target: 10.0.1.40:443 (payment gateway)
- Expected: Allowed

**Test 3:** Internet to CDE
- Source: External IP
- Target: 10.0.1.0/24
- Expected: Denied

## Section 10: Resources and References

### 10.1 PCI DSS Documentation

- PCI DSS v4.0 Requirements
- Information Supplement: PCI DSS Scoping and Segmentation
- PCI DSS Glossary of Terms
- PCI DSS Network Segmentation Good Practices

### 10.2 Useful Tools

**Network Documentation:**
- Draw.io: Network diagrams
- Lucidchart: Process flows
- Microsoft Visio: Professional diagrams

**Testing Tools:**
- Nmap: Network scanning
- Metasploit: Penetration testing
- Wireshark: Traffic analysis
- Nessus: Vulnerability scanning

**Compliance Management:**
- ServiceNow GRC
- RSA Archer
- MetricStream
- Compliance.ai

### 10.3 Training Resources

- PCI SSC Training Programs
- SANS Security Training
- (ISC)² Certification Programs
- Cloud Security Alliance

## Conclusion

Implementing effective network segmentation for PCI DSS compliance requires careful planning, methodical execution, rigorous testing, and ongoing maintenance. This guide provides the foundation for a successful segmentation project that will:

- Reduce PCI DSS scope
- Enhance security posture
- Streamline compliance efforts
- Protect cardholder data

Remember that segmentation is not a one-time project but an ongoing process that requires regular review, testing, and updates to remain effective.

## Appendices

### Appendix A: Glossary

- **CDE:** Cardholder Data Environment
- **CHD:** Cardholder Data
- **PAN:** Primary Account Number
- **PCI DSS:** Payment Card Industry Data Security Standard
- **QSA:** Qualified Security Assessor
- **SAD:** Sensitive Authentication Data
- **VLAN:** Virtual Local Area Network

### Appendix B: Sample Documentation

See templates throughout this guide for:
- Scope determination
- Data flow diagrams
- Testing procedures
- Network diagrams
- Firewall rule documentation

### Appendix C: Checklist

**Pre-Implementation:**
- [ ] Scope defined and documented
- [ ] Network diagrams created
- [ ] Segmentation strategy approved
- [ ] Resources allocated
- [ ] Timeline established

**Implementation:**
- [ ] Infrastructure deployed
- [ ] Firewalls configured
- [ ] VLANs implemented
- [ ] Access controls deployed
- [ ] Monitoring configured

**Testing:**
- [ ] Test plan created
- [ ] Testing completed
- [ ] Findings remediated
- [ ] Re-testing completed
- [ ] Results documented

**Documentation:**
- [ ] Network diagrams updated
- [ ] Firewall rules documented
- [ ] Test results recorded
- [ ] Scope documentation completed
- [ ] Evidence collected

**Ongoing:**
- [ ] Monitoring in place
- [ ] Change management implemented
- [ ] Regular reviews scheduled
- [ ] Staff trained
- [ ] Audit readiness maintained
