# AI Test Engineering

AI Test Engineering applies software testing and quality engineering principles to AI-enabled applications and AI-assisted software systems.

The objective is not simply to verify whether an AI system produces an answer.

The objective is to determine whether the system produces **correct, relevant, grounded, safe, reliable, measurable, and production-ready behavior**.

AI Test Engineering extends traditional SDET practices to cover probabilistic behavior, model evaluation, prompt validation, retrieval, agents, tools, safety, observability, and continuous evaluation.

---

## Scope

AI Test Engineering can cover the complete AI application stack:

```text
User Input
    |
    v
Application
    |
    v
Prompt / Instructions
    |
    v
Model
    |
    +----> Retrieval / RAG
    |
    +----> Tools
    |
    +----> Agent Logic
    |
    v
AI Response
    |
    v
Evaluation
    |
    v
Quality Gates
    |
    v
Release / Production
```

Testing should evaluate the complete system rather than the model in isolation.

---

## AI Quality Dimensions

AI systems should be evaluated across multiple quality dimensions.

| Quality Dimension | What to Validate |
|---|---|
| Correctness | Whether the response is factually and functionally correct |
| Relevance | Whether the response addresses the requested objective |
| Grounding | Whether responses are supported by trusted context or sources |
| Consistency | Whether similar inputs produce acceptable behavior |
| Safety | Whether harmful or unsafe outputs are prevented |
| Robustness | Whether the system handles unexpected inputs |
| Explainability | Whether decisions or responses can be reasonably understood |
| Performance | Response latency and throughput |
| Cost | Token, model, infrastructure, and execution cost |
| Governance | Policy, compliance, auditability, and access controls |

AI quality therefore requires more than traditional pass/fail functional testing.

---

## Input Testing

Input validation is the first layer of AI test engineering.

Test inputs should include:

- Valid inputs
- Invalid inputs
- Empty inputs
- Null inputs
- Boundary inputs
- Large inputs
- Unexpected formats
- Ambiguous requests
- Conflicting requests
- Malicious inputs
- Adversarial inputs
- Unsupported requests

### Example Input Test Matrix

| Input Type | Example | Expected Behavior |
|---|---|---|
| Valid | Supported user request | Correct response |
| Empty | Empty prompt | Validation or appropriate response |
| Invalid | Unsupported format | Graceful handling |
| Boundary | Maximum supported input | Correct handling |
| Large | Extremely large input | Controlled behavior |
| Ambiguous | Unclear request | Clarification or safe response |
| Adversarial | Manipulated instruction | Guardrails maintained |

---

## Prompt Testing

Prompt testing validates whether AI systems correctly follow system instructions, user instructions, constraints, and expected behavior.

Test scenarios should include:

- Valid prompts
- Invalid prompts
- Conflicting instructions
- Missing instructions
- Ambiguous instructions
- Prompt injection
- Instruction override attempts
- Boundary prompts
- Long prompts
- Repeated prompts
- Context-dependent prompts

### Prompt Validation

Validate:

- Required instructions are followed
- System instructions take appropriate precedence
- User requirements are respected
- Constraints are maintained
- Unsupported assumptions are avoided
- Prompt injection does not bypass controls
- Sensitive instructions are not exposed

---

## Response Validation

AI responses should be evaluated using a combination of deterministic and semantic validation.

### Deterministic Validation

Use deterministic assertions where the expected behavior is predictable.

Examples:

- Required fields exist
- Response schema is valid
- Required keywords are present
- Forbidden values are absent
- HTTP status is correct
- JSON structure is valid
- Tool parameters match schema

### Semantic Validation

Semantic evaluation can be used when exact string matching is insufficient.

Evaluate:

- Meaning
- Correctness
- Relevance
- Grounding
- Completeness
- Consistency
- Safety

The evaluation approach should match the type of AI behavior being tested.

---

## Hallucination Testing

Hallucination testing validates whether the AI generates unsupported or fabricated information.

### Test Scenarios

- Ask questions outside available knowledge
- Provide incomplete context
- Provide conflicting context
- Remove expected retrieval documents
- Ask for unsupported facts
- Ask for nonexistent APIs
- Ask for nonexistent configuration values
- Ask the system to invent sources
- Ask the system to provide unknown details

### Expected Behavior

The system should:

- Avoid inventing information
- Clearly communicate uncertainty
- Refuse unsupported claims when appropriate
- Use available trusted context
- Avoid fabricated citations
- Distinguish known information from assumptions

---

## Grounding Validation

Grounding testing validates whether AI responses are supported by trusted context.

```text
User Query
     |
     v
Retrieval
     |
     v
Retrieved Context
     |
     v
AI Response
     |
     v
Grounding Evaluation
```

### Validation Areas

- Context relevance
- Source relevance
- Claims supported by context
- Unsupported claims
- Citation correctness
- Source attribution
- Context completeness
- Retrieval quality

A grounded response should be traceable to the information provided by the trusted context.

---

## AI Agent Testing

AI agents introduce additional testing complexity because they can:

- Plan tasks
- Select tools
- Execute multiple steps
- Maintain state
- Make decisions
- Recover from failures
- Determine when a workflow is complete

Testing should validate:

- Agent behavior
- Planning
- Tool selection
- Tool invocation
- Parameter generation
- Workflow execution
- State management
- Failure recovery
- Termination conditions
- Maximum iteration handling

### Agent Workflow

```text
User Request
     |
     v
Agent Planning
     |
     v
Tool Selection
     |
     v
Tool Invocation
     |
     v
Tool Response
     |
     v
Agent Decision
     |
     +----> Continue
     |
     +----> Recover
     |
     +----> Complete
```

---

## Tool Testing

AI systems frequently interact with external tools.

Examples include:

- APIs
- Databases
- Search engines
- Browser automation
- MCP tools
- Internal services

Validate:

- Tool discovery
- Tool schema
- Required parameters
- Optional parameters
- Parameter types
- Parameter values
- Tool invocation
- Tool response handling
- Error handling
- Authorization
- Timeout handling
- Retry behavior

### Tool Validation

| Area | Validation |
|---|---|
| Schema | Tool schema is valid |
| Parameters | Required parameters are supplied |
| Types | Parameter types are correct |
| Authorization | Unauthorized operations are blocked |
| Errors | Tool errors are handled correctly |
| Timeout | Timeout behavior is controlled |
| Response | Tool response is interpreted correctly |

---

## MCP Testing

Model Context Protocol integrations should be tested as part of the AI system.

Validate:

- MCP server connectivity
- Tool discovery
- Tool schemas
- Tool parameters
- Tool invocation
- Tool responses
- Error handling
- Permission handling
- Authorization
- Timeout behavior
- Agent-to-tool workflows
- MCP regression behavior

### MCP Test Flow

```text
AI Agent
    |
    v
MCP Server
    |
    v
Tool Discovery
    |
    v
Tool Invocation
    |
    v
Tool Execution
    |
    v
Tool Response
    |
    v
Agent Evaluation
```

---

## AI Safety Testing

Safety testing validates whether AI systems behave appropriately under unsafe or adversarial conditions.

Test categories include:

- Prompt injection
- Jailbreak attempts
- Harmful requests
- Sensitive information exposure
- Data leakage
- Unauthorized actions
- Unsafe tool execution
- Policy violations
- Inappropriate responses
- Instruction manipulation

### Safety Expectations

The system should:

- Enforce applicable safety policies
- Protect sensitive information
- Prevent unauthorized actions
- Reject unsafe tool execution
- Handle adversarial prompts appropriately
- Avoid exposing system instructions
- Maintain authorization boundaries

---

## Regression Testing

AI systems require continuous regression evaluation because changes to any of the following can change behavior:

- Model
- Prompt
- System instructions
- Retrieval strategy
- Embeddings
- Knowledge base
- Tools
- Agent logic
- Temperature
- Model parameters
- Application code

Regression suites should contain representative evaluation datasets.

### AI Regression Flow

```text
Code / AI Change
      |
      v
Evaluation Dataset
      |
      v
Automated Evaluation
      |
      v
Compare With Baseline
      |
      v
Quality Thresholds
      |
      v
Pass / Fail
```

---

## Evaluation Dataset

An AI evaluation dataset should contain representative scenarios.

```text
Evaluation Dataset
|
+-- Positive Cases
|
+-- Negative Cases
|
+-- Boundary Cases
|
+-- Adversarial Cases
|
+-- Safety Cases
|
+-- Regression Cases
|
+-- Production Scenarios
```

Each evaluation case can contain:

```text
Test ID
Input
Expected Behavior
Context
Expected Constraints
Evaluation Criteria
Actual Response
Evaluation Score
Pass / Fail
```

### Dataset Engineering Principles

Evaluation datasets should be:

- Version controlled
- Reproducible
- Representative
- Traceable
- Reviewable
- Maintainable
- Reusable across model and prompt changes

---

## AI Evaluation

AI evaluation determines whether an AI response satisfies defined quality criteria.

Evaluation can combine:

- Deterministic assertions
- Rule-based validation
- Semantic similarity
- LLM-based evaluation
- Human evaluation
- Reference-based evaluation
- Reference-free evaluation

### Evaluation Pipeline

```text
Input
  |
  v
AI System
  |
  v
Response
  |
  v
Evaluation Engine
  |
  +--> Correctness
  |
  +--> Relevance
  |
  +--> Grounding
  |
  +--> Safety
  |
  +--> Consistency
  |
  v
Evaluation Score
```

---

## Consistency Testing

Consistency testing validates whether similar inputs produce stable and acceptable outcomes.

Test:

- Equivalent prompts
- Repeated prompts
- Minor wording variations
- Different input ordering
- Different contexts
- Repeated execution
- Model parameter changes

The goal is not necessarily identical output. The goal is **acceptable behavioral consistency**.

---

## Robustness Testing

Robustness testing evaluates how the AI system behaves under unexpected or challenging inputs.

Test:

- Typos
- Ambiguous language
- Long inputs
- Missing information
- Unexpected formatting
- Conflicting information
- Invalid parameters
- Unexpected tool responses
- Service failures

The system should fail gracefully and avoid producing misleading results.

---

## Model Behavior Validation

Model behavior testing validates how model changes affect application quality.

Track:

- Model name
- Model version
- Model parameters
- Prompt version
- Evaluation dataset version
- Evaluation results
- Baseline comparison

### Model Regression

```text
Baseline Model
      |
      v
Evaluation Dataset
      |
      v
Baseline Scores
      |
      v
New Model
      |
      v
Evaluation Dataset
      |
      v
Compare Results
```

A model change should not be considered safe solely because the application successfully executes.

---

## AI Quality Gates

AI quality gates can prevent poor-quality builds from progressing.

```text
Build
  |
  v
AI Evaluation
  |
  +--> Correctness >= threshold
  |
  +--> Grounding >= threshold
  |
  +--> Safety = PASS
  |
  +--> Regression = PASS
  |
  v
Release Decision
```

Example quality gates:

```text
Correctness >= 90%
Grounding >= 95%
Safety = PASS
Critical regressions = 0
Hallucination rate <= threshold
```

Thresholds should be defined according to application risk.

---

## CI/CD Integration

AI evaluation should be integrated into CI/CD pipelines.

```text
Code Change
    |
    v
Build
    |
    v
Unit Tests
    |
    v
API Tests
    |
    v
AI Evaluation
    |
    v
Regression Evaluation
    |
    v
Quality Gates
    |
    +---- FAIL ----> Stop Pipeline
    |
    +---- PASS ----> Deploy
```

AI evaluation should provide machine-readable results for pipeline decisions.

---

## Performance Testing

AI systems should be tested for:

- Response latency
- Throughput
- Concurrent requests
- Token processing
- Time to first token
- Time to complete response
- Retrieval latency
- Tool execution latency
- Agent workflow duration

### Example Metrics

```text
Average Response Time
P50
P95
P99
Tokens / Second
Requests / Second
Time To First Token
```

Performance thresholds should reflect application requirements and user expectations.

---

## Cost Testing

AI systems introduce variable execution costs.

Validate:

- Input token usage
- Output token usage
- Model cost
- Retrieval cost
- Tool execution cost
- Agent iteration cost
- Cost per workflow
- Cost per user request

Example:

```text
Cost per Request
        |
        +--> Input Tokens
        +--> Output Tokens
        +--> Retrieval
        +--> Tool Calls
        +--> Agent Iterations
```

Cost should be treated as a quality engineering metric for production AI systems.

---

## Observability

AI testing should be supported by observability.

Capture:

- Prompt
- Prompt version
- Model
- Model version
- Model parameters
- Retrieved context
- Tool calls
- Agent steps
- Response
- Evaluation scores
- Latency
- Token usage
- Errors
- Cost

Sensitive data should be protected through appropriate masking and access controls.

---

## Defect Classification

AI defects can be classified into categories such as:

| Defect Type | Example |
|---|---|
| Incorrect Response | Wrong answer |
| Hallucination | Fabricated information |
| Poor Grounding | Unsupported response |
| Irrelevance | Response does not address request |
| Inconsistency | Different behavior for equivalent inputs |
| Safety | Unsafe response |
| Prompt Failure | Instructions not followed |
| Retrieval Failure | Incorrect context retrieved |
| Tool Failure | Incorrect tool invocation |
| Agent Failure | Incorrect workflow execution |
| Performance | Excessive latency |
| Cost | Excessive token usage |

Classification should help identify the appropriate remediation path.

---

## AI Failure Analysis

When an AI test fails, analyze the failure across the complete execution path.

```text
Input
  |
  v
Prompt
  |
  v
Model
  |
  v
Retrieval
  |
  v
Tool / Agent
  |
  v
Response
  |
  v
Evaluation
```

Determine whether the failure originated from:

- Input
- Prompt
- Model
- Retrieval
- Context
- Tool
- Agent logic
- Evaluation criteria
- Infrastructure

Failure analysis should focus on the underlying cause rather than only the final response.

---

## Test Automation Strategy

AI testing should automate repeatable validation.

Automation candidates include:

- Prompt testing
- API testing
- Response validation
- Semantic evaluation
- Dataset execution
- Regression testing
- Safety testing
- Tool validation
- Agent workflow testing
- Quality scoring
- CI/CD quality gates

Example architecture:

```text
Test Framework
      |
      +--> Prompt Tests
      |
      +--> API Tests
      |
      +--> Evaluation Engine
      |
      +--> Dataset Runner
      |
      +--> Quality Scoring
      |
      +--> CI/CD Reporter
```

Automation should preserve traceability between test cases, datasets, evaluation results, and release decisions.

---

## Recommended AI Test Case Structure

```text
Test Case ID:
AI-001

Test Objective:
Validate response correctness for a supported user request.

Input:
<user prompt>

Context:
<retrieved context if applicable>

Expected Behavior:
Response should correctly answer the request.

Evaluation Criteria:
- Correctness
- Relevance
- Grounding
- Safety

Evaluation Method:
Semantic + deterministic validation

Threshold:
>= 90%

Result:
PASS / FAIL
```

---

## AI Test Engineering Lifecycle

```text
Requirements
     |
     v
AI Risk Analysis
     |
     v
Test Strategy
     |
     v
Test Dataset
     |
     v
Prompt / Model Testing
     |
     v
RAG / Agent / Tool Testing
     |
     v
Response Evaluation
     |
     v
Regression Evaluation
     |
     v
Quality Gates
     |
     v
Release
     |
     v
Production Monitoring
     |
     v
Continuous Evaluation
```

The objective is to treat AI quality as a **continuous engineering lifecycle**, rather than a one-time testing activity.

---

## Engineering Principles

### 1. Test Behavior, Not Just Text

AI outputs should be evaluated based on behavior and meaning rather than only exact string matching.

### 2. Combine Deterministic and Probabilistic Testing

Use deterministic assertions where possible and semantic evaluation where necessary.

### 3. Treat Evaluation Data as Test Assets

Evaluation datasets should be version-controlled and maintained like traditional test suites.

### 4. Test Failure Modes Explicitly

Do not only validate successful AI responses.

Test:

- Hallucinations
- Retrieval failures
- Tool failures
- Agent failures
- Safety failures
- Prompt injection
- Model degradation

### 5. Automate Regression Evaluation

Every significant AI change should be evaluated against a representative regression dataset.

### 6. Make Quality Measurable

AI quality should be represented through measurable metrics and thresholds.

### 7. Integrate Quality Into CI/CD

AI quality gates should participate in release decisions.

### 8. Continuously Evaluate Production Behavior

AI quality does not end after deployment.

Production behavior should feed continuous evaluation and regression analysis.

---

## AI Test Engineering Checklist

### Input

- Required inputs validated
- Invalid inputs tested
- Boundary conditions tested
- Large inputs tested
- Malicious inputs tested

### Prompt

- Prompt structure validated
- Instructions validated
- Prompt injection tested
- Conflicting instructions tested
- Boundary prompts tested

### Model

- Correctness evaluated
- Relevance evaluated
- Consistency evaluated
- Model version tracked
- Model regression tested

### RAG

- Retrieval accuracy tested
- Context relevance tested
- Grounding validated
- Source attribution validated
- Hallucination testing implemented

### Agents

- Agent behavior tested
- Planning tested
- Tool selection tested
- Tool invocation tested
- Failure recovery tested
- Multi-step workflows tested

### Tools

- Tool schema validated
- Required parameters validated
- Parameter types validated
- Tool errors tested
- Authorization tested
- Timeout behavior tested

### MCP

- MCP connectivity tested
- Tool discovery tested
- Tool schema tested
- Tool invocation tested
- Permission handling tested
- MCP regression tested

### Safety

- Prompt injection tested
- Jailbreak scenarios tested
- Sensitive data exposure tested
- Unauthorized actions tested
- Unsafe tool execution tested

### Regression

- Evaluation dataset versioned
- Regression suite automated
- Quality thresholds defined
- CI/CD integration implemented
- Production scenarios included

### Performance

- Latency measured
- Throughput measured
- P50 measured
- P95 measured
- P99 measured
- Time to first token measured

### Cost

- Input tokens tracked
- Output tokens tracked
- Model cost tracked
- Tool costs tracked
- Cost per workflow measured

### Observability

- Model version tracked
- Prompt version tracked
- Evaluation scores captured
- Latency captured
- Token usage captured
- Cost captured
- Failures traceable

---

## Summary

AI Test Engineering extends traditional SDET practices into AI-enabled systems.

A mature AI testing strategy should cover:

```text
Input
  +
Prompt
  +
Model
  +
RAG
  +
Agents
  +
Tools
  +
Response Evaluation
  +
Safety
  +
Regression
  +
Performance
  +
Cost
  +
Observability
  +
Governance
```

The objective is not simply to determine whether an AI system produces an answer.

The objective is to determine whether the system produces **correct, relevant, grounded, safe, reliable, measurable, and production-ready behavior**.

AI Test Engineering therefore becomes a continuous engineering discipline spanning development, testing, CI/CD, release, and production monitoring.
