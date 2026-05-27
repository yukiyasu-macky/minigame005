# Screen Flow

This document preserves the current planning baseline for あわねこ湯屋 screen hierarchy and transitions. It is lightweight by design and should evolve during implementation.

## Status

Confirmed:

- `HomeScreen` is the main hub.
- `HomeScreen -> PuzzleScreen` is the core play entry.
- `PuzzleScreen -> ResultScreen` always happens after an exploration/puzzle run.
- `ResultScreen -> RevealScreen` happens only when the result includes a rescued cat.
- Non-cat results return value through materials, furniture resources, recipes, letters, photos, event items, collection progress, or Home growth.
- All screens must follow the ad-safe layout constraints in `docs/ui_safe_area_spec.md`.

Tentative:

- Exact routing implementation.
- Exact modal stack behavior.
- Exact screen names in code.
- Final placement of event, shop, mission, mail, and notice entry points.

## Global Layout Assumption

Every screen is planned around:

- persistent top header area
- persistent bottom banner ad area
- mobile safe areas and notch devices
- LIFF/browser chrome
- intermittent popup/interstitial ad interruption

Important controls and readable content must stay inside the safe gameplay/content area, never behind the top header, bottom banner ad, or device safe-area overlays.

## Top-Level Flow

```text
TitleScene
  -> HomeScreen
      -> PuzzleScreen
          -> ResultScreen
              -> RevealScreen -> CatDetailScreen -> HomeScreen
              -> HomeScreen
      -> OnsenEditScreen
      -> AlbumScreen
      -> EventScreen
      -> MissionScreen
      -> ShopScreen
      -> MailScreen
      -> NoticeScreen
      -> SettingsScreen
```

## TitleScene

Purpose:

- booting
- loading
- Tap to Start
- event announcement
- notice entry

Expected to evolve:

- loading state design
- whether event/notice entries are visible before login or after boot completion

## HomeScreen

Purpose:

- central game hub
- onsen display
- cat observation
- idle reward collection
- exploration entry
- onsen growth preview
- new furniture, cat, and event notifications

Primary transitions:

- `HomeScreen -> PuzzleScreen` through exploration start
- `HomeScreen -> OnsenEditScreen`
- `HomeScreen -> AlbumScreen`
- `HomeScreen -> EventScreen`
- `HomeScreen -> MissionScreen`
- `HomeScreen -> ShopScreen`
- `HomeScreen -> MailScreen`
- `HomeScreen -> NoticeScreen`
- `HomeScreen -> SettingsScreen`

Planning note:

Home must make rewards and discoveries feel connected to the onsen, cats, furniture, events, and collections without adding pressure or battle-like goals.

## OnsenEditScreen

Purpose:

- onsen layout editing
- theme changes
- seasonal decoration
- onsen expansion
- cat appearance modifier confirmation

Child flow:

- `OnsenEditScreen -> InventoryScreen`
- `InventoryScreen` includes furniture category, furniture detail, furniture placement, and furniture upgrade.

Expected to evolve:

- whether `InventoryScreen` is a full route or modal.
- how furniture placement preview handles the bottom banner ad.

## AlbumScreen

Purpose:

- collection progress
- cat records
- adopted-cat memories
- rewards and achievements

Child flow:

- `AlbumScreen -> CatDexScreen`
- adopted-cat memory records
- collection rewards
- album progress
- achievement display

`CatDexScreen` includes:

- discovered cats
- undiscovered cats
- rare cat records
- personality list
- habit list
- appearance condition hints

Adopted-cat memory records include:

- memory photos
- letters
- revisit records
- growth records

Confirmed:

Album and CatDex are related but not identical. CatDex is the cat discovery index; Album also includes memories, adoption records, progress, and rewards.

## EventScreen

Purpose:

- seasonal events
- limited cats
- event furniture
- event materials
- event missions
- event exchange

Expected to evolve:

- event screen depth and routing should stay simple for MVP.

## MissionScreen

Purpose:

- daily missions
- weekly missions
- achievements
- beginner missions

Planning note:

Missions should support gentle progression and should not pressure the player during puzzle/ad interruptions.

## ShopScreen

Purpose:

- bubble power recovery
- limited furniture
- seasonal themes
- skins
- value packs

Planning note:

Shop UI must not push glossy monetization styling into the visual direction. It still follows the soft watercolor/wood/paper UI language.

## MailScreen

Purpose:

- operation gifts
- adopted-cat letters
- event rewards
- system notices

## NoticeScreen

Purpose:

- announcements
- event notices
- maintenance notices

## SettingsScreen

Purpose:

- sound settings
- notification settings
- accessibility
- data transfer
- terms

## PuzzleScreen

Purpose:

- washing play
- bubble use
- dirt removal
- combo
- pause
- ad pause
- retire confirmation

Confirmed:

- Popup/interstitial ads pause puzzle gameplay.
- Timers, combo logic, stamina, rewards, reveal animation, progression, and input must not continue behind ads.

## ResultScreen

Purpose:

- score display
- wash rate
- koban gained
- bubble material gained
- onsen material gained
- furniture material gained
- photo discovery
- letter discovery
- recipe discovery
- event material gained
- rescued-cat discovery check
- rare result presentation
- Home growth preview

Confirmed:

- `ResultScreen` does not always lead to cat reveal.
- If a cat is found, transition to `RevealScreen`.
- If no cat is found, return to `HomeScreen` after rewards are accepted.
- Non-cat rewards must connect back to Home growth, furniture, onsen upgrades, collection progress, or event progress.

## RevealScreen

Purpose:

- bubble effect
- silhouette
- cat reveal
- name display
- personality display
- rare effect
- rescue presentation

Confirmed:

- Reveal happens only when exploration result includes a cat.
- Effects should remain gentle and should not become battle-like.
- Ad interruption must pause reveal animation and resume safely.

Primary transition:

- `RevealScreen -> CatDetailScreen`

## CatDetailScreen

Purpose:

- cat status
- personality
- habits
- stay days
- affection
- cleanliness
- photo capture
- care
- favorite onsen check
- furniture compatibility check
- adoption condition check
- adoption send-off

Primary transition:

- `CatDetailScreen -> HomeScreen`

Expected to evolve:

- exact adoption requirements
- how photo capture is implemented
- whether care actions are immediate or modal-based

## Common Overlay / Modal

Shared overlays:

- `PauseOverlay`
- `AdOverlay`
- `RewardPopup`
- `RareCatRevealModal`
- `AdoptionResultModal`
- `FurnitureUnlockModal`
- `EventNoticeModal`
- `ConfirmationModal`
- `OfflineNoticeModal`
- `DailyBonusModal`
- `MaintenanceModal`

Architecture note:

Overlays should be separated from screen routes. `AdOverlay` must be able to disable gameplay input and pause runtime systems without losing the underlying route state.

