# Data Schema Planning

This document defines the minimum save/state planning shape for あわねこ湯屋. It is schema planning only and does not implement runtime persistence.

Future implementation should begin with localStorage-compatible data and preserve a clean migration path to Firebase. Store stable ids, seeds, and state patches where possible.

## Status

Confirmed:

- Save data should not store raw image data.
- Save data should not store particles, render cache, animation frame state, or every-frame coordinates.
- Generated cats should be saved by stable ids and seeds.
- Result rewards should be saved through small operation-confirmed patches.
- Result reward mapping should use `runId` idempotency. See `docs/result_reward_mapping.md`.
- UI/ad state should support pause and resume around popup/interstitial ads.

Tentative:

- exact field names in runtime code
- exact migration strategy
- exact timestamp format
- exact cat generator seed fields
- exact inventory quantity types

## SaveData Root

Minimum root shape:

```json
{
  "version": 1,
  "player": {},
  "cats": {},
  "home": {},
  "inventory": {},
  "album": {},
  "missions": {},
  "stamina": {},
  "settings": {},
  "adState": {},
  "timestamps": {}
}
```

Required sections:

- `version`
- `player`
- `cats`
- `home`
- `inventory`
- `album`
- `missions`
- `stamina`
- `settings`
- `adState`
- `timestamps`

## CatData

Minimum cat instance fields:

```json
{
  "instanceId": "cat_instance_id",
  "speciesId": "cat_species_id",
  "seed": "seed_value",
  "rarity": "normal",
  "personality": "gentle",
  "quirks": [],
  "favoriteTheme": "default_onsen",
  "discoveredAt": "timestamp",
  "adopted": false,
  "adoptedAt": null,
  "bondLevel": 0,
  "currentState": "staying"
}
```

Planning notes:

- `instanceId` identifies this individual cat.
- `speciesId` supports CatDex species/record tracking.
- `seed` supports seed-based individuality and visual/personality generation.
- `currentState` may include values such as `staying`, `adoption_requested`, `adopted`, or `auto_adopted`.
- Cats should basically not duplicate as ResultScreen rewards; prefer undiscovered species when possible.

## HomeState

Minimum Home state fields:

```json
{
  "activeTheme": "default_onsen",
  "unlockedThemes": [],
  "placedFurniture": [],
  "unlockedFurniture": [],
  "activeCats": [],
  "environmentEffects": [],
  "expansionLevel": 0,
  "unlockedAreas": []
}
```

Planning notes:

- `activeCats` should reference cat `instanceId` values.
- Home growth should be visible or stateful after Result rewards.
- Furniture and themes should be driven by stable ids.

## InventoryData

Minimum inventory fields:

```json
{
  "furnitureMaterials": {},
  "onsenMaterials": {},
  "bubbleRelatedItems": {},
  "unlockedRecipes": [],
  "duplicateRecipeConversions": [],
  "eventMaterials": {},
  "craftedFurniture": []
}
```

Planning notes:

- MVP reward table includes furniture materials, onsen materials, bubble-related support rewards, recipes, event materials, and cats.
- Generic koban/currency is intentionally excluded from the MVP reward table for now.
- Duplicate recipes should convert into materials for MVP.

## AlbumData

Minimum Album state fields:

```json
{
  "catDex": {
    "discoveredSpecies": [],
    "rareRecords": [],
    "personalityRecords": [],
    "quirkRecords": []
  },
  "adoptionRecords": [],
  "photos": [],
  "letters": [],
  "collectionRewardsClaimed": []
}
```

Planning notes:

- Photos and letters are memory rewards after adoption/send-off, not normal exploration loot.
- `CatDexScreen` belongs under Album but focuses on species and discovery records.

## StaminaData

Minimum stamina fields:

```json
{
  "current": 3,
  "max": 3,
  "lastRecoveredAt": null,
  "adRecoveryUsedToday": 0,
  "paidRecoveryCountToday": 0
}
```

Initial config values:

- `maxStamina`: 3
- `costPerPuzzle`: 1
- `adRecovery`: +1
- `adRecoveryLimitPerDay`: 1
- `paidFullRecoveryPrice`: 100 yen
- `paidFullRecoveryEffect`: full stamina recovery

Planning note:

Bubble is not stamina. Bubble-related items belong in inventory as puzzle boost/support materials.

## AdState

Minimum ad state fields:

```json
{
  "isAdShowing": false,
  "reservedAdHeight": 0,
  "safeAreaInsets": {
    "top": 0,
    "right": 0,
    "bottom": 0,
    "left": 0
  },
  "pausedRoute": null,
  "pausedSessionId": null
}
```

Planning notes:

- During popup/interstitial ads, gameplay, timers, progression, animations, reward flow, and input must pause.
- After ads close, resume from previous state without duplicate rewards or skipped Result/Reveal progression.

## Operation-Confirmed Patches

Future implementation should save small patches for meaningful operations:

- `inventoryUpdate`
- `catUpdate`
- `recipeUnlock`
- `homeGrowthProgress`
- `adoptionUpdate`
- `staminaUpdate`
- `adStateUpdate`

Result reward patches should be applied before or during Result presentation so LIFF reloads, app closes, or ad interruption do not lose rewards.

`SavePatch` should include `runId`, `rewardTier`, and `rewardType` when applying Puzzle results. It should not include Result text, animation data, render cache, or temporary UI state.

## Config-Driven Systems

Future implementation should use config files/tables instead of hardcoded magic numbers:

- `rewardConfig`
- `staminaConfig`
- `adoptionConfig`
- `resultPresentationConfig`
- `catGenerationConfig`
- `homeGrowthConfig`

Systems/game logic should consume config values. Rendering/components should display resolved state and should not own reward, stamina, adoption, or save rules.
