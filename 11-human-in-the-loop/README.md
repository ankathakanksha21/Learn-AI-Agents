# 11 · Human in the Loop (HITL)

Production AI projects use a human in the loop (HITL) as the trust layer for customers. Instead of letting the AI run as a fully automated system, HITL adds checkpoints where humans review, approve or correct its outputs.

<p align="center">
  <img src="images/human-ai-loop.png" alt="A loop between a human and the AI, with the AI's output going back to the human." width="500">
</p>

## How it works

1. **Automation.** The AI processes data and generates an output.
2. **Critical checkpoints.** At uncertain or high-stakes decision points, the process pauses for human review.
3. **Human intervention.** A human gives feedback, makes a decision or corrects the work.
4. **Incorporating feedback.** The system takes that input and resumes.
5. **Continuous improvement.** The AI learns from it, improving future performance.

<p align="center">
  <img src="images/hitl-cycle.png" alt="The HITL cycle: automation, critical checkpoints, human intervention, feedback and continuous improvement, then back to automation." width="500">
</p>

## Why it matters

- **Accuracy.** Human judgment catches errors the AI alone would miss.
- **Complexity.** AI struggles with ambiguous situations, emotional intelligence and ethical dilemmas.

## Where to put humans in the workflow

The goal is to insert humans where they add value the agent alone can't. Here are three ways to do it.

### 1. Confidence-based escalation

The agent drafts a response and evaluates its own confidence. Only low-confidence responses get escalated to a human.

<p align="center">
  <img src="images/confidence-escalation.png" alt="A confidence gauge reading low hands the response to a human." width="500">
</p>

These signals trigger escalation:

- Confidence is below a threshold.
- A policy conflict is detected.
- User sentiment is negative.
- The request falls outside known patterns.

This keeps humans focused on what matters: edge cases, ambiguity and novel situations.

### 2. Guidance on high-stakes decisions

For critical calls, the agent identifies the decision point and presents the action along with its tradeoffs. A human reviews the reasoning, selects the direction and authorizes it. The agent then executes, with the human's decision logged.

<p align="center">
  <img src="images/approve-reject.png" alt="The LLM hands off to a human, who either approves and sends the flow to one node or rejects and sends it to another." width="500">
</p>

This keeps the agent doing what it's good at, like data gathering, formatting and tool orchestration. Humans do what they're good at: strategy, priorities and tradeoffs.

### 3. The feedback loop

Every human correction should make the agent smarter. When a human overrides the agent's decision, that correction gets logged with the reasoning behind it, and the agent learns from what humans kept catching.

That's how escalation volume can drop over time, making the system gradually more autonomous.

<p align="center">
  <img src="images/escalation-drops.png" alt="Bar chart with a downward trend, showing escalations dropping over time." width="500">
</p>

HITL is the design behind AI products that have earned real trust.
