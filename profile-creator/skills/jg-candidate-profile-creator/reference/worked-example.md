# Worked example — Andrew H., Senior Software Engineer

The shipped template comes populated with this candidate. Use it as the bar for bullet depth and tone, then replace it. All of it came from a real resume — nothing was invented.

## Data shape

Candidate data lives in `renderVals()` in `ProfileVariant.dc.html`:

```js
name: 'Andrew H.',                    // first name + last initial only
role: 'Senior Software Engineer',     // role being pitched
monogram: 'AH',                       // fallback if no headshot
tagline: 'Java-Focused Backend & Cloud Engineer  ·  With Jahnel Group since 2019',
intro: '…',                           // 3–4 sentences
industries: 'Gaming Platform  ·  E-Commerce & Personalization  ·  Investment Consulting',
entries: [ { title, dates, role, industry, bullets: [...], stack } ],
skills:  [ { cat, items: [...] } ],   // items array; the dot-list is derived
education: 'B.S. Neuroscience & Mathematics',
school: 'Wesleyan University',
funFact: 'I like to go on walks in the city!',
```

## What a strong bullet looks like

Good — names the system, the technology, and what it accomplished:

> Led migration of core backend workers to AWS Fargate with encrypted Redis clusters and automated TLS rotation for NATS messaging.

> Built GDPR deletion alerts, parental-consent systems, and age/country data mapping, plus MFA, OAuth/JWT and scoped IAM policies.

> Resolved composite-fund data discrepancies safeguarding the accuracy of client-facing financial reporting.

Weak — vague, or contribution-level rather than ownership-level:

> ~~Contributed to a full rewrite of the platform.~~ → *Drove a full rewrite of the client's online product configurator, modernizing the platform end to end.*

> ~~Worked on backend services.~~ → *Anchored a multi-year engagement as a trusted long-term engineer, owning core backend services across the platform.*

## Consolidating to fit

When the page overflows, merge related facts instead of deleting them. Two bullets:

> Built enterprise-scale Java Spring Boot services managing SQL financial-asset data on AWS Lambda and DynamoDB.
> Resolved composite-fund data discrepancies safeguarding client-facing financial reporting.

become one, with nothing lost:

> Built enterprise-scale Java Spring Boot services managing SQL financial-asset data on AWS Lambda and DynamoDB, resolving composite-fund discrepancies that safeguarded client-facing financial reporting.

## Skill category ordering by role

Same skills, reordered per opening:

| Target role | Category order |
|---|---|
| Backend / platform engineer | Languages & Frameworks → Cloud, Platforms & Tools → Security & Compliance |
| Front-end engineer | Front-End & Frameworks → Languages → Cloud & Tools |
| DevOps / cloud engineer | Cloud, Platforms & Tools → Languages & Frameworks → Security & Compliance |
| Security-focused role | Security & Compliance → Cloud, Platforms & Tools → Languages & Frameworks |

Never drop skills to make a category look sharper, and never mark any skill as stronger or weaker than another.
