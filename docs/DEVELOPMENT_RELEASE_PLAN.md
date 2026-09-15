# SableOS development release plan

Status: **normative product direction for the current development train**.

This document exists to prevent implementation drift. Before starting work for a milestone, read this document and the milestone-specific document in the owning repository. If an implementation choice is not covered here or in the owning repository, do not silently invent a new product direction. Record the proposed change in documentation first, then implement it after the direction is explicit.

The `R6`, `R7`, `R8`, `R9`, and `R10+` names below are development milestones, not public semantic SableOS product versions. Public product versioning remains governed by `platform_sable/docs/RELEASE_MODEL.md` and exact complete builds remain identified by revision-pinned manifests and artifact hashes.

## 1. Immediate objective

The current priority is to turn the validated Panther build into a dependable daily-driver phone as quickly as possible while preserving SableOS architecture boundaries and evidence-based validation.

The shortest path is not to replace every Android application. The shortest path is:

1. make Sable Start a complete and reliable launcher;
2. finish the approved Sable Start production surfaces without reintroducing demo data or unnecessary privilege;
3. prove core phone functionality on the validated substrate;
4. establish one coherent Sable design/customization system before multiplying Sable-owned applications;
5. build simple, high-value Sable utilities where replacement risk is low;
6. replace complex inherited applications only when there is a clear privacy, UX, maintenance, or architectural benefit.

The daily-driver baseline is:

- incoming and outgoing phone calls;
- contacts sufficient for calling and messaging workflows;
- SMS;
- MMS;
- Wi-Fi connectivity;
- cellular data connectivity;
- Internet/browser access;
- notifications;
- Settings access for platform functions;
- camera/photo viewing sufficient for normal use;
- files access;
- clock/alarm functionality;
- calculator functionality;
- a launcher that exposes the user's launchable applications and can launch them reliably.

No milestone should expand into unrelated polish while one of these foundational capabilities is still unproven.

## 2. Product layering rule

SableOS is deliberately layered:

```text
Sable applications and Sable Start
        |
        | stable Sable product semantics and design contracts
        v
platform_sable common contracts/services/adapters
        |
        | bounded Android integration
        v
Android / validated upstream substrate
        |
        | device-specific integration
        v
device_sable_* + vendor/BSP/firmware
```

The user-facing product does not need Sable-owned replacements for every lower-level Android component in order to be SableOS.

### 2.1 Platform/substrate responsibilities

Keep security- and hardware-critical mechanisms in the validated Android/substrate implementation unless a separately justified architecture change exists. Examples include:

- radio/telephony framework integration;
- IMS/carrier integration;
- Wi-Fi and cellular networking;
- Android permissions and AppOps;
- notification infrastructure;
- package/user/profile management;
- system Settings plumbing;
- SELinux and Binder boundaries;
- HAL and vendor integration.

Sable may provide user-facing entry points or semantic wrappers, but presentation code must not duplicate or bypass these mechanisms.

### 2.2 Sable integration responsibilities

Sable-owned common code should provide stable semantics where the product benefits from a consistent cross-device abstraction. Examples include:

- Sable Start application inventory and search behavior;
- product-level design/theme contracts;
- future Sable service contracts;
- bounded adapters around changing Android APIs;
- cross-device product settings that are truly Sable-owned.

### 2.3 Sable application responsibilities

Sable-owned applications are appropriate when the application is sufficiently self-contained, can be maintained safely, and provides a clear product benefit. The first intended example is Sable Calculator.

Do not create a new Sable application merely because an inherited application exists. The replacement must have an explicit purpose and acceptance criteria.

## 3. Application-source policy

The exact default Phone, Messaging, Contacts, Browser, Camera, Files, Clock, and other application package choices are **not all locked yet**. R7 includes an inventory/selection gate. Do not silently choose an AOSP or GrapheneOS-derived application during implementation without recording the decision and its licensing, maintenance, privilege, integration, and update implications.

The default selection policy is:

| Class | Initial direction | Examples |
| --- | --- | --- |
| Security/platform critical | use validated platform/substrate implementation | networking, telephony framework, permissions, Settings plumbing |
| Complex interoperability application | retain/provision a proven implementation first | Phone, SMS/MMS, Camera, browser |
| Simple self-contained utility | strong Sable-owned candidate | Calculator, Notes, selected Clock UI |
| Content/provider application | use explicit provider/adapter architecture; avoid hard dependency | Weather, Maps, search/content providers |

AOSP-derived or GrapheneOS-derived application reuse must be evaluated component-by-component. Source availability alone is not sufficient; licensing, trademark/branding, privileged permissions, dependencies, update model, security ownership, and compatibility all matter.

Maps and Weather applications currently installed for testing should remain installed through R6 because they provide useful third-party inventory fixtures. They are not baseline SableOS dependencies.

## 4. R6 — Real Sable Start launcher

### 4.1 Goal

Sable Start must stop behaving like a preview/demo and become a reliable launcher surface for the applications that Android says are launchable for the current accessible user profiles.

Detailed normative requirements live in `packages_apps_SableStart/docs/R6_ALL_APPS_AND_GREETING.md`.

### 4.2 Required user-visible behavior

R6 must provide:

- a complete All Apps catalog containing every enabled launcher-visible activity returned through the supported Android launcher APIs for every accessible user profile;
- real application labels and icons;
- deterministic ordering;
- an app count that reflects the same inventory rendered by the UI;
- reliable launch behavior using the exact user/profile and component identity;
- live inventory refresh when packages are added, removed, enabled, disabled, or materially changed;
- a Search surface backed by the same live inventory rather than a second or hard-coded data source;
- search by application label, with package/component identity available as a secondary matching/disambiguation mechanism;
- a device-local time-aware Start greeting;
- preservation of Android user/profile distinctions in the model even if the first UI treatment is simple.

### 4.3 Greeting behavior

The first R6 greeting policy is intentionally simple and deterministic, based on the phone's local time/time zone and requiring no location permission:

```text
05:00–11:59  Good morning
12:00–16:59  Good afternoon
17:00–21:59  Good evening
22:00–04:59  Good night
```

The greeting must refresh when the visible Start surface resumes and when relevant system time/time-zone changes are observed. This policy can become customizable later, but R6 must not expand into a general customization project.

### 4.4 All Apps semantic boundary

"All Apps" means all enabled **launcher-visible activities**, not literally every installed Android package.

Do not fill the launcher with packages that have no user-launchable activity, such as services, providers, overlays, internal framework packages, or other implementation-only packages. A complete installed-package/system inventory may be added later as a diagnostic/settings function, but it is not the R6 launcher catalog.

### 4.5 R6 acceptance direction

R6 is not complete on compilation alone. Runtime evidence must establish, at minimum:

- independently observed Android launcher inventory count;
- Sable Start UI inventory count;
- zero missing expected launcher entries;
- zero unexpected launcher entries relative to the chosen Android launcher semantic;
- successful launch of at least one platform/stock application;
- successful launch of at least one separately installed third-party application;
- live removal of an expendable test application from the Sable inventory after uninstall/removal without requiring a Sable Start process restart, where Android callbacks permit it;
- Search results coming from the same live inventory;
- greeting correctness around representative time buckets or through deterministic clock abstraction tests plus runtime observation;
- no unintended HOME-role/default-launcher mutation unless a separate gate explicitly authorizes it.

### 4.6 R6 non-goals

Do not add during R6 unless separately approved:

- folders;
- application categories;
- recommendation/ranking systems;
- cloud search;
- complex recents intelligence;
- hidden-app policy;
- theme editor;
- icon packs;
- arbitrary launcher layout customization;
- custom Phone/Messaging implementations.

## 5. R7 — Sable Start production surfaces + daily-driver foundation

### 5.1 Goal

R7 has two coordinated responsibilities:

1. promote the approved Sable Metro prototype surfaces into real production Sable Start behavior without demo data, unnecessary permissions, or Quickstep replacement; and
2. prove that the Panther reference device can function as a normal phone using the validated Android/GrapheneOS-derived substrate plus Sable product integration.

Launcher-specific normative requirements live in `packages_apps_SableStart/docs/R7_PRODUCTION_SURFACES.md`.

The Panther-specific runtime matrix lives in `device_sable_panther/docs/R7_DAILY_DRIVER_VALIDATION.md`.

R7 closure requires both launcher-surface qualification and daily-driver capability evidence. Visual expansion must not hide an unproven phone baseline, and daily-driver qualification must not leave approved launcher surfaces as demo-only code.

### 5.2 Sable Start production surfaces

The production launcher should promote these prototype surfaces:

- Start;
- All Apps;
- App Context;
- Search;
- Pinned & Recent;
- Sable Start Settings;
- Live local data;
- Lock preview.

The promotion must obey these boundaries:

- All Apps/Search remain driven by the real R6 launcher-visible inventory;
- real Android-provided launcher icons replace generated/demo marks where available;
- pinned state is local and explicit;
- recent state initially means successful launches made through Sable Start, avoiding Usage Stats permission;
- Sable Start Settings owns only launcher-local behavior and must not clone Android Settings plumbing;
- Live data remains local and permission-gated, with no network permission added simply to populate the design;
- Lock remains a visual preview and does not replace Keyguard/SystemUI;
- Launcher3 Quickstep remains the recents/gesture provider.

R7 must not reintroduce hard-coded application lists, fake notification counts, fake Settings state, or fake search results into the production HOME path.

### 5.3 Required capability groups

#### Telephony

Prove:

- outbound voice call setup;
- inbound voice call reception;
- in-call audio in both directions;
- hangup/termination behavior;
- speaker/earpiece/basic audio routing sufficient for ordinary use;
- contacts-to-call flow;
- lock-screen/notification behavior as applicable;
- no Sable integration regression to emergency/platform telephony behavior.

Carrier- and IMS-specific features must be recorded separately rather than implied by a single successful call.

#### SMS and MMS

Prove:

- outbound SMS;
- inbound SMS;
- conversation persistence across application restart/reboot where the chosen application/platform normally provides it;
- outbound MMS with an attachment;
- inbound MMS with an attachment;
- cellular-data/carrier interaction required for MMS;
- notification and tap-through behavior.

Do not claim RCS support unless separately proven and intentionally in scope.

#### Wi-Fi

Prove:

- discovery of an access point;
- successful connection;
- DHCP/IP configuration;
- DNS and Internet reachability;
- reconnect after radio toggle or device sleep as appropriate;
- Settings control remains usable.

#### Cellular data

Prove:

- data registration;
- Internet reachability over cellular with Wi-Fi disabled;
- DNS;
- transition between Wi-Fi and cellular data;
- basic data-toggle behavior;
- no assumption that one carrier proves all carrier behavior.

#### Browser/Internet

Use a proven browser first. Prove:

- normal HTTPS navigation;
- DNS/network path;
- link opening from another application;
- downloads/open-in workflow where appropriate;
- no requirement that the browser be Sable-owned in R7.

#### Notifications

Prove notifications relevant to daily-driver use, especially calls/messages and at least one ordinary application. Validate posting, visible presentation, tap-through, and dismissal at a practical level.

#### Camera/photos/files

R7 requires usable baseline workflows, not a custom Sable replacement:

- capture a photo;
- view a captured photo;
- select/open media from another application where applicable;
- browse/open a normal file;
- confirm storage/media permission behavior is coherent.

#### Clock/alarm

A working alarm/clock implementation is required for daily-driver readiness. It may initially be inherited. Sable-owned Clock work is a later decision unless a blocking defect requires earlier replacement.

#### Calculator

A working calculator is part of the user baseline. R7 may temporarily use an inherited calculator if present. The first intended Sable-owned utility application is delivered in R9.

### 5.4 R7 default-app selection gate

Before declaring R7 complete, record for each baseline application:

- package/component selected;
- source/provenance;
- license and redistribution suitability;
- whether it requires privileged/system permissions;
- dependencies on Google/Graphene/AOSP-specific services;
- update/maintenance ownership;
- whether it is temporary, preferred, or scheduled for Sable replacement;
- runtime evidence status.

Do not make a product-wide statement such as "use AOSP apps" or "use Graphene apps" without this component-level review.

### 5.5 R7 non-goals

R7 is not blocked on:

- Sable-owned Dialer;
- Sable-owned Messaging;
- Sable-owned Camera;
- Sable-owned Browser;
- full R8 theme customization;
- production Keyguard/lock-screen replacement;
- global Usage Stats-based recents;
- perfect visual consistency in inherited applications.

A working, secure, maintainable phone and a real, privacy-preserving Sable Start take priority.

## 6. R8 — Sable design system and customization foundation

### 6.1 Goal

Establish the shared visual/product contract before multiple Sable-owned applications independently invent styling and customization behavior.

Detailed requirements live in `platform_sable/docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md`.

### 6.2 Required first-stage theme capabilities

At minimum establish shared tokens/contracts for:

- semantic colors;
- typography roles;
- spacing scale;
- shape/corner scale;
- icon treatment guidance;
- elevation/surface semantics where used;
- motion/duration guidance;
- light and dark color schemes;
- system-following mode;
- accent selection abstraction;
- accessibility/contrast expectations.

### 6.3 Initial customization scope

The first user-facing customization scope should be small:

- Follow system / Light / Dark;
- accent choice;
- persistence of the chosen Sable setting;
- shared consumption by Sable Start and later Sable applications.

Later candidates include launcher density, tile sizing, icon treatment, Start layout, background style, greeting options, clock format, animation levels, and accessibility sizing. These are not automatically part of R8 merely because the architecture supports them.

### 6.4 Architectural rule

Theme/customization state must be product-owned and typed. Avoid scattering literal colors/dimensions and independent preference keys across applications. The point of R8 is to prevent Sable Start, Calculator, Notes, Clock, and later apps from drifting into separate design systems.

## 7. R9 — First native Sable utilities

### 7.1 Goal

Prove that the Sable application model can produce small, high-quality, low-privilege utilities that share the Sable design system and validation approach.

### 7.2 First application: Sable Calculator

Sable Calculator is the intended first native Sable utility because it is:

- self-contained;
- low privilege;
- easy to validate deterministically;
- broadly useful;
- a good template for build integration, theming, accessibility, packaging, testing, and release provenance.

Initial Calculator requirements should be written before source implementation. At minimum the requirements should specify:

- supported arithmetic operations;
- numeric/error behavior;
- input model;
- history policy, if any;
- accessibility semantics;
- orientation/window behavior;
- theme integration;
- no network permission;
- no unnecessary sensitive permissions;
- deterministic unit/host tests for expression behavior;
- runtime smoke/interaction evidence.

Do not expand the first calculator into a scientific/programmer/conversion suite unless the requirements are intentionally broadened first.

### 7.3 Next utility candidates

After Calculator is validated, candidates include:

- Notes;
- selected Clock UI/functionality;
- Sable Files presentation/semantic layer;
- other simple offline utilities.

Each is a separate decision. Candidate status is not authorization to implement it.

## 8. R10+ — Deliberate inherited-application replacement

R10+ is a policy horizon, not a commitment to replace everything.

Before replacing an inherited application, document:

1. the concrete user/security/privacy/maintenance benefit;
2. required platform privileges and security surface;
3. compatibility burden;
4. data migration/interoperability requirements;
5. upstream update/security ownership;
6. accessibility and internationalization requirements;
7. test matrix;
8. fallback/rollback plan;
9. licensing/trademark implications;
10. why the replacement belongs in SableOS rather than remaining a normal third-party choice.

Phone, Messaging/MMS, Browser, and Camera are high-complexity replacements and should not be undertaken merely for branding consistency.

## 9. Milestone dependency/order

The intended order is:

```text
R5  migrated-source build/reconstruction closure
 |
 v
R6  real Sable Start launcher + local-time greeting
 |
 v
R7  Sable Start production surfaces + daily-driver phone validation + explicit default-app decisions
 |
 v
R8  shared Sable design/theme/customization foundation
 |
 v
R9  first native Sable utilities, beginning with Calculator
 |
 v
R10+ deliberate replacement/expansion based on documented value
```

Work may be researched in parallel, but closure claims must respect dependencies. For example, R8 design documentation may be drafted while R7 testing proceeds, but an unproven R7 daily-driver baseline must not be hidden by theme work.

## 10. Repository ownership for this plan

| Repository | Milestone responsibility |
| --- | --- |
| `.github` | organization-wide product direction, roadmap, contribution/process expectations |
| `packages_apps_SableStart` | R6 launcher inventory/search/greeting behavior and R7 production launcher surfaces/tests |
| `platform_sable` | release model, common semantic contracts, R8 theme/customization contracts, cross-app product APIs |
| `device_sable_panther` | R7 Panther runtime qualification and device-specific evidence |
| `vendor_sable` | product composition/default package inclusion/overlays/common product configuration; no app source copies |
| `platform_manifest` | exact multi-repository composition and milestone/release revision pinning |
| `build` | build/reconstruction/evidence gates and host tooling |

Future Sable application repositories should own their application-specific requirements while referencing this document for product direction and `platform_sable` for common contracts.

## 11. Requirements precedence and change control

When building a milestone, use this precedence:

1. explicit current user/product decision recorded in the repository;
2. milestone-specific normative requirements in the owning repository;
3. this organization-wide plan;
4. architecture/portability/release policies;
5. implementation convenience.

Implementation convenience never overrides a recorded requirement.

If two documents conflict, stop and resolve the documentation conflict before implementing the disputed behavior.

A requirement change should identify:

- old requirement;
- new requirement;
- reason;
- affected repositories/components;
- compatibility/migration impact;
- evidence that will close the revised requirement.

Do not silently reinterpret a requirement after a build/test failure merely to make the gate pass.

## 12. Evidence and closure policy

Every milestone needs explicit evidence proportional to its claim. Compilation alone proves only compilation.

Typical closure layers are:

```text
source identity
  -> build identity
  -> package/artifact identity
  -> install/integration identity
  -> runtime behavior
  -> user-visible behavior
  -> reconstruction from revision-pinned composition
```

Not every small feature needs every layer independently, but a product-level milestone must not claim more than its evidence establishes.

Exact hashes, commit/tree identities, manifest revisions, device/build identity, authorization boundaries, and test conditions should be recorded where they materially affect reproducibility.

## 13. Current fixtures and assumptions

The primary current reference target is Pixel 7 (`panther`) against the validated GrapheneOS 2026081300 / Android 17 substrate work.

Maps and Weather applications that were added during location/live-surface work should remain installed through the initial R6 inventory validation unless there is a separate reason to remove them. They provide useful non-core applications for completeness, search, icon, launch, and package-removal testing. Their presence does not make either application part of the SableOS required default-app set.

## 14. Definition of success for the current development train

The current train has achieved its practical objective when a fresh, revision-pinned SableOS composition can be built and installed on the qualified Panther target and a user can reliably:

- unlock and use Sable Start;
- find and launch installed applications;
- place and receive calls;
- send and receive SMS/MMS;
- connect by Wi-Fi and cellular data;
- browse the Internet;
- receive actionable notifications;
- take/view a photo and work with normal files;
- use clock/alarm and calculator functionality;
- use a coherent Sable light/dark/accent foundation for Sable-owned UI;
- reproduce the build and validation claims from recorded source identities and evidence.

That baseline matters more than prematurely replacing every inherited Android application.
