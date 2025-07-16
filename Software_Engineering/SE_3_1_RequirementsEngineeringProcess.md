# Module 3.1: Requirements Engineering Process

## Table of Contents
- [Overview](#overview)
- [Requirements Elicitation](#requirements-elicitation)
- [Requirements Analysis and Specification](#requirements-analysis-and-specification)
- [Requirements Validation and Verification](#requirements-validation-and-verification)
- [Requirements Management](#requirements-management)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Overview

Requirements engineering (RE) is the systematic process of discovering, documenting, validating, and managing the requirements for a software system. It is foundational to the success of any software project, as requirements errors are among the most costly and difficult to correct later in the lifecycle.

**Definition:**
> Requirements engineering is a set of activities concerned with identifying and communicating the purpose of a software-intensive system, and the contexts in which it will be used. (Zave, 1997)

**Key Activities:**
- Elicitation: Discovering requirements from stakeholders and other sources
- Analysis: Refining and structuring requirements
- Specification: Documenting requirements in a clear, precise, and testable manner
- Validation: Ensuring requirements are correct, complete, and feasible
- Management: Handling changes and maintaining traceability

**Importance:**
- Studies (e.g., Standish Group CHAOS Report) show that poor requirements are a leading cause of project failure.
- Requirements errors propagate: fixing them late is 10–100x more expensive than early.
- Good RE improves stakeholder satisfaction, reduces rework, and increases project success rates.

**Real-World Example:**
The Ariane 5 rocket failure (1996) was due to a requirements oversight—reuse of software from Ariane 4 without proper requirements analysis for the new context, resulting in a $370 million loss.

**Research Trends:**
- Automated requirements analysis using NLP and AI
- Model-driven requirements engineering
- Requirements engineering for agile and DevOps

**Best Practices:**
- Involve all key stakeholders early and continuously
- Use multiple elicitation techniques
- Maintain a living requirements document

## Requirements Elicitation

Requirements elicitation is the process of collecting requirements from stakeholders, domain experts, and other sources. It is often the most challenging and critical part of RE, as stakeholders may not know, articulate, or agree on what they want.

**Techniques:**
- **Interviews:** Structured, semi-structured, or unstructured conversations with stakeholders. Useful for deep insights but time-consuming.
- **Surveys/Questionnaires:** Efficient for large groups, but may lack depth.
- **Workshops/Focus Groups:** Collaborative sessions to generate ideas, resolve conflicts, and build consensus.
- **Observation/Ethnography:** Watching users in their environment to uncover tacit requirements.
- **Prototyping:** Building mockups or prototypes to clarify needs and get early feedback.
- **Document Analysis:** Reviewing existing documentation, regulations, or legacy systems.

**Advanced Elicitation:**
- Use of requirements games (e.g., Innovation Games)
- Storyboarding and scenario-based elicitation
- Goal-oriented requirements engineering (GORE)

**Challenges:**
- Stakeholder availability and engagement
- Communication barriers (language, culture, technical jargon)
- Conflicting requirements and priorities
- Tacit knowledge and unspoken assumptions

**Real-World Example:**
In large healthcare IT projects, ethnographic observation often reveals workflow requirements that stakeholders cannot articulate in interviews.

**Best Practices:**
- Triangulate using multiple techniques
- Record and validate findings with stakeholders

## Requirements Analysis and Specification

Requirements analysis involves refining, structuring, and prioritizing requirements to ensure they are clear, complete, consistent, and feasible. Specification is the process of documenting these requirements in a form suitable for validation, development, and testing.

**Analysis Activities:**
- **Classification:** Grouping requirements (e.g., functional, non-functional, domain, interface)
- **Prioritization:** MoSCoW, Kano model, value-based prioritization
- **Conflict Resolution:** Negotiation, trade-off analysis
- **Feasibility Analysis:** Technical, economic, legal, and operational feasibility

**Specification Techniques:**
- **Software Requirements Specification (SRS):** IEEE 830/29148 standard
- **User Stories:** "As a [user], I want [goal] so that [reason]"
- **Use Cases:** UML-based scenarios describing interactions
- **Formal Methods:** Z, VDM, Alloy for safety-critical systems

**Tools:**
- Requirements management tools: IBM DOORS, Jama, ReqIF Studio
- Modeling tools: Enterprise Architect, Visual Paradigm

**Research Trends:**
- Automated requirements analysis and inconsistency detection
- Natural language processing for requirements quality

**Best Practices:**
- Use templates and checklists for consistency
- Involve stakeholders in reviews

## Requirements Validation and Verification

Requirements validation ensures that the documented requirements accurately reflect stakeholder needs and are feasible. Verification checks that requirements are well-formed, consistent, and testable.

**Validation Techniques:**
- **Reviews and Inspections:** Systematic examination by stakeholders and experts
- **Prototyping:** Early models to validate requirements
- **Model Validation:** Using UML or formal models to check logic
- **Acceptance Criteria:** Defining clear, testable acceptance conditions

**Verification Techniques:**
- **Traceability Analysis:** Ensuring each requirement is linked to design, code, and tests
- **Test Case Generation:** Deriving tests from requirements
- **Consistency and Completeness Checks:** Automated or manual

**Real-World Example:**
In safety-critical systems (e.g., aviation), formal verification of requirements is mandated by standards like DO-178C.

**Best Practices:**
- Involve end-users in validation
- Use traceability matrices to manage changes

## Requirements Management

Requirements management is the process of documenting, analyzing, tracing, prioritizing, and agreeing on requirements, and controlling changes throughout the project lifecycle.

**Key Activities:**
- **Traceability:** Mapping requirements to design, implementation, and tests
- **Change Management:** Handling evolving requirements, impact analysis
- **Version Control:** Tracking changes and rationale
- **Prioritization:** Revisiting priorities as project evolves

**Tools:**
- Requirements traceability matrices (RTM)
- Change management systems (e.g., JIRA, Jama)

**Research Trends:**
- Automated traceability using AI
- Requirements management in agile and DevOps

**Best Practices:**
- Establish a change control board (CCB)
- Communicate changes to all stakeholders

## Review Questions
1. Define requirements engineering and discuss its impact on software project success, citing real-world examples.
2. Compare and contrast at least four requirements elicitation techniques, including their strengths and limitations.
3. What are the main activities in requirements analysis, and how do they contribute to high-quality specifications?
4. Explain the difference between requirements validation and verification, and describe advanced techniques for each.
5. Discuss the role of requirements management in agile and large-scale projects. How do modern tools support this process?
6. What are current research trends in requirements engineering, and how might they shape the future of the field?

## Answers to Review Questions
1. Requirements engineering is the systematic process of discovering, documenting, validating, and managing requirements. Its impact is profound: studies show that poor requirements are a leading cause of project failure (e.g., Ariane 5 rocket, Denver Airport baggage system). Good RE reduces rework, increases stakeholder satisfaction, and improves project outcomes.
2. Interviews (deep insights, but time-consuming), surveys (broad reach, less depth), workshops (consensus-building, but may be dominated by strong personalities), observation (uncovers tacit needs, but may miss rationale), prototyping (clarifies needs, but can set unrealistic expectations).
3. Main activities: classification (organizes requirements), prioritization (focuses effort), conflict resolution (ensures agreement), feasibility analysis (ensures practicality). These activities ensure requirements are clear, complete, and actionable, leading to high-quality specifications.
4. Validation ensures requirements meet stakeholder needs (e.g., reviews, prototyping, acceptance criteria). Verification checks requirements are well-formed and testable (e.g., traceability analysis, test case generation, formal methods). Advanced techniques include model checking and automated consistency analysis.
5. Requirements management is critical in agile (where requirements evolve rapidly) and large-scale projects (where traceability and change control are essential). Modern tools (e.g., JIRA, Jama) automate traceability, change management, and impact analysis, supporting collaboration and compliance.
6. Current research trends include AI-driven requirements analysis, model-driven engineering, and requirements management for agile/DevOps. These trends promise to automate quality checks, improve traceability, and better integrate RE with modern development practices.
