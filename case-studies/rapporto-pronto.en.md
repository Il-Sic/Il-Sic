# Rapporto Pronto

[Italiano](rapporto-pronto.md) · English · [Profile](../README.en.md)

I’m building Rapporto Pronto for field technicians who document maintenance visits. After a visit, the technician knows what happened on site. The office needs that account in writing. The app lets the technician dictate a note and check the resulting service report.

## From a note to a reviewed report

A technician can dictate or type a note. For a recording, they can read the transcript during review and replay the passage behind a detail in the report. If a part name looks wrong, they can check what they said, correct the draft and confirm the report.

I built the review interface with React, TypeScript and Next.js. The Node.js backend processes notes in the background and stores reports in PostgreSQL. I kept the original note available during review because a plausible sentence can still describe the wrong repair.

## AI integrations and simulated mode

The first prototype simulated text interpretation. In the current version, I’ve integrated Deepgram to transcribe audio and OpenAI to interpret the text and populate the draft report. For development and testing, I can also simulate both steps using sample transcripts and responses without calling either service.

## Project status

Note capture, drafting and review are implemented in the development build. Rapporto Pronto is not yet in production.
