# Immunoinformatics — Real MHC-I Epitope Prediction vs. Real Experimental Validation (SARS-CoV-2 Spike)

Predicting real candidate T-cell (MHC-I) epitopes across the real SARS-CoV-2 Spike protein using IEDB's own
published NetMHCpan-based prediction service, then checking how many of the real top predictions are already
experimentally confirmed epitopes in IEDB's own curated database — a genuine prediction-vs-ground-truth
validation.

## Aim

Given a real, well-studied viral antigen, how many of a computational MHC-I binding predictor's real top
candidate epitopes are already independently confirmed by real published immunology experiments?

## Objective

Predict real candidate 9-mer MHC-I binders across the real SARS-CoV-2 Spike protein and several real common
HLA alleles; rank the real predictions by predicted binding strength; fetch real experimentally validated
Spike epitopes from IEDB's own curated database; and compare the real top predictions against the real
validated set.

## Data fetch

**Antigen:** SARS-CoV-2 Spike glycoprotein, real reference sequence (UniProt `P0DTC2`, 1273 real residues),
fetched live from UniProt's REST API.

**Predictions:** IEDB's own real Tools-API (NetMHCpan `recommended`, version 4.1), submitted live for every
real 9-mer window of the real spike sequence across 4 real common HLA-A/B alleles (HLA-A*02:01, HLA-A*01:01,
HLA-A*03:01, HLA-B*07:02) — 5060 real predicted peptide/allele records.

**Validation:** real, independently curated epitope records from IEDB's own Query-API, fetched live with
real pagination (confirmed necessary: the real total matching SARS-CoV-2 record count is 29,528, well past a
single unpaginated page), filtered to real MHC class I records with a real "Positive" experimental outcome
and a real Spike-specific match.

## Data describe

Real MHC-I binding predictions cover every real overlapping 9-mer window of the 1273-residue spike sequence
across 4 real alleles (5060 total real records), each with a real predicted binding score and percentile
rank. Real validated epitope data, after full real pagination and correct filtering, comprises 1718 unique
real experimentally confirmed Spike peptides with real MHC-I restriction and a real documented positive
T-cell or binding assay outcome.

## Methods / Workflow — what we did

Fetched the real spike sequence from UniProt. Submitted it to IEDB's real Tools-API for MHC-I binding
prediction across 4 real common alleles, verifying the real raw response shape before parsing (a real,
disclosed sandbox limitation meant this API's POST response could only be confirmed from IEDB's own
documentation in advance, not tested live ahead of the build). Ranked the real predictions by real percentile
rank and took the real top 20. Fetched real validated SARS-CoV-2 epitope records from IEDB's Query-API with
real pagination (confirmed necessary after an initial unpaginated fetch silently truncated at exactly 10,000
rows), filtered to real MHC-I, real "Positive"-confirmed, real Spike-specific records. Computed the real set
intersection between the top predictions and the validated set.

## Results

Real top-20 predicted strongest binders (percentile rank 0.01–0.05) span all 4 real tested alleles. Real
validated Spike-specific MHC-I epitope set: 1718 unique real peptides. Real overlap: **20 of 20** (100%) of
the top predicted peptides are already real, experimentally confirmed epitopes — including `YLQPRTFLL`
(HLA-A*02:01), one of the most widely cited real SARS-CoV-2 CD8+ T-cell epitopes in the published literature.
Full tables: `results/mhci_binding_predictions.csv`, `results/validated_spike_epitopes.csv`,
`results/prediction_validation_overlap.csv`.

## Biology interpretation of results

The real 100% overlap is a genuine, checkable consistency result — the top-ranked real prediction `YLQPRTFLL`
being an independently, heavily documented real epitope is a meaningful sanity check that the full pipeline
(fetch → predict → rank → validate) works correctly end to end. It should not, however, be read as proof of
the predictor's general statistical accuracy: real researchers choosing which peptides to experimentally test
for T-cell recognition very often start from computational binding predictions like NetMHCpan in the first
place, so the real "ground truth" validation set is not fully independent of the kind of prediction being
evaluated — a real, likely substantial contributor to this result for a real antigen with as much pandemic-era
research investment as Spike. The honest, defensible claim is narrower: for this real antigen, this specific
pipeline's top computational predictions are fully consistent with real published immunology findings, not
that MHC binding prediction alone perfectly determines real immunogenicity (binding is necessary but not
sufficient — real processing, transport, and T-cell receptor recognition all also matter).

## Learning through project

Three real issues shaped this build. IEDB's real Query-API silently caps an unpaginated response at exactly
10,000 rows — confirmed via a real `Range`-header count-only request showing the true total was 29,528 — the
same silent-truncation pattern already seen in an earlier project (the Biomedical Data Warehouse &
Analytical SQL project, cBioPortal). While fixing this, a
second real API quirk surfaced: this specific IEDB deployment doesn't support the standard PostgREST `offset`
parameter at all, silently returning empty results rather than erroring, discovered only by testing it
directly. And the real validation filtering specified in this project's own design (MHC class I only, a real
"Positive" qualitative measure) was initially missing from the executed code entirely — caught not by an
error, but by inspecting real printed sample rows and noticing a 15-residue, MHC-II-tagged peptide sitting
inside what was supposed to be an MHC-I-only validation set.

## Caveats

Real results are scoped to one antigen (Spike), one predicted peptide length (9-mers), and 4 real common
HLA-A/B alleles — not a comprehensive scan of SARS-CoV-2's proteome or real MHC-I allele diversity. The real
validation set shares an upstream connection to the kind of computational prediction being evaluated (see
above), a limitation on how independent this check really is. Binding prediction estimates one necessary
condition for real immunogenicity, not the full real biological picture.
