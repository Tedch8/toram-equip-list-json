# Equipment Data Doc

The JSON structure is so dumb, i lost a braincell everytime i tried to read it! and i read it as "JASON"; and there's nothing you can do about it!

## Contents Table

---

- [Equipment Data Doc](#equipment-data-doc)
  - [Contents Table](#contents-table)
  - [JSON Version](#json-version)
  - [JSON Structure and Type](#json-structure-and-type)
    - [Equipment Json Structure](#equipment-json-structure)
    - [Crysta JSON Structure](#crysta-json-structure)
  - [JSON Data Docs and Definition](#json-data-docs-and-definition)
    - [Equipment JSON Structure](#equipment-json-structure-1)
    - [Crysta JSON Structure](#crysta-json-structure-1)
  - [Closing](#closing)

---

## JSON Version

The data includes a version, formatted as a integer total days of current date compared to offset date (current_date - offset_date), to track the last update. This version is used for caching purposes to determine whether new data needs to be fetched from client local storage.

## JSON Structure and Type

The structure of the JSON varies based on the item type. Currently, the only available structures are Equipment and Crysta.

### Equipment Json Structure

All equipment, including armor, additional gear, and special gear, share the same data structure and typing. Examples are provided below:

&nbsp;

```javascript
[
  {
    name: "string", // STRING
    origin: 1, // INTEGER
    base: 1.0, // FLOAT
    stab: 1, // INTEGER
    pot: 1, // INTEGER
    amount: [0, 0, "string"], //ARRAY< INTEGER, INTEGER, STRING >
    stats: {
      general: ["string", 1.0], // ARRAY< STRING, FLOAT >
    }, // OBJECT< Key: Value >
    source: ["string", 1, "string", []], //ARRAY<STRING,INTEGER,STRING,ARRAY<NULL || STRING>>
    recipe: {
      lvl: 1, // INTEGER
      diff: 1, // INTEGER
      mats: ["string", 1], // ARRAY<STRING, INT>
    }, // OBJECT<key: value>
  },
];
```

&nbsp;

### Crysta JSON Structure

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

As stated before, the structures for Equipment and Crysta differ slightly. Below are the labels and definitions for both JSON structures.

### Equipment JSON Structure

For Equipment the key of object can be defined as:

1. name key:
   The "name" key refers to the items name or label.
2. origin key:
   The "origin" key refers to the item's source, which is defined by a code. The code indicates whether the item was crafted by an NPC, crafted by a player, or dropped by mobs. The code consisted number: 0 (Player's craft), 1 (NPC's craft), 2 (Drop from mobs).
   This code can be used as a list or array index, or as an object key, as shown in the examples below:

   &nbsp;

   ```javascript
   // Array or list:
   ["Player", "NPC", "Drop"]

   // Object or Hashtable:
   {
     "0" : "Player",
     "1" : "NPC".
     "2" : "Drop"
   }
   ```

   &nbsp;

3. base key:
   The "base" key refers to base WATK or DEF of equipments. WATK refers to all Weapon and Sub-weapon (excluding Shield).
   DEF referse to all Armor, Additional Gear, Special Gear, and Shield.
4. stab key:
   The "stab" key referse to the base stablility of some equipments.
   The base stability only applied to weapon and sub-weapon (excluding Shield), the "Stability %" status is separated from the base stability.
5. pot key:
   The "pot" Key refers to potential value, this only available for Player Craft weapon.
6. amount key:
   The amount key referes to items sell and process value.
   Index 0: Spina value, Index 1: Process material value, index 2: Process material label.

   &nbsp;

   ```javascript
   // array examples => value type = [Integer, Integer, String]

   const examples = [100, 20, "Wood"];
   ```

   &nbsp;

7. stats key:
   The "stats" key refers to status of items, the "general" key is the default status of items, conditional status like "Shield Only" will be added as new key and status.
   The examples of data can be seen below:

   &nbsp;

   ```javascript
   // Each key contain array, which has even number length of data.
   // Even index = status label, Odd index = status value/quantity;
   // [status_label, status_value] => type = [String, Float]

   const examples = {
     general: ["ATK %", 5.0, "STR", 10.0, "STR %", 2.0],
     "Shield Only": ["Guard Break %", 10.0],
   };
   ```

   &nbsp;

8. source key:
   The "source" key refers to the specific details of the weapon’s origin. It indicates whether the item comes from an NPC or a player. For NPCs, this includes rewards from limited-time events, with additional information about the event’s location. For items obtained from mobs, the name, level, location, and dye will be provided if available.
   Weapon Dye will be provided soon.
   Example can be seen below:

   &nbsp;

   ```javascript
   // source examples, [name, ?lvl, location] => type [String, ?Integer, String, Dye[]]
   // Mobs:
   const examples1 = ["Colon", 5, "Rugio Ruins", []];
   // Player:
   const examples2 = ["Player", null, "N/A", []];
   // NPC:
   const examples3 = ["NPC", null, "Sofya: Blacksmith", []];
   // Event Venue Exchange:
   const examples4 = [
     "Snowball Event NPC",
     null,
     "Snowball Mini Game Venue",
     [],
   ];
   ```

   &nbsp;

9. recipe key:
   The "recipe" key only available for Player and NPC (Not including reward exchanges for item from limited time event). Below is the examples of the "recipe" key value:

   &nbsp;

   ```javascript
   const examples = {
     // item level
     lvl: 190,
     // item difficulty
     diff: 200,
     // list of mats to craft items, including spina [mats_label, mats_quantity] => type [String, Integer]
     mats: ["Colon Nut", 30, "Kijimu Stick", 60, "[Book of Bullsh-]", 1],
   };
   ```

   &nbsp;

### Crysta JSON Structure

As for Crysta or Xtall the key of object can be defined as:

1. name key:
   The "name" key refers to the items name or label.
2. type key
   the "type" key refers to Crysta classification, the classification can be done with form of object/hashtable, map/hashmap, and array/list. Type: -1 mean the classification was unidentified or unknown.
   Below is the crysta classification in form of object:

   &nbsp;

   ```javascript
   const examples = {
     0: "Normal",
     1: "Weapon",
     2: "Armor",
     3: "Additional Gear",
     4: "Special Gear",
     5: "Enhancer Normal",
     6: "Enhancer Weapon",
     7: "Enhancer Armor",
     8: "Enhancer Additional Gear",
     9: "Enhancer Special Gear",
   };
   ```

   &nbsp;
   The Crysta classification in form of array or list:
   &nbsp;

   ```javascript
   const examples = [
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

3. amount key:
   The amount key referes to items sell and process value.
   Index 0: Spina value, Index 1: Process material value, index 2: Process material label.

   ```javascript
   // array examples => value type = [Integer, Integer, String]
   const examples = [1, 1, "Colon"];
   ```

   &nbsp;

4. source key:
   The "source" key refers to the specific details of the crysta origin. this includes rewards and crafts from limited-time events, with additional information about the event’s location. For items obtained from mobs, the name, level, and location will be provided if available.
   Example can be seen below:

   &nbsp;

   ```javascript
   // source examples, [name, ?lvl, location] => type [String, ?Integer, String]
   // Mobs:
   const examples1 = ["Evil Lefina", 200, "El Scaro"];
   // NPC:
   const examples2 = ["NPC", null, "Sofya: Synthesis"];
   ```

   &nbsp;

5. recipe key:
   The "recipe" key only available for Player and NPC (Not including reward exchanges for item from limited time event). Below is the examples of the "recipe" key value:

   &nbsp;

   ```javascript
   const examples = {
     // item level
     lvl: 190,
     // item difficulty
     diff: 200,
     // list of mats to craft items, including spina [mats_label, mats_quantity] => type [String, Integer]
     mats: ["Colon Nut", 30, "Kijimu Stick", 60, "[Book of Bullsh-]", 1],
   };
   ```

   &nbsp;

   &nbsp;

   The modulo operator can be used on the Recipe's "mats" array to determine the label/name and quantity of each item. Alternatively, this can be achieved using a while loop with a +2 increment. The +2 increment works because the "mats" array length is always even.

   Examples:

   &nbsp;

   ```javascript
   // Array examples
   const examples_mats = [
     "Spina",
     300,
     "Colon Leaf",
     20,
     "[Book of Dancer]",
     1,
   ];

   // Using for Loop
   const context = ["Name", "Quantity"];

   for (let i = 0; i < examples_mats.length; i++) {
     console.log(`${context[i % 2]} => ${examples_mats[i]}`);
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
   // and change => value = examples_mats[i+1], label = examples_mats[i]

   while (i < examples_mats.length) {
     const label = examples_mats[i - 1];
     const value = examples_mats[i];
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

6. upgrade key:
   The "upgrade" key refers to the name of ancestor/previous crysta, this only available for Enhancer Crysta. In the future will be add serial code to connected enhancer crysta to it's ancestors, and add crysta descendant/next upgrade.

---

## Closing

Damn... My head hurt... F-
