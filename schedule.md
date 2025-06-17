# Schedule

### Current Schedule Total: ~14 weeks as of 5/21/25

## Build Quiz Functionality - 5 Days

- Set up database tables to handle an understandable csv format for easy importing, exporting, editing, etc
  - Adjust imports to better handle all questions
- Write quiz access logic/practice exam logic

## Build Exam Application - 5 Days

- Take fields from current application and build in the new UI
- Connect fields individually to proper database tables and test
- Separate form as a component so that submission logic is part of higher order functions
  - Pass query strings for course data, user data, etc through component
    functions instead of signals

## Lesson Content Pool - 5 Days

_Discussion_

- Decide on navigation format and options
- Decide on edition/language management structure
- Discuss sidebar in lesson pages/navigation instructions

## Data Migrations - 5 Days

- Run rounds of migrations from MGC database and airtable into postgres, editing queries as necessary
- Preserve migration structure so they are repeatable in the future without duplicating old data

## Set up Hosting - 3 Days

_Discussion_

- Options to discuss: GCP, Render, Fly, DO, …..etc.
- Set up deployment for both server and database
- Build testing, linting and security pipeline with Github Actions to ensure code quality/integration
- Set up issues in repository for future update management

## Authentication/Authorization - 5 Days

_Discussion_

- Add basic password authentication, possibly using same encoding model as wordpress to carry over old hashed passwords
- Build authorization profiles: admin, re-seller, org-manager, etc.

## Stripe Api Integration (Payments) - 3 Days

- Get api key for MGC
- Set up products for courses, and variable products for add on’s/other services
  - possibly in memory or in db, feed stripe that data for each
    transaction
  - Base schema on reporting structure
  - Seller column with MGC as default

## Sendgrid Api Integration - 2 Days

- Set up email from support, discuss support management pipeline

## Finish Re-Seller/Organization Portals - 3 Days

_Discussion_

- Discuss discount structure, logic for orgs, and responsibility for org
  management

## Build Home Page and Blog as Static - 5 Days

- Serve blog entries from static files for best SEO performance and load speeds

## Build Certification Management Portal - 15 Days

_Discussion_

Set up ReTool to mimic airtable and add features/reporting capabilities
Play with mild airtable integration depending on rollout strategy
Email functionality
Document generation (using object storage)
