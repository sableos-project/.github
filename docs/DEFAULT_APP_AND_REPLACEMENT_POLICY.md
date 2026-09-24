# Default application and replacement policy

> **Current execution overlay — 2026-09-24:** Panther R9 has completed product/runtime acceptance. The active first-party set includes standalone SableLauncher plus Calculator, Games, Media, Reader, Text Reader, Hub/Messages, Mail, Weather and Calendar. Reader/Text Reader are separate products. K1/K2 is merged; new default/replacement work must remain common-product-first and device-adapter-bounded.


Status: **normative product/application selection framework.**

SableOS does not require every user-facing application to be Sable-owned. Application selection is capability-by-capability and must distinguish standalone qualification from product adoption/default-role replacement.

## 1. Core rule

```text
qualified application
    != shipping product application
    != selected default/role holder
    != proven runtime replacement
```

Each transition requires its own evidence.

## 2. Selection classes

| Class | Initial direction | Examples |
| --- | --- | --- |
| platform/security critical | retain validated Android/substrate mechanism | permissions, networking, telephony framework, Settings plumbing |
| complex interoperability app | retain a proven implementation until replacement is justified | Phone, Messaging, Browser, Camera |
| bounded/self-contained utility | strong Sable-owned/reuse candidate | Calculator, Convert, Games, Reader capability, selected Media |
| content/provider app | explicit provider/data policy required | Weather, Maps, cloud/content services |

Do not select an implementation solely because it exists in the current reference workspace. Do not reject a working inherited implementation solely because it is not Sable-branded.

## 3. R7 baseline versus R8 application train

R7 daily-driver qualification may use inherited implementations for baseline capabilities, including Calculator.

R8 now qualifies a coherent set of Sable-owned/reused applications:

- Calculator + Convert;
- Games;
- Reader publication capability from Vaachak Mobile;
- Reader TXT/share/TTS/OCR capability from Vaachak Text Reader;
- Media.

Their inclusion in R8 source qualification does **not** automatically replace the inherited product application. Replacement/product adoption happens only after standalone gates, exact artifact freeze, product wiring and runtime validation.

The old policy sequence "Calculator becomes Sable-owned in R9" is superseded. Calculator belongs to R8-B, while the replacement threshold remains unchanged.

## 4. Required record before shipping a Sable application

Record:

```text
capability
package/application ID
source repository + exact commit
upstream/reuse source + exact commit when applicable
license/redistribution status
qualification workflow/run
APK SHA-256
permissions/AppOps
exported activities/services/providers/receivers
native ABI/library inventory
third-party dependency/provenance inventory
product partition/install path
signing/update model
maintenance/security owner
runtime evidence
rollback/fallback
status: candidate / qualified / product-integrated / default / replacement
```

`TBD` is preferable to an undocumented assumption.

## 5. Product integration gate

A qualified APK becomes a product integration candidate only after an exact artifact freeze.

The integration layer must prove:

```text
sealed APK
 -> declared product import/module
 -> selected package
 -> PRODUCT_OUT install identity
 -> installed-files / target-files identity
 -> image membership
 -> runtime package/component identity
```

The accepted Panther R9 integration proved the product import/composition path used by that release. Future substrates or artifact kinds must still prove their own exact import/signing/partition semantics rather than assuming Panther behavior.

Do not drop manually built APKs into `vendor_sable` without reproducible source/workflow/hash provenance.

## 6. Default-role replacement gate

For HOME, Dialer, SMS, Browser or another Android role/default handler, product inclusion and default selection are separate decisions.

Before replacing a working default, prove as applicable:

- role/default-handler transition;
- privileged permissions/allowlists;
- exported-component expectations;
- data migration/interoperability;
- process/reboot persistence;
- rollback to the previous implementation;
- no stale product/overlay/permission references.

During development it is acceptable to install/qualify a package without making it the default.

## 7. Permission/privilege rule

Do not grant a permission, role, privileged status or allowlist entry merely to simplify implementation.

Every nontrivial authority must map to an accepted product requirement. Remove obsolete grants when the owning package is replaced/removed through a separately validated cleanup change.

## 8. Reader policy

Sable Reader is one product identity composed from separately qualified capability sources where useful.

- Vaachak Mobile / Readium supplies the publication/EPUB path.
- Vaachak Text Reader supplies TXT/share/process-text/TTS/OCR capability.

Do not ship two competing Sable Reader launcher entries simply because upstream qualification happens against two repositories/APKs.

Network-backed upstream behavior is not automatically accepted. Text Reader translation/model acquisition and Vaachak Mobile network surfaces require an explicit privacy/product policy before inclusion.

## 9. Media policy

Sable Media may combine local Music and Internet Radio because they share playback/session ownership.

The Internet permission is explicit and justified only for radio/network features. Local music should use Android user-granted media/document APIs rather than broad storage authority.

## 10. Complex inherited applications

Phone, Messaging, Browser and Camera remain proven-inherited-first categories. A future Sable replacement must show concrete privacy/security/UX/maintenance value and must qualify the additional privilege/interoperability burden.

A new implementation compiling successfully is not a replacement argument.

## 11. Test fixtures and optional applications

Development fixtures such as Maps/Weather or one-off testing APKs are not thereby shipping defaults.

Documentation should distinguish:

```text
required product app
qualified candidate
optional/recommended app
development/test fixture
user-installed app
```

## 12. Replacement cleanup checklist

When replacing a product application audit:

- product package lists/imports;
- source/artifact provenance;
- roles/default handlers;
- overlays;
- privileged permission allowlists;
- intent associations;
- SELinux rules where applicable;
- signing/update expectations;
- package data/migration;
- launcher/search visibility;
- stale files/references;
- rollback path.

Historical evidence remains preserved even after replacement.

## 13. Decision threshold

Adopt/replace only when the result is supportable across:

```text
security/privacy
least privilege
functional completeness
accessibility
update/maintenance ownership
source/dependency provenance
product integration complexity
data migration/interoperability
rollback
runtime evidence
```

Brand consistency alone is not sufficient.