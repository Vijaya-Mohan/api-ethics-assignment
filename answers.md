# API Ethics Assignment

## Task 1 — Classify and Handle PII Fields

The dataset contains the following fields:

full_name, email, date_of_birth, zip_code, job_title, diagnosis_notes

Each field was evaluated for its ability to directly or indirectly identify an individual and handled according to data minimisation and privacy‑by‑design principles.

| Field | PII Classification | Handling Decision | Justification |
|------|-------------------|------------------|---------------|
| full_name | Direct PII | Drop | A full name uniquely identifies an individual and is unnecessary for external research analysis. Removing it eliminates re‑identification risk. |
| email | Direct PII | Drop | Email addresses directly identify and allow contact with individuals. They provide no analytical value to external researchers. |
| date_of_birth | Indirect PII | Mask (e.g. age or year only) | Full dates of birth can enable re‑identification when combined with other fields. Masking preserves analytical usefulness while reducing risk. |
| zip_code | Indirect PII | Mask (e.g. first 2–3 digits only) | Precise location data can identify individuals, especially in small populations. Partial ZIP codes support regional analysis safely. |
| job_title | Indirect PII | Pseudonymise / Generalise | Specific or rare job titles can enable re‑identification. Mapping to broader job categories reduces this risk. |
| diagnosis_notes | Direct / Highly Sensitive PII | Pseudonymise or heavily redact | Free‑text diagnosis notes may contain identifiers and sensitive health data. Strong de‑identification is required before sharing. |

---

## Task 2 — Audit the API Script for Ethical Compliance


```python
import os
import requests
import time

API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.environ.get("HEALTHSTATS_API_KEY")

records = []
for page in range(1, 11):  # reduced scope to respect free-tier limits
    response = requests.get(
        API_URL,
        params={"page": page},
        headers={"Authorization": f"Bearer {API_KEY}"}
    )
    response.raise_for_status()
    data = response.json()
    records.extend(data["results"])
    time.sleep(1)  # basic rate limiting
```
The script stores raw patient‑level records permanently, including sensitive and identifying health data.
This violates ethical principles such as **data minimisation**, **purpose limitation**, and **storage limitation**, and may breach healthcare data protection regulations.
