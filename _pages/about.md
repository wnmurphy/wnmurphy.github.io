---
layout: single
author_profile: true
permalink: /about/
title: About Me
---

<script>
  const x = new Date();
  const currentMonth = x.getMonth() + 1;
  const currentYear = x.getYear();
  const startDate = new Date("2016-01-01");
  const startYear = startDate.getYear();
  const monthsProgramming = (((currentYear - startYear) * 12) - (12 - currentMonth));
  const yearsProgramming = monthsProgramming / 12;
  const roundedYearsProgramming = yearsProgramming.toFixed(2);
</script>

Hi. I'm Neil. I'm a senior software engineer with <script>document.write(roundedYearsProgramming)</script> years of experience, specializing in generative AI and cloud infrastructure. Currently living in the San Francisco Bay Area.

![Photo](/assets/images/onsen.jpg)

## Professional Highlights

### <i class="fas fa-heartbeat"></i> [Vida Health](https://www.vida.com/)

#### Generative AI

Pioneered Vida Health's foundational generative AI strategy and successfully transformed it into user-facing features powered by Large Language Models (LLM).

Crafted a virtual healthcare assistant chatbot with advanced capabilities: memory of prior interactions, user intent extraction, awareness of real-time user medical data, dynamic multi-prompt routing, injection of proprietary healthcare + tech support content using GCP Vector Store (Matching Engine) and Retrieval-Augmented Generation (RAG), and generated user smart replies.

Invented "sticky routing" technique to enhance the natural flow of chatbot conversations.

Lead prompt engineer. Built evaluation suite to assess groundedness/relevance of RAG results + chatbot routing precision/recall against synthetic data.

Established standardized prompt management practices at Vida Health, encompassing source control, versioning, templating, and seamless integration of contextual user data from our databases and feature stores.

Designed and developed smart replies for healthcare providers, auto-generated drafts for professional correspondence based on prior patient conversations.

Built an at-a-glance patient snapshot feature, summarizing medical data and progress for healthcare providers.

Implemented real-time contextual awareness of aggregated user medical data across all gen AI features using a standardized injectable user snapshot.

RLHF/explicit training. Implemented a comprehensive prediction logging system that captures predictions, prompts, and parameters as well as subsequent user feedback, comments, and any user-initiated modifications for further model/prompt tuning and evaluation.

Model deployment. Wrote Jupyter notebooks for Custom Prediction Routine (CPR) containers to deploy open source models like Falcon-7B and GPT-JT to private Vertex AI endpoints for model evaluation, installing/configuring CUDA libraries for GPU inference acceleration.

LangChain open-source contributor.

#### Infrastructure

SRE on-call 24/7 in 2-wk rotation for platform with 40,000+ monthly active users.

Managed weekly service deployments. Enabled one-command deployment rollbacks by added database migration backwards-compatibility check in pre-commit hook and rewriting deployment process.

Designed architecture and end-to-end flow for prescribing feature and integrated with 3rd-party prescription service, enabling Vida to offer prescription medication as a service.

Designed and implemented provider tagging features, enabling providers to see an inbox of priority clinical notes they've been tagged on.

Designed and implemented provider referrals feature, automatically configuring new provider-user relationships on approval.

Improved performance of our MyFitnessPal-style food logger by 100x (~12s to ~120ms) by moving nested calls from GraphQL to a database join in the resolving service.

### <i class="fas fa-medkit"></i> [Roche](https://www.roche.com/)

Platform technical lead. First FT onsite engineering hire for a new digital healthcare backend team. 

Drove unification of backend engineers into a consolidated, standalone product team with a single backlog, which was critical for coordination and building reusable services. Drove implementation of best practices: code reviews, "definition of done," etc.

Defined general, reusable platform services and features from product-specific designs in collaboration with product owners.

Coordinated and drove PI planning estimation sessions across multiple continents.

Designed complete set of standard, generalizable platform resources and data models to support a variety of applications, including table schemas, endpoints, etc.

Designed and implemented platform-wide resource configurability.

Enabled a single common patient model across all apps by designing patient profiles. Consolidation of all PII in a single resource enabled easy deletion-on-request (GDPR).

Drove standardization of mobile-first API design (consistent response codes and formatting, pagination, etc.)

Designed platform-level roles and role-based access control (RBAC) by generalizing specific product requirements across client applications.

Designed user registration codes.

Enabled faster development by writing a seed data service, automating creation of a default set of resources on dev and test environments.

Wrote new features in Vert.x and Spring Boot.

Wrote Lambda Authorizers for secure server-to-server export of patient data for clinical analysis.

### <i class="fas fa-capsules"></i> [Proteus Digital Health](https://www.proteus.com/)

Architected and implemented a highly-configurable rules engine from the ground up. Consumes a patient's data in real time and calculates their triage score, so that doctors can rapidly identify priority patients. Written in node.js on AWS Lambda, DynamoDB, Kinesis, SQS, API Gateway. Complete unit test coverage with Mocha and Chai.

Owned, developed, maintained, and provided troubleshooting and support for mock data generation tool used by all product teams. Written in Python 3. Used Behave to convert BDD statements to cloud API calls with templated request bodies. Used by developers to develop new product features by generating simulated user resources and metric data in Cloud. Created unit test suite and integrated into build to prevent regression bugs. Converted tool into importable package, so it could be used directly in other teams' test suites.

Built an automated test suite for our microchip manufacturing facility in Hayward.

Overhauled our user configuration logic. Created new API endpoints, DynamoDB tables and a lambda to handle API requests and validation. Wrote database backfill functions to migrate old data structures to new ones in production. Implemented caching on lambda initialization to drop global resource lookup time from 1500ms to 500ms.

Implemented Python command-line tools for listening to DynamoDB stream for debugging and uploading files to S3 from local machine using Boto3 SDK.

Wrote a set of tools for patient data migration/archiving, allowing us to preserve protected data while freeing up an environment.

### <i class="fas fa-camera"></i> [GoPro](https://gopro.com/en/us/)

Built GoPro's developer documentation platform and wrote 80+ pages of API reference and quickstart tutorial content with sample code for 8 services. 

Worked on backend team, providing integration support to devs from internal product teams, acquired companies, and partner orgs. 

### <i class="fas fa-code"></i> [Hack Reactor](https://www.hackreactor.com/)

Studied my tail off to get into a grueling software engineering program which had a 3% acceptance rate at that time. Built a location-sharing app called IRL (React.js, Socket.io). Built a collaborative project management app called Cliqueboard (AngularJS, express.js).

### <i class="fas fa-satellite"></i> [Space Systems Loral](http://sslmda.com/)

Wrote electrical assembly and test procedures for 18 commercial communication satellites currently in orbit.


## Industry Experience

I've worked in these spaces:

- Generative AI.
- Event-driven, serverless architecture.
- Medical devices and pharmaceuticals.
- FDA regulatory compliance.
- IoT prescription drugs.
- Internal tooling.
- HIPAA compliance.
- Aerospace.

## My Philosophy of Software Development

### I am addicted to simplicity.

Simplicity scales, complexity fails. Simple designs are easier to build, eaier to maintain, and easier to extend in the future.

### I strive to write clean, consistent, maintainable code with no suprises.

Code should not be surprising. I like to keep code [SOLID](https://github.com/ryanmcdermott/clean-code-javascript#solid) and [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). I design for scale, avoid nested iteration and extraneous database calls, and cache all the things. I care about code quality, and set aside dedicated time for code reviews.

### I strive to make life easy for whoever touches the code after me.

I care about minimizing the cognitive load of context switching. Given the choice to write clever code or legible code, legible wins every time. I use intuitive variable names and write clear, useful comments. I take documentation seriously. 

### I strive to add value to my team.

I care about creating a non-toxic workplace and always check my ego at the door. I like to help others be successful, and am happy to spend time answering questions for stakeholders and other developers.

## Bonus Skills

I have additional professional experience with:
- **technical writing**. Most engineers hate writing docs. I actually enjoy crafting well-written documentation and understand how to convey technical concepts.
- **training**. I enjoy explaining concepts and teaching others.
- **customer success**. I enjoy helping other people achieve their technical goals.

## Miscellaneous

I like OSX in dark mode, [Sublime Text 3](https://www.sublimetext.com/3), a modified [Glowfish theme](https://github.com/czettnersandor/st3-glowfish-theme) for that nice vintage green-on-black, and [Source Code Pro Extra Light](https://github.com/adobe-fonts/source-code-pro) for a font that's easy on the eyes.

I love the feeling of closing browser tabs _en masse_ after a PR is merged.

I really like markdown.

I can't watch HBO's _Silicon Valley_. It's way too real.
