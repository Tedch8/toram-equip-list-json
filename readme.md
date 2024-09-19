# Equipment Data Doc

The structure is so dumb, i lost a braincell everytime i tried to read it! and i read it JASON, and there's nothing you can do about it!

## Contents Table

---

- [Equipment Data Doc](#equipment-data-doc)
  - [Contents Table](#contents-table)
  - [JSON Version](#json-version)
  - [JSON Structure and Type](#json-structure-and-type)
  - [JSON Data Docs and Definition](#json-data-docs-and-definition)
  - [Closing](#closing)

---

## JSON Version

The data includes a version, formatted as a integer total days of current date compared to offset date (current_date - offset_date), to track the last update. This version is used for caching purposes to determine whether new data needs to be fetched from client local storage.

## JSON Structure and Type

All Equipment including armor, additional gear and special gear has the same data structures and typing, for examples down below:
  
&nbsp;

```javascript
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
  
&nbsp;

As for Crysta or Xtal, it almost has the same structure but with some modification:
  
&nbsp;

```javascript
[
  {
    name: "string", // STRING
    code: -1, // INTEGER
    amount: [0, 0, "string"], //ARRAY< INTEGER, INTEGER, STRING >
    stats: {
      general: ["string", 1.0], // ARRAY< STRING, FLOAT >
    }, // OBJECT< Key: Value >
    source: ["string", 1, "string"], //ARRAY<STRING,INTEGER,STRING,ARRAY<NULL || STRING>>
    recipe: {
      lvl: 1, // INTEGER
      diff: 1, // INTEGER
      mats: ["string", 1], // ARRAY<STRING, INT>
    }, // OBJECT<key: value>
    upgrade: "",
  },
];
```
  
&nbsp;

To understand key, label, object and array/list definition, check out [JSON Data Docs and Definition](#json-data-docs-and-definition) Section.

---

## JSON Data Docs and Definition

For Equipment the key of object can be defined as:

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

As for Crysta or Xtall the key of object can be defined as:

- Name => (Items name or label)
- code => (Crysta classification)
  You can copy the crysta classification object and array below:
  
  &nbsp;

  ```javascript
  {
    "0": "Normal",
    "1": "Weapon",
    "2": "Armor",
    "3": "Additional Gear",
    "4": "Special Gear",
    "5": "Enhancer Normal",
    "6": "Enhancer Weapon",
    "7": "Enhancer Armor",
    "8": "Enhancer Additional Gear",
    "9": "Enhancer Special Gear"
  }
  ```
  
  &nbsp;

  And down below is the Crysta classification in form of array:
  
  &nbsp;

  ```javascript
  [
    "Normal",
    "Weapon",
    "Armor",
    "Additional Gear",
    "Special Gear",
    "Enhancer Normal",
    "Enhancer Weapon",
    "Enhancer Armor",
    "Enhancer Additional Gear",
    "Enhancer Special Gear",
  ];
  ```
  
  &nbsp;

  The value "-1" mean unidentified or unknown.

- base => (Base WATK or DEF of items)
- stab => (Base Stablility of items)
- pot => (Potential value, only available for origin: 0)
- amount => (sell and process value of items value)
  
  &nbsp;

  Examples:

  &nbsp;

  ```javascript
  [1, 1, "Colon Nut"]; // [spina_value, mats_value, mats_label]
  ```

  &nbsp;

- stats => status of items, "general" is the main status of items [status_label, status_value]
- source => more details of weapon origin, [mob_name, mob_level, location]
- Recipe => only available for NPC and Player
  Recipe key contain value of object with structure:

  &nbsp;

  ```javascript
    {
      // item level
      "lvl": 1,
      // item difficulty
      "diff": 1,
      // list of mats to craft items, including spina [mats_label, mats_quantity]
      "mats" : ["mats_label", 1]
    }
  ```

  &nbsp;

  Modulo operator could be used on Recipe "mats" to determined which one label/name and quantity, or this also can be done by using while loop with +2 increment.

  &nbsp;

  +2 increment could be done because the "mats" array length will absolutely have even length.

  &nbsp;

  Examples:
  
  &nbsp;

  ```javascript
  // Array examples
  const mats_list = ["Spina", 300, "Colon Leaf", 20, "[Book of Dancer]", 1];

  // Using for Loop
  const context = ["Name", "Quantity"];

  for (let i = 0; i < mats_list.length; i++) {
    console.log(`${context[i % 2]} => ${mats_list[i]}`);
  }

  /**
   * output:
   * Name => Spina
   * Quantity => 300
   * Name => Colon Leaf
   * Quantity => 20
   * Name => [Book of Dancer]
   * Quantity => 1
   */

  // Using while loop and +2 increment

  let i = 1;

  // "i" could be set as 0
  // and change => value = mats_list[i+1], label = mats_list[i]

  while (i < mats_list.length) {
    const label = mats_list[i - 1];
    const value = mats_list[i];
    console.log(`${label}: ${value}`);
    i += 2;
  }

  /**
   * output:
   * Spina: 300
   * Colon Leaf: 20
   * [Book of Dancer]: 1
   */
  ```

---

## Closing

Lee... LEEGMA NU-!
