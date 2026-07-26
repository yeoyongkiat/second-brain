# service-requirements — INDEX

Singapore **Healthcare Services Act (HCSA) legislation**, downloaded from [Singapore Statutes Online](https://sso.agc.gov.sg). Each regulation is stored as a **paired official PDF + markdown** (the markdown OCR-converted via Mistral, tables preserved). Every file's frontmatter carries its statute number and SSO source URL.

Organised into 6 leaf subfolders (each holds only paired PDF+MD legislation, so they are summarized here rather than each getting its own index):

## Subfolders

- **`healthcare-services-act-hcsa/`** — the parent **Healthcare Services Act 2020** (Act 3 of 2020) + the Act (Exemption) Order 2021.
- **`general-regulations/`** — HCS (General) Regulations 2021.
- **`advertisements-regulations/`** — HCS (Advertisement) Regulations 2021 + (Advertisement) (Exemption) Order 2021.
- **`fees-regulations/`** — HCS (Fees) Regulations 2021 (the large fee-schedule tables).
- **`appeals-regulations/`** — HCS (Appeals) Regulations 2022.
- **`service-specific/`** — 15 service regulations (paired PDF+MD each) covering the 16 licensable services. Note two are combined: *Clinical Laboratory + Radiological* (one reg) and *Emergency Ambulance + Medical Transport* (one reg). Also includes the shared *Collaborative Prescribing Service* regulation.

## Retrieval

- Need the legal text of a specific service's requirements → `service-specific/<service> Regulations.md`.
- Cross-cutting rules (fees, advertising, appeals, general) → the matching top-level subfolder.
- **Authoritative citation:** prefer the `.pdf` (exact source); the `.md` is the readable/searchable layer and links back to its SSO URL.

## Maintenance

Update this index if a regulation is added/removed or a subfolder changes. The markdown is OCR-derived — for load-bearing figures, verify against the paired PDF.
