# ARC N-BRIDGE

Nigerian multilingual voice communication, built on the N-ATLaS models (Hausa, Yoruba, Igbo and Nigerian-accented English).
A research prototype prepared for the National AI Innovation Challenge.

It has two jobs:

1. **Evaluation Lab** - test speech-to-text, translation and text-to-speech one at a time, score the results with a person's judgement, WER and CER.
2. **VoiceBridge** - two people speaking different languages hold a conversation: speak, check the recognised text, translate, hear the reply.

Everything the app shows comes from the database. If a model, a language or a voice is not available, the app says so; it never fabricates a result.

## How it fits together

```
Browser  --->  PHP app (XAMPP: Apache + MySQL)  --->  Python AI service (FastAPI)  --->  models
 mic, UI        login, roles, database, files          X-API-Secret header                 N-ATLaS ASR (Hugging Face)
                reports, evidence                      runs on your PC or on Colab         NLLB / N-ATLaS LLM (translation)
                                                                                           MMS-TTS (voice)
```

* The browser never sees the AI service secret or the Hugging Face token. Only PHP talks to Python.
* Recordings and generated files live in `arc_nbridge/storage/` and are served only through PHP after a login and permission check.

## Folders

| Folder | What it is |
|---|---|
| `arc_nbridge/` | The PHP web app. Put it (or point Apache) at `http://localhost/N-ATLAS/arc_nbridge/`. |
| `arc_nbridge/database/schema.sql` | MySQL schema and safe default settings. |
| `arc_nbridge_ai/` | The Python FastAPI AI service, with its tests in `tests/`. |
| `colab/ARC_NBridge_Colab.ipynb` | Runs the AI service on a Colab GPU (use this on a slow connection). |
| `docs/` | Installation, Colab, verification and troubleshooting guides. |

## Pages

| Page | Who | Purpose |
|---|---|---|
| Dashboard | everyone | live counts and AI service status |
| Evaluation Lab | admin, researcher | speech to text, translation, text to speech tests with evaluation |
| VoiceBridge | everyone | two-person translated conversation; participants see only their own |
| Participants | admin, researcher | pseudonymous participants and recorded consent |
| Reports | admin, researcher | CSV, Excel and PDF from the database, with filters |
| Evidence | admin, researcher | recording integrity check and a verifiable evidence pack (ZIP) |
| Settings | admin | AI service address and secret, languages, translation, voice, privacy |

## Start here

1. Follow `docs/INSTALLATION.md`.
2. If your connection is slow, use `docs/COLAB_SETUP.md` for the AI service.
3. Read `docs/VERIFICATION.md` before you quote any result: it lists what has been tested and what has not.

## Privacy defaults

No names, phone numbers, addresses or ID numbers are stored for participants. Consent is recorded per participant; withdrawing it removes that participant's records from every report and export. Demo records are excluded from reports unless you ask for them. Passwords are stored as hashes, and the AI secret is stored encrypted.

## Model terms

The N-ATLaS models are published by NCAIR1 on Hugging Face under their own terms of use and are gated: you must accept the terms with the account whose token you use. Read those terms before any wider deployment. The optional MMS-TTS voices (Meta) are published under a non-commercial licence. NLLB-200 has its own licence. Check each model card yourself.
