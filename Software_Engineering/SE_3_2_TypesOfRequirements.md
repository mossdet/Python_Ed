# Module 3.2: Types of Requirements

## Table of Contents
- [Overview](#overview)
- [Functional Requirements](#functional-requirements)
- [Non-Functional Requirements](#non-functional-requirements)
- [System vs. User Requirements](#system-vs-user-requirements)
- [Requirements Prioritization](#requirements-prioritization)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Overview

Requirements are the foundation of any software system. They describe what the system should do (functional) and how it should perform (non-functional). A deep understanding of requirement types is essential for effective requirements engineering, risk management, and successful project outcomes.

**Advanced Theory:**
- Requirements can be explicit (stated by stakeholders) or implicit (assumed or derived from context, standards, or regulations).
- Requirements are often layered: business, user, system, and derived requirements.
- Requirements quality directly impacts downstream activities (design, testing, maintenance).

**Research Trends:**
- Automated classification of requirements using NLP and machine learning.
- Requirements mining from legacy systems and user feedback.

**Best Practices:**
- Use clear, unambiguous language and avoid technical jargon for user requirements.
- Validate requirements with all stakeholders to avoid misinterpretation.

## Functional Requirements

Functional requirements define the specific behavior, functions, and features the system must provide. They answer the question: "What should the system do?"

**Characteristics:**
- Should be clear, complete, consistent, and testable.
- Expressed as use cases, user stories, or functional specifications.
- Often decomposed into sub-functions and mapped to system modules.

**Examples:**
- User authentication and authorization
- Data processing and transformation
- Generating reports and analytics
- Integration with external APIs

**Real-World Example:**
In an online banking system, a functional requirement might be: "The system shall allow users to transfer funds between accounts."

**Best Practices:**
- Use acceptance criteria to make requirements testable.
- Involve end-users in reviewing functional requirements.

## Non-Functional Requirements

Non-functional requirements (NFRs) specify the quality attributes, constraints, and standards the system must meet. They answer the question: "How well should the system perform its functions?"

**Categories:**
- Performance (e.g., response time, throughput)
- Security (e.g., authentication, encryption, auditability)
- Usability (e.g., learnability, accessibility)
- Reliability (e.g., uptime, fault tolerance)
- Scalability (e.g., ability to handle growth)
- Maintainability (e.g., modularity, documentation)
- Compliance (e.g., GDPR, HIPAA)

**Advanced Theory:**
- NFRs are often cross-cutting and impact architectural decisions.
- Some NFRs may conflict (e.g., security vs. usability) and require trade-off analysis.

**Examples:**
- "The system must handle 10,000 concurrent users."
- "System uptime must be at least 99.99%."
- "All user data must be encrypted at rest and in transit."

**Real-World Example:**
For a medical device, regulatory compliance (e.g., FDA approval) is a critical non-functional requirement.

**Best Practices:**
- Quantify NFRs wherever possible (e.g., "response time < 2s").
- Validate NFRs through performance, security, and usability testing.

## System vs. User Requirements

**User Requirements:**
- High-level statements of what users need the system to do.
- Written in natural language, diagrams, or mockups for non-technical stakeholders.
- Focus on goals, not implementation.

**System Requirements:**
- Detailed, technical descriptions of system functions, interfaces, and constraints.
- Used by developers, testers, and maintainers.
- Often derived from user requirements and refined for implementation.

**Traceability:**
- User requirements are refined into system requirements through analysis and decomposition.
- Traceability matrices ensure every user requirement is addressed by one or more system requirements.

**Real-World Example:**
User requirement: "The system should allow customers to view their order history."
System requirement: "The system shall retrieve and display all orders associated with a user's account from the database within 2 seconds."

**Best Practices:**
- Maintain bidirectional traceability between user and system requirements.
- Use tools to automate traceability and impact analysis.

## Requirements Prioritization

Not all requirements are equally important; prioritization is essential for project planning, risk management, and scope control.

**Techniques:**
- **MoSCoW:** Must have, Should have, Could have, Won't have
- **Kano Model:** Classifies features as basic, performance, or excitement factors
- **Value-Based Prioritization:** Focuses on business value and ROI
- **Risk-Based Prioritization:** Addresses high-risk or high-impact requirements first
- **100-Dollar Test:** Stakeholders allocate a budget to requirements

**Advanced Theory:**
- Prioritization is iterative and may change as the project evolves.
- In large projects, automated tools and algorithms (e.g., AHP, cost-value approach) are used.

**Real-World Example:**
In agile development, the product backlog is continuously reprioritized based on stakeholder feedback and changing business needs.

**Best Practices:**
- Involve all key stakeholders in prioritization decisions.
- Revisit priorities regularly, especially in agile and dynamic environments.

## Review Questions
1. Explain the difference between functional and non-functional requirements, and discuss why both are critical for project success.
2. Compare user requirements and system requirements, providing examples of each and explaining the importance of traceability.
3. Why is requirements prioritization essential in large and agile projects? Describe at least three prioritization techniques and their advantages.
4. List and explain at least four categories of non-functional requirements, and discuss how they influence system architecture.
5. How can conflicting non-functional requirements be identified and resolved during requirements engineering?
6. Discuss current research trends in requirements classification and prioritization.

## Answers to Review Questions
1. Functional requirements specify what the system should do (e.g., login feature); non-functional requirements specify how the system should perform (e.g., response time < 2s). Both are critical: missing functional requirements leads to incomplete systems, while missing non-functional requirements leads to poor quality or unusable systems.
2. User requirements are high-level, user-focused statements (e.g., "The system should allow users to reset their password"); system requirements are detailed, technical specifications (e.g., "The system shall send a password reset email within 1 minute of request"). Traceability ensures all user needs are addressed and facilitates change management.
3. Prioritization is essential to deliver value early, manage scope, and address risks. Techniques: MoSCoW (simple, stakeholder-friendly), Kano (focuses on user satisfaction), value-based (aligns with business goals). Each has strengths depending on context.
4. Performance (affects user experience and scalability), security (protects data and compliance), usability (drives adoption), reliability (ensures uptime and trust). These influence architecture, technology choices, and testing.
5. Conflicting NFRs (e.g., security vs. usability) can be identified through stakeholder workshops and trade-off analysis. Resolution involves negotiation, prototyping, and architectural decisions.
6. Research trends include automated classification of requirements using NLP, mining requirements from user feedback, and algorithmic prioritization using AI and value models.
