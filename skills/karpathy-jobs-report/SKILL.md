---
name: karpathy-jobs-report
description: >
  Use this skill when you need to generate a structured analytical report from the
  karpathy/jobs repository. It helps the agent inspect the repository, identify the
  most important source files, extract evidence about occupations, AI exposure, pay,
  education, and growth, and then produce a grounded markdown report with clear caveats.
---

# Karpathy Jobs Report Generator

## Purpose

This skill generates a concise but evidence-based report from the `karpathy/jobs` repository.

The repository is a US job market exploration project built on Bureau of Labor Statistics
Occupational Outlook Handbook data, with additional LLM-generated occupation scoring.
It is useful for producing:
- executive summaries
- labor market overviews
- AI exposure summaries
- occupation comparisons
- role-model or workforce transformation memos
- briefings for strategy teams

## When to use this skill

Use this skill when:
- the task references `https://github.com/karpathy/jobs`
- the user asks for a report, memo, summary, analysis, or briefing based on the repository
- the user wants insight into occupations, pay, growth, education, or AI exposure
- the user wants a reusable markdown artifact grounded in repository data

Do not use this skill when:
- the user only wants repository setup help
- the user wants code changes unrelated to data analysis
- the user asks for external labor-market claims that are not supported by the repository

## Output contract

Always produce:
1. a markdown report
2. a short executive summary at the top
3. a methodology section
4. a findings section grounded in repository data
5. a limitations section
6. a final section with implications or recommendations only if the task asks for them

The report must be factual, explicit about uncertainty, and must distinguish:
- raw source data from the repo
- inferred interpretations
- subjective conclusions

## Default report shape

Unless the user asks for another format, generate the report with these sections:

1. Title
2. Executive summary
3. About the dataset
4. Method used
5. Key findings
6. Occupation highlights
7. AI exposure patterns
8. Risks / caveats / limitations
9. Conclusion

## Grounding rules

Treat these files as primary sources, in roughly this priority order:
1. `prompt.md`
2. `occupations.json`
3. `scores.json`
5. `README.md`

If present, also inspect:
- `pages/` for individual occupation markdown pages
- `site/data.json` for merged site-ready structured data

Never present LLM-generated scores as hard facts.
Always label them as estimates, scores, or model judgments.

Never claim:
- that a job will disappear
- that the score is a rigorous forecast
- that the repository proves causality

Instead say:
- "estimated AI exposure"
- "LLM-derived exposure score"
- "repository scoring suggests"
- "the project frames this as transformation risk, not guaranteed replacement"

## Operating procedure

### Step 1 — Understand the repository purpose

Read `README.md` and summarize:
- what the repository does
- what the data source is
- what the scoring pipeline does
- what kinds of conclusions are safe vs unsafe

Capture the distinction between:
- BLS-derived structured fields
- LLM-derived occupation exposure scores

### Step 2 — Locate the best source for report generation

Prefer `prompt.md` when it exists, because it is designed as a single-file bundle for LLM analysis.

If `prompt.md` is missing or too large to use directly:
- fall back to `occupations.json` for structured facts
- fall back to `scores.json` for AI exposure estimates
- optionally cross-check interesting roles in `pages/`

### Step 3 — Identify the user’s report angle

Infer the report type from the request. Common patterns:

- **General repo summary**
  Focus on dataset, pipeline, and major patterns.

- **AI impact report**
  Focus on exposure scores, job families, transformation patterns, and caveats.

- **Role model / workforce strategy report**
  Focus on how job families may evolve, what human capabilities remain durable,
  and how organizations should adapt.

- **Occupation comparison**
  Compare named occupations across pay, growth, education, and AI exposure.

If the user provides no angle, default to:
**"What this repository suggests about AI exposure across the US labor market."**

### Step 4 — Extract evidence systematically

Build evidence notes for:
- dataset scope
- occupation count
- notable high / medium / low exposure patterns
- pay vs exposure patterns if visible
- education vs exposure patterns if visible
- growth outlook vs exposure tensions if visible

When extracting examples:
- prefer diverse occupations from different sectors
- do not cherry-pick only extreme cases
- include at least one counterintuitive example if supported

### Step 5 — Write the report

Write in clear markdown.

Requirements:
- be concrete
- refer to occupations by name
- explain what is directly observed vs inferred
- include cautious language around LLM scoring
- avoid hype

Style:
- analytical
- precise
- readable for business and policy audiences
- not overly academic unless explicitly requested

### Step 6 — Validate before finalizing

Before returning the report, verify:
- every major claim traces to repository content
- AI exposure is described as an estimate, not destiny
- no unsupported numerical claims are invented
- limitations are explicitly included
- the final text is internally consistent

## Heuristics for stronger reports

### A. What usually matters most
Prioritize these patterns if they are visible in the repo:
- high digital-document work tends to score higher on digital AI exposure
- physical/manual/context-rich jobs may score lower on current digital AI exposure
- high exposure does not automatically mean lower demand
- some highly exposed jobs may still grow due to productivity expansion

### B. Good comparative framing
Useful contrasts:
- software / office / analysis roles vs field / physical / care roles
- pay vs exposure
- education requirement vs exposure
- growth outlook vs exposure
- transformation vs replacement

### C. Good limitation language
Use language like:
- "This repository is best read as an exploratory analysis tool."
- "The AI exposure layer is a modeled judgment rather than an observed labor outcome."
- "The scores indicate where work may be reshaped, not whether occupations disappear."
- "The output should inform discussion, not act as a standalone forecast."

## Report assembly instructions

If the user did not specify a filename, create:
`report_karpathy_jobs.md`

If the user asked for a role-model or organizational adaptation view,
rename the report title accordingly, for example:
- "Role Model for the Sber Workforce in an AI-Exposed Labor Market"
- "Implications of Digital AI Exposure for Enterprise Roles"

## Default markdown template

Use the template in `references/report_template.md` if available.
If not available, use this structure:

# <Report Title>

## Executive summary
2–5 short paragraphs summarizing the main insights.

## About the repository
Explain what `karpathy/jobs` is, what data it uses, and what the scoring layer adds.

## Method
Describe which files were used and how the analysis was performed.

## Key findings
Present the most important patterns.
Use subsections if needed:
- Exposure patterns
- Pay and education patterns
- Growth and demand tensions
- Notable occupation examples

## Interpretation
Explain what the findings may imply for employers, workers, or policymakers,
but separate interpretation from direct observation.

## Limitations
Be explicit about:
- BLS source boundaries
- LLM-based scoring uncertainty
- non-predictive nature of exposure scores
- absence of causal proof

## Conclusion
Summarize the takeaways in a few paragraphs.

## If the user asks for a "role model"

Translate the findings into a workforce role model with categories such as:
- AI-amplified knowledge workers
- judgment-heavy coordinators
- human-trust and relationship roles
- field / physical execution roles
- hybrid supervisors of AI-enhanced workflows

In that case:
- ground each category in repository patterns
- avoid claiming the repository directly defines those categories
- present them as a reasoned synthesis

## Failure mode handling

If the repository contents are partially unavailable:
- say exactly which files were accessible
- generate the report from the available sources only
- clearly mark any resulting blind spots

If the data appears inconsistent:
- prefer the most directly structured artifacts
- mention the inconsistency rather than hiding it

## Example invocation phrases

This skill should activate for prompts like:
- "Generate a report from karpathy/jobs"
- "Summarize the labor market insights in the karpathy jobs repo"
- "Create an AI exposure memo based on the jobs repository"
- "Build a workforce role model from karpathy/jobs"