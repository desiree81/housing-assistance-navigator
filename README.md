# Housing Assistance Navigator
A multi-agent tool that helps a family work out which income-based housing programs they plausibly qualify for, what each option would actually cost, and which documents to gather before they ever call a leasing office.

**Status, stated plainly:** This is a personal reference project built on published public data (HUD income limits and publicly listed program rents). It is not connected to any employer work. It exists to demonstrate a forward deployed engineering method end to end: take a confusing, high-stakes process that families currently navigate by phone tag and guesswork, and give it structure, evidence, and a human decision at the end.

## The problem
Income-based housing in the United States is not one program. It is a stack of them: public housing, housing choice vouchers, and Low-Income Housing Tax Credit properties, each with its own income limits by household size, fixed rent tiers that are not a simple percentage of income, and waitlists that open and close without notice. Families routinely spend weeks calling properties they never qualified for, or rule themselves out of homes they were eligible for, because the rules are published in fragments and nobody assembles them.

## The design
Specialized agents, each with one job, coordinated against structured contracts:

- **Intake agent** — collects household size, income, location, and constraints (bedrooms, accessibility, maximum commute) into a single structured household profile. No free-text ambiguity downstream.
- **Rules agent** — assembles the current published rules for the target area: HUD income limits by household size, program types, and each property's tier structure, with the source and date attached to every rule it uses.
- **Eligibility agent** — evaluates the household profile against each option and returns a structured verdict per property: qualifies at this tier, does not qualify, or borderline with the deciding variable named. Borderline cases state exactly which income figure or household count flips the answer.
- **Cost agent** — computes the real monthly picture per qualifying option: rent at the applicable tier, deposit and fee patterns where published, and flags where a "cheaper" headline rent is not the cheaper outcome.
- **Preparation agent** — drafts the call script and document checklist for the family's top options: the exact questions that determine eligibility on the property's current income-limit sheet, and the paperwork to have in hand before the first call.
- **Review step** — the tool recommends and prepares. It never applies, never submits, and never tells a family they are approved. The final judgment stays with the family and the leasing office, which is the only place it legitimately lives.

## How it is built
- **Google ADK** for agent orchestration and tool use.
- **Python with Pydantic** models for every input and output, so eligibility verdicts are validated structured data with the rule source attached, never a paragraph of confident prose.
- **Spec-Driven Development:** agent behavior, data contracts, acceptance criteria, and the evaluation plan were specified before implementation. The spec ships in this repository.
- **Evaluation:** a fixed set of household cases with hand-computed expected verdicts, run against every change, because an eligibility tool that is wrong in a confident tone is worse than no tool.

## What this demonstrates
- Forward deployed method applied outside a corporate setting: start from a real family's decision, then design backward.
- Handling of messy, fragmented public rules with sources and dates treated as first-class data.
- Human-in-the-loop by design: the system narrows, explains, and prepares. People decide.
- A pattern that transfers to any eligibility-driven domain: benefits, grants, licensing, and program enrollment.
