# Rapporto Pronto

[Italiano](rapporto-pronto.md) · English · [Profile](../README.en.md)

I’m building Rapporto Pronto for field technicians who document maintenance visits. After a visit, the technician knows what happened on site. The office needs that account in writing. The app lets the technician dictate a note and check the resulting service report.

<img src="img/rapporto-pronto/percorso.gif" alt="Rapporto Pronto on a technician’s phone, in just over twenty seconds. The technician holds the microphone and describes the job. The app fills in the report, shows the words in the note that the duration comes from and asks how the job ended. The technician answers “to be completed” and hands the phone to the customer, who reads the document from the top and signs with a finger." width="320">

<img src="img/rapporto-pronto/nota-dal-computer.png" alt="The same page in a computer browser. At the top is the technician’s voice note with its transcript. Below it is the report the app drew from it, with the outcome missing and the question “How did it end: fixed, not fixed or to be completed?”. At the bottom is the microphone for the next note." width="820">

## From a note to a reviewed report

A technician can dictate or type a note. For a recording, they can read the transcript during review and replay the passage behind a detail in the report. If a part name looks wrong, they can check what they said, correct the draft and confirm the report.

<img src="img/rapporto-pronto/bozza-in-revisione.png" alt="A draft maintenance report under review on a computer. For the duration and the compressor, the voice note transcript is open, with the words behind each detail highlighted and a Listen button to replay them. At the bottom are the buttons to confirm the report and to download a PDF preview." width="820">

I built the review interface with React, TypeScript and Next.js. The Node.js backend processes notes in the background and stores reports in PostgreSQL. I kept the original note available during review because a plausible sentence can still describe the wrong repair.

## AI integrations and simulated mode

The first prototype simulated text interpretation. In the current version, I’ve integrated Deepgram to transcribe audio and OpenAI to interpret the text and populate the draft report. For development and testing, I can also simulate both steps using sample transcripts and responses without calling either service.

## Technical choices

- **Data separated by company.** Each company’s data is isolated in PostgreSQL with Row Level Security. I access the database with Drizzle.
- **Invite-only access.** People join with a single-use invite, handled by Better Auth.
- **Long jobs in a queue.** Transcription and drafting go through a pg-boss queue, with a worker separate from the web application.
- **PDF.** PDF reports are generated with pdfkit.
- **Tests and checks.** I write tests with Vitest and Playwright and run them on GitHub Actions. Simulated mode lets them run without calling the external services.
- **Deployment.** The Docker environment is already prepared, with encrypted backups and error monitoring, but it is not live yet.

<img src="img/rapporto-pronto/rapportino-pdf.png" alt="The PDF service report with the company’s name: customer, date, work and travel time, work done, outcome, parts, next step and notes. A line at the top notes that the values come from simulated mode, and below it the report says who confirmed it, who signed it and that it is not yet approved. At the bottom is the customer’s signature with a stamp reading “FIRMATO” (signed), their name, the date and the time." width="560">

## Project status

Note capture, drafting, review and the customer’s signature are implemented in the development build. Rapporto Pronto is not yet in production.

The code is private because the product is still in development. I’m happy to walk through it on a call.
