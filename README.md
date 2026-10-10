# Dang Viet Hoang — Business Analyst (Fresher)

DOMO is a community challenge platform on web and Android that lets participants build up achievements on a single account across many challenges run by vetted organizers.
I am Dang Viet Hoang, Business Systems Analyst and UI/UX Designer on the project, responsible for agreeing the MVP scope with stakeholders, specifying requirements, processes, data, screens and APIs, and designing acceptance tests.
This repo extracts the account and role administration module: the traceability matrix, requirements and business rules, process diagrams, data dictionary, user stories, screen specifications with mockups, API specification and UAT test cases.
The documents let stakeholders make scope decisions based on assessed impact, while developers and testers work from one shared specification in which every requirement has a matching acceptance test.

**LinkedIn:** [linkedin.com/in/hoangdv2004](https://www.linkedin.com/in/hoangdv2004/) | **Email:** [hoangdangviet52@gmail.com](mailto:hoangdangviet52@gmail.com)

> **Note:** DOMO is a simulated project built to demonstrate business analysis skills, not a commercial product. This repo is an extract of the full project documentation; the module was chosen to show one complete analysis chain from business need to acceptance test. Sample data in the documents (emails, account IDs, example reasons) is fictitious.
>
> **Language:** this README is in English; all documents in `docs/` are written in Vietnamese.

## Context

In the MVP phase, the core problem is controlling **who can do what with other people's accounts and data**: the operations team needs to look up accounts, change roles and handle violations safely, with a recorded reason and a full audit trail, while the root admin account can never be touched. The **account and role administration** module addresses this problem directly, so it is presented end to end instead of spreading thinly across the whole project.

## How to navigate this repo

```text
domo-ba-portfolio/
├── README.md
├── docs/
│   ├── 00-traceability-matrix.md     Requirements traceability matrix
│   ├── 01-scope-baseline.md          Context, business goals, MVP scope
│   ├── 02-stakeholder-actor.md       Stakeholders and actors
│   ├── 03-requirements-and-rules.md  Requirements, business rules, constraints
│   ├── 04-business-processes.md      Business processes
│   ├── 05-data-dictionary.md         Data model and data dictionary
│   ├── 06-user-stories/              3 user stories, 50 acceptance criteria
│   ├── 07-screen-specs/              3 screen specifications
│   ├── 08-api-spec-extract.md        API specification
│   ├── 09-system-api-tests/          18 test scenarios, 88 test cases
│   ├── 09-uat-scenarios.md           UAT scenarios
│   └── 10-change-decision-log.md     Change and decision log
├── diagrams/                         Process diagrams, ERD, stakeholder matrix
└── mockups/                          Mockups grouped by screen
```

## Skills & Tools

| Group | Skill or tool | Evidence or usage context |
|---|---|---|
| BA methodologies & documentation | Requirements Gathering & Elicitation | Ran discovery and JAD sessions with stakeholders to agree the scope baseline - [01](docs/01-scope-baseline.md) |
| BA methodologies & documentation | Scope Baseline, Business Requirements, MoSCoW | 14 business requirements prioritised with MoSCoW, 40 out-of-scope items with reasons - [01](docs/01-scope-baseline.md) |
| BA methodologies & documentation | Stakeholder Analysis (Power/Interest Matrix), Actor Definition | Power/interest matrix, 7 actors with permissions and limits - [02](docs/02-stakeholder-actor.md) |
| BA methodologies & documentation | Functional & Non-functional Requirements, Business Rules, Decision Tables | Stakeholder, functional and measurable non-functional requirements, business rules with decision tables - [03](docs/03-requirements-and-rules.md) |
| BA methodologies & documentation | UML Design (Activity, State, Sequence) | 5 diagrams with main, alternative and exception flow notes - [04](docs/04-business-processes.md) |
| BA methodologies & documentation | ERD, Data Dictionary | ERD of 3 tables, dictionary of 39 data fields with rules and examples - [05](docs/05-data-dictionary.md) |
| BA methodologies & documentation | User Stories & Acceptance Criteria (Gherkin) | 3 user stories, 50 Given-When-Then scenarios - [06](docs/06-user-stories/) |
| BA methodologies & documentation | Screen Specification, API Specification | 3 screens covering every state, with mockups - [07](docs/07-screen-specs/), 4 endpoints with sample requests, responses and error codes - [08](docs/08-api-spec-extract.md) |
| BA methodologies & documentation | UAT Testing, API Testing | 88 system and API test cases designed to cover all 50 scenarios - [09](docs/09-system-api-tests/), 5 business UAT scenarios run by the operations team through the UI, with entry/exit criteria and sign-off - [09](docs/09-uat-scenarios.md) |
| BA methodologies & documentation | Requirements Traceability (RTM), Change Request, Decision Log | Two-way traceability from business need to acceptance test - [00](docs/00-traceability-matrix.md), change requests and decisions with the options considered - [10](docs/10-change-decision-log.md) |
| BA methodologies & documentation | Agile/Scrum | Maintained the backlog and checked the Definition of Ready before sprint planning |
| Tools | draw.io | 7 diagrams: processes, ERD and stakeholder matrix - [diagrams](diagrams/) |
| Tools | Figma | 30 mockups for 3 screens and dialogs - [mockups](mockups/) |
| Tools | Postman | Tested authorization, invalid input and expired sessions directly against the API |
| Tools | SQL (PostgreSQL) | Queried the database to verify fields against the [data dictionary](docs/05-data-dictionary.md) |
| Tools | GitLab | Document version control |

## My BA Approach

| Step | Activity | Output |
|---|---|---|
| 1 | Understand the context, define business goals and scope boundaries | [01](docs/01-scope-baseline.md) |
| 2 | Identify who has needs, who uses the system and what each party cares about | [02](docs/02-stakeholder-actor.md) |
| 3 | Turn needs into testable requirements and clear business rules | [03](docs/03-requirements-and-rules.md) |
| 4 | Model workflows and data, including alternative and exception flows | [04](docs/04-business-processes.md), [05](docs/05-data-dictionary.md) |
| 5 | Write user stories with acceptance criteria for every scenario | [06](docs/06-user-stories/) |
| 6 | Specify screens and APIs so design and development work from one description | [07](docs/07-screen-specs/), [08](docs/08-api-spec-extract.md) |
| 7 | Turn acceptance criteria into test cases, and define business acceptance scenarios for users to confirm the solution before release | [09](docs/09-system-api-tests/), [09](docs/09-uat-scenarios.md) |
| 8 | Record changes and decisions and keep traceability end to end | [10](docs/10-change-decision-log.md), [00](docs/00-traceability-matrix.md) |

Three principles run through every step: each requirement has its own ID and is traceable both ways; exception flows are specified with the same care as the main flow; every decision is recorded with the rejected options and the reason.

## Contact

**LinkedIn:** [linkedin.com/in/hoangdv2004](https://www.linkedin.com/in/hoangdv2004/) | **Email:** [hoangdangviet52@gmail.com](mailto:hoangdangviet52@gmail.com)
