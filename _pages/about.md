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
  const yearsProgramming = monthsProgramming/12;
  const roundedYearsProgramming = yearsProgramming.toFixed(2);
</script>

Hi. I'm Neil. I'm a senior software engineer with <script>document.write(roundedYearsProgramming)</script> years of experience; I started full stack, and now specialize in backend tech and cloud infrastructure. Currently living in the San Francisco Bay Area.

![Obligatory looking-at-laptop photo](/assets/images/datface.jpg)

## Professional Highlights

### <i class="fas fa-capsules"></i> [Proteus Digital Health](https://www.proteus.com/)

Architected and implemented a highly-configurable rules engine from the ground up. Consumes a patient's data in real time and calculates their triage score, so that doctors can rapidly identify priority patients. Written in node.js on AWS Lambda, DynamoDB, Kinesis, SQS, API Gateway. Complete unit test coverage with Mocha and Chai.

Owned, developed, maintained, and provided troubleshooting and support for mock data generation tool used by all product teams. Written in Python 3. Used Behave to convert BDD statements to cloud API calls with templated request bodies. Used by developers to develop new product features by generating simulated user resources and metric data in Cloud. Created unit test suite and integrated into build to prevent regression bugs. Converted tool into importable package, so it could be used directly in other teams' test suites.

Built a test suite for our manufacturing facility in Hayward.

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

### <i class="fas fa-coffee"></i> Personal

Wrote the stock trading tool for volume spread analysis that I always wished I had.

## Industry Experience

I've worked in these spaces:

- Event-driven, serverless architecture.
- Internal tooling.
- IoT prescription drugs.
- FDA regulatory compliance.
- HIPAA compliance.
- Medical devices and pharmaceuticals.
- Aerospace.

## My Philosophy of Software Development

### I strive to write clean, consistent, maintainable code with no suprises.

I'm mindful of best practices, like keeping code [SOLID](https://github.com/ryanmcdermott/clean-code-javascript#solid) and [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). I design for scale, avoid nested iteration and extraneous database calls, and cache all the things. I care about code quality, and set aside dedicated time for code reviews.

### I strive to make life easy for whoever touches the code after me.

I care about minimizing the cognitive load of context switching. Given the choice to write clever code or legible code, legible wins every time. I use intuitive variable names and write clear, useful comments. I take documentation seriously. 

### I strive to add value to my team.

I care about creating a non-toxic workplace and always check my ego at the door. I like to help others be successful, and am happy to spend time answering questions for stakeholders and other developers.

I love my work.

## Bonus Skills

I have additional backgrounds in:
- **technical writing**. Most engineers hate writing docs. I actually enjoy crafting well-written documentation and understand how to convey technical concepts.
- **training**. I enjoy explaining concepts and teaching others.
- **customer success**. I enjoy helping other people achieve their technical goals.

## Miscellaneous

I like OSX in dark mode, [Sublime Text 3](https://www.sublimetext.com/3), a modified [Glowfish theme](https://github.com/czettnersandor/st3-glowfish-theme) for that nice vintage green-on-black, and [Source Code Pro Extra Light](https://github.com/adobe-fonts/source-code-pro) for a font that's easy on the eyes.

I love the feeling of closing browser tabs _en masse_ after a PR is merged.

I really like markdown.

I can't watch HBO's _Silicon Valley_. It's way too real.

## Technology

I've worked with the following tech:

| Category | Tech | Level |
| ------ | ------ | ------ |
| *Languages* | | |
| | Python 3 | Strong |
| | JavaScript | Strong |
| | node.js | Strong |
| | Ruby | Familiar |
| | Java | Rusty |
| | Go | Rusty |
| *Amazon Web Services* | | |
| | Cloudwatch | Strong |
| | DynamoDB | Strong |
| | Kinesis | Strong |
| | Lambda | Strong |
| | SQS | Strong |
| | S3 | Strong |
| | RDS/Aurora | Familiar |
| | Secrets Manager | Familiar |
| *Databases & ORMs* | | |
| | MySQL | Familiar |
| | Postgres | Familiar |
| | MongoDB | Familiar |
| | Redis | Familiar |
| | Sequelize ORM | Familiar |
| | Bluebird ORM | Familiar |
| | Mongoose ORM | Familiar |
| *Testing* | | |
| | Mocha | Strong |
| | Chai | Strong |
| | Behave | Strong |
| | nose2 | Strong |
| | Sinon | Familiar |
| | PhantomJS | Familiar |
| *Front End* | | |
| | AngularJS | Familiar |
| | React.js | Familiar |
| | HTML5 | Familiar |
| | CSS3 | Familiar |
| *Miscellaneous* | | |
| | Git | Strong |
| | JIRA | Strong |
| | Confluence | Strong |
| | BitBucket | Strong |
| | *NIX | Strong |