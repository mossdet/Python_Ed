# Module 2.5: Projects to Build

## Table of Contents
- [Introduction](#introduction)
- [Project 1: Requirements Management System](#project-1-requirements-management-system)
- [Project 2: Automated Testing Framework](#project-2-automated-testing-framework)
- [Project 3: DevOps CI/CD Pipeline](#project-3-devops-cicd-pipeline)
- [Project 4: Agile Project Tracker](#project-4-agile-project-tracker)
- [Project 5: SRE Monitoring Dashboard](#project-5-sre-monitoring-dashboard)
- [Review Questions](#review-questions)
- [Answers to Review Questions](#answers-to-review-questions)

---

## Introduction
This module provides hands-on projects that reinforce the concepts and practices of the SDLC. Each project is designed to integrate requirements engineering, design, implementation, testing, deployment, and maintenance, using both traditional and modern SDLC approaches. Projects are suitable for individual or team work and can be adapted for research or industry contexts.

## Project 1: Requirements Management System
- **Objective:** Build a web-based tool for capturing, managing, and tracing software requirements.
- **Key Features:** Requirements CRUD, traceability matrix, versioning, stakeholder roles, export/import.
- **SDLC Focus:** Requirements engineering, documentation, change management.
- **Advanced:** Integrate NLP for requirements extraction, support for regulatory compliance (e.g., ISO 9001).

### Example: Minimal Flask-based Requirements Manager
```python
from flask import Flask, request, jsonify
app = Flask(__name__)

requirements = []

@app.route('/requirements', methods=['GET', 'POST'])
def manage_requirements():
    if request.method == 'POST':
        req = request.json
        req['id'] = len(requirements) + 1
        requirements.append(req)
        return jsonify(req), 201
    return jsonify(requirements)

@app.route('/requirements/<int:req_id>', methods=['GET', 'PUT', 'DELETE'])
def requirement_detail(req_id):
    req = next((r for r in requirements if r['id'] == req_id), None)
    if not req:
        return jsonify({'error': 'Not found'}), 404
    if request.method == 'PUT':
        data = request.json
        req.update(data)
        return jsonify(req)
    if request.method == 'DELETE':
        requirements.remove(req)
        return '', 204
    return jsonify(req)

if __name__ == '__main__':
    app.run(debug=True)
```
**Implementation Notes:**
- Extend with user authentication, versioning, and export/import (e.g., CSV, JSON).
- For NLP, use `spaCy` or `transformers` to extract requirements from documents.
- For traceability, implement a matrix as a table mapping requirements to test cases.

## Project 2: Automated Testing Framework
- **Objective:** Develop a framework for automated unit, integration, and system testing.
- **Key Features:** Test case management, test execution, reporting, CI/CD integration.
- **SDLC Focus:** Verification and validation, test automation, continuous integration.
- **Advanced:** Support for property-based and model-based testing, mutation testing, AI-based test generation.

### Example: Minimal Python Test Runner
```python
import unittest

class TestMath(unittest.TestCase):
    def test_add(self):
        self.assertEqual(1 + 1, 2)

    def test_subtract(self):
        self.assertEqual(5 - 3, 2)

if __name__ == '__main__':
    suite = unittest.TestLoader().loadTestsFromTestCase(TestMath)
    unittest.TextTestRunner(verbosity=2).run(suite)
```
**Implementation Notes:**
- Use `pytest` for advanced features (fixtures, parameterization).
- For property-based testing, use `hypothesis`:
```python
from hypothesis import given
import hypothesis.strategies as st

@given(st.integers(), st.integers())
def test_add_commutative(a, b):
    assert a + b == b + a
```
- For mutation testing, use `mutmut` or `cosmic-ray`.

## Project 3: DevOps CI/CD Pipeline
- **Objective:** Implement a complete CI/CD pipeline using modern DevOps tools.
- **Key Features:** Source control, automated build/test/deploy, monitoring, rollback, infrastructure as code.
- **SDLC Focus:** Deployment, automation, monitoring, feedback loops.
- **Advanced:** Integrate security scanning (DevSecOps), blue-green/canary deployments, pipeline analytics.

### Example: Python CI/CD with GitHub Actions
**.github/workflows/python-app.yml**
```yaml
name: Python application
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: '3.9'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run tests
      run: |
        pytest
```
**Implementation Notes:**
- Use `Docker` for containerization and `docker-compose` for multi-service orchestration.
- For monitoring, integrate with `Prometheus` and `Grafana`.
- For blue-green/canary deployments, use `Kubernetes` and `Argo Rollouts`.

## Project 4: Agile Project Tracker
- **Objective:** Create a tool for managing Agile projects (Scrum/Kanban).
- **Key Features:** Backlog management, sprint planning, Kanban board, burndown charts, velocity tracking.
- **SDLC Focus:** Agile planning, iterative development, stakeholder collaboration.
- **Advanced:** Support for scaled Agile (SAFe), integration with communication tools (Slack, Teams).

### Example: Minimal Python Kanban Board (CLI)
```python
class Task:
    def __init__(self, title, status='To Do'):
        self.title = title
        self.status = status

tasks = [Task('Setup repo'), Task('Write docs')]

def show_board():
    for status in ['To Do', 'In Progress', 'Done']:
        print(f'\n{status}:')
        for t in tasks:
            if t.status == status:
                print(f'  - {t.title}')

def move_task(title, new_status):
    for t in tasks:
        if t.title == title:
            t.status = new_status

show_board()
move_task('Setup repo', 'In Progress')
show_board()
```
**Implementation Notes:**
- Extend with a web UI using `Flask` or `Django`.
- Add burndown chart generation using `matplotlib`.
- Integrate with Slack using `slack_sdk` for notifications.

## Project 5: SRE Monitoring Dashboard
- **Objective:** Build a dashboard for monitoring SLIs, SLOs, and error budgets.
- **Key Features:** Real-time metrics, alerting, incident tracking, blameless postmortems.
- **SDLC Focus:** Site reliability engineering, observability, incident response.
- **Advanced:** Predictive analytics for incident prevention, automated incident response, integration with cloud monitoring APIs.

### Example: Minimal SLI/SLO Monitor in Python
```python
import time
import random

SLO = 0.99  # 99% uptime
window = 1000  # requests
successes = 0
for i in range(window):
    if random.random() < 0.995:  # simulate 99.5% success
        successes += 1
    time.sleep(0.001)
SLI = successes / window
print(f'SLI: {SLI:.3f}, SLO: {SLO}, Error Budget: {1 - SLO:.3f}')
if SLI < SLO:
    print('Alert: SLO violated!')
```
**Implementation Notes:**
- Use `Flask` and `Plotly Dash` for a real-time dashboard.
- Integrate with Prometheus for real metrics.
- Store incidents and postmortems in a database (e.g., SQLite, PostgreSQL).

## Review Questions
1. For each project, identify which SDLC phases are most critical and why.
2. How can automation and AI enhance the effectiveness of these projects?
3. Discuss the challenges of integrating compliance and security into SDLC projects.
4. How would you adapt these projects for a regulated industry (e.g., healthcare, finance)?
5. Propose metrics to evaluate the success of each project.

## Answers to Review Questions
1. Requirements management: requirements and change management; testing framework: verification/validation; CI/CD: deployment/monitoring; Agile tracker: planning/iteration; SRE dashboard: monitoring/incident response.
2. Automation and AI can accelerate requirements extraction, test generation, deployment, monitoring, and incident response, improving quality and speed.
3. Integrating compliance/security requires early consideration, automated checks, and traceability. Challenges include evolving regulations and tool integration.
4. For regulated industries, add audit trails, access controls, compliance reporting, and validation workflows.
5. Metrics: requirements coverage, test pass rate, deployment frequency, lead time, incident MTTR, user satisfaction, compliance rate.
