# 🐉 Dungeon Mayhem API

> An unofficial public API for the **Dungeon Mayhem** card game by Wizards of the Coast.  
> Static JSON data hosted via GitHub Pages — free, open, and community-driven.

---

## ⚠️ Disclaimer

This project is **unofficial and not affiliated with Wizards of the Coast**. Dungeon Mayhem is a trademark of Wizards of the Coast LLC. This API is a fan-made project for educational and non-commercial purposes only.

---

## 📖 Overview

This repository exposes all Dungeon Mayhem card and character data as a structured JSON API, designed to power unofficial web implementations of the game.

It is used as the data layer for [dungeon-mayhem-online](#) *(link to your game repo)*.

---

## 🚀 Base URL

```
https://<your-username>.github.io/dungeon-mayhem-api/
```

---

## 📦 Endpoints

| Endpoint | Description |
|---|---|
| `/characters.json` | All playable characters |
| `/characters/{id}.json` | Single character with deck & powers |
| `/cards.json` | All cards across all characters |
| `/cards/{id}.json` | Single card data |

---

## 📐 Data Structure

### Character

```json
{
  "id": "lia",
  "name": "Lia",
  "hp": 10,
  "image": "https://...",
  "powers": [
    {
      "id": "lia-power-1",
      "name": "Recovery",
      "description": "Choose a card from your discard pile and put it back in your hand.",
      "effects": [
        {
          "type": "retrieve_from_discard",
          "target": "self",
          "select": "choose",
          "value": 1
        }
      ]
    },
    {
      "id": "lia-power-2",
      "name": "Armor Destruction",
      "description": "Destroy all Armor cards in play (including yours).",
      "effects": [
        {
          "type": "destroy_by_tag",
          "target": "all_players",
          "tag": "armor"
        }
      ]
    }
  ],
  "deck": []
}
```

### Card

```json
{
  "id": "azzan-berserker-rage",
  "character": "azzan",
  "name": "Berserker Rage",
  "image": "https://...",
  "quantity": 2,
  "effects": [
    {
      "type": "damage",
      "target": "choose_opponent",
      "value": 3
    },
    {
      "type": "draw",
      "target": "self",
      "value": 1
    }
  ]
}
```

> `quantity` defines how many copies of this card exist in the character's deck.

---

## 🎯 Effect Types

### Player Effects

| type | description |
|---|---|
| `damage` | Deal damage to target(s) |
| `shield` | Add shields to target(s) |
| `heal` | Restore HP to target(s) |
| `draw` | Draw cards |
| `discard` | Discard a chosen number of cards |
| `discard_hand` | Discard entire hand |
| `steal_shield` | Steal shields from an opponent |
| `destroy_by_tag` | Destroy all active effects matching a tag |
| `retrieve_from_discard` | Pick a card from the discard pile |
| `armor` | Apply an armor effect to target(s) |

### Board Effects

| type | description |
|---|---|
| `extra_play` | Play an additional card this turn |
| `extra_turn` | Take another full turn |
| `skip_turn` | Skip a target's next turn |
| `foreach_opponent` | Apply sub-effects once per opponent |
| `trigger_power` | Trigger a character's special power |

---

## 🎯 Target Types

| target | description |
|---|---|
| `self` | The current player |
| `choose_opponent` | Player chooses one opponent |
| `choose_any_player` | Player chooses any player (including self) |
| `all_opponents` | All opponents |
| `all_players` | Every player including self |
| `current_opponent` | Used inside `foreach_opponent` loops |
| `lowest_hp` | The player with the lowest HP |

---

## ⏱️ Duration (for persistent effects like Armor)

| value | description |
|---|---|
| `permanent` | Until end of game |
| `turn` | Until end of current turn |
| `round` | Until this player's next turn |
| `3` *(number)* | For X turns |

---

## 🔀 Complex Effects

### `foreach_opponent`
Applies sub-effects once per opponent, in turn order.

```json
{
  "type": "foreach_opponent",
  "actions": [
    { "type": "heal", "target": "self", "value": 1 },
    { "type": "damage", "target": "current_opponent", "value": 1 }
  ]
}
```

### `trigger_power`
Triggers a character's special power from within a card effect.

```json
{
  "type": "trigger_power",
  "power_id": "lia-power-1",
  "requires_input": true
}
```

> `requires_input: true` signals that the game engine must pause and wait for player interaction before continuing.

---

## 🃏 Characters

| ID | Name | HP |
|---|---|---|
| `azzan` | Azzan the Undying | 10 |
| `lia` | Lia the Radiant | 10 |
| `sutha` | Sutha the Skullcrusher | 10 |
| `calliope` | Calliope the Melodious | 10 |

> More characters may be added as extensions are supported.

---

## 🛠️ Used By

- [dungeon-mayhem-online](#) — Unofficial web multiplayer implementation  
  *(Vue 3 + Pinia · Fastify + Socket.io · Firebase)*

---

## 🤝 Contributing

Spotted a wrong card value or missing effect? PRs are welcome!

1. Fork the repo
2. Edit the relevant `.json` file
3. Open a Pull Request with a description of the change

Please keep the data structure consistent with the existing format.

---

## 📄 License

Data structure and code: **MIT License**  
All Dungeon Mayhem card names, characters, and game concepts are property of **Wizards of the Coast LLC**.