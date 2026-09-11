# Default application and replacement policy

Status: **normative decision framework; exact R7 component selections remain to be recorded after inventory/validation.**

This document answers a recurring product question: when should SableOS use an AOSP application, a GrapheneOS-derived/substrate application, another proven implementation, or a new Sable-owned application?

The answer is intentionally not "one source for everything." Different application classes have different interoperability, privilege, maintenance, and security costs.

## 1. Governing principle

Choose the lowest-risk implementation that satisfies the product requirement while keeping future replacement possible.

SableOS differentiation should come from product behavior, privacy/security choices, stable Sable semantics, coherent UX, and evidence—not from replacing mature applications solely to change branding.

## 2. Four implementation classes

### Class A — platform/security critical

Examples:

- telephony framework and radio integration;
- IMS/carrier integration;
- Wi-Fi and cellular networking;
- Android permission/AppOps infrastructure;
- package/user/profile management;
- notification infrastructure;
- system Settings plumbing;
- SELinux/Binder/HAL/device integration.

Direction: **inherit the validated substrate/platform implementation unless an explicitly reviewed platform architecture change is required.**

These are not first-wave Sable application replacement targets.

### Class B — complex interoperability applications

Examples:

- Dialer/Phone;
- SMS/MMS messaging;
- browser;
- camera;
- contacts when tightly coupled to call/message flows;
- applications with substantial provider/carrier/media/browser-engine integration.

Direction: **start with a proven implementation and validate it as part of the daily-driver baseline.** Replacement requires a separate product/security decision.

A Sable-branded rewrite is not assumed.

### Class C — simple or bounded utilities

Examples:

- Calculator;
- Notes;
- selected Clock UI/functionality;
- simple offline tools.

Direction: **preferred early Sable-owned application candidates** when requirements are written and the implementation can remain low privilege.

Sable Calculator is the first planned example.

### Class D — provider/content applications

Examples:

- Weather;
- Maps;
- remote content/search services.

Direction: **treat provider choice as replaceable and explicit.** Avoid making a third-party provider application or API an implicit product dependency unless the product requirements intentionally choose it.

## 3. Component-level decision record

Before a baseline application becomes a SableOS default, record:

| Field | Required information |
| --- | --- |
| Capability | Phone, Messaging, Browser, Camera, etc. |
| Package/component | exact package and relevant activity/service identities |
| Source/provenance | AOSP, GrapheneOS-derived/substrate, Sable-owned, other upstream |
| Upstream revision | exact revision/tag when source-built |
| License | redistribution/modification implications |
| Branding/trademark | whether upstream names/assets may be redistributed |
| Privilege | system/privileged permissions, roles, providers, shared UID if any |
| Dependencies | Android services, Google services, Graphene-specific services, native libraries, provider contracts |
| Data ownership | databases/providers/files and migration concerns |
| Update owner | who monitors vulnerabilities and upstream updates |
| Current status | temporary / preferred / Sable replacement planned / undecided |
| Validation evidence | tests that establish daily-driver suitability |

No broad policy such as "all AOSP apps" or "all Graphene apps" substitutes for this table.

## 4. Initial capability direction

These are **directions**, not yet exact package selections.

### Phone / Dialer

Initial direction: proven inherited implementation.

Reasons:

- telecom framework/role integration;
- emergency call behavior;
- contact/call-log interaction;
- in-call UI/audio routing;
- carrier and device edge cases;
- notification and lock-screen behavior.

A custom Sable Phone application should not be an early daily-driver dependency.

### Messaging / SMS / MMS

Initial direction: proven inherited implementation.

Reasons:

- default SMS role behavior;
- telephony provider interaction;
- carrier MMS configuration and transport;
- attachment handling;
- notification/reply flows;
- database migration and reliability.

RCS is not implicitly required by the SMS/MMS baseline and must not be claimed without separate requirements/evidence.

### Contacts

Initial direction: use a proven contacts implementation/provider compatible with the chosen Phone and Messaging stack.

A future Sable contacts experience is possible, but contact-provider compatibility and migration must be designed first.

### Browser

Initial direction: proven security-maintained browser.

The exact browser must be selected during R7 after reviewing provenance, engine/security-update model, integration, and redistribution. A custom browser is not an early SableOS objective.

### Camera

Initial direction: proven implementation compatible with the Panther camera stack and ordinary capture/view/share workflows.

A custom camera has significant hardware and image-processing complexity and is not required for initial daily-driver readiness.

### Files

Initial direction: retain a proven baseline file/document UI for R7. A Sable Files experience is a later candidate if it can sit on supported Android document/storage APIs rather than reinventing storage authority.

### Clock / Alarm

Initial direction: working inherited implementation satisfies R7. A Sable Clock may be considered after Calculator and theme contracts, especially if it can use supported alarm APIs without privileged shortcuts.

### Calculator

Initial direction: **first Sable-owned utility application**.

Reasons:

- no carrier/device/provider coupling;
- no need for network access;
- minimal privilege;
- deterministic functional tests;
- useful proving ground for Sable theme, accessibility, packaging, app architecture, and release gates.

### Weather / Maps

Initial direction: external/provider-driven capability; no mandatory SableOS dependency yet.

Existing installed Maps/Weather apps should remain useful test fixtures during R6 launcher inventory validation. Their eventual default status is separate from their use as test data.

## 5. Replacement threshold

Replacing a proven inherited application requires a written case covering all of the following:

1. **Benefit** — what measurable privacy, security, usability, portability, or maintenance improvement will users receive?
2. **Authority** — what permissions/roles/providers/system privileges does the replacement require?
3. **Compatibility** — what Android contracts, file/data formats, carrier/provider behaviors, and external intents must remain compatible?
4. **Security ownership** — who will monitor vulnerabilities and maintain the application over time?
5. **Data migration** — what happens to existing data when moving to or away from the Sable application?
6. **Accessibility/i18n** — what minimum accessibility and localization behavior is required?
7. **Testing** — what host/unit/instrumentation/runtime tests close the feature?
8. **Rollback** — how can the product return to the prior proven implementation?
9. **Licensing** — what upstream code/assets may legally be used and under what obligations?
10. **Product fit** — why should this be part of the OS rather than a user-selected third-party application?

If the case is weak, keep the proven implementation.

## 6. Sable-owned application rules

New Sable applications should default to:

- no network permission unless the application's core requirement needs it;
- no sensitive permission unless directly justified by a user feature;
- supported public/stable Android APIs where practical;
- clear data ownership and export/delete semantics;
- shared Sable design tokens rather than copied visual constants;
- deterministic business-logic tests where applicable;
- accessibility semantics from the first functional release;
- exact build/source provenance;
- explicit runtime acceptance criteria.

Do not grant privileged/system permissions merely to simplify implementation.

## 7. Product composition boundary

`vendor_sable` may choose which validated packages are included/defaulted by a Sable product configuration, but it must not become a storage location for copied application source trees.

Application source belongs in its owning repository. Device repositories must not fork common app source merely to support one device.

`platform_manifest` records exact source composition; `vendor_sable` records common product integration; `device_sable_*` records bounded device-specific integration.

## 8. Decision timing

R6 does not need to settle the complete default-app set. It needs a correct launcher inventory and launch/search behavior.

R7 must settle enough of the default-app set to prove a reliable daily-driver baseline.

R8 establishes the Sable design/customization foundation.

R9 starts native Sable utility delivery with Calculator.

R10+ revisits high-complexity inherited applications only with explicit justification.

## 9. Anti-drift rule

When implementing a feature, do not substitute a different default application, add a new privileged dependency, or begin a Sable rewrite because it appears convenient. Update the relevant decision record first. Future work should be able to determine *why* a component exists by reading GitHub without reconstructing intent from source code or chat history.