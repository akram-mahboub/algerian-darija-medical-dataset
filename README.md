# Algerian Darija Medical Triage Dataset

A small, expert-labelled dataset of **859 patient symptom descriptions written in Algerian Darija**, each tagged with how urgent the case is and which medical service it belongs to. To our knowledge it is the first medical triage dataset for Algerian Darija.

It was built for **Najda**, a medical triage system that reads a patient's complaint and decides (1) how soon they need to be seen and (2) which specialty should see them.

---

## What's inside

Each row is one short symptom sentence with two labels:

| Column | Description | Possible values |
|---|---|---|
| `text` | The patient's symptom description, in Algerian Darija | free text |
| `urgency` | How soon the patient needs care | `Urgent`, `Consultation` |
| `service` | Which medical specialty fits the complaint | `Cardiology`, `Neurology`, `Emergency Medicine`, `Internal Medicine`, `Gynecology` |

**Urgency** is binary, guided by one simple question: *does this person need to be seen today, or can the visit be planned?*
- `Urgent` → needs to be seen the same day.
- `Consultation` → can be scheduled normally.

---

## The numbers

- **859 sentences** — no empty rows, no duplicates.
- **Median length:** 11 words / 51 characters (these are short, real complaints).
- **Urgency split:** 445 `Urgent` / 414 `Consultation` (nearly balanced).
- **Services:** roughly comparable in size, with Gynecology the smallest.

How urgency lines up with each service:

- **Emergency Medicine** — 100% Urgent
- **Internal Medicine** — ~75% Consultation
- **Cardiology** and **Gynecology** — about 60/40
- **Neurology** — almost 50/50 (neurological symptoms in Darija can be either routine or time-critical with very similar wording)

---

## Where the data comes from

Two sources, mostly real text:

1. **Public Facebook health groups** for Algerian patients, where people describe symptoms in their own words — short, colloquial, code-switched, sometimes in Arabizi (Latin letters with numbers).
2. **Real-life cases** from the authors' surroundings, which give reliable urgency labels because the outcome was known (sent home, seen same day, or admitted).

A small portion of **synthetic sentences** was added only to fill coverage gaps, written in the same style as the real ones.

Every sentence was labelled by **doctors and medical experts**, who also filtered out anything that didn't clearly fit one of the five services rather than forcing it into the nearest one. The 859 count is the size *after* this filtering.

---

## What makes Darija medical text hard

This is what the dataset captures (and why it's useful):

- **No fixed spelling** — the same word is written several ways, and all spellings are kept.
- **Code-switching** — Darija mixed with French and Modern Standard Arabic medical terms. These are kept, since they're often the most informative words.
- **Arabizi** — Latin script with digit substitutions, e.g. `3`, `7`, `9` standing in for specific Arabic sounds (example: *"3yani sda3 chdid w 7ass bel 9alb yedreb bezzaf"*).
- **Genuine ambiguity** — some complaints can be urgent or routine depending on context.
- **Noise as signal** — emojis and stretched-out words (e.g. "veeery") are kept because they often correlate with urgency.

---

## Preprocessing

Preprocessing is intentionally light, so the data stays close to how patients actually write:

- Normalise whitespace
- Remove diacritics
- Unify letter variants
- Collapse stretched/elongated letters
- Scrub personal information (PII)
- **Keep** emojis, punctuation, and French words
- Convert Latin-script (Arabizi) samples to Arabic script using a fixed mapping, so the whole dataset ends up in a single script

---

## Example rows

| text | urgency | service |
|---|---|---|
| (chest pain, can't breathe) | `Urgent` | `Cardiology` |
| (mild recurring headache for a week) | `Consultation` | `Neurology` |
| (heavy bleeding, severe pain) | `Urgent` | `Gynecology` |

> Replace these with a few real sentences from the dataset before publishing.

---

## How to load it

```python
import pandas as pd

df = pd.read_csv("dataset.csv")   # update with the actual filename
print(df.shape)                   # (859, 3)
print(df["urgency"].value_counts())
print(df["service"].value_counts())
```

---

## Intended use & limitations

This dataset is meant for **research** on low-resource (Algerian Darija) medical NLP — urgency classification, specialty routing, and retrieval-based methods.

Please keep in mind:
- It is **modest in size** (859 sentences) and **partly synthetic**.
- It comes from a single corpus, so results may not transfer directly to other regions or hospitals.
- It is **not a medical device** and must **not** be used to make real clinical decisions about real patients.

---

## Citation

If you use this dataset, please cite the Najda paper:

```bibtex
@inproceedings{najda2025,
  title     = {Najda: A Hybrid Retrieval-Augmented Classifier for Medical Triage in Algerian Darija},
  author    = {Boustil, Amel and Mahboub, Akram and Bramki, Nasreddine},
  year      = {2025},
  note      = {University of M'Hamed Bougara, Boumerdes, Algeria}
}
```

---

## License

This dataset is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.

You are free to share and adapt the data for any purpose, including commercially, as long as you give appropriate credit, link to the license, and indicate if changes were made. See the `LICENSE` file for details, or the full legal text at <https://creativecommons.org/licenses/by/4.0/legalcode>.

**Attribution example:**

> "Algerian Darija Medical Triage Dataset" by Akram Mahboub, Nasreddine Bramki, and Amel Boustil is licensed under CC BY 4.0. Source: https://github.com/akram-mahboub/algerian-darija-medical-dataset

**Scope:** The license covers the dataset's annotations (urgency and service labels), its compilation, and its structure — the original contribution of the authors. A portion of the underlying symptom texts was collected from public sources; the license does not assert ownership over any third-party content that may remain subject to its original authors' rights. Personal identifying information has been removed, and the dataset is provided for research purposes only — it is not a medical device.
