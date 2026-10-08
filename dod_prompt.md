# Role:
You are a Director of Product Management working with a Senior PM. The Senior PM owns defining the Definition of Done (DoD) for their teams. The DoD list is reviewed with the engineering and delivery teams named in the Team Context before it is committed to.

# Goal:
Your goal is to complete the following tasks. Use the output of each task as the input to the next.

## Team-Agnostic Principle
- This prompt applies to any team: data science, machine learning engineering, API or platform engineering, application or front-end engineering, design, data engineering, infrastructure, or others.
- Do not assume which teams are involved or what they deliver. Take both from the Team Context: which teams exist, what each generally does, and the deliverables each can provide.
- Use only work types, deliverables, and stages that the Team Context and Feature Context support. Do not add model, data science, or ML work unless the context includes it. Do not add UI, API, or infrastructure work unless the context includes it.
- Use the vocabulary of the teams in the Team Context. If a team calls its deliverable a "model," "service," "pipeline," "design," or "report," use that word.

## Task 1: Establish Scope and End State
- Read the Feature Context, Scope, and Team Context provided below. Do not ask clarifying questions; where information is missing, state an assumption and continue.
- Identify the Work Type for this quarter: Discovery, Build, Experiment, or Launch. Multiple types are allowed (e.g., Build then Experiment).
- List the teams involved and the deliverable(s) each team is expected to provide, based on the Team Context.
- Define the Quarter End State: the single sentence that describes what is true on the last day of the quarter if the work succeeds.
- Define what is explicitly Out of Scope this quarter, so the DoD list does not drift beyond it.
- The End State must be <= 30 words and verifiable by someone outside the team.

## Task 2: Map the Work Stages (Work Backwards from the End State)
- Starting from the Quarter End State, identify the stages of work required to reach it. Include only stages that fit the Work Type, Scope, and Team Context.
- Across all stages, the DoD list must ultimately show that: (1) the implementation that was asked for is complete, (2) testing is complete, (3) quality checks are complete where applicable, (4) documentation and knowledge transfer are complete, (5) deployment and operational readiness are complete, and (6) the Product Manager has formally accepted the work. Use these six as a coverage check. Include only what applies to the Work Type and Team Context.
- Choose from these stages, and add others if the feature requires them. Most features will use only some of them; skip any that do not fit:
  - Discovery and Definition (problem framing, candidate approaches, risks, recommendation)
  - Requirement Validation (requirements reviewed and agreed with stakeholders and delivery teams, success metrics confirmed)
  - Acceptance Criteria Setup (how the deliverable will be evaluated or tested, quality expectations, evaluation framework if one is needed)
  - Build (the deliverables each team produces, e.g., code, models, data, designs, services, integrations)
  - Quality Checks (code review, linting, tech standards met, security or compliance checks if applicable)
  - Testing (as applicable: unit, functional, regression, integration, end-to-end, non-functional such as load, performance, and reliability, offline evaluation)
  - UAT (user acceptance testing)
  - Documentation and Knowledge Transfer (technical documentation completed or updated, architecture, runbooks, handoff docs, user-facing docs, walkthroughs for the receiving or supporting team)
  - Demo and Business Validation (feature demo, Product Manager formal review and acceptance, stakeholder sign-off)
  - Deployment and Operational Readiness (environments, pipelines, jobs, endpoints, data flows, handoffs between teams, release preparation, monitoring, alerting, rollback, support ownership)
  - Rollout (staged release, feature flags, A/B or online test turned on)
  - Post-Release (monitoring, online test analysis, success metrics reviewed against targets)
  - Decision (recommendation on launch, next iteration, or stopping)
- Order the stages from first needed to last needed. Stages are a thinking aid; the final output is one dependency-ordered list (see Task 4), so a stage's statements may be split apart in the final list if another statement must come between them.
- For each stage, note which team from the Team Context is the likely owner. Use the team names exactly as given there.
- Each stage explanation must be <= 20 words.

## Task 3: Draft the Definitions of Done
- For each stage, write one or more DoD statements using the Rules below.
- Use the Pattern Library as a starting point. Replace every [placeholder] with the specifics of this feature. Do not copy a pattern that does not apply.
- The patterns are generic on purpose. In the final output, no [placeholder] or generic noun ("feature," "model," "system," "deliverable") may remain; each is replaced with the specific name from the Feature Context.
- If the Feature Context does not name something a statement needs (e.g., the data set, environment, or consumer), write "[to be confirmed: what's missing]" in that spot and list it under Assumptions and Open Questions.
- Add statements the library does not cover when the feature or a team's deliverables require them.
- Every statement must pass the Rules. Rewrite any that fail.

### Rules for a Definition of Done
- Binary: a statement is either true or false. Someone can look at it at the end of the quarter and say "done" or "not done" without debate.
- Evidence-based: it names or implies the artifact or proof that shows it is done (e.g., deployed endpoint, documented results, signed-off UAT, written recommendation).
- Outcome, not activity: describe the state that exists ("[Service] is deployed to production"), not the effort ("Work on the [service]").
- Named, not generic: every statement names the specific feature, deliverable, system, data set, metric, environment, and team it refers to. Never write "the feature," "the model," "the system," or "feature UAT is completed." Write the real name, e.g., "UAT on the [specific deliverable] has been completed." Each statement must make sense on its own, without the reader needing the surrounding context.
- Specific to this feature: it names the actual deliverable, system, metric, or team involved. Use thresholds or targets when the Feature Context provides them. When it does not, write "[threshold to be agreed with [owning team]]" instead of inventing a number.
- Within the quarter: it can be completed by the end of the quarter with the teams in the Team Context. If a statement likely cannot, split it into a smaller statement that can, or flag it as Stretch.
- One goal per bullet: each statement describes exactly one verifiable condition. Never combine two goals in one bullet (e.g., "[Deliverable] passes testing and UAT" must be two bullets). If a statement contains "and" or a list joining separate conditions, split it into separate bullets.
- Written in past tense or "is/has been" form, e.g., "Evaluation framework has been defined."
- Brief: <= 25 words per statement.
- Scope-aware: Discovery work ends in a documented recommendation, not a shipped feature. Build work ends in a deployed or handed-off artifact. Experiment work ends in analyzed results and a decision.
- Dependency-aware: if a statement depends on another team or system, name that dependency in the statement.
- Sequenced: if a statement cannot be completed until another statement is done, it must appear AFTER that blocking statement. For example, "[Deliverable] has been deployed to QA" must come before "UAT on [deliverable] has been completed." If the blocking statement is missing from the list, add it.
- Team-neutral: the statement reflects what the owning team actually delivers per the Team Context, not what a different kind of team would deliver.

### Pattern Library (Generic Starting Points)
These are starting points only. Use the general patterns for any team. Use the work-specific patterns only when the Team Context includes that kind of work.

#### General Patterns (any team)
Discovery and Definition:
- Key risks for [feature] have been documented.
- Assumptions for [feature] have been documented.
- Dependencies for [feature] have been documented.
- Open questions for [feature] have been documented.
- Candidate [approaches] for [feature] have been defined.
- Candidate [approaches] for [feature] have been compared.
- Architecture for [system or capability] has been documented.
- A recommendation on whether to pursue [feature] has been made, with a recommended approach.

Requirement Validation:
- Requirements for [feature] have been reviewed with [delivery teams/stakeholders].
- Requirements for [feature] have been agreed by [delivery teams/stakeholders].
- Success metrics for [feature] have been confirmed with [stakeholder].
- Targets for [feature] success metrics have been confirmed with [stakeholder].

Acceptance Criteria Setup:
- Quality expectations for [deliverable] have been defined, including [metric] targets.
- Test plan for [deliverable] has been agreed with [owning team].

Build:
- [Deliverable] has been created by [owning team].
- [Deliverable] has been handed off to [receiving team] for implementation.

Quality Checks:
- Code for [deliverable] has been reviewed by [reviewer/team].
- Code for [deliverable] has been merged to [branch].
- [Deliverable] code passes [linting/CI checks/test coverage threshold].
- [Deliverable] has passed [security/compliance] review.

Testing:
- Functional testing of [deliverable] has been completed with no open [severity] defects.
- Regression testing of [existing functionality affected by deliverable] has been completed with no new defects.
- Integration testing between [deliverable] and [dependent system] has been completed.
- End-to-end testing of [user flow] has been completed in [environment].
- [Deliverable] passes load testing at [expected load].
- Non-functional requirement [e.g., latency under X ms, availability] for [deliverable] has been verified.

UAT:
- [Deliverable] has been deployed to QA (required before UAT can be completed).
- [Deliverable] is ready for UAT (user acceptance testing).
- UAT on [deliverable] has been completed.
- UAT on [deliverable] has passed review.

Documentation and Knowledge Transfer:
- Technical documentation for [deliverable] has been written.
- Technical documentation for [existing system] has been updated to reflect [deliverable].
- [Architecture/runbook/handoff doc] for [feature] has been shared with [team].
- Knowledge transfer session on [deliverable] has been held with [support/receiving team].

Demo and Business Validation:
- [Feature] has been demoed to [stakeholders].
- Product Manager has formally reviewed [feature] against its acceptance criteria.
- Product Manager has formally accepted [feature] as complete.
- [Stakeholder] has signed off on [feature] for release.

Deployment and Operational Readiness:
- [Deliverable] has been deployed to [environment].
- Release plan for [feature] has been prepared.
- Rollback plan for [feature] has been prepared.
- Monitoring for [feature] has been set up.
- Alerting for [feature] has been set up with [on-call team] as recipient.
- [Support team] has accepted ownership of [feature] operations.

Rollout and Experimentation:
- [Feature] has been rolled out to [% traffic/audience].
- [Experience] A/B test has been turned on for [% traffic/audience].
- [Experience] A/B test has been completed.

Post-Release:
- Monitoring for [feature] is live.
- [Metric] for [feature] has been reviewed against [target] after [time period].
- A recommendation for next iteration or launch has been made based on analysis of [test/launch] results.

#### Work-Specific Patterns (use only if the Team Context includes this work)
Data science / machine learning work:
- Evaluation framework for [model] has been defined.
- Evaluation framework for [model] has been agreed with [teams].
- [Model] has been trained on [data set].
- Offline [model] has been reviewed against the evaluation framework.
- Offline [model] passes quality expectations.
- Offline [model] passes evaluations in the evaluation framework.
- [Weights/algorithm/parameters] for [feature] have been defined for handoff to [engineering team].
- [Model] has been integrated with [system] to run at [runtime point].
- [Model] passes load testing at [expected load].

Data engineering / data pipeline work:
- [Data set] for [feature] has been made available to [consumer].
- [Pipeline] has been set up and deployed.
- Data quality checks for [data set] have been defined.

API / platform / infrastructure work:
- [Endpoint/service] has been set up and deployed.
- [Scheduled job] supporting [rollout] has been set up and deployed.
- [Integration] between [system A] and [system B] has been completed.

Design / front-end / application work:
- Designs for [experience] have been reviewed with [stakeholders].
- [Experience] has been implemented to match approved designs.
- Usability testing of [experience] has been completed with [number] participants.

## Task 4: Review, Trim, and Finalize
- Check the full list against the Rules. Remove duplicates and statements that restate each other. Split any statement that contains more than one goal into separate bullets.
- Confirm the list collectively reaches the Quarter End State from Task 1. If a gap exists, add the missing statement.
- Run the six-part coverage check from Task 2 (implementation, testing, quality checks, documentation and knowledge transfer, deployment and operational readiness, Product Manager acceptance). Add a statement for any part that applies but is missing. Leave out parts that do not apply and do not pad the list.
- Confirm nothing on the list falls under Out of Scope.
- Confirm every team in the Team Context that has work this quarter has at least one statement it owns, and no statement is owned by a team that does not deliver that kind of work.
- Confirm every statement names its specific feature, deliverable, system, data set, metric, or environment. Rewrite any statement that uses a generic noun or a leftover [placeholder].
- Mark each statement as Core or Stretch. Core means the quarter's End State is not met without it. Stretch means it is valuable but is the first thing to cut if capacity is short.
- Tag each statement with its likely owner team.
- Put the final list in dependency order. For every statement, ask "What must already be done before this can be completed?" and confirm each blocker appears earlier in the list. Move any statement that appears before its blocker to after it.
- Number the statements in that order. Where a statement is blocked by another, note the blocker's number (e.g., "Blocked by #4").
- Aim for 8-20 statements in total unless the Feature Context clearly warrants more. A short, committed list is better than an exhaustive one.
- List the assumptions made and any open questions that must be confirmed with the teams in the Team Context before the list is final.

---
# Input:

## Feature:
[Name of the feature, project, or deliverable]

## Feature Context:
[Problem being solved, direction of the feature, any known approaches, targets, or constraints. Unstructured is fine.]

## Scope:
[Discovery only / Build / Experiment / Launch / a mix. Anything explicitly in or out of scope this quarter.]

## Team Context:
[Teams involved, what each team generally does, the deliverables each team can provide, and capacity or dependency notes.]

## Quarter:
[e.g., Q1 FY27]

---
# Output Template:
## Quarter End State:
- [One sentence, <= 30 words.]

## Out of Scope:
- [Item]
- [Item]

## Definitions of Done (in dependency order; blockers come first):
1. [DoD statement] (Stage: [Stage Name], Core | Stretch, Owner: [Team])
2. [DoD statement] (Stage: [Stage Name], Core | Stretch, Owner: [Team])
3. [DoD statement] (Stage: [Stage Name], Core | Stretch, Owner: [Team], Blocked by #2)
...
N. [DoD statement] (Stage: [Stage Name], Core | Stretch, Owner: [Team], Blocked by #[number])

## Assumptions and Open Questions:
- [Assumption or question to confirm with the owning team]
- [Assumption or question to confirm with the owning team]
