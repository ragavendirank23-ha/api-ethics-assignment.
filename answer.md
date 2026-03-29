# API Ethics Assignment

## Task 1 — Classify and Handle PII Fields

| Field | Classification | Action | Justification |
| :--- | :--- | :--- | :--- |
| **full_name** | Direct PII | **Drop / Mask** | Directly identifies an individual; not needed for statistical research. |
| **email** | Direct PII | **Drop** | Contact information is highly sensitive and carries high risk if leaked. |
| **date_of_birth** | Indirect PII | **Generalize** | Exact DOB can lead to re-identification; convert to "Age" or "Birth Year". |
| **zip_code** | Indirect PII | **Mask/Truncate** | Can be used with other data to ID someone; provide only the first 3 digits. |
| **job_title** | Indirect PII | **Keep/Group** | Useful for socio-economic research, but rare titles should be generalized. |
| **diagnosis_notes** | Sensitive PII | **Pseudonymize** | Contains PHI (Protected Health Information); must be detached from names. |

---

## Task 2 — Audit the API Script for Ethical Compliance

### Violation 1: Rate Limiting & API Abuse
**Problem:** The script loops through 100 pages without any delay (`time.sleep`). This can overwhelm the server and likely violates the "free tier" Terms of Service (ToS).
**Correction:**
```python
import time
# ... inside the loop
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    # Add a delay to respect rate limits
    time.sleep(1)