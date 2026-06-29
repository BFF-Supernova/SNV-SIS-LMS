# SNV SIS — Design System

A configuration-driven design system for **SNV SIS / StudentFlow**, the Egyptian education
platform from Supernova Middle East. Built directly from the three scope documents
(Scope & Setup Wizard · Wizard Decision Logic · SaaS Control Plane).

Everything is **standalone HTML + CSS** — no build step, no dependencies. It renders as-is in
Claude Design, Replit, or any static host, and is meant to be reviewed/assessed in either.

## How to view

Open `design/index.html` in a browser. It links every foundation, component, and screen.

## Structure

```
design/
├── index.html                 # Design system overview & map
├── styles/
│   ├── tokens.css             # Single source of truth: color, type, spacing, radius, elevation
│   └── base.css               # RTL-first base + component styles (buttons, cards, forms, tables, shell…)
├── foundations/
│   ├── colors.html            # Nova-violet brand, gold star accent, semantic + neutral scales
│   ├── typography.html        # Tajawal/Cairo Arabic-first type scale, numerals
│   └── spacing.html           # 4pt grid, radii, elevation
├── components/
│   ├── buttons-badges.html    # Buttons + tenant-lifecycle / plan-tier badges
│   ├── forms.html             # Inputs, selects, toggles, single-select option cards
│   └── wizard-kit.html        # Stage shell, progress rail, Express/Custom, dependency toast
└── screens/
    ├── wizard-welcome.html        # Stage 0 — locale + Egypt regulatory pack + Express/Custom
    ├── wizard-profile.html        # Stage 1 — organization type (the master switch) + live module preview
    ├── wizard-academic.html       # Stage 2 — academic structure + auto-dependency resolution
    ├── wizard-provisioning.html   # Stage 12 — "Applying your configuration…"
    ├── app-dashboard-school.html  # Tenant app, School profile — full SIS+LMS navigation
    ├── app-dashboard-teacher.html # Tenant app, Teacher profile — ~6 menus (config-gating proof)
    ├── control-tenant-directory.html  # Control plane — tenant directory, MRR, health
    └── control-tenant-detail.html     # Control plane — lifecycle, usage meters, PDPL-safe impersonation
```

## How it maps to the specs

| Spec idea | Where it shows up |
|---|---|
| Arabic-first / RTL | `dir="rtl"`, Tajawal type, every tenant screen |
| Master-switch org type sets defaults | `wizard-profile.html` (live "modules that will turn on" preview) |
| Auto-dependency resolution ("we turned that on too") | `wizard-academic.html` + `wizard-kit.html` toast |
| Express vs Custom | `wizard-welcome.html`, `wizard-kit.html` |
| Egypt regulatory pack (PDPL/ETA/National ID), non-overridable | `wizard-welcome.html` info banner, forced toggle in `forms.html` |
| Config-driven "hide, don't disable" | School vs Teacher dashboards (same components, different nav) |
| Two-plane architecture (tenant vs control plane) | tenant screens (RTL/AR) vs control-plane screens (LTR/EN) |
| Tenant lifecycle states | `buttons-badges.html` pills + `control-tenant-detail.html` rail |
| Usage metering & limits | `control-tenant-detail.html` meters |
| PDPL-safe impersonation (reason-coded, time-boxed, scoped, audited, tenant-visible) | `control-tenant-detail.html` modal + audit table |
| Isolation model (pooled+RLS / schema / DB-per-tenant) | `control-tenant-directory.html` column |

## Design tokens

`styles/tokens.css` is the contract. Brand is **nova-violet** (trust + intelligence) with a
**gold star** accent for premium add-ons and Supernova brand moments. Semantic colors are
reserved strictly for state. Spacing is a 4pt grid.

## Syncing to Claude Design

Each preview file starts with a `<!-- @dsCard group="…" -->` marker so the Claude Design
**Design System pane** can index it automatically. This web environment can't run
`/design-login`, so the project lives here in-repo. To push it into a claude.ai/design
project, run the `/design-sync` skill from a local Claude Code terminal (or use Claude
Design's "Send to Claude Code Web"), targeting this `design/` directory.
