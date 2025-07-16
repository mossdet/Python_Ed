# Module 3.3: Requirements Elicitation Techniques

## Table of Contents
- [Overview](#overview)
- [Interviews and Surveys](#interviews-and-surveys)
- [Observation and Ethnography](#observation-and-ethnography)
- [Workshops and Brainstorming](#workshops-and-brainstorming)
- [Prototyping for Requirements](#prototyping-for-requirements)
- [Use Cases and User Stories](#use-cases-and-user-stories)
- [Advanced and Hybrid Techniques](#advanced-and-hybrid-techniques)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Overview

Requirements elicitation is the process of gathering information from stakeholders, domain experts, users, and other sources to discover what a system should do. It is a complex, iterative, and often creative activity that requires a blend of technical, analytical, and interpersonal skills.

**Advanced Theory:**
- Elicitation is not just about asking questions; it is about uncovering needs, constraints, and assumptions that may not be explicitly stated.
- Requirements are often emergent and evolve as stakeholders better understand their needs.
- Cognitive biases (e.g., confirmation bias, anchoring) can affect both stakeholders and analysts during elicitation.

**Research Trends:**
- Automated elicitation using chatbots and AI agents.
- Mining requirements from user reviews, support tickets, and system logs.

**Best Practices:**
- Use a combination of techniques to triangulate and validate requirements.
- Document assumptions and rationale for each requirement.

## Interviews and Surveys

**Interviews:**
- One-on-one or group sessions with stakeholders to gather detailed insights.
- Types: structured (fixed questions), semi-structured (guided but flexible), unstructured (open conversation).
- Useful for exploring complex or sensitive topics.

**Surveys/Questionnaires:**
- Used to collect data from a large group efficiently.
- Can be paper-based or online (e.g., Google Forms, SurveyMonkey).
- Useful for quantitative analysis, trend identification, and prioritization.

**Advanced Theory:**
- Interviews can be combined with laddering techniques to uncover underlying motivations.
- Surveys can use Likert scales, ranking, or open-ended questions for richer data.

**Real-World Example:**
In a public sector IT project, interviews with end-users revealed regulatory constraints not captured in initial documentation.

**Best Practices:**
- Prepare questions in advance, use open-ended questions, record sessions, and validate findings with participants.
- Avoid leading questions and be aware of stakeholder bias.

## Observation and Ethnography

**Observation:**
- Watching users interact with current systems, processes, or environments to uncover tacit requirements and pain points.
- Types: passive (no interaction), active (asking clarifying questions during observation).

**Ethnography:**
- Immersive, long-term observation in the user's environment.
- Reveals context, workarounds, and cultural or organizational factors.

**Advanced Theory:**
- Shadowing: Analyst follows a user through their daily tasks.
- Contextual inquiry: Combines observation with real-time questioning.

**Real-World Example:**
Ethnographic studies in hospitals reveal workflow issues and safety risks not captured in interviews, leading to better system design.

**Best Practices:**
- Take detailed notes, use video/audio recording (with consent), triangulate findings with other methods.

## Workshops and Brainstorming

**Workshops:**
- Collaborative sessions with multiple stakeholders to generate, clarify, and prioritize requirements.
- Types: Joint Application Development (JAD), requirements review workshops, prioritization workshops.
- Foster consensus, resolve conflicts, and build shared understanding.

**Brainstorming:**
- Creative group technique to generate ideas rapidly without criticism.
- Can be structured (round-robin) or unstructured.

**Advanced Theory:**
- Requirements games (e.g., Innovation Games) and storyboarding can make workshops more engaging and productive.
- Nominal Group Technique (NGT) for structured idea generation and ranking.

**Real-World Example:**
In a fintech project, a requirements workshop with business, IT, and compliance teams surfaced conflicting priorities, which were resolved through facilitated negotiation.

**Best Practices:**
- Use skilled facilitators, set clear goals, document outcomes, encourage participation from all attendees.

## Prototyping for Requirements

**Prototyping:**
- Building mockups, wireframes, or interactive models to clarify requirements and get early feedback.
- Types: low-fidelity (paper sketches, wireframes), high-fidelity (interactive software, clickable demos).

**Benefits:**
- Reduces ambiguity, uncovers hidden needs, supports validation and buy-in.
- Enables early usability testing and design iteration.

**Pitfalls:**
- Stakeholders may mistake prototypes for final products, leading to unrealistic expectations.
- Can be time-consuming if not scoped appropriately.

**Advanced Theory:**
- Throwaway vs. evolutionary prototyping: throwaway is discarded, evolutionary becomes part of the final system.

**Real-World Example:**
In e-commerce, rapid prototyping of checkout flows helps identify usability issues before development.

## Use Cases and User Stories

**Use Cases:**
- Describe system interactions from the user's perspective, often using UML diagrams.
- Useful for complex systems, formal documentation, and identifying system boundaries.
- Include main flow, alternate flows, and exceptions.

**User Stories:**
- Short, simple descriptions of a feature told from the perspective of the user ("As a user, I want...").
- Common in agile development; focus on value and outcomes.
- Should be INVEST: Independent, Negotiable, Valuable, Estimable, Small, Testable.

**Advanced Theory:**
- Use case maps and story mapping for visualizing user journeys.

**Real-World Example:**
In a SaaS product, user stories drive sprint planning, while use cases document complex integrations.

**Best Practices:**
- Use acceptance criteria for clarity and testability.
- Keep stories small and focused; split large stories as needed.

## Advanced and Hybrid Techniques

**Goal-Oriented Requirements Engineering (GORE):**
- Focuses on stakeholder goals and decomposes them into functional and non-functional requirements.
- Uses goal models (e.g., KAOS, i*) to structure requirements.

**Scenario-Based Elicitation:**
- Uses real-world scenarios, storyboards, or scripts to uncover requirements and validate understanding.

**Hybrid Approaches:**
- Combine multiple techniques (e.g., interviews + prototyping + workshops) for comprehensive coverage and cross-validation.

**Research Trends:**
- Automated elicitation using NLP, mining requirements from user feedback, logs, and social media.
- Use of AI agents and chatbots for interactive elicitation.

**Best Practices:**
- Tailor elicitation approach to project context, stakeholder profiles, and system complexity.

## Review Questions
1. Compare and contrast interviews, surveys, and workshops as elicitation techniques. Discuss their strengths, weaknesses, and when each is most effective.
2. How does ethnography and contextual inquiry contribute to requirements discovery, and what are their limitations?
3. What are the benefits and risks of using prototyping in requirements elicitation? How can pitfalls be avoided?
4. Explain the difference between use cases and user stories. Provide examples and discuss when each is preferred.
5. Describe a hybrid elicitation approach, its advantages, and potential challenges.
6. What are current research trends in automated and AI-driven requirements elicitation?
7. How can cognitive biases affect requirements elicitation, and what strategies can mitigate them?

## Answers to Review Questions
1. Interviews provide depth and flexibility but are time-consuming and may be biased; surveys reach many but lack detail and context; workshops foster consensus and creativity but require skilled facilitation and can be dominated by strong personalities. Interviews are best for complex or sensitive topics, surveys for broad input, workshops for consensus-building.
2. Ethnography and contextual inquiry uncover tacit requirements and real-world context by observing users in their environment, but can be time-consuming, intrusive, and may not capture rare events.
3. Prototyping clarifies needs, supports validation, and uncovers hidden requirements, but may set unrealistic expectations if mistaken for the final product. Pitfalls can be avoided by clear communication and scoping.
4. Use cases are detailed, formal, and suited for complex systems (e.g., "User logs in and views dashboard"); user stories are brief, agile-friendly, and ideal for iterative development (e.g., "As a user, I want to view my dashboard"). Use cases for documentation and integration, user stories for agile planning.
5. Hybrid approaches (e.g., combining interviews, workshops, and prototyping) provide comprehensive coverage, cross-validation, and stakeholder engagement, but can be resource-intensive and require careful coordination.
6. Trends include automated elicitation using NLP, mining requirements from user feedback, AI-driven chatbots, and scenario-based techniques.
7. Cognitive biases (e.g., confirmation bias, anchoring) can distort elicitation. Mitigation strategies include using multiple techniques, involving diverse stakeholders, and structured facilitation.
