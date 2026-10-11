# Changelog

All notable changes to the LazyDragon Oculus-16 analog IR sensor array are
documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- `code/oculus_16.ino`: ISR-driven 16-channel reader for the onboard 74HCT4067
  mux (non-blocking scan, per-channel calibration, integer position, pattern
  classification) — mirrors the Oculus-8 reader with the same function names.

### Changed
- Reader mux wiring moved to analog pins only: `S0`..`S3` on `A0`..`A3` (PORTC
  0..3), common `C0` on `A4` (ADC4).

## [1.0.0] - 2026-10-08

### Added
- Hardware: Oculus-16 — 16x QRE1113GR reflective IR sensors multiplexed through
  a 74HCT4067PW 16:1 analog mux down to a single analog output.
- Documentation: `README.md`, `MANUAL.md`, `DEBUG.md`.
- Licensing: hardware under CC BY-NC-SA 4.0 (`LICENSE`), firmware under
  PolyForm Noncommercial 1.0.0 (`LICENSE-CODE`).
- `COMMERCIAL-LICENSE.md` for paid commercial/royalty licensing.
- README license badges, documentation index, and no-warranty notice.
- Repository structure aligned to the LazyDragon standard (`assets/`,
  `datasheets/`, `fabrication/`, `code/`; docs at root).
