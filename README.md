# Persian Pharmacology Handbook Pipeline + Live Auto-Trading System

**This folder is not one project and the name only describes the second one.** It holds two
unrelated bodies of work plus the operational scripts for the second, sharing one directory:
a large document-generation pipeline that builds a Persian pharmacology handbook from scanned
source pages, and the live crypto/stock auto-trading system deployed to a VPS - including three
snapshots of the code that is actually running on that server.

**Suggested repo name:** split it - `pharm-handbook-pipeline` and `tsetmc-gmgn-autotrader`. Do not publish as one repo.
**Stack:** Python 3 + openpyxl/python-docx/PyMuPDF + Pillow (handbook); Python 3 trading engine with Telegram/GMGN/TradingView ingestion; ~79 paramiko/SSH ops scripts
**Status:** active - both parts in use
**Last modified:** 2026-09-19
**Scale:** 372 `.py` (349 at this root), 1 868 `.png`, 46 `.docx`, 17 `.pdf`, 50 `.json`

## Part A - pharmacology handbook pipeline

Turns scanned Persian pharmacology pages into a structured 182-drug handbook in DOCX/PDF, in
several editorial variants (pure / with notes / no notes / verbatim / with guide).

- `populate_all_handbook_pages.py` (1 474 lines) and `clean_populator.py` (1 433 lines) - page
  population and cleanup of the scanned corpus.
- `generate_master_data.py` (1 269), `handbook_data.py` (1 084), `generate_v4_dataset.py` (686)
  - dataset builders; `handbook_master_data.py`, `handbook_verbatim_data.py`,
  `handbook_pure_verbatim_data.py` are the parallel data variants.
- `build_the_ultimate_182_pages_handbook.py`, `build_full_master_handbook.py`,
  `build_complete_182_drugs_catalog.py`, `build_entire_pharmacology_database.py`,
  `assemble_final_handbook.py`, `assemble_complete_mega_handbook.py`, `build_full_handbook.py`
  - successive assembly passes; `build_strict_pure_verbatim_data.py` enforces the verbatim mode.
- `chunk_antibiotics_all.py` / `chunk_antibiotics_part2.py` - section chunking.
- Reference harvesting for the substance entries targets `erowid.org`, `psychonautwiki.org`,
  `ncbi.nlm.nih.gov`, `pubmed.ncbi.nlm.nih.gov` and `doi.org`.
- Input/output directories (~20): `scanned_pages_1`, `scanned_pages_all`, `scanned_headers_1`,
  `scanned_overview`, `scanned_titles`, `all_pages`, `all_pages_pdf1`, `all_pages_pdf2`,
  `all_titles_pdf1`, `all_titles_pdf2`, `batch_pages`, `extracted_batch`, `enrich_batches`,
  `overview_pdf1`, `overview_pdf2`, `nokat`, `nokat_grids`, `handbook_previews*` (five variants),
  `handbook_icons`, `fonts`. These are intermediate renders, not source.
- Related work living elsewhere that belongs to the same product family:
  `Documents\Projects\monograph-ocr`, `Downloads\pdf-extract`, `Documents\antigravity\brave-fermi`,
  and the `piru` substance dose journal.

## Part B - live auto-trading system

An LLM-gated execution bot for Iranian and crypto markets, deployed to and driven from a VPS.

- Engine (present in three snapshots - `vps_ahura_code`, `vps_current`, `server_inspect`):
  `main.py`, `ingestion.py`, `analysis.py`, `execution.py`, `strategy_trend_pullback.py`
  (`server_inspect` additionally has `sp2l_strategy.py`), plus `positions.json`.
- `ai_gatekeeper.py` and `ai_gatekeeper_antichase.py` - a pre-trade veto layer: the candidate
  order is judged by an LLM before execution, with an explicit anti-chasing rule.
- `bitpin_client_vps.py` (BitPin exchange), `check_gmgn_funds.py`, `check_gmgn_positions.py`,
  `clean_gmgn_positions.py`, `apply_gmgn_vps.py`, `check_main_gmgn.py`, `bp_wallets_script.py`,
  `check_live_bp_wallets.py` (GMGN / Binance-smart-chain wallet tracking),
  `tradingview_analyzer.py`, `twitter_scraper.py` (1 079 lines, signal source),
  `test_tabdeal.py` (TabDeal broker), `update_altfins_vps.py`, `profit_tracker.py`,
  `analyze_vps_trades.py`.
- ~79 remote-ops scripts at this root: `check_vps_*`, `verify_*_vps`, `deploy_*`,
  `upload_to_server.py`, `trace_*_vps`, `test_*_vps` - health, memory, dry-run and state checks
  against the running box.

## Notes - read before publishing any of this

- **`config.json` in this root and in each of `vps_ahura_code\`, `vps_current\`,
  `server_inspect\` holds live credentials**, and the trading configuration runs with
  `dry_run = False`. The ~79 ops scripts embed SSH hosts and root passwords in plaintext.
  **None of Part B is publishable until every one of those is stripped and the credentials are
  rotated.** Committing them even briefly to a public repo is unrecoverable - history keeps them
  after deletion.
- `vps_ahura_code\`, `vps_current\` and `server_inspect\` are **copies of code deployed to a live
  server**. Publishing them discloses your production topology, service names and paths, not just
  source. Treat them as infrastructure artifacts, not repo content, and keep them out of git.
- Part A's ~20 image directories and 1 868 PNGs are pipeline intermediates. Add them to
  `.gitignore`; only the scripts and the committed dataset JSON belong in the repo.
- Part A's DOCX/PDF outputs contain clinical reference material. If any of it derives from a
  copyrighted source text, the *outputs* carry that copyright even though the scripts are yours -
  publish the pipeline, not the generated handbook.
- Two unrelated systems sharing one folder will keep producing misfiled work. Splitting them is
  the single highest-value cleanup available here.
