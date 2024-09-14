# Equipment Data Structure Doc

The structure is so dumb, i lost a braincell everytime i tried to read it!

## Contents Table

---

- [Equipment Data Structure Doc](#equipment-data-structure-doc)
  - [Contents Table](#contents-table)
  - [Json Structure and Type](#json-structure-and-type)
  - [Json Data Docs and Definition](#json-data-docs-and-definition)
  - [How To Use](#how-to-use)

---

## Json Structure and Type

All Equipment including armor, additional gear and ring has the same data structures and typing, for examples down below:

```json
[
  {
    "name": "string", // STRING
    "origin": 1, // INTEGER
    "base": 1.0, // FLOAT
    "stab": 1, // INTEGER
    "pot": 1, // INTEGER
    "amount": [0, 0, "string"], //ARRAY< INTEGER, INTEGER, STRING >
    "stats": {
      "general": ["string", 1.0] // ARRAY< STRING, FLOAT >
    }, // OBJECT< Key: Value >
    "source": ["string", 1, "string", []], //ARRAY<STRING,INTEGER,STRING,ARRAY<NULL || STRING>>
    "recipe": {
      "lvl": 1, // INTEGER
      "diff": 1, // INTEGER
      "mats": ["string", 1] // ARRAY<STRING, INT>
    } // OBJECT<key: value>
  }
]
```

To understand key, label, object and array/list definition, check out [Json Data Docs and Definition](json-data-docs-and-definition) Section.

---

## Json Data Docs and Definition

- Name => (Items name or label)
- Origin => (Items origin either NPC, Player, or Drop {0: Player, 1: NPC, 2: Drop})
- base => (Base WATK or DEF of items)
- stab => (Base Stablility of items)
- pot => (Potential value, only available for origin: 0)
- amount => (sell and process value of items value: [spina_value, mats_value, mats_label])
- stats => status of items, "general" is the main status of items [status_label, status_value]
- source => more details of weapon origin, [mob_name, mob_level, location]
- recipe => only available for NPC and Player
  - lvl: items level
  - diff: items difficulty
  - math: list of mats required to create items (including spina) [mats_label, mats_value]

---

## How To Use

fok it.
