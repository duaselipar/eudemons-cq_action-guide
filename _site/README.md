# Eudemons Online `cq_action` Table Guide

Welcome to the ultimate guide for understanding the `cq_action` table used in Eudemons Online private servers. This table plays a critical role in controlling game behavior — from NPC dialogs and item rewards to experience gains, system messages, and conditional logic.

Whether you're new to EO development or looking to refine your scripting skills, this guide will walk you through:
- The purpose and structure of the `cq_action` table
- Common `type` codes and how they work
- Practical examples like giving items, showing NPC dialogs, and triggering system messages
- Tips for chaining actions using `id_next` and handling conditions with `id_nextfail`

All content is written clearly and includes real-world examples to help you build, debug, and expand your server’s functionality faster.

> ⚙️ Built by the EO dev community — for the EO dev community.

Explore the sections below and level up your EO server customization!

- [What is `cq_action`?](#what-is-cq_action)
- [Field Explanations](#field-explanations)
- [Common Action Types](#common-action-types)
- [Type Explaination Download](#download-full-action-types)
- [Action Type Guides](#action-type-guides)

---

## What is `cq_action`?

![Alt text](assets/images/actiontable.png)

The `cq_action` table controls sequences of actions triggered in-game — like when you click an NPC, receive an item, or complete a quest. It uses an ID-based flow system with support for conditions and multiple outcomes.

---

## Field Explanations


>```INSERT INTO `cq_action` (`id`, `id_next`, `id_nextfail`, `type`, `data`, `param`) VALUES (1000, 0000, 0000, 0501, 810007, '');```

| Field         | Description |
|---------------|-------------|
| `id`          | Unique action ID |
| `id_next`     | Next action if success/condition true |
| `id_nextfail` | Next action if condition fails |
| `type`        | Type of action (dialog, give item, etc.) |
| `data`        | Depends on type (item ID, message ID, etc.) |
| `param`       | Usually contains message text or command |

---

## Common Action Types

| Type Code | Description                  |
|-----------|------------------------------|
| `0101`    | NPC Dialog Text              |
| `0102`    | NPC Options (buttons)        |
| `0125`    | System Announcement (Global) |
| `0126`    | System Tip (Local)           |
| `501`     | Give Item                    |
| `1001`    | Give EXP, Money, Stones      |

---


## Download Full Action Types

You can download the full explanation of action types here.

- [Download Here](assets/ScriptTypeExplanation.txt)

---


## Action Type Guides

<div style="border-left: 6px solid #4CAF50; background: #f0fdf4; padding: 15px 20px; margin: 20px 0; border-radius: 6px; font-size: 15px; line-height: 1.6;">
  <strong style="display: inline-block; margin-bottom: 6px;">✅ Tip:</strong><br>
  <strong>Old Engine</strong> = 3-class system (2008)<br>
  ➤ Reference: 
  <a href="https://www.elitepvpers.com/forum/eo-pserver-guides-releases/191378-release-manequin-english-server-db-client-ready-use-one-link.html" target="_blank" style="color:#2e7d32; text-decoration:underline;">
    2008 Release (Mannequin DB)
  </a>
  <br><br>
  <strong>New Engine</strong> = 9-class system with new features (2022)<br>
  ➤ Reference:
  <a href="https://www.elitepvpers.com/forum/eo-pserver-guides-releases/5076188-release-version-1655-engine-celebrity-hall-divine-fire-statue.html" target="_blank" style="color:#2e7d32; text-decoration:underline;">
    2022 Release (v1655 Engine)
  </a>
</div>




<details>
  <summary>🗨️ <strong>Create Dialog (Type 101, 102, 104 and 120)</strong></summary>
  <br>

  <p>This action shows a dialog when the player interacts with an NPC or item.</p>
  <img src="assets/images/101.png" style="max-width:100%; border-radius:8px; margin:10px 0;">

  <h4>💡 Example SQL:</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1001, 1002, 0000, 0101, 0, 'I can sell it only for an hour every day.');
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 0101, 0, 'Do you want to give it a go?');
REPLACE INTO `cq_action` VALUES (1003, 1004, 0000, 0102, 0, 'Just~one,~please. 1007');
REPLACE INTO `cq_action` VALUES (1004, 1005, 0000, 0102, 0, 'Sorry,~but~I~dont~need~it. 0');
REPLACE INTO `cq_action` VALUES (1005, 1006, 0000, 0104, 0, '0 0 34');
REPLACE INTO `cq_action` VALUES (1006, 0000, 0000, 0120, 0, '');
  </pre>

  <h4>🧾 Required Task Entries (cq_task)</h4>
  <pre>
REPLACE INTO `cq_task` VALUES (1001, 1001, 0000, '', '', 0, 0, 999, -100000, 100000, 0999, 0000, 0, -1, 0);
REPLACE INTO `cq_task` VALUES (1007, 1007, 0000, '', '', 0, 0, 999, -100000, 100000, 0999, 0000, 0, -1, 0);
  </pre>

  <h4>📘 Explanation Table:</h4>
  <table style="width:100%; border-collapse:collapse;">
    <thead>
      <tr style="background:#f0f0f0;">
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Type</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Purpose</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">param Explanation</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>101</code></td>
        <td style="padding:8px;">Show dialog text</td>
        <td style="padding:8px;">NPC or item dialog string (≈ 40–45 chars per line)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>102</code></td>
        <td style="padding:8px;">Dialog options / buttons</td>
        <td style="padding:8px;">Text ~ spacing + task_id (use <code>0</code> for close)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>104</code></td>
        <td style="padding:8px;">Dialog face image</td>
        <td style="padding:8px;">x y pic_id — from Ani/NpcFace.ANI (e.g. Face34)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>120</code></td>
        <td style="padding:8px;">End of dialog</td>
        <td style="padding:8px;">(none)</td>
      </tr>
    </tbody>
  </table>

  <h4>💡 Tips:</h4>
  <ul>
    <li>Every <code>type 101</code> must have a corresponding <code>cq_task</code> entry</li>
    <li>Every <code>task_id</code> used in <code>type 102</code> must also exist in <code>cq_task</code></li>
    <li>Use <code>~</code> for spacing inside all dialog text and option strings</li>
    <li>Use <code>0104</code> to show NPC face image (optional)</li>
    <li>End your dialog with <code>0120</code> to ensure it closes cleanly</li>
  </ul>

  <div style="border-left: 4px solid #2196f3; background: #e3f2fd; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🔧 <strong>Reminder:</strong> If the dialog doesn't show or options do nothing, double check your <code>cq_task</code> entries!
  </div>
</details>


<details>
  <summary>💠 <strong>Modern NPC Dialog (Type 132 & 133)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>This dialog system creates stylish, story-driven NPC interactions with portrait images, background panels, and up to 4 interactive options.This feature is only available on the 🆕 New Engine.</p>


  <h4>🖼️ Example: Linleria Dialog</h4>
  <img src="assets/images/133-0.png" style="width:80%; max-width:480px; border-radius:8px; margin-bottom:12px;">


  <h4>📜 Linleria SQL</h4>
  <pre>
REPLACE INTO cq_action VALUES (1001, 1002, 0000, 0101, 0, 'Hello~i`am~Linleria~the~designer,~do~you~have~enough~essence~to~trade~off~with~these~cosmetic~items?');
REPLACE INTO cq_action VALUES (1002, 1003, 0000, 0102, 0, 'Yes,send~me~there. 1006');
REPLACE INTO cq_action VALUES (1003, 1004, 0000, 0102, 0, 'Close. 1005');
REPLACE INTO cq_action VALUES (1004, 0000, 0000, 0132, 0, '0 Tasksystem_GeneralbotPic Linleria 0 Tasksystem_WnbeishangPic');
REPLACE INTO cq_action VALUES (1005, 0000, 0000, 133, 0, '');
  </pre>

  <ul>
    <li><strong>`Linleria`</strong>: Displayed name (customizable)</li>
    <li><strong>`Tasksystem_GeneralbotPic`</strong>: Dialog background (default, don’t change)</li>
    <li><strong>`Tasksystem_WnbeishangPic`</strong>: Portrait image (can change, from control.ani)</li>
    <li>Image preview path: <code>data/interface/style01/Tasksystem/God/WnbeishangPic.dds</code></li>
  </ul>

  <h4>🖼️ Example: Skuld Dialog</h4>
  <img src="assets/images/133-2.png" style="width:80%; max-width:480px; border-radius:8px; margin-bottom:12px;">

  <h4>📜 Skuld SQL</h4>
  <pre>
REPLACE INTO cq_action VALUES (1001, 1002, 0000, 0101, 0, 'Do~You~see~the~Sword~there?~This~sword~is~belong~to~King~of~the~Four~Seas.~I~need~your~help~to~stop~him. sound/dice_move.wav');
REPLACE INTO cq_action VALUES (1002, 1003, 0000, 0102, 0, 'I~am~going~to~absorb~the~energy. 1006');
REPLACE INTO cq_action VALUES (1003, 1004, 0000, 0102, 0, 'Close. 1005');
REPLACE INTO cq_action VALUES (1004, 0000, 0000, 0132, 0, 'Novice_BottomBG Novice_RolePic 21650');
REPLACE INTO cq_action VALUES (1005, 0000, 0000, 0133, 2, '');
  </pre>

  <ul>
    <li><strong>`Novice_BottomBG`</strong>: Background (default)</li>
    <li><strong>`Novice_RolePic`</strong>: NPC portrait name (can change, from control.ani)</li>
    <li>Image preview path: <code>data/interface/style01/Rookie/RolePic.dds</code></li>
    <li><strong>`21650`</strong>: NPC label ID (used for Skuld style)</li>
  </ul>

  <h4>⚠️ Important Notes</h4>
  <ul>
    <li><strong>Maximum 4 options</strong> (<code>0102</code>) per dialog</li>
    <li>One option <strong>must be a Close</strong> (linked to <code>type 133</code>)</li>
    <li><strong>Use <code>~</code> instead of spaces</strong> in all dialog lines or they won't display!</li>
    <li><code>Type 132</code> requires the player to be very close to the NPC (within ~5 tiles)</li>
    <li><strong>An NPC object is required</strong> — dialog won’t trigger if no valid NPC is nearby</li>
    <li><code>type 133</code> must use <code>data = 2</code> to allow proper closing for some UIs</li>
    <li>Supports ambient sound: add file at end of <code>param</code> like <code>sound/dice_move.wav</code> (Skuld dialog only!)</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> All dialog lines and button text must use <code>~</code> instead of spaces. Example:<br>
    <code>Hello~brave~warrior!</code> ✅ &nbsp;&nbsp;&nbsp;&nbsp; <code>Hello brave warrior!</code> ❌
  </div>

</details>



<details>
  <summary>🎁 <strong>Sent Items (Type 501, 525, 540, 550)</strong></summary>
  <br>

  <p>Sends specific items to the player when triggered. Used for quest rewards, shop systems, monster drops, and more.</p>


  <h4>📜 Basic Example – Type 501 (Give Item)</h4>
  <pre>
-- Give 1 item (basic)
REPLACE INTO cq_action VALUES (1000, 1002134, 0000, 0501, 754826, '');
  </pre>


  <h4>📜 Custom Stats – Type 501</h4>
  <pre>
-- With custom stats (Old Engine)
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0501, 1081990, '0 0 0 0 0 0 0 0 0 0 0 0 1');

-- Old Engine Params:
amount amount_limit ident gem1 gem2 magic1 magic2 magic3 data warghostexp gemtype availabletime MONOPOLY
  </pre>

  <pre>
-- With custom stats (New Engine)
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0501, 1081990, '0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0');

-- New Engine Params:
amount amount_limit ident gem1 gem2 magic1 magic2 magic3 data warghostexp gemtype availabletime MONOPOLY
eudemon_attack1 eudemon_attack2 eudemon_attack3 eudemon_attack4 special_effect Gem3 GodExp prescription
  </pre>


  <h4>📌 Special Variants:</h4>


  <pre>
-- Type 501 (New Engine): Give Necro Eudemon Egg
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0501, 1081610, '1');

-- Notes:
When giving a Eudemon Egg (e.g., 1081610), setting the first param to <code>1</code> marks it as a <strong>Necro Eudemon</strong>.
This applies only in the <strong>New Engine</strong> and only for specific Necro Eudemon egg items.
  </pre>

  <img src="assets/images/501-1.png" style="max-width:100%; border-radius:8px; margin:10px 0;">

  <pre>
-- Type 525 (New Engine Only): Same as 501 but "availabletime" becomes "aging days".This type can only be used for 820xxxx items only.
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0525, 820646, '0 0 0 0 0 0 0 0 0 0 0 30');
-- 30 = 30 days
  </pre>

  <pre>
-- Type 540 (Both Engines): Modifies stackable item count
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0540, 1020171, 'amount += 1');

-- Notes:
Supports: +=, -=, =, >, <, >=, <=
Can also be used to remove items: 'amount += -1'
Only works for stackable items
Recommended for adjusting count instead of deleting via 502
  </pre>

  <pre>
-- Type 550 (New Engine Only): Same as 501 but with client-side display effect
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0550, 1600261, '');
  </pre>

  <img src="assets/images/550.png" style="max-width:100%; border-radius:8px; margin:10px 0;">


  <h4>💡 Tips:</h4>
  <ul>
    <li>Use <code>type 503</code> to check item count before giving or removing</li>
    <li><code>540</code> is more flexible and safer than deleting stackable items with <code>502</code></li>
    <li>To show visual reward feedback, use <code>550</code> (New Engine only)</li>
    <li><code>525</code> is useful for timed items like trial gear or events</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Type 540 works in both Old and New Engine</strong>. Use it for adjusting stackable item amounts, both add and subtract.
  </div>
</details>



<details>
  <summary>🗑️ <strong>Delete Item (Type 502)</strong></summary>
  <br>

  <p><strong>Type 502</strong> is used to remove one or more items from the player’s inventory based on type or name.</p>


  <h4>📦 Syntax Behavior</h4>
  <ul>
    <li><code>data</code> = <strong>ItemTypeID</strong> (or <code>0</code> to match by name)</li>
    <li><code>param</code> = quantity or item name (if using <code>data = 0</code>)</li>
    <li>Use <code>id_nextfail</code> to handle missing items or not enough quantity</li>
  </ul>


  <h4>📜 Example: Delete Single Quantity by Item Type</h4>
  <pre>
REPLACE INTO cq_action VALUES (1001, 1003, 1002, 0502, 1020030, '1 1');
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You don`t have enough item.');
  </pre>


  <h4>📜 Example: Delete Stack of Items</h4>
  <pre>
REPLACE INTO cq_action VALUES (1001, 1003, 1002, 0502, 810002, '10');
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You don`t have enough item.');
  </pre>


  <h4>📜 Example: Delete Unstackable Items (Repeat Action)</h4>
  <pre>
-- Delete 2x [ItemID: 111006] if item is NOT stackable
REPLACE INTO cq_action VALUES (1001, 1002, 1003, 0502, 111006, 0);
REPLACE INTO cq_action VALUES (1002, 1004, 1003, 0502, 111006, 0);
REPLACE INTO cq_action VALUES (1003, 0000, 0000, 0126, 0, 'You don`t have enough item.');
  </pre>


  <h4>📜 Example: Delete Items by Name</h4>
  <pre>
-- Delete 2x "Blue Crest Helmet" by item name
REPLACE INTO cq_action VALUES (1001, 1002, 1003, 0502, 0, 'Blue~Crest~Helmet');
REPLACE INTO cq_action VALUES (1002, 1004, 1003, 0502, 0, 'Blue~Crest~Helmet');
REPLACE INTO cq_action VALUES (1003, 0000, 0000, 0126, 0, 'You don`t have enough item.');
  </pre>


  <h4>✅ For Stack Items: Use <code>Type 540</code> (Recommended)</h4>
  <pre>
-- Safely subtract 5x stackable item (e.g., EXP Scroll)
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0540, 1020116, 'amount += -5');
  </pre>


  <h4>💡 Best Practice:</h4>
  <ul>
    <li>Use <strong><code>type 503</code></strong> to check quantity before deleting items</li>
    <li>Use <code>type 0126</code> to notify player if the item is missing</li>
    <li>For <strong>stack items</strong>, <code>type 540</code> is cleaner and more reliable</li>
  </ul>

  <div style="border-left: 4px solid #f44336; background: #ffebee; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Reminder:</strong> For non-stackable items, repeat <code>type 502</code> multiple times to match how many you want to delete.
  </div>
</details>




<details>
  <summary>🔍 <strong>Check Item (Type 503)</strong></summary>
  <br>

  <p><strong>Type 503</strong> is used to check if a player has a specific item, with optional quantity/durability validation.</p>


  <h4>📦 Syntax Behavior</h4>
  <ul>
    <li><code>data</code> = <strong>ItemTypeID</strong> (or <code>0</code> for name-based check)</li>
    <li><code>param</code> = quantity or full item name (if <code>data = 0</code>)</li>
    <li>If only checking existence (not quantity), set <code>param = 0</code></li>
    <li>If item is missing or quantity is not enough → goes to <code>id_nextfail</code></li>
  </ul>


  <h4>📜 Example 1: Check If Player Has 5x of Item</h4>
  <pre>
-- Check if player has at least 5x of ItemTypeID 111007
REPLACE INTO cq_action VALUES (0001, 0002, 0003, 0503, 111007, 5);

-- Success
REPLACE INTO cq_action VALUES (0002, 0000, 0000, 0126, 0, 'You have this item');

-- Failure
REPLACE INTO cq_action VALUES (0003, 0000, 0000, 0126, 0, 'You don`t have this item');
  </pre>


  <h4>📜 Example 2: Check If Player Has Item (No Quantity Required)</h4>
  <pre>
REPLACE INTO cq_action VALUES (0001, 0002, 0003, 0503, 111007, 0);
REPLACE INTO cq_action VALUES (0002, 0000, 0000, 0126, 0, 'You have this item');
REPLACE INTO cq_action VALUES (0003, 0000, 0000, 0126, 0, 'You don`t have this item');
  </pre>


  <h4>📜 Example 3: Check by Item Name</h4>
  <pre>
REPLACE INTO cq_action VALUES (0001, 0002, 0003, 0503, 0, 'Blue~Crest~Helmet');
REPLACE INTO cq_action VALUES (0002, 0000, 0000, 0126, 0, 'You have this item');
REPLACE INTO cq_action VALUES (0003, 0000, 0000, 0126, 0, 'You don`t have this item');
  </pre>


  <h4>💡 Notes:</h4>
  <ul>
    <li>Item name matching must be same with cq_itemtype</li>
    <li>This type is often used before <code>type 502</code> to verify quantity before removing</li>
    <li>Very useful for quest checking, crafting checks, and condition-based actions</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Always use <code>id_nextfail</code> for fallback messaging.
  </div>
</details>



<details>
  <summary>⚙️ <strong>Modify Player Attributes (Type 1001)</strong></summary>
  <br>

  <p>This action modifies or checks player attributes using logic like <code>+=</code>, <code>==</code>, <code>&lt;</code>, etc.</p>

  <h4>💡 Example SQL:</h4>
  <pre>
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 1001, 0, 'e_money += 13500');  // add
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 1001, 0, 'e_money += -13500'); // remove
  </pre>

  <h4>📘 Supported Attributes & Operators:</h4>
  <div style="background:#f4f4f4; padding:12px 16px; border-radius:8px; font-size:14px; line-height:1.7; overflow:auto; max-height:500px;">
    <ul style="margin:0; padding-left:20px;">
      <li><code>life</code>, <code>mana</code>, <code>money</code>, <code>exp</code>, <code>pk</code>, <code>energy(stamina)</code> — <strong>(+=, ==, &lt;)</strong></li>
      <li><code>xp</code>, <code>nob_rank_donate</code> — <strong>(+=)</strong></li>
      <li><code>profession</code> — <strong>(==, set, &gt;=, &lt;=)</strong></li>
      <li><code>level</code>, <code>force</code>, <code>dexterity</code>, <code>speed</code>, <code>health</code>, <code>soul</code> — <strong>(+=, ==, &lt;)</strong></li>
      <li><code>rank</code>, <code>rankshow</code> — <strong>(==, &lt;)</strong></li>
      <li><code>iterator</code> — <strong>(=, &lt;=, +=, ==)</strong></li>
      <li><code>crime</code> — <strong>(==, set)</strong></li>
      <li><code>gamecard</code>, <code>gamecard2</code>, <code>totalbattlelev</code> — <strong>(==, &gt;=, &lt;=)</strong></li>
      <li><code>metempsychosis</code> — <strong>(==, &lt;)</strong></li>
      <li><code>mercenary_rank</code>, <code>mercenary_exp</code>, <code>exploit</code> — <strong>(==, &lt;, +=)</strong></li>
      <li><code>maxlifepercent</code> — <strong>(+=, ==, &lt;)</strong></li>
      <li><code>tutor_exp</code>, <code>tutor_level</code> — <strong>(==, &lt;, +=, =)</strong></li>
      <li><code>eudemon_boord_size</code> — <strong>(==, &lt;, +=, =)</strong></li>
      <li><code>syn_proffer</code>, <code>maxeudemon</code>, <code>soul_value</code> — <strong>(&lt;, +=, =)</strong></li>
      <li><code>ahlife</code> — <strong>(-=)</strong></li>
      <li><code>moonmoney</code>, <code>starmoney</code> — <strong>(==, +=, &lt;&gt;)</strong></li>
      <li><code>BftitlePlaneid</code> — <strong>(==, +=, =)</strong></li>
      <li><code>DstitleId</code> — <strong>(==, &lt;)</strong></li>
      <li><code>crystalmoney</code>, <code>honourmoney</code>, <code>olggodmoney</code> — <strong>(+=, &lt;, ==)</strong></li>
      <li><code>energy</code> — <strong>(&lt;, +=, =)</strong></li>
      <li><code>goddesspoint</code> — <strong>(==, &lt;, +=)</strong></li>
      <li><code>godexp</code> — <strong>(+=, ==, &lt;)</strong></li>
      <li><code>exp_percent</code>, <code>godexp_percent</code> — <strong>(+=)</strong></li>
    </ul>
  </div>

  <p style="margin-top:12px;">🧠 Most attributes use <code>+=</code> to add, <code>==</code> to compare, and <code>&lt;</code> to check thresholds.</p>

  <div style="border-left: 4px solid #FF9800; background: #fff8e1; padding: 12px 16px; margin-top: 15px; border-radius: 6px;">
    ⚠️ <strong>Note:</strong> Only some attributes are available in the old engine.<br>
    Refer to your <code>cq_user</code> or <code>cq_user_new</code> table to verify which fields are available in your version.<br><br>
    📌 <strong>Old Engine:</strong> <code>cq_user</code><br>
    🆕 <strong>New Engine:</strong> <code>cq_user_new</code>
  </div>
</details>



<details>
  <summary>🎲 <strong>Random Chance & Random Action (Type 121 & 122)</strong></summary>
  <br>

  <p>These types allow you to introduce <strong>randomness</strong> into your server logic:</p>

  <ul>
    <li><strong>Type 121</strong> – Checks if a random chance is successful (e.g., 20% chance to win)</li>
    <li><strong>Type 122</strong> – Randomly selects and executes one of 8 possible actions</li>
  </ul>

  <h4>🎯 Type 121 – Random Chance</h4>
  <p><code>param</code> format: <code>"chance total"</code></p>
  <pre>
INSERT INTO cq_action VALUES (1000, 1001, 1002, 0121, 0, '20 100');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'succeed');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'fails');
  </pre>
  <p>This means: <strong>20 out of 100</strong> (20%) chance of success.</p>
  <ul>
    <li>If the check <strong>succeeds</strong>, it continues to <code>id_next</code> (1001)</li>
    <li>If the check <strong>fails</strong>, it continues to <code>id_nextfail</code> (1002)</li>
  </ul>

  <h4>🔀 Type 122 – Random Action Select</h4>
  <p><code>param</code> format: <code>"action0 action1 action2 ... action7"</code></p>
  <pre>
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0122, 0, '1001 1002 1003 1004 1005 1006 1007 1008');
  </pre>
  <p>This means: randomly choose <strong>ONE</strong> action ID from the list above.<br>
Each ID must refer to a valid <code>cq_action</code> that will be executed.</p>

  <div style="border-left: 4px solid #FF9800; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🧠 <strong>Tip:</strong> You can repeat the same ID multiple times to increase its chance of being selected.<br>
    Example: <code>'1001 1001 1001 1002 1002 1003 1004 1005'</code><br>
    This gives <code>1001</code> a higher weight than the others (appears 3 out of 8).
  </div>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Use Case:</strong> 
    <code>type 121</code> is great for chance-based triggers (e.g. success/failure),
    while <code>type 122</code> is ideal for random branching events, rewards, or NPC reactions.
  </div>
</details>


<details>
  <summary>💬 <strong>Message Box (Type 126)</strong></summary>
  <br>

  <p><strong>Type 126</strong> displays a simple message box popup on the client with a <code>YES</code> button. It is for informational purposes only — it does <u>not</u> execute any action on click.</p>

  <h4>🖼️ Example Output:</h4>
  <img src="assets/images/126.png" alt="Message Box Screenshot" style="max-width:100%; border-radius:8px; margin:10px 0;">

  <h4>💡 Example SQL:</h4>
  <pre>
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Your text here.');
  </pre>

  <h4>📌 Notes:</h4>
  <ul>
    <li><strong>param:</strong> The message to display. Spaces are allowed — you do <strong>not</strong> need to use <code>~</code>.</li>
    <li><strong>Text limit:</strong> Keep your message short and concise. Long messages will overflow or be cut off.</li>
    <li>Useful for showing warnings, notifications, or short instructions.</li>
  </ul>

</details>


<details>
  <summary>📢 <strong>Broadcast Message (Type 125)</strong></summary>
  <br>

  <p><strong>Type 125</strong> sends system-wide broadcast messages across different UI locations depending on the <code>data</code> value (channel type).</p>

  <h4>💡 Example SQL:</h4>
  <pre>
-- Top-left corner (Old & New Engine)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0125, 2005, 'Your text here');

-- Center screen classic popup (Old & New Engine)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0125, 2011, 'Your text here');

-- Bottom-left in chat (New Engine only)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0125, 2007, 'Your text here');

-- Center screen modern popup (New Engine only)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0125, 2024, 'Your text here');
  </pre>

  <h4>📸 Broadcast Styles:</h4>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Channel</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Location</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Preview</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Engine</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>2007</code></td>
        <td style="padding:8px;">Bottom-left (chat area)</td>
        <td style="padding:8px;"><img src="assets/images/125-2007.png" alt="Chat Message" style="width:80%;"></td>
        <td style="padding:8px;">New Engine Only</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2005</code></td>
        <td style="padding:8px;">Top-left system notice</td>
        <td style="padding:8px;"><img src="assets/images/125-2005.png" alt="Top Left Message" style="width:80%;"></td>
        <td style="padding:8px;">Old & New Engine</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2024</code></td>
        <td style="padding:8px;">Modern popup (center screen)</td>
        <td style="padding:8px;"><img src="assets/images/125-2024.png" alt="Popup Message" style="width:80%;"></td>
        <td style="padding:8px;">New Engine Only</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2011</code></td>
        <td style="padding:8px;">Classic popup (center screen)</td>
        <td style="padding:8px;"><img src="assets/images/125-2011.png" alt="Center Screen Message" style="width:80%;"></td>
        <td style="padding:8px;">Old & New Engine</td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Choose <code>data</code> based on how visible the message should be.<br>
    Use <code>2007</code> for chat-style messages, <code>2024</code> for clean centered alerts.
  </div>
</details>




<details>
  <summary>💡 <strong>Local System Message (Type 1010)</strong></summary>
  <br>

  <p><strong>Type 1010</strong> is used for sending client-side local messages, popups, and web links. The display behavior is defined by the <code>data</code> (channel) value.</p>

  <h4>💡 Example SQL:</h4>
  <pre>
-- Local chat system notice
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1010, 2000, 'Your text here.');

-- Modern pop-up (new engine only)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1010, 2024, 'You`ve arrived in Forgotten Realms');

-- Web link
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1010, 2105, 'http://www.google.com');
  </pre>

  <h4>📘 Message Channels (data values):</h4>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Data</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Channel</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Message Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>2000/2007</code></td>
        <td style="padding:8px;">System Chat Channel</td>
        <td style="padding:8px;">Welcome to Knightfall.</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2001</code></td>
        <td style="padding:8px;">Whisper Chat Channel</td>
        <td style="padding:8px;">Announcer: reminder has end.</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2002</code></td>
        <td style="padding:8px;">[Action] Chat Channel</td>
        <td style="padding:8px;">Eliminate 16 Elite monsters.</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2003</code></td>
        <td style="padding:8px;">Team Chat Channel</td>
        <td style="padding:8px;">%user_name has used Double EXP Potion.(your teammate cannot see this.)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2023</code></td>
        <td style="padding:8px;">Boss Chat Channel</td>
        <td style="padding:8px;">King MadBull has Appear!</td>
      </tr>  
       <tr>
        <td style="padding:8px;"><code>2025</code></td>
        <td style="padding:8px;">Help Chat Channel</td>
        <td style="padding:8px;">You don't have enough gold.</td>
      </tr>     
      <tr>
        <td style="padding:8px;"><code>2005</code></td>
        <td style="padding:8px;">Top-left Notice</td>
        <td style="padding:8px;">You learned Tornado Skill.</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2011</code></td>
        <td style="padding:8px;">Center Screen - [System]</td>
        <td style="padding:8px;">You donated 1000 HP and the Hatred Wraith revived!</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2024</code></td>
        <td style="padding:8px;">Modern Popup (New Engine)</td>
        <td style="padding:8px;">You’ve arrived in Cronus</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2105</code></td>
        <td style="padding:8px;">Open Web Link</td>
        <td style="padding:8px;"><code>http://www.google.com</code></td>
      </tr>
    </tbody>
  </table>

  <h4>⚠️ Behavior Notes:</h4>
  <ul>
    <li><strong>All messages are local</strong> (only visible to the player)</li>
    <li><strong>data 2105</strong> will immediately open the provided web URL</li>
    <li>Text limits apply to some channels; keep messages concise</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use <code>type 1010</code> for immersive local feedback like quests, status effects, zone tips, or helper links — without global broadcast noise.
  </div>
</details>


<details>
  <summary>🎒 <strong>Check Inventory Space (Type 508)</strong></summary>
  <br>

  <p><strong>Type 508</strong> checks if the player has enough remaining space in a specific inventory bag. If not enough space is available, it goes to <code>id_nextfail</code>.</p>

  <h4>🧠 param format:</h4>
  <pre><code>required_space weight pack_type</code></pre>
  <ul>
    <li><strong>required_space</strong>: Number of free slots needed</li>
    <li><strong>weight</strong>: Usually <code>0</code> (reserved for future logic)</li>
    <li><strong>pack_type</strong>: Type of inventory bag to check</li>
  </ul>

  <h4>📦 Supported <code>pack_type</code> Values:</h4>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Pack Type</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Engine</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>50</code></td>
        <td style="padding:8px;">Common item bag (main backpack)</td>
        <td style="padding:8px;">✅ Old & New Engine</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>52</code></td>
        <td style="padding:8px;">Eudemons Egg bag</td>
        <td style="padding:8px;">✅ Old & New Engine</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>53</code></td>
        <td style="padding:8px;">Eudemons Inventory</td>
        <td style="padding:8px;">✅ Old & New Engine</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>46</code></td>
        <td style="padding:8px;">Quest item bag</td>
        <td style="padding:8px;">🆕 New Engine Only</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>47</code></td>
        <td style="padding:8px;">Material item bag</td>
        <td style="padding:8px;">🆕 New Engine Only</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>43</code></td>
        <td style="padding:8px;">Divine Fire item bag</td>
        <td style="padding:8px;">🆕 New Engine Only</td>
      </tr>
    </tbody>
  </table>

  <div style="margin-top:12px; font-size:15px;">
    <strong>🧭 Compatibility Summary:</strong><br>
    ✅ <strong>Old Engine:</strong> <code>50</code>, <code>52</code>, <code>53</code><br>
    🆕 <strong>New Engine:</strong> <code>46</code>, <code>47</code>, <code>43</code>, <code>50</code>, <code>52</code>, <code>53</code>
  </div>

  <h4>💡 Example Usage:</h4>
  <pre>
-- Check 1 slot in main backpack
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0508, 0, '1 0 50');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your backpack is full!');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0501, 191740, '');

-- Check 1 slot in egg bag
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0508, 0, '1 0 52');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your egg bag is full.');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0501, 1083060, '');


-- Check 1 slot in Eudemon Inventory
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0508, 0, '1 0 53');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You need 1 free space in your inventory.');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0526, 0, '');
  </pre>
  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Always use <code>type 126</code> to show user-friendly messages when space is insufficient.
  </div>
</details>


<details>
  <summary>⏰ <strong>Check Time (Type 123)</strong></summary>
  <br>

  <p><strong>Type 123</strong> checks the current server time using various time-based formats. It can be used to limit actions to certain dates, times, or days of the week.</p>

  <h4>🧠 param format:</h4>
  <pre><code>Depends on data (0 to 6) – see formats below</code></pre>


  <h4>📆 Supported Time Types (<code>data</code> values):</h4>

  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Data</th>
        <th style="text-align:left; padding:8px;">Format</th>
        <th style="text-align:left; padding:8px;">Example</th>
        <th style="text-align:left; padding:8px;">Description</th>
        <th style="text-align:left; padding:8px;">Engine</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>0</code></td>
        <td style="padding:8px;"><code>%d-%d-%d %d:%d %d-%d-%d %d:%d</code></td>
        <td style="padding:8px;">2024-04-01 10:00 2024-04-01 18:00</td>
        <td style="padding:8px;">Full date & time range</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>1</code></td>
        <td style="padding:8px;"><code>%m-%d %H:%M %m-%d %H:%M</code></td>
        <td style="padding:8px;">04-01 09:00 04-01 17:00</td>
        <td style="padding:8px;">Month-day with time range</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>2</code></td>
        <td style="padding:8px;"><code>%d %H:%M %d %H:%M</code></td>
        <td style="padding:8px;">15 10:00 15 12:00</td>
        <td style="padding:8px;">Day of month with time range</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>3</code></td>
        <td style="padding:8px;"><code>%d %H:%M %d %H:%M</code></td>
        <td style="padding:8px;">2 14:00 2 18:00</td>
        <td style="padding:8px;">Day of week (1=Monday) with time range</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>4</code></td>
        <td style="padding:8px;"><code>%H:%M %H:%M</code></td>
        <td style="padding:8px;">08:00 17:00</td>
        <td style="padding:8px;">Any time of day (daily range)</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>5</code></td>
        <td style="padding:8px;"><code>%d %d</code></td>
        <td style="padding:8px;">10 20</td>
        <td style="padding:8px;">Minute range in current hour</td>
        <td style="padding:8px;">Old & New</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>6</code></td>
        <td style="padding:8px;"><code>%d %d %d %d %d %d %d</code></td>
        <td style="padding:8px;">0 1 1 0 0 0 0</td>
        <td style="padding:8px;">Day of week (Mon–Sun as 1/0 mask)</td>
        <td style="padding:8px;">🆕 New Engine Only</td>
      </tr>
    </tbody>
  </table>


  <h4>💡 Examples:</h4>

  <pre>
-- Run only from 9 AM to 5 PM daily
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0123, 4, '09:00 17:00');

-- Run only on Tuesday and Wednesday
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0123, 6, '0 1 1 0 0 0 0');

-- Run only from minute 10 to 20 of each hour
INSERT INTO cq_action VALUES (1000, 1002, 1001, 0123, 5, '10 20');
  </pre>


  <h4>🔗 Reference:</h4>
  <a href="https://www.elitepvpers.com/forum/eo-pserver-guides-releases/419390-guide-teoretical-pracitce-usage-time-type-action.html#post3854279" target="_blank">
    elitepvpers: Time Type Action Guide
  </a>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🧠 <strong>Reminder:</strong> Use <code>id_next</code> for when the time check passes and <code>id_nextfail</code> for when it fails.<br>
    ⚠️ <strong>Note:</strong> <code>data = 6</code> (weekday mask) is only supported in the <strong>new engine</strong>.
  </div>
</details>

<details>
  <summary>📰 <strong>Update Announcement Window (Type 130 & 131)</strong></summary>
  <br>

  <p><strong>Type 130</strong> displays in-game announcement window (like patch notes or news). It supports multiple lines of text chained via <code>id_next</code>.</p>

  <h4>🖼️ Example Display:</h4>
  <img src="assets/images/130.png" alt="Update Announcement">

  <h4>📜 Example SQL Script:</h4>
  <pre>
-- Start of announcement chain
INSERT INTO cq_action VALUES (1000064, 1000065, 0000, 0130, 0, 'Knightfall newest update! Information is showed below:');
INSERT INTO cq_action VALUES (1000065, 1000066, 0000, 0130, 0, 'New class and new features come to Cronus!');
INSERT INTO cq_action VALUES (1000066, 1000067, 0000, 0130, 0, '1.New Class `Star Guardian` and brand new skill!');
INSERT INTO cq_action VALUES (1000067, 1000068, 0000, 0130, 0, '2.Celebrity hall fully fixed and updated.');
INSERT INTO cq_action VALUES (1000068, 1000069, 0000, 0130, 0, '3.Skytower introduced! limit level up to 310 floor!');
INSERT INTO cq_action VALUES (1000069, 1000070, 0000, 0130, 0, '4.Complete demon rising expansion to explore!');
INSERT INTO cq_action VALUES (1000070, 1000071, 0000, 0130, 0, '5.New domain! Get high stats with new talisman');
INSERT INTO cq_action VALUES (1000071, 1000072, 0000, 0130, 0, '6.New divine fire level 85 with latest tomb to conquer');

-- End of announcement
INSERT INTO cq_action VALUES (1000072, 0000, 0000, 0131, 0, '0');
  </pre>

  <h4>🔧 Usage:</h4>
  <ul>
    <li>Trigger it when a player enters the game</li>
    <li>Each <code>type 130</code> is one line of text</li>
    <li>End the sequence with <code>type 131</code> to close the window</li>
    <li>Max ~7–10 lines for best layout appearance</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use this for new patch notes, event schedules, server updates, or rules.
  </div>
</details>



<details>
  <summary>🎁 <strong>God’s Treasure (Type 188)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 188</strong> opens the <em>God’s Treasure</em> spinning interface, where players can draw from 8 randomized rewards. This feature is only available on the <strong>🆕 New Engine</strong>.</p>


  <h4>🖼️ Example Display:</h4>
  <img src="assets/images/188.png" alt="Gods Treasure Interface">


  <h4>🧠 param format:</h4>
  <pre><code>ItemID Quantity DisplayActionID RandomWeight</code></pre>
  <p>There must be <strong>exactly 8 entries</strong>, separated by commas.</p>


  <h4>💰 Pricing:</h4>
  <ul>
    <li><code>data</code> = total price (in <strong>EP</strong>) to <code>Claim All</code> items</li>
    <li>Set <code>data = 0</code> to disable the Claim All button</li>
  </ul>


  <h4>💡 Example SQL:</h4>
  <pre>
-- Open interface (Claim All disabled)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0188, 0, 
'1026624 3 1001 1,
1026625 1 1002 20,
1026624 1 1003 20,
1026624 1 1004 20,
1026625 3 1005 1,
1026626 3 1006 1,
1026626 1 1007 20,
1026626 1 1008 20');

-- Rewards for each draw
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0540, 1026624, 'amount += 3');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0540, 1026625, 'amount += 1');
INSERT INTO cq_action VALUES (1003, 0000, 0000, 0540, 1026624, 'amount += 1');
INSERT INTO cq_action VALUES (1004, 0000, 0000, 0540, 1026624, 'amount += 1');
INSERT INTO cq_action VALUES (1005, 0000, 0000, 0540, 1026625, 'amount += 3');
INSERT INTO cq_action VALUES (1006, 0000, 0000, 0540, 1026626, 'amount += 3');
INSERT INTO cq_action VALUES (1007, 0000, 0000, 0540, 1026626, 'amount += 1');
INSERT INTO cq_action VALUES (1008, 0000, 0000, 0540, 1026626, 'amount += 1');
  </pre>


  <h4>📝 Use Case Ideas:</h4>
  <ul>
    <li>🎯 Boss drops that trigger a lucky spin</li>
    <li>🎁 Treasure chests with randomized loot</li>
    <li>🐉 RNG rewards from event monsters or world bosses</li>
    <li>🧧 Event-based lotteries or login prizes</li>
  </ul>

  <h4>📌 Parameter Breakdown:</h4>
<ul>
  <li><code>ItemID</code>: The item to show in the gift box</li>
  <li><code>Quantity</code>: How much to display</li>
  <li><code>DisplayActionID</code>: What action runs when claimed</li>
  <li><code>RandomWeight</code>: The higher the number, the easier it is to get. The lower the number, the rarer it becomes (e.g., <code>1</code> = rare, <code>20</code> = common).</li>
</ul>


  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine <code>type 188</code> with <code>type 121/122</code> if you want conditional treasure or tiered draw access.
  </div>
</details>





<details>
  <summary>🧾 <strong>Temporary Dialog & In-Game Web (Type 189)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 189</strong> is used for two purposes depending on the <code>data</code> value:</p>
  <ul>
    <li><code>data = 204</code>: Temporary dialog box (shows for 4–5 seconds)</li>
    <li><code>data = 202</code>: Opens a web browser window in-game (linked to <code>areaid.ini</code>)</li>
  </ul>

  <h4>💬 Example: Temporary Dialog (data = 204)</h4>
  <img src="assets/images/189-204.png" style="max-width:100%; border-radius:8px; margin-bottom:12px;" />

  <pre>
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 204,
  'Lorem~ipsum~dolor~sit~amet. Tasksystem_GeneralbotPic|1 null -520 30 250 -30 1 Tasksystem_xnhlhxtgwbsx Title~Manager|100|217|1 500 500');
  </pre>

  <h5>🧩 Parameter Breakdown (dialog format):</h5>
  <ul>
    <li><code>Lorem~ipsum~dolor~sit~amet</code>: Dialog content (must use <code>~</code> for spacing)</li>
    <li><code>Tasksystem_GeneralbotPic</code>: Background panel (black theme, default)</li>
    <li><code>|1</code>: Unknown, typically set to 1</li>
    <li><code>null</code>: Placeholder for optional elements (e.g., sound/dice_move.wav)</li>
    <li><code>-520 30</code>: X and Y position of the dialog box (X: left/right, Y: up/down)</li>
    <li><code>250 -30</code>: Text position (left/right, up/down)</li>
    <li><code>1</code>: Dialog screen location (1 = center screen, 2 = top right)</li>
    <li><code>Tasksystem_xnhlhxtgwbsx</code>: Portrait (from <code>control.ani</code>)</li>
    <li><code>Title~Manager</code>: Name below portrait (use <code>~</code> instead of spaces)</li>
    <li><code>|100</code>: Name X offset (right = bigger, left = smaller)</li>
    <li><code>|217</code>: Name Y offset (down = bigger, up = smaller)</li>
    <li><code>|1</code>: Name alignment/location (guess)</li>
    <li><code>500 500</code>: Unknown values (typically left untouched)</li>
  </ul>

  <h5>📜 Alternate Example:</h5>
  <pre>
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 204,
  'Well?~Is~that~the~young~Ranger~who~fought~off~the~enemy~at~the~city~gates~you~are?~Not~bad. Archer_PhoenixPic null -520 30 250 -250 1');
  </pre>
  <p>Some portraits already include name overlays and background. Browse: <code>data/interface/style01/Tasksystem/God/</code></p>

  <hr>

  <h4>🌐 Example: Open In-Game Website (data = 202)</h4>
  <img src="assets/images/189-202.png" style="max-width:100%; border-radius:8px; margin-bottom:12px;" />

  <pre>
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 202, '6 6');
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 202, '8 654');
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 202, '9 1396');
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0189, 202, '6 4');
  </pre>

  <h5>🌐 Parameter Breakdown (browser format):</h5>
  <ul>
    <li><code>size</code>: Window layout (see chart below)</li>
    <li><code>areaid</code>: Web index (linked to <code>areaid.ini</code>)<br>
    Example: <code>URL654 = http://knightfall.my.to/newbie/</code></li>
  </ul>

  <h5>📏 Browser Window Sizes:</h5>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr><th>Size</th><th>Dimensions</th></tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>300 × 400</td></tr>
      <tr><td>2</td><td>400 × 500</td></tr>
      <tr><td>3</td><td>500 × 600</td></tr>
      <tr><td>4</td><td>500 × 500</td></tr>
      <tr><td>5</td><td>580 × 475</td></tr>
      <tr><td>6</td><td>900 × 650</td></tr>
      <tr><td>7</td><td>427 × 469</td></tr>
      <tr><td>8</td><td>820 × 568</td></tr>
      <tr><td>9</td><td>900 × 650</td></tr>
      <tr><td>10</td><td>990 × 730</td></tr>
      <tr><td>11</td><td>905 × 655</td></tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Note:</strong> Make sure the web URL is properly set in your client <code>areaid.ini</code> as <code>URL{areaid}</code>.
  </div>
</details>







<details>
  <summary>🔢 <strong>Global Variable Control (Type 191 & 192)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only </strong>
  </div>
  <p><strong>Type 191</strong> and <strong>Type 192</strong> are used to manipulate global or session-wide variables stored in <code>cq_vardata</code> and <code>cq_varstrdata</code> respectively.</p>

  <h4>📌 Type 191 – Numeric Variables (<code>cq_vardata</code>)</h4>
  <p>Used to store and manipulate numbers like counters, flags, or progress markers.</p>

  <pre>
-- Add +1 to variable index 3
INSERT INTO cq_action VALUES (1000, 0000, 0, 191, 0, 'var(3) += 1');

-- Set variable index 3 to 999
INSERT INTO cq_action VALUES (1000, 0000, 0, 191, 0, 'var(3) set 999');

-- Conditional check: If var(3) equals 100
INSERT INTO cq_action VALUES (1000, 1001, 1002, 191, 0, 'var(3) == 100');
  </pre>

  <ul>
    <li>Supports: <code>set</code>, <code>+=</code>, <code>/=</code>, <code>*=</code>, <code>==</code></li>
    <li>For conditional checks, use <code>id_nextfail</code> for failure path</li>
  </ul>

  <p>📥 <strong>Access value in dialog:</strong> <code>%public_var_data3</code></p>

  <h5>📤 Show numeric value to player (example):</h5>
  <pre>
-- Display value of public_var_data3
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, '%public_var_data3');
  </pre>


  <h4>📌 Type 192 – Text/String Variables (<code>cq_varstrdata</code>)</h4>
  <p>Used to store names, states, or any string-based logic values.</p>

  <pre>
-- Set string var(3) to "Hello"
INSERT INTO cq_action VALUES (1000, 0000, 0, 192, 0, 'var(3) set Hello');

-- Check if var(3) is "Hello"
INSERT INTO cq_action VALUES (1000, 1001, 1002, 192, 0, 'var(3) == Hello');
  </pre>

  <ul>
    <li>Supports: <code>set</code>, <code>==</code></li>
    <li>Use <code>id_nextfail</code> for string mismatch handling</li>
  </ul>

  <p>📥 <strong>Access value in dialog:</strong> <code>%public_var_str3</code></p>

  <h5>📤 Show string value to player (example):</h5>
  <pre>
-- Display value of public_var_str3
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, '%public_var_str3');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 101</code> or <code>125</code> for dynamic storytelling or debugging purposes.
  </div>
</details>


<details>
  <summary>🛠️ <strong>Check or Modify NPC Attributes (Type 201 & 206)</strong></summary>
  <br>

  <p><strong>Type 201</strong> and <strong>Type 206</strong> are used to <u>check or modify NPC attributes</u> such as <code>data</code>, <code>datastr</code>, <code>life</code>, <code>lookface</code>, and more. These can apply to both <code>cq_npc</code> and <code>cq_dynanpc</code> tables.</p>

  <h4>🔁 Type 201 — Check or Modify (Same Thread)</h4>

  <p><strong>Type 201</strong> is used when the NPC is within the same map or thread. It can check values, compare, or perform operations like +=, pass, etc. This is commonly used to control quest flow or internal NPC states.</p>

  <h5>📜 Example – Conditional Check:</h5>
  <pre>
-- Check if npc.data3 >= 21
INSERT INTO cq_action VALUES (1000, 1001, 1002, 0201, 0, 'data3 >= 21 20257');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your data3 is more than 21.');
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Your data3 is less than 21.');
  </pre>

  <h5>📜 Example – Modify data field:</h5>
  <pre>
-- Decrease data1 by 1
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0201, 0, 'data1 += -1 20362');
  </pre>

  <h5>🧠 Syntax:</h5>
  <ul>
    <li><code>attr opt value npc_id</code> (e.g., <code>data3 >= 21 20257</code>)</li>
    <li><strong>attr:</strong> data0~3, datastr, life, maxlife, ownerid, ownertype, lookface</li>
    <li><strong>opt:</strong> =, ==, +=, >=, <=, >, <, pass</li>
    <li><strong>value:</strong> numerical value or string depending on attr</li>
  </ul>

  <h5>✅ Supported Attributes:</h5>
  <ul>
    <li><code>data0 ~ data3</code>, <code>datastr</code></li>
    <li><code>life</code>, <code>maxlife</code></li>
    <li><code>ownerid</code>, <code>ownertype</code>, <code>lookface</code></li>
    <li><strong>New Engine only:</strong> <code>newdata1 ~ newdata8</code>, <code>newdatastr1 ~ newdatastr2</code></li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>pass</code> to adjust date/time values ,e.g., <code>data2 pass +3</code> increases DateStamp by 3 days (Just theory never seen in database).
  </div>

  <h4>✏️ Type 206 — Modify Only (Cross Thread)</h4>

  <p><strong>Type 206</strong> is used when the target NPC is outside the current execution thread (e.g. different map or unrelated script). It can only be used to directly assign values using <code>=</code> (no checking, no math).</p>

  <h5>📜 Example:</h5>
  <pre>
-- Set data0 of npc_id 10018 to 1
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0206, 0, '10018 data0 = 1');
  </pre>

  <h5>🧠 Syntax:</h5>
  <ul>
    <li><code>npc_id attr = value</code></li>
    <li>Only supports <strong>=</strong> operator</li>
  </ul>

  <h5>⚙️ Supported Attributes:</h5>
  <ul>
    <li><code>data0 ~ data3</code>, <code>datastr</code>, <code>lookface</code></li>
    <li><strong>New Engine only:</strong> <code>newdata1 ~ newdata8</code>, <code>newdatastr1 ~ newdatastr2</code></li>
  </ul>

  <h5>📜 New Engine Example:</h5>
  <pre>
-- Set newdatastr1 to HelloWorld
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0206, 0, '10018 newdatastr1 = HelloWorld');
  </pre>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Note:</strong> Type 206 is limited to <code>=</code> only. Old Engine only supports <code>data</code> fields (data0~3).
  </div>

  <div style="border-left: 4px solid #03A9F4; background: #e1f5fe; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Reminder:</strong> For both <code>cq_npc</code> and <code>cq_dynanpc</code>, only <code>datastr</code>, <code>newdatastr1</code>, and <code>newdatastr2</code> can store letters and numbers (A–Z, 0–9). All other fields must use numeric values only.
  </div>

</details>


<details>
  <summary>🧹 <strong>Delete Dynamic NPC (Type 205)</strong></summary>
  <br>

  <p><strong>Type 205</strong> is used to delete a dynamic NPC from the map. It only works on NPCs created in <code>cq_dynanpc</code> — not static NPCs from <code>cq_npc</code>. Once deleted, the NPC is permanently removed and cannot be used in any further action steps.</p>

  <h5>📜 Basic Example:</h5>
  <pre>
-- Delete current dynamic NPC
INSERT INTO cq_action VALUES (8410200, 8410201, 0000, 0205, 0, '');
  </pre>

  <h5>📌 Important Behavior:</h5>
  <ul>
    <li>Only applies to <code>cq_dynanpc</code> (dynamic NPCs)</li>
    <li>Must be placed as the <strong>last</strong> line in the action chain</li>
    <li>Any actions listed <strong>after</strong> a 0205 will <u>not</u> be executed</li>
  </ul>

  <h5>🧠 Syntax Options:</h5>
  <ul>
    <li><strong>data ≠ 0:</strong> Deletes all dynamic NPCs with matching <code>type</code> in the <strong>current map</strong></li>
    <li><strong>param:</strong> Can be used to delete by <code>map_id type</code> pair</li>
  </ul>

  <h5>📜 Map-Wide Delete by Type:</h5>
  <pre>
-- Delete all dynanpc with type = 999 in current map
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0205, 999, '');
  </pre>

  <h5>📜 Delete by Map + Type (New Engine):</h5>
  <pre>
-- Delete NPCs of type 888 from map 1002
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0205, 0, '1002 888');
  </pre>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Warning:</strong> Once this action runs, the dynamic NPC is permanently deleted. Do <u>not</u> add further actions after a <code>0205</code> or they will never run.
  </div>

</details>


<details>
  <summary>⚔️ <strong>Check Legion War Time (Type 226)</strong></summary>
  <br>

  <p><strong>Type 226</strong> checks whether the current time is within the configured Legion War period. It's used to control war-based logic like teleportation, dialogue conditions, or reward distribution.</p>

  <h4>🧠 Behavior:</h4>
  <ul>
    <li>If war is currently ongoing → goes to <code>id_next</code></li>
    <li>If not within Legion War period → goes to <code>id_nextfail</code></li>
  </ul>

  <h4>📜 Example SQL:</h4>
  <pre>
-- Check if it's Legion War time
REPLACE INTO cq_action VALUES (1001, 1002, 1003, 0226, 0, '');

-- If war is on
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Legion War is already start');

-- If war is off
REPLACE INTO cq_action VALUES (1003, 0000, 0000, 0126, 0, 'Legion war not started yet');
  </pre>

  <h4>📌 Use Cases:</h4>
  <ul>
    <li>NPC war entry control</li>
    <li>Trigger different dialogs based on war state</li>
    <li>Prevent or allow map teleportation</li>
  </ul>

  <h4>💡 Tip:</h4>
  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; border-radius: 6px;">
    Combine with <code>type 126</code> to give user-friendly feedback, or teleport (type 1006) if war is active.
  </div>
</details>


<details>
  <summary>🗝️ <strong>Clear Warehouse Password (Type 228, 229, 1058)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p>These actions manage player warehouse passwords, including checks and clearance. Used for secure item storage systems.</p>

  <h4>🛡️ Type 228 – Check Warehouse Lock Status</h4>
  <p><strong>Purpose</strong>: Verifies if the warehouse is password-locked before clearance.</p>
  <p><strong>Behavior</strong>:</p>
  <ul>
    <li>If locked → proceeds to <code>id_next</code> (clear password with <strong>229</strong>).</li>
    <li>If unlocked → jumps to <code>id_nextfail</code> (error message).</li>
  </ul>

  <h4>📜 Example SQL:</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0228, 0, '');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Warehouse not locked!');
  </pre>

  <h4>🗝️ Type 229 – Clear Warehouse Password</h4>
  <p><strong>Purpose</strong>: Removes the warehouse password (requires prior <strong>228</strong> check).</p>
  <p><strong>Behavior</strong>:</p>
  <ul>
    <li>No parameters needed. Always follows <strong>228</strong>'s <code>id_next</code>.</li>
  </ul>

  <h4>📜 Example SQL:</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1001, 1003, 0000, 0229, 0, '');
  </pre>

  <h4>✅ Type 1058 – Confirm Clearance</h4>
  <p><strong>Purpose</strong>: Validates if the password was cleared successfully.</p>
  <p><strong>Behavior</strong>:</p>
  <ul>
    <li>If success → <code>id_next</code> (confirmation message).</li>
    <li>If failure → <code>id_nextfail</code> (error message).</li>
  </ul>

  <h4>📜 Example SQL:</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1003, 1004, 1005, 1058, 0, '');
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'Password cleared!');
REPLACE INTO `cq_action` VALUES (1005, 0000, 0000, 0126, 0, 'Clearance failed.');
  </pre>

  <h4>🔗 Full Workflow Example</h4>
  <pre>
-- 1. Check lock status
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0228, 0, '');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'No password set.');

-- 2. Clear password
REPLACE INTO `cq_action` VALUES (1001, 1003, 0000, 0229, 0, '');

-- 3. Confirm result
REPLACE INTO `cq_action` VALUES (1003, 1004, 1005, 1058, 0, '');
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'Success!');
REPLACE INTO `cq_action` VALUES (1005, 0000, 0000, 0126, 0, 'Failed.');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Chain with <code>type 126</code> for user feedback. Use <code>id_nextfail</code> rigorously for error handling.
  </div>
</details>

<details>
  <summary>🛒 <strong>Exchange Store System (Type 250)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>This system allows players to exchange items via NPCs using Type 250 actions and the <code>cq_exchangegoods</code> table.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius: 6px;">
    ⚠️ <strong>Important:</strong> 
    <ul>
      <li>Type 250 can only be triggered by NPCs, not item packages</li>
      <li>The <code>data</code> value must always be <strong>601</strong> for exchange shop interface</li>
    </ul>
  </div>

  <h4>🖼️ Exchange Store Interface</h4>
  <img src="assets/images/250-601.png" style="max-width:100%; border-radius:8px; margin:10px 0; border:1px solid #ddd;">

  <h4>📜 Type 250 – Open Exchange Store</h4>
  <p><strong>Purpose</strong>: Opens the exchange store interface.</p>
  <p><strong>Parameters</strong>:</p>
  <ul>
    <li><code>data</code>: Fixed value <strong>601</strong> (required for interface)</li>
    <li><code>param</code>: Unused (leave empty)</li>
  </ul>

  <h4>📜 Example NPC Dialog Chain:</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (80000000, 80000001, 0, 0101, 0, 'Welcome honored guest. I can exchange various');
REPLACE INTO `cq_action` VALUES (80000001, 80000002, 0, 0101, 0, 'robes for you. A well-crafted robe can enhance');
REPLACE INTO `cq_action` VALUES (80000002, 80000003, 0, 0101, 0, 'your charm. Would you like to choose one?');
REPLACE INTO `cq_action` VALUES (80000003, 80000004, 0, 0102, 0, 'I~want~to~exchange~items. 80000007');
REPLACE INTO `cq_action` VALUES (80000004, 80000005, 0, 0102, 0, 'No~thanks. 0');
REPLACE INTO `cq_action` VALUES (80000005, 80000006, 0, 0104, 0, '10 10 10');
REPLACE INTO `cq_action` VALUES (80000006, 0, 0, 0120, 0, '');
REPLACE INTO `cq_action` VALUES (80000007, 0, 0, 0250, 601, '');
  </pre>

  <h4>📦 cq_exchangegoods Table Structure</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>id</code></td>
        <td style="padding:8px;">Unique entry ID</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>ownerid</code></td>
        <td style="padding:8px;">NPC ID (from <code>cq_npc</code> or <code>cq_dynanpc</code>)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>itemtype</code></td>
        <td style="padding:8px;">Item ID to give</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>price</code></td>
        <td style="padding:8px;">Quantity of <code>needitem</code> required</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>needitem</code></td>
        <td style="padding:8px;">Item ID required for exchange</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>maxcount</code></td>
        <td style="padding:8px;">Daily exchange limit (-1 = unlimited)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>RetainField</code></td>
        <td style="padding:8px;">Unused (set to 0)</td>
      </tr>
    </tbody>
  </table>

  <h4>📜 Sample Exchange Data</h4>
  <pre>
INSERT INTO `cq_exchangegoods` VALUES
(0001, 1044, 753231, 18, 1020157, -1, 0),  -- Unlimited exchanges
(0002, 1044, 753232, 8, 1025644, -1, 0),
(0003, 1044, 753233, 12, 1025644, -1, 0),
(0004, 1044, 753234, 18, 1020157, -1, 0),
(0005, 1044, 753235, 8, 1025644, -1, 0),
(0006, 1044, 753236, 12, 1025644, -1, 0);
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tips:</strong>
    <ul>
      <li>Stock resets daily at 24:00 or server restart</li>
      <li>Use <code>maxcount=-1</code> for unlimited exchanges</li>
      <li>All exchange shops use interface ID <strong>601</strong></li>
      <li><code>ownerid</code> must match an existing NPC ID</li>
    </ul>
  </div>
</details>




<details>
  <summary>🚚 <strong>Move NPC (Type 301)</strong></summary>
  <br>

  <p>This action moves static NPCs (from <code>cq_npc</code>) to specified map coordinates or hides them by moving to (0,0).</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius: 6px;">
    ⚠️ <strong>Important:</strong> 
    <ul>
      <li>Only works with <code>cq_npc</code> (not dynamic NPCs from <code>cq_dynanpc</code>)</li>
      <li>Moving to (0,0) effectively hides the NPC</li>
    </ul>
  </div>

  <h4>📜 Type 301 Parameters</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Usage</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>data</code></td>
        <td style="padding:8px;">NPC ID (from <code>cq_npc</code>)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>param</code></td>
        <td style="padding:8px;"><code>"map_id posX posY"</code> coordinates</td>
      </tr>
    </tbody>
  </table>

  <h4>📜 Example SQL</h4>
  <h5>Move NPC 2678 to map 1000 at (174,388):</h5>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0301, 2678, '1000 174 388');
  </pre>

  <h5>Hide NPC 2678 (move to 0,0):</h5>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0301, 2678, '1000 0 0');
  </pre>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Create NPCs that "travel" between locations</li>
    <li>Hide/show NPCs for time-limited events</li>
    <li>Reposition NPCs after map changes</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 123</code> (time check) for NPCs that appear/disappear at specific times.
  </div>
</details>


<details>
  <summary>👥 <strong>Map User Check (Type 302)</strong></summary>
  <br>

  <p>This action checks player counts on specified maps, with special handling for large map IDs (>2.1 billion) in New Engine.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius: 6px;">
    ⚠️ <strong>New Engine Only:</strong> The extended format (data=0) is only available in New Engine versions.
  </div>

  <h4>📜 Standard Parameters (data ≠ 0)</h4>
  <pre>
INSERT INTO `cq_action` VALUES (1000, 1001, 1002, 0302, 8764, 'alive_user == 1');
INSERT INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'There are 1 user in that map');
INSERT INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'There are 0 or more than 1 user in that map');
  </pre>
  <table style="width:100%; border-collapse:collapse; font-size:14px; margin-bottom:16px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Field</th>
        <th style="text-align:left; padding:8px;">Value</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>data</code></td>
        <td style="padding:8px;">Map ID (from <code>cq_map</code>)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>param</code></td>
        <td style="padding:8px;"><code>"cmd opt quantity"</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>cmd</code></td>
        <td style="padding:8px;"><code>map_user</code> (all users) or <code>alive_user</code> (non-ghost users)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>opt</code></td>
        <td style="padding:8px;"><code>==</code>, <code><=</code>, or <code>>=</code></td>
      </tr>
    </tbody>
  </table>

  <h4>📜 Extended Parameters (data=0) - New Engine Only</h4>
  <pre>
INSERT INTO `cq_action` VALUES (1000, 1001, 1002, 0302, 0, '1200 map_user <= 0');
INSERT INTO `cq_action` VALUES (1000, 1001, 1002, 0302, 0, '4070000001 map_user <= 0'); -- instance map
INSERT INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'There are no user in that map');
INSERT INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'There are user in that map');
  </pre>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Field</th>
        <th style="text-align:left; padding:8px;">Value</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>data</code></td>
        <td style="padding:8px;">Must be <code>0</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>param</code></td>
        <td style="padding:8px;"><code>"map_id cmd opt quantity"</code></td>
      </tr>
    </tbody>
  </table>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Restrict access to maps when crowded (<code>map_user >= 50</code>)</li>
    <li>Trigger events when first player enters (<code>alive_user == 1</code>)</li>
    <li>Create solo-instance maps (<code>map_user <= 0</code> check before teleport)</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 1006</code> (teleport) to create player-limited zones.<br>
    <strong>Old Engine:</strong> Only supports standard format (data ≠ 0) with map IDs ≤ 2.1 billion
  </div>
</details>


<details>
  <summary>📢 <strong>Map Broadcast Message (Type 303)</strong></summary>
  <br>

  <p>Sends a broadcast message to all players on a specific map.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius: 6px;">
    ⚠️ <strong>Important:</strong> 
    <ul>
      <li><code>data</code> field requires a valid map ID</li>
      <li>Messages are only visible to players on the specified map</li>
    </ul>
  </div>

  <h4>🖼️ Example Output</h4>
  <img src="assets/images/303.png" style="max-width:100%; border-radius:8px; margin:10px 0; border:1px solid #ddd;">

  <h4>📜 Type 303 Parameters</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Usage</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>data</code></td>
        <td style="padding:8px;">Target map ID</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>param</code></td>
        <td style="padding:8px;">Message text</td>
      </tr>
    </tbody>
  </table>

  <h4>📜 Example SQL</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0303, 1000, 'Congratulations! %user_name has combined a Super Amber.');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Note:</strong> For global announcements, use <code>type 125</code> instead.
  </div>
</details>



<details>
  <summary>🎁 <strong>Map Drop Item (Type 304)</strong></summary>
  <br>

  <p>Spawns items at specified map coordinates with optional lifetime and protection periods.</p>

  <h4>📜 Standard Parameters</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0304, 0, '1010030 1000 492 320');
  </pre>

  <h4>📜 New Engine Extended Parameters</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0304, 0, '1010030 1000 492 320 1800 900');
  </pre>

  <h4>📦 Parameter Breakdown</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Parameter</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>idItemType</code></td>
        <td style="padding:8px;">Item ID to spawn</td>
        <td style="padding:8px;">1010030</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>idMap</code></td>
        <td style="padding:8px;">Target map ID</td>
        <td style="padding:8px;">1000</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>nPosX nPosY</code></td>
        <td style="padding:8px;">Spawn coordinates</td>
        <td style="padding:8px;">492 320</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>alivesecs</code></td>
        <td style="padding:8px;">Item lifetime (seconds)<br><em>New Engine only</em></td>
        <td style="padding:8px;">1800 (30 mins)</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>priv_secs</code></td>
        <td style="padding:8px;">Protection period (seconds)<br><em>New Engine only</em></td>
        <td style="padding:8px;">900 (15 mins)</td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>New Engine Behavior:</strong>
    <ul>
      <li>When <code>alivesecs</code> and <code>priv_secs</code> are 0 or empty, uses server defaults</li>
      <li>Old Engine only use Standard Parameters</li>
    </ul>
  </div>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Event item spawns (holiday gifts, rare drops)</li>
    <li>Quest objective items</li>
    <li>Boss fight rewards</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 303</code> to announce important item spawns.
  </div>
</details>


<details>
  <summary>🗺️ <strong>Map Attribute Control (Type 306)</strong></summary>
  <br>

  <p>Checks or modifies specific map attributes. Supports conditional checks and direct modifications.</p>

  <h4>📜 Basic Syntax</h4>
  <pre>param = "field opt data [idMap]"</pre>

  <h4>📜 Example</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0306, 0, 'mapdoc == 8773');
  </pre>

  <h4>📦 Supported Operations</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Operations</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>synid</code></td>
        <td style="padding:8px;">==, =</td>
        <td style="padding:8px;">Synarchy ID operations</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>status</code></td>
        <td style="padding:8px;">test, set, reset</td>
        <td style="padding:8px;">Map status flags</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>type</code></td>
        <td style="padding:8px;">test</td>
        <td style="padding:8px;">Map type check</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>res_lev</code></td>
        <td style="padding:8px;">=, ==, &lt;</td>
        <td style="padding:8px;">Resource level</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>mapdoc</code></td>
        <td style="padding:8px;">=, ==</td>
        <td style="padding:8px;">Map document ID</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>portal0_x</code></td>
        <td style="padding:8px;">=</td>
        <td style="padding:8px;">Main portal X coordinate</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>portal0_y</code></td>
        <td style="padding:8px;">=</td>
        <td style="padding:8px;">Main portal Y coordinate</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>castle</code></td>
        <td style="padding:8px;">==</td>
        <td style="padding:8px;">Castle association</td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Notes:</strong>
    <ul>
      <li>When <code>idMap</code> is omitted, affects current map</li>
      <li>Use <code>id_nextfail</code> for conditional failure paths</li>
    </ul>
  </div>

  <h4>🔧 Practical Examples</h4>
  <pre>
-- Check map document ID
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0306, 0, 'mapdoc == 8773');

-- Modify resource level
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0306, 0, 'res_lev = 3 1002');

-- Test castle association
REPLACE INTO `cq_action` VALUES (1002, 1003, 1004, 0306, 0, 'castle == 5');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 1006</code> for conditional map teleports.
  </div>
</details>



<details>
  <summary>👀 <strong>Region Monitoring (Types 307,322,323)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Checks entity counts within specific map regions with different detection methods.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius: 6px;">
    ⚠️ <strong>Note:</strong> Type 307 references are scarce - recommended to use 322/323 instead.
  </div>

  <h4>📜 Type 322 - Circular Area Check</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0322, 1, '1000 527 361 5 > 1');
  </pre>
  <p><strong>Parameters</strong>: <code>mapid x y radius > value</code></p>
  <ul>
    <li><code>data=1</code>: Exclude ghost players</li>
    <li>Radius defines circular detection area</li>
    <li>Supports <code>></code>, <code>==</code>, <code>>=</code></li>
  </ul>

  <h4>📜 Type 323 - Rectangular Area Check</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0323, 1, '1000 106 406 12 12 > 3');
  </pre>
  <p><strong>Parameters</strong>: <code>mapid x y width height > value</code></p>
  <ul>
    <li>Similar to Type 322 but rectangular area</li>
    <li>Width/Height define detection rectangle</li>
  </ul>

  <h4>📦 Parameter Comparison</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Type</th>
        <th style="text-align:left; padding:8px;">Area Shape</th>
        <th style="text-align:left; padding:8px;">Ghost Handling</th>
        <th style="text-align:left; padding:8px;">Operators</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">322</td>
        <td style="padding:8px;">Circle (radius)</td>
        <td style="padding:8px;"><code>data=0/1</code></td>
        <td style="padding:8px;">&gt;, ==, &gt;=</td>
      </tr>
      <tr>
        <td style="padding:8px;">323</td>
        <td style="padding:8px;">Rectangle (w/h)</td>
        <td style="padding:8px;"><code>data=0/1</code></td>
        <td style="padding:8px;">&gt;, ==, &gt;=</td>
      </tr>
    </tbody>
  </table>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Boss room player limits</li>
    <li>Event participation checks</li>
    <li>Safe zone monitoring</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Implementation Guide:</strong>
    <ol>
      <li>Use 322 for radial detection (e.g. spawn points)</li>
      <li>Use 323 for zone-based checks (e.g. rectangular arenas)</li>
      <li>Set <code>data=1</code> for alive-player-only events</li>
    </ol>
  </div>
</details>


<details>
  <summary>✨ <strong>Map Effects (Type 312)</strong></summary>
  <br>

  <p>Displays visual effects at specified map coordinates. Supports hundreds of built-in effect animations.</p>

  <h4>📜 Basic Syntax</h4>
  <pre>param = "idMap x y EffectName"</pre>

  <h4>📜 Example</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0312, 0, '2010 325 438 fire555');
  </pre>

  <h4>📦 Parameter Breakdown</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Parameter</th>
        <th style="text-align:left; padding:8px;">Description</th>
        <th style="text-align:left; padding:8px;">Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>idMap</code></td>
        <td style="padding:8px;">Target map ID</td>
        <td style="padding:8px;">2010</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>x y</code></td>
        <td style="padding:8px;">Effect coordinates</td>
        <td style="padding:8px;">325 438</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>EffectName</code></td>
        <td style="padding:8px;">Predefined effect identifier</td>
        <td style="padding:8px;">fire555</td>
      </tr>
    </tbody>
  </table>

  <h4>🎆 Effect Names (Refer ini/3deffect.ini inside your client)</h4>
  <div style="column-count:3; column-gap:20px; font-size:13px;">
    <ul style="margin-top:0;">
      <li>fire555</li>
      <li>skill281</li>
      <li>CoolPose03</li>
      <li>chgdamage</li>
      <li>pet31</li>
      <li>gem16</li>
      <li>bless</li>
      <li>zfgx06</li>
      <li>FF09</li>
      <li>FF15</li>
    </ul>
    <ul>
      <li>skill335</li>
      <li>gcyh10</li>
      <li>zfgx01</li>
      <li>pilidan</li>
      <li>gcyh9</li>
      <li>other12</li>
      <li>pet40</li>
      <li>skill090</li>
      <li>FF14</li>
      <li>HeroGem</li>
    </ul>
    <ul>
      <li>skill323</li>
      <li>callpet05</li>
      <li>skill064</li>
      <li>monster02</li>
      <li>noconfirm001</li>
      <li>PetEvo</li>
      <li>skill156</li>
      <li>FF19</li>
      <li>zhongqiu1</li>
      <li>organ08</li>
    </ul>
  </div>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Note:</strong> 
    <ul>
      <li>Effects are client-side only</li>
      <li>Duration depends on effect type</li>
      <li>Coordinates are map-specific</li>
    </ul>
  </div>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Quest completion markers</li>
    <li>Event decorations</li>
    <li>Boss spawn animations</li>
    <li>Treasure location highlights</li>

  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 303</code> for coordinated effect+announcement combos.
  </div>
</details>

<details>
  <summary>🗺️ <strong>Map Variables (Types 316,317,318,319)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Manipulates persistent variables tied to specific maps, supporting both numeric and text data.</p>

  <h4>📜 Type 316 - Initialize Numeric Variable</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0316, 0, 'map_var(1000,10) set 0');
  </pre>
  <p><strong>Format</strong>: <code>map_var(map_id,index) set value</code></p>
  <ul>
    <li>Initializes variable to 0 (or specified value)</li>
    <li>Variables: <code>%map_iter_var_data{index}</code></li>
  </ul>

  <h4>📜 Type 317 - Modify/Compare Numeric Variable</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0317, 0, 'map_var(1200,0) += 1');
  </pre>
  <p><strong>Operations</strong>: <code>= == += -= *= /=</code></p>

  <h4>📜 Type 318 - Compare Text Variable</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0318, 0, 'map_var(%user_map_id,2) == Tree');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'This is Tree');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'This is not Tree');
  </pre>
  <p><strong>Note</strong>: Case-insensitive comparison</p>

  <h4>📜 Type 319 - Initialize Text Variable</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0319, 0, 'map_var(1000,5) set %user_name');
  </pre>
  <p><strong>Variables</strong>: <code>%map_iter_var_str{index}</code></p>

  <h4>📦 Variable Index Range</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Type</th>
        <th style="text-align:left; padding:8px;">Index Range</th>
        <th style="text-align:left; padding:8px;">Variable Format</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">Numeric</td>
        <td style="padding:8px;">0-19</td>
        <td style="padding:8px;"><code>%map_iter_var_data{index}</code></td>
      </tr>
      <tr>
        <td style="padding:8px;">Text</td>
        <td style="padding:8px;">0-19</td>
        <td style="padding:8px;"><code>%map_iter_var_str{index}</code></td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Special Map IDs:</strong>
    <ul>
      <li><code>%user_map_id</code> - Current player's map</li>
      <li>Numeric IDs reference <code>cq_map</code></li>
    </ul>
  </div>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Map-specific event counters</li>
    <li>Boss spawn conditions</li>
    <li>Dynamic puzzle states</li>
    <li>Player progress tracking</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 201</code> (NPC attributes) for complex map behaviors.
  </div>
</details>


<details>
  <summary>💉 <strong>Monster HP Check (Type 325)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Retrieves current or max HP of monsters on a map and stores in variables.</p>

  <h4>📜 Basic Syntax</h4>
  <pre>param = "mapmonsterhp(map_id,monster_type_id,var_index,var_type,hp_mode)"</pre>

  <h4>📦 Parameter Breakdown</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Parameter</th>
        <th style="text-align:left; padding:8px;">Description</th>
        <th style="text-align:left; padding:8px;">Values</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>map_id</code></td>
        <td style="padding:8px;">Target map ID</td>
        <td style="padding:8px;">Number or <code>%user_map_id</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>monster_type_id</code></td>
        <td style="padding:8px;">Monster ID from <code>cq_monstertype</code></td>
        <td style="padding:8px;">e.g. 26171</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>var_index</code></td>
        <td style="padding:8px;">Variable storage slot</td>
        <td style="padding:8px;">0-99</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>var_type</code></td>
        <td style="padding:8px;">Variable scope</td>
        <td style="padding:8px;">
          0 = Personal (<code>%iter_var_data#</code>)<br>
          1 = Global (<code>%public_var_data#</code>)
        </td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>hp_mode</code></td>
        <td style="padding:8px;">HP retrieval mode</td>
        <td style="padding:8px;">
          0 = Current HP<br>
          1 = Max HP
        </td>
      </tr>
    </tbody>
  </table>

  <h4>👤 Personal Variable Examples</h4>
  <pre>
-- Check current HP
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0325, 0, 'mapmonsterhp(%user_map_id,26171,3,0,0)');
REPLACE INTO `cq_action` VALUES (1001, 0, 0, 0126, 0, 'HP:%iter_var_data3');
REPLACE INTO `cq_action` VALUES (1002, 0, 0, 0126, 0, 'Get failed, the monster does not exist or HP is 0');

-- Check max HP
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0325, 0, 'mapmonsterhp(%user_map_id,26171,3,0,1)');
REPLACE INTO `cq_action` VALUES (1001, 0, 0, 0126, 0, 'Max HP:%iter_var_data3');
REPLACE INTO `cq_action` VALUES (1002, 0, 0, 0126, 0, 'Get failed, the monster does not exist or max HP is 0');
  </pre>

  <h4>🌍 Global Variable Examples</h4>
  <pre>
-- Check current HP
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0325, 0, 'mapmonsterhp(%user_map_id,26171,3,1,0)');
REPLACE INTO `cq_action` VALUES (1001, 0, 0, 0126, 0, 'HP:%public_var_data3');
REPLACE INTO `cq_action` VALUES (1002, 0, 0, 0126, 0, 'Get failed, the monster does not exist or HP is 0');

-- Check max HP
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0325, 0, 'mapmonsterhp(%user_map_id,26171,3,1,1)');
REPLACE INTO `cq_action` VALUES (1001, 0, 0, 0126, 0, 'Max HP:%public_var_data3');
REPLACE INTO `cq_action` VALUES (1002, 0, 0, 0126, 0, 'Get failed, the monster does not exist or max HP is 0');
  </pre>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Behavior Notes:</strong>
    <ul>
      <li>Returns <code>false</code> if monster doesn't exist or is dead</li>
      <li>May return 0 even when successful</li>
      <li>Global variables don't require player context</li>
    </ul>
  </div>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Boss health percentage displays</li>
    <li>Event triggers at specific HP thresholds</li>
    <li>Dynamic difficulty adjustment</li>
    <li>Raid mechanics implementation</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 317</code> to create HP-based event counters.
  </div>
</details>

<details>
  <summary>🗑️ <strong>Delete Current Item (Type 498)</strong></summary>
  <br>

  <p>Deletes the item that triggered the action chain. Requires specifying the ItemTypeID and should be the last action in a sequence.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius:6px;">
    ⚠️ <strong>Critical:</strong> 
    <ul>
      <li><code>data</code> field <strong>must</strong> contain the ItemTypeID</li>
      <li>Always place this action <strong>last</strong> in your chain</li>
      <li>Only deletes if the current item matches the specified ID</li>
    </ul>
  </div>

  <h4>📜 Strict Syntax</h4>
  <pre>data = ItemTypeID
param = "" (empty)</pre>

  <h4>📜 Valid Examples</h4>
  <pre>
-- Delete current item (verified)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0498, 729011, '');

-- Full item exchange workflow:
REPLACE INTO `cq_action` VALUES (1001, 1002, 0000, 0501, 500002, '1 1 2 0 0 0 0 0 200 0 0 0'); -- Give reward
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 1010, 2005, 'You received Eudemon Crystal.'); -- Announce
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0498, 729011, ''); -- Delete original item (LAST!)
  </pre>

  <div style="border-left: 4px solid #f44336; background: #ffebee; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ❌ <strong>Invalid Usage:</strong>
    <pre>REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0498, 0, ''); -- ERROR: Missing ItemTypeID</pre>
  </div>

  <h4>🔧 Required Workflow</h4>
  <ol>
    <li>Process rewards/effects</li>
    <li>Show notifications</li>
    <li><strong>Finally</strong> delete source item with type 498</li>
  </ol>
</details>

<details>
  <summary>🔄 <strong>Multi-Item Operations (Types 506 & 507)</strong></summary>
  <br>

  <p>Handles batch operations for ranges of items with binding status control.</p>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-bottom: 16px; border-radius:6px;">
    ⚠️ <strong>Note:</strong> 
    <ul>
      <li>Supports item ranges (ID1 to ID2)</li>
      <li>Old Engine only supports <code>data=0</code></li>
    </ul>
  </div>

  <h4>📜 Type 507 - Multi-Item Check</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0507, 0, '743032 743032 10');
  </pre>
  <p><strong>Parameters</strong>: <code>"start_id end_id quantity"</code></p>

  <h4>📜 Type 506 - Multi-Item Delete</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1001, 1003, 0000, 0506, 0, '743032 743032 10');
  </pre>

  <h4>📦 Binding Status Control (data field)</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;"><code>data</code></th>
        <th style="text-align:left; padding:8px;">Behavior</th>
        <th style="text-align:left; padding:8px;">Engine Support</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">0</td>
        <td style="padding:8px;">Ignore binding status (delete/check all)</td>
        <td style="padding:8px;">Old & New Engine</td>
      </tr>
      <tr>
        <td style="padding:8px;">1</td>
        <td style="padding:8px;">Delete/check both bound and unbound</td>
        <td style="padding:8px;">New Engine Only</td>
      </tr>
      <tr>
        <td style="padding:8px;">2</td>
        <td style="padding:8px;">Only affect bound items</td>
        <td style="padding:8px;">New Engine Only</td>
      </tr>
    </tbody>
  </table>

  <h4>🔧 Complete Workflow Example</h4>
  <pre>
-- Check for 10 Flower Cards (ID 743032)
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0507, 0, '743032 743032 10');

-- Success path: Delete 10 Flower Cards
REPLACE INTO `cq_action` VALUES (1001, 1003, 0000, 0506, 0, '743032 743032 10');
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'You have given 10 flower cards');

-- Failure path
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Sorry, but you don`t have 10 flower cards');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Advanced Usage:</strong>
    <ul>
      <li>Use identical start/end IDs for single item types</li>
      <li>Combine with <code>type 501</code> for exchange systems</li>
    </ul>
  </div>
</details>




<details>
  <summary>🛡️ <strong>Equipment Check (Type 511)</strong></summary>
  <br>

  <p>Verifies if a player has specific equipment in a particular slot.</p>

  <h4>📜 Basic Syntax</h4>
  <pre>
data = Equipment slot position
param = ItemTypeID to check
  </pre>

  <h4>📜 Example</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0511, 4, '480001');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Weapon equipped');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Weapon not equipped');
  </pre>

  <h4>📦 Equipment Slot Positions</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px;">Position</th>
        <th style="text-align:left; padding:8px;">Value</th>
        <th style="text-align:left; padding:8px;">Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">Helmet</td>
        <td style="padding:8px;">1</td>
        <td style="padding:8px;">Head slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Necklace</td>
        <td style="padding:8px;">2</td>
        <td style="padding:8px;">Amulet/necklace slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Armor</td>
        <td style="padding:8px;">3</td>
        <td style="padding:8px;">Armor slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Weapon</td>
        <td style="padding:8px;">4</td>
        <td style="padding:8px;">Weapon slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Ring</td>
        <td style="padding:8px;">7</td>
        <td style="padding:8px;">Ring slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Shoes</td>
        <td style="padding:8px;">8</td>
        <td style="padding:8px;">Foot slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Casual</td>
        <td style="padding:8px;">12</td>
        <td style="padding:8px;">Casual slot</td>
      </tr>
      <tr>
        <td style="padding:8px;">Wardrobe Toy</td>
        <td style="padding:8px;">38</td>
        <td style="padding:8px;">Toy slot (New Engine)</td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Note:</strong>
    <ul>
      <li>Position 38 (Wardrobe Toy) is New Engine only</li>
      <li>Use <code>id_next</code> for equipped case</li>
      <li>Use <code>id_nextfail</code> for unequipped case</li>
    </ul>
  </div>

  <h4>🔧 Practical Uses</h4>
  <ul>
    <li>Class-specific quest requirements</li>
    <li>Equipment-based skill unlocks</li>
    <li>Cosmetic item verification</li>
    <li>Special event participation checks</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Combine with <code>type 503</code> to check both equipment and inventory items.
  </div>
</details>




<details>
  <summary>👥 <strong>Team Teleport Scroll (Type 517)</strong></summary>
  <br>

  <p>Enables user to teleport to team members location. Requires party membership.</p>

  <h4>📜 Basic Usage</h4>
  <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0517, 0, '');
  </pre>

  <h4>📦 Requirements</h4>
  <ul>
    <li>User must be in a party</li>
    <li>Target teammate must be on same map</li>
    <li>Typically used with item ID 810001 (Portal Scroll)</li>
  </ul>

  <h4>🔧 Full Implementation Example</h4>
  <pre>
-- Portal Scroll item action chain
REPLACE INTO `cq_action` VALUES (1000, 1001, 0, 0517, 0, '');
REPLACE INTO `cq_action` VALUES (1001, 0, 0, 0126, 0, 'Teleported to teammate!');
  </pre>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
    ⚠️ <strong>Behavior Notes:</strong>
    <ul>
      <li>Teleports user to teammate's position</li>
      <li>Consumes the scroll on use</li>
      <li>Some maps may restrict teleportation</li>
    </ul>
  </div>

</details>


<details>
  <summary>🔍 <strong>Identify Item (Type 519)</strong></summary>
  <br>

  <p>Used to identify unknown equipment using a scroll, like the <strong>Equipment Scroll</strong> (ItemID: <code>810004</code>).</p>

  <h4>🧪 Purpose:</h4>
  <ul>
    <li>Removes the “unidentified” status from an item.</li>
  </ul>

  <h4>📜 Example SQL</h4>
  <pre>
-- Use Equipment Scroll (ItemID 810004) to identify an item
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0519, 0, '');
  </pre>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
  <h4>⚙️ Behavior:</h4>
  <ul>
    <li>No `data` or `param` values needed; the system auto-identifies the target item equipped or selected.</li>
  </ul>
  </div>


</details>



<details>
  <summary>🔍 <strong>Identify Eudemons (Type 520)</strong></summary>
  <br>

  <p>Used to identify an Eudemon using an identification scroll, checking its stats and quality. This action is triggered by using the <strong>Eudemon Scroll</strong> (ItemID: <code>810002</code>).</p>

  <h4>🧪 Purpose:</h4>
  <ul>
    <li>Reveals the hidden stats and quality of a Eudemon.</li>
  </ul>

  <h4>📜 Example SQL</h4>
  <pre>
-- Use Eudemon Scroll (ItemID 810002) to identify an Eudemon's stats
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0520, 0, '');
  </pre>


  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
  <h4>⚙️ Behavior:</h4>
  <ul>
    <li>The scroll reveals the Eudemon's characteristics based on pre-defined data in the <code>cq_eudemons</code> table.</li>
  </ul>
  </div>

</details>


<details>
  <summary>🐉 <strong>Create Universal O/XO (Types 526, 527, 530, 531)</strong></summary>
  <br>

  <p>These actions allow players to create Eudemons directly in their inventory. Each type corresponds to different star ratings of Eudemons:</p>

  <h4>🧪 Purpose:</h4>
  <ul>
    <li><code>Type 526</code>: Create a 12-star XO Eudemon directly in the player's inventory.</li>
    <li><code>Type 527</code>: Create an 8-star O Eudemon directly in the player's inventory.</li>
    <li><code>Type 530</code>: Create a 6-star O Eudemon directly in the player's inventory.</li>
    <li><code>Type 531</code>: Create a 19-star XO Eudemon directly in the player's inventory.</li>
  </ul>

  <h4>📜 Full SQL Example for Creating a 12-Star Eudemon</h4>
  <pre>
-- Check if the player has enough space in the inventory (Eudemon bag 53)
REPLACE INTO `cq_action` VALUES (1000, 1001, 1003, 0508, 0, '1 0 53');

-- Create 12-Star XO Eudemon (ID 815038)
REPLACE INTO `cq_action` VALUES (1001, 1002, 0000, 0526, 0, '');

-- Remove the used item (creation item used)
REPLACE INTO `cq_action` VALUES (1002, 1004, 0000, 0502, 815038, '');

-- Notify the player of insufficient space if needed
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'You need 1 free space in your inventory.');

-- Confirm the success of the creation
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'Congratulations! You have obtained a 12-Star UniversalXO!');
  </pre>


  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
  <h4>⚙️ Behavior Notes:</h4>
  <ul>
    <li><strong>Type 503</strong> should be used before this action to check if there is enough space in the player's inventory (bag 53) to hold the Eudemon.</li>
    <li><strong>Type 526-531</strong> actions correspond to different star-level Eudemons. For example, Type 526 creates a 12-star XO Eudemon, and Type 527 creates an 8-star O Eudemon.</li>
    <li><code>Type 0502</code> is used to remove the item (used for the Eudemon creation) after successful creation.</li>
    <li><code>Type 0126</code> provides feedback messages, like informing the player if there is insufficient space or confirming the successful creation of the Eudemon.</li>
  </ul>
  </div>


  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
  <h4>📝 Tips:</h4>
  <ul>
    <li>Ensure the player checks for sufficient inventory space using <code>Type 503</code> before attempting to create the Eudemon.</li>
    <li>The creation action (Type 526-531) will only set the star level of the Eudemon. The <code>Type 503</code> action should handle the check for available space before the creation.</li>
    <li>Use <code>Type 0126</code> to notify the player if their inventory is full, or confirm successful creation with a custom message.</li>
  </ul>
  </div>

</details>




<details>
  <summary>⚡ <strong>Change Eudemon Attribute (Type 523 & 524)</strong></summary>
  <br>

  <p>Used for checking and modifying Eudemon stats. A common example is changing an Eudemon’s elemental type using an item like <strong>Thunder Juice</strong> (ItemID: <code>831002</code>).</p>

  <h4>🧪 Purpose:</h4>
  <ul>
    <li><code>Type 523</code>: Checks multiple conditions related to the Eudemon — ownership, summon status, level, state, etc.</li>
    <li><code>Type 524</code>: Applies a stat change to the Eudemon — such as setting its damage, level, or element. Only one condition per statement allowed.</li>
  </ul>

  <h4>📜 Full SQL Sequence – Use Thunder Juice</h4>
  <pre>
-- Check: must be user's Eudemon, summoned, alive, level 1, and not already thunder
REPLACE INTO `cq_action` VALUES (1000, 1002, 1001, 0523, 0, 'ismyselfeudemon == 1,iscallout == 1,isalive == 1,level == 1,damage != 5');

-- Failure Message: not valid target
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Thunder Juice can only be used on level 1 Eudemons.');

-- Set Eudemon damage element to 5 (Thunder)
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 0524, 0, 'damage 5');

-- Delete the used Thunder Juice item
REPLACE INTO `cq_action` VALUES (1003, 1004, 0000, 0498, 831002, '');

-- Announce to local system message
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 1010, 2005, '%target_name has changed its race to Thunder!');
  </pre>


  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
  <h4>⚙️ Behavior Notes:</h4>
  <ul>
    <li><strong>Type 523:</strong> Allows multiple checks in one param string, separated by commas</li>
    <li><strong>Type 524:</strong> Can only apply <strong>ONE</strong> attribute at a time (e.g., <code>damage 5</code> or <code>level 2</code>)</li>
    <li><code>ismyselfeudemon</code> ensures the target belongs to the player</li>
    <li><code>iscallout</code> ensures the Eudemon is currently summoned</li>
    <li><code>isalive</code> ensures it's not dead or dismissed</li>
    
  </ul>
  </div>

  <h4>📘 Common Conditions for 523:</h4>
  <ul>
    <li><code>ismyselfeudemon == 1</code> → belongs to the player</li>
    <li><code>iscallout == 1</code> → summoned</li>
    <li><code>isalive == 1</code> → not dead</li>
    <li><code>level == 1</code> → required level</li>
    <li><code>damage != 5</code> → not already thunder</li>
    <li><code>godlevel == 1</code> → required level</li>

  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
  <h4>📝 Tips:</h4>
  <ul>
    <li>Always handle <code>id_nextfail</code> for 523 to show rejection reason</li>
    <li>Stack actions for longer flows — check ➝ modify ➝ delete item ➝ show message</li>
  </ul>
  </div>

</details>



<details>
  <summary>🌟 <strong>Create 25-Star XO Eudemon (Type 538)</strong></summary>
  <br>

  <p>This action creates a 25-star XO Eudemon directly on the player's body. The <code>param</code> determines the number of stars, with a value of 0 indicating a 25-star XO Eudemon. Other values can be used to create Eudemons with different star ratings.</p>

  <h4>🧪 Purpose:</h4>
  <ul>
    <li><code>Type 538</code>: Creates a specified star-rated XO Eudemon directly on the player's body.</li>
    <li>When <code>param = 0</code>, a 25-star XO Eudemon is created.</li>
    <li>For other values (e.g., <code>param = 3500</code>), it creates a corresponding number of stars (e.g., 35-star XO Eudemon).</li>
  </ul>

  <h4>📜 Full SQL Example for Creating a 25-Star Eudemon</h4>
  <pre>
-- Check for free space in the player's Eudemon bag (inventory bag 53)
REPLACE INTO `cq_action` VALUES (1000, 1002, 1001, 0508, 0, '1 0 53');

-- Failure Message: not enough space in inventory
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You need 1 free space in your inventory.');

-- Remove 1 used creation item (e.g., Eudemon creation item)
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 0540, 1024838, 'amount += -1');

-- Create 25-star XO Eudemon directly on the player's body
REPLACE INTO `cq_action` VALUES (1003, 1004, 0000, 0538, 0, '2500');

-- Success Message: Notify the player of the Eudemon creation
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'You received 25-star XO Eudemons.');
  </pre>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin:16px 0; border-radius:6px;">
  <h4>⚙️ Behavior Notes:</h4>
  <ul>
    <li><code>Type 0508</code> checks if there is enough space in the player's Eudemon bag (inventory bag 53) before proceeding with the Eudemon creation.</li>
    <li><code>Type 0540</code> removes the used creation item (such as a scroll or other item used to trigger the Eudemon creation).</li>
    <li><code>Type 0538</code> actually creates the Eudemon on the player's body with a specified number of stars (e.g., 25 stars). The param value can be customized to create Eudemons with other star ratings (e.g., <code>param = 3500</code> creates a 35-star XO Eudemon).</li>
    <li><code>Type 0126</code> sends feedback messages to the player — either about inventory space or confirmation of the Eudemon creation.</li>
  </ul>
    </div>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
  <h4>📝 Tips:</h4>
  <ul>
    <li>Always ensure that the player has enough space in their inventory before attempting to create the Eudemon using <code>Type 0508</code>.</li>
    <li>For other star-level Eudemons, modify the <code>param</code> value accordingly (e.g., <code>3500</code> for 35 stars).</li>
    <li>Consider chaining actions with failure conditions using <code>id_nextfail</code> to handle space issues or prevent creation if prerequisites aren’t met.</li>
  </ul>
  </div>

</details>


<details>
  <summary>🎁 <strong>Monster Item Drop (Type 801)</strong></summary>
  <br>

  <p><strong>Type 801</strong> is used to drop items, money, or traps after a monster is killed. The parameters supported are `dropitem`, `dropmoney`, and `droptrap`. You can specify the item type, money amount, or a trap to be dropped. The new engine extension also allows for specifying the quantity for item drops. Below are examples of how to use this action type:</p>

  <h4>💡 Example SQL:</h4>

  <pre>
-- Drop an item with ItemTypeID 131000
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0801, 0, 'dropitem 131000');
          
-- Drop 100,000 money
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0801, 0, 'dropmoney 100000');
          
-- Drop a trap with ID 3000 (trap ID from cq_trap) and life period 30 (seconds)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0801, 0, 'droptrap 3000 30');
          
-- New Engine Only: Drop 3 items of ItemTypeID 723003
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0801, 0, 'dropitem 723003 3');
  </pre>

  <h4>📘 Parameter Explanation:</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Operator</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">dropitem</td>
        <td style="padding:8px;"><code>itemtype</code></td>
        <td style="padding:8px;">Drop a specified item by ItemTypeID</td>
        <td style="padding:8px;">`dropitem 131000` (drop item with ItemTypeID 131000)</td>
      </tr>
      <tr>
        <td style="padding:8px;">dropmoney</td>
        <td style="padding:8px;"><code>money</code></td>
        <td style="padding:8px;">Drop a specified amount of money</td>
        <td style="padding:8px;">`dropmoney 100000` (drop 100,000 money)</td>
      </tr>
      <tr>
        <td style="padding:8px;">droptrap</td>
        <td style="padding:8px;"><code>trap_id lifeperiod</code></td>
        <td style="padding:8px;">Drop a trap with a specified ID (from <code>cq_trap</code>) and a life period</td>
        <td style="padding:8px;">`droptrap 3000 30` (drop trap with ID 3000, with a life period of 30 seconds)</td>
      </tr>
      <tr>
        <td style="padding:8px;">dropitem (New Engine Only)</td>
        <td style="padding:8px;"><code>itemtype quantity</code></td>
        <td style="padding:8px;">Drop a specified quantity of a given item</td>
        <td style="padding:8px;">`dropitem 723003 3` (drop 3 items of ItemTypeID 723003)</td>
      </tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
  <h4>💡 Tips:</h4>
  <ul>
    <li>Use `dropitem` to specify the item you want to drop by its ItemTypeID. For the new engine, you can also specify the quantity of items to drop.</li>
    <li>Use `dropmoney` to drop a specified amount of money after a monster is killed.</li>
    <li>Use `droptrap` to drop a trap with a specified trap ID. The second parameter (`lifeperiod`) defines the duration for which the trap remains active. This is typically in seconds.</li>
    <li>The new engine supports specifying the quantity for item drops with `dropitem` (e.g., `dropitem 723003 3` to drop 3 items).</li>
  </ul>
    </div>

</details>

<details>
  <summary>🔥 <strong>Skill Check/Learn/Upgrade (Type 802)</strong></summary>
  <br>

  <p><strong>Type 802</strong> is used to check, learn, or upgrade player skills (spells). The <code>param</code> supports multiple modes:</p>
  <ul>
    <li><code>check type</code> – Check if the player has learned the skill with type <code>type</code>.</li>
    <li><code>check type level</code> – Check if the player has learned a skill of type <code>type</code> at a specific level.</li>
    <li><code>learn type</code> – Learn a spell of <code>type</code> at level 0.</li>
    <li><code>uplevel type</code> – Upgrade an existing spell of <code>type</code> to the next level.</li>
  </ul>

  <p><strong>Note:</strong> The <code>type</code> value refers to the spell ID from <code>cq_magictype</code>.</p>

  <h4>💡 Example SQL – Check & Learn Skill:</h4>
  <pre>
-- Check if skill 8001 is already learned
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0802, 0, 'check 8001');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 1010, 2005, 'You already learned this skill');

-- Learn the skill 8001 if not yet learned
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 0802, 0, 'learn 8001');
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 1010, 2005, 'You learned a new skill!');
  </pre>

  <h4>💡 Example SQL – Upgrade Existing Skill:</h4>
  <pre>
-- Upgrade skill 3011 to the next level
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0802, 0, 'uplevel 3011');
  </pre>

  <h4>💡 Example SQL – Check Skill Level:</h4>
  <pre>
-- Check if player has learned skill 3011 at level 0
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 0802, 0, 'check 3011 0');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your skill is level 1');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your skill is not level 1');
  </pre>

  <h4>📘 Parameter Modes:</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Format</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Function</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>check type</code></td>
        <td style="padding:8px;">Check if the player has learned this skill</td>
        <td style="padding:8px;"><code>check 8001</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>check type level</code></td>
        <td style="padding:8px;">Check if the skill is at the given level</td>
        <td style="padding:8px;"><code>check 3011 0</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>learn type</code></td>
        <td style="padding:8px;">Learn the skill (adds skill at level 0)</td>
        <td style="padding:8px;"><code>learn 8001</code></td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>uplevel type</code></td>
        <td style="padding:8px;">Increase the spell's level by 1</td>
        <td style="padding:8px;"><code>uplevel 3011</code></td>
      </tr>
    </tbody>
  </table>

  <h4>💡 Tips:</h4>
  <ul>
    <li>Use <code>check</code> before <code>learn</code> to prevent duplicate skills.</li>
    <li>All type values must refer to IDs in <code>cq_magictype</code>.</li>
    <li>Use <code>uplevel</code> carefully — it assumes the player already has the skill.</li>
    <li>Always use <code>id_nextfail</code> when checking skills to show appropriate fallback messages.</li>
  </ul>
</details>


<details>
  <summary>💎 <strong>VIP Level Check & Set (Type 5001)</strong></summary>
  <br>

  <p><strong>Type 5001</strong> checks the player's VIP level or sets it directly (new engine only). You can use this to branch actions depending on the player's VIP status, or upgrade the VIP level programmatically. Supported operators include <code>==</code>, <code>&gt;</code>, <code>&lt;</code>, <code>&gt;=</code>, <code>&lt;=</code>, and <code>=</code> (only in new engine for setting VIP).</p>

  <h4>💡 Example SQL – Check VIP Level:</h4>
  <pre>
-- Check if VIP level is exactly 4
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 5001, 0, '== 4');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You`re vip 4');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You`re not vip 4');

-- Check if VIP is greater than 3
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 5001, 0, '> 3');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your vip is higher than 3');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your vip is less than or equal to 3');

-- Check if VIP is less than 4
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 5001, 0, '< 4');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your vip is less than 4');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your vip is 4 or higher');

-- Check if VIP is 5 or higher
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 5001, 0, '>= 5');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your vip is 5 and above');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your vip is less than 5');
  </pre>

  <h4>⚙️ Example SQL – Set VIP Level (New Engine Only):</h4>
  <pre>
-- Set player's VIP level to 4
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 5001, 0, '= 4');
  </pre>

  <h4>📘 Notes:</h4>
  <ul>
    <li><strong>Checking VIP level</strong>: Use <code>==</code>, <code>&gt;</code>, <code>&lt;</code>, <code>&gt;=</code>, or <code>&lt;=</code> to perform condition checks.</li>
    <li><strong>Setting VIP level</strong>: Use <code>=</code> to directly assign a VIP level to the player. Only works in the <strong>New Engine</strong>.</li>
    <li><strong>Account sync warning:</strong> For VIP assignment to work, <code>account database</code> and <code>player database</code> must be in the same database schema.</li>
    <li>VIP changes won’t be reflected until the player <strong>relogs</strong>.</li>
  </ul>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; border-radius: 6px;">
    ⚠️ <strong>New Engine Info:</strong> Using <code>=</code> to set VIP level is only supported in the new engine.<br>
    Ensure databases are merged and user relogs to see the update.
  </div>
</details>


<details>
  <summary>⚙️ <strong>Eudemon Attributes Check/Delete (Type 1503, 1504)</strong></summary>
  <br>

  <p><strong>Type 1503</strong> is used to check various attributes of a player's summoned Eudemon, such as its star rating, callout status, damage type, and other attributes. <strong>Type 1504</strong> is used for deleting Eudemons, with specific checks related to the summoned Eudemon. The <strong>New Engine</strong> introduces the <code>newtype</code> parameter to more specifically refer to Eudemons, as well as additional support for <code>deskknight</code> and <code>syndicate</code> checks.</p>

  <h4>💡 Example SQL – Check Eudemon Attributes:</h4>
  <pre>

-- Check if Eudemon's damage type is 5 (Thunder) and if it's summoned
REPLACE INTO `cq_action` VALUES (1000, 1001, 1004, 1503, 0, 'damagetype == 5 callout == 1');

-- Check if Eudemon's type is 42 (Warrior Lulu) and if it's summoned
REPLACE INTO `cq_action` VALUES (1001, 1002, 1005, 1503, 0, 'type == 42 callout == 1');

-- Check if Eudemon's type is 42 (Warrior Lulu) and then delete it
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 1504, 0, 'type == 42 callout == 1');

-- Display message if Eudemon has been deleted
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'Your Warrior Lulu has been deleted.');

-- Display message if Eudemon is not of Thunder damage type
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'Your pet is not thunder.');

-- Display message if no Warrior Lulu is summoned
REPLACE INTO `cq_action` VALUES (1005, 0000, 0000, 0126, 0, 'Please summon Warrior Lulu first.');

  </pre>

  <h4>📘 New Engine Modifications:</h4>
  <p><strong>New Engine</strong> introduces <code>newtype</code> to replace <code>type</code>, with the algorithm <code>itemtype / 10</code> to specify the exact Eudemon. Additionally, <code>deskknight == 1</code> and <code>syndicate == 1</code> can be added to check whether the desk knight and the legion totem have been registered.</p>

  <h4>💡 Example SQL – New Engine Modifications: (use , as separator)</h4>
  <pre>
-- Check if Eudemon's star is less than 500.
REPLACE INTO `cq_action` VALUES (1000, 1001, 1004, 1503, 0, 'star > 500,callout == 1');

-- Use newtype (itemtype / 10) for more specific Eudemon type
REPLACE INTO `cq_action` VALUES (1001, 1003, 1005, 1503, 0, 'newtype == 107019,callout == 1');  -- Eudemon with itemtype 107019, newtype = 107019

-- Delete Eudemon of newtype 107019
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 1504, 0, 'newtype == 107019,callout == 1');

-- Display message if Eudemon star is less than 500.
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'Your eudemons is less than 5 star.');

-- Display message if not specific Eudemon type.
REPLACE INTO `cq_action` VALUES (1005, 0000, 0000, 0126, 0, 'Please summon Kong first.');
  </pre>

  <h4>📘 Parameter Explanation:</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Field</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Operator</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Description</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Example</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;">star</td>
        <td style="padding:8px;"><code><, >, ==, >=, <=</code></td>
        <td style="padding:8px;">Check or compare the star level of the Eudemon</td>
        <td style="padding:8px;">`star < 500` (check if star is less than 500)</td>
      </tr>
      <tr>
        <td style="padding:8px;">callout</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check if the Eudemon is summoned</td>
        <td style="padding:8px;">`callout == 1` (check if summoned)</td>
      </tr>
      <tr>
        <td style="padding:8px;">damagetype</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check the damage type of the Eudemon</td>
        <td style="padding:8px;">`damagetype == 5` (check if the damage type is Thunder)</td>
      </tr>
      <tr>
        <td style="padding:8px;">type / newtype</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check the Eudemon type</td>
        <td style="padding:8px;">`type == 42`</td>
      </tr>
            <tr>
        <td style="padding:8px;">newtype (new engine)</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check the Eudemon specific type</td>
        <td style="padding:8px;">`newtype == 107019` (check for a specific Eudemon ID)</td>
      </tr>
      <tr>
        <td style="padding:8px;">deskknight (new engine)</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check if the desk knight is registered</td>
        <td style="padding:8px;">`deskknight == 1`</td>
      </tr>
      <tr>
        <td style="padding:8px;">syndicate (new engine)</td>
        <td style="padding:8px;"><code>==</code></td>
        <td style="padding:8px;">Check if the syndicate (legion totem) is registered</td>
        <td style="padding:8px;">`syndicate == 1`</td>
      </tr>
    </tbody>
  </table>

  <h4>💡 Tips:</h4>
  <ul>
    <li>Use the <code>callout</code> field to check if an Eudemon is currently summoned (1 for summoned, 0 for not summoned).</li>
    <li><strong>New Engine Only</strong>: Use <code>newtype</code> to refer to Eudemons more specifically, calculated as <code>itemtype / 10</code>.</li>
    <li>For <code>damagetype</code>, use 5 for Thunder element Eudemons.</li>
  </ul>
</details>



<details>
  <summary>💫 <strong>Revive Eudemons (Type 1531)</strong></summary>
  <br>

  <p><strong>Type 1531</strong> is used to resurrect dead Eudemons. The behavior of the action is controlled by the <code>data</code> field:</p>

  <h4>💡 Example SQL:</h4>
  <pre>
-- Old Engine: Revive only summoned dead Eudemons
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1531, 0, '');

-- New Engine: Revive all dead Eudemons (including unsummoned ones)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1531, 1, '');
  </pre>

  <h4>📘 Parameter Explanation:</h4>
  <table style="width:100%; border-collapse:collapse; font-size:14px;">
    <thead style="background:#f0f0f0;">
      <tr>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Data</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Engine</th>
        <th style="text-align:left; padding:8px; border-bottom:1px solid #ccc;">Effect</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="padding:8px;"><code>0</code></td>
        <td style="padding:8px;">Old & New</td>
        <td style="padding:8px;">Revives only currently summoned (in-battle) dead Eudemons.</td>
      </tr>
      <tr>
        <td style="padding:8px;"><code>1</code></td>
        <td style="padding:8px;">New Engine Only</td>
        <td style="padding:8px;">Revives all dead Eudemons, including unsummoned ones.</td>
      </tr>
    </tbody>
  </table>

  <h4>💡 Tips:</h4>
  <ul>
    <li>Use <code>data = 0</code> for compatibility with both old and new engines.</li>
    <li>Use <code>data = 1</code> only if your server runs the <strong>new engine</strong> and you want to fully restore all fallen Eudemons.</li>
    <li>Ideal for NPC healers, rebirth rituals, or revival buffs in battle events.</li>
  </ul>
</details>


<details>
  <summary>❤️ <strong>Resurrect Character & Eudemons (Type 1539 & 1540)</strong></summary>
  <br>

  <p><strong>Type 1539</strong> is used to resurrect the player character, but only if the player is dead and the internal cooldown time has passed. <strong>Type 1540</strong> is used to instantly revive all Eudemons, including those not currently summoned, without any time check. Useful for full revives during events or special rewards.</p>

  <h4>💡 Example SQL – Type 1539 (Character Resurrection with Time Check)</h4>
  <pre>
-- Check if player is dead
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1001, 0, 'life == 0');

-- Resurrect character if time condition is met
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 1539, 0, '');
  </pre>

  <h4>📘 Type 1539 Notes</h4>
  <ul>
    <li>Only works if player is dead (<code>life == 0</code>).</li>
    <li>Resurrection only triggers if the cooldown time has passed.</li>
    <li>If not enough time has passed, nothing happens.</li>
  </ul>

  <h4>💡 Example SQL – Type 1540 (Resurrect All Eudemons Instantly)</h4>
  <pre>
-- Instantly revive all Eudemons, summoned or not
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1540, 0, '');
  </pre>

  <h4>📘 Type 1540 Notes</h4>
  <ul>
    <li>Works on all Eudemons, regardless of summon status.</li>
    <li>No cooldown or time check – executes instantly.</li>
    <li>Good for full revive functions (event prize, healer NPC, etc).</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use Type 1539 when resurrection needs to be limited by time.</li>
    <li>Use Type 1540 when you want to revive all Euds at once without checks.</li>
    <li>Can be combined with <code>0126</code> to display messages like "You have been revived!"</li>
  </ul>
</details>

<details>
  <summary>🚚 <strong>Delivery System (Type 1511,1512,1513)</strong></summary>
  <br>

  <p>These action types control the log delivery system using a cart (style 991–994). Players can begin a delivery run with a specified wood quantity, unit price, and time limit. During delivery, teleporting is restricted. Delivery ends when the task is complete or aborted manually.</p>

  <h4>💡 Example SQL – Start Delivery (Type 1511)</h4>
  <pre>
-- Start delivery: 1000 logs, 100 gold each, 3600 seconds (1 hour), cart style 991
REPLACE INTO `cq_action` VALUES (1001, 1004, 1003, 1511, 991, '1000 100 3600');

-- Player can't buy logs due to flying status or insufficient gold
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'Unable to buy these logs when you are flying or don`t have enough gold.');

-- Delivery start confirmation message
REPLACE INTO `cq_action` VALUES (1004, 0000, 0000, 0126, 0, 'You must deliver 1,000 logs to the destination within one hour.');
  </pre>

  <h4>💡 Example SQL – End Delivery (Type 1512)</h4>
  <pre>
-- End the delivery run manually or upon completion
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1512, 0, '');
  </pre>

  <h4>💡 Example SQL – Check Delivery Status (Type 1513)</h4>
  <pre>
-- Check if the player is currently in delivery
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1513, 0, '');

-- Player is delivering, block teleport
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Sorry, you can`t be teleported during delivery.');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><strong>1511</strong> – Begins delivery with cart style and parameters:
      <ul>
        <li><code>woodquantity</code> – Logs to deliver (e.g. 1000)</li>
        <li><code>unitprice</code> – Gold per log (e.g. 100)</li>
        <li><code>endtime</code> – Time limit in seconds (e.g. 3600 = 1 hour)</li>
      </ul>
    </li>
    <li><strong>1512</strong> – Ends the delivery mission.</li>
    <li><strong>1513</strong> – Checks if delivery is in progress (blocks certain actions).</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Cart styles are 991, 992, 993, 994 – use different IDs for cosmetic or logical variation.</li>
    <li>Delivery can be interrupted if the player teleports; use <code>1513</code> to block such actions.</li>
    <li>Always display clear messages using <code>0126</code> so players know delivery status or failure reasons.</li>
  </ul>
</details>

<details>
  <summary>🪓 <strong>Life Skills (Type 1514)</strong></summary>
  <br>

  <p><strong>Type 1514</strong> manages life skills such as Delivery Skill (ID 300). It supports:</p>
  <ul>
    <li><code>learn</code> – To learn a new life skill</li>
    <li><code>check</code> – To check if the player has the skill, or verify its level</li>
    <li><code>addexp</code> – To increase skill EXP</li>
  </ul>

  <h4>💡 Example SQL – Learn Delivery Skill</h4>
  <pre>
-- Learn delivery skill (ID 300)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1514, 0, 'learn 300');
  </pre>

  <h4>💡 Example SQL – Check Skill Level</h4>
  <pre>
-- Check if skill 300 is level 9
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1514, 0, 'check 300 9');
  </pre>

  <h4>💡 Example SQL – Add EXP to Skill</h4>
  <pre>
-- Add 1000 EXP to skill 300
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1514, 0, 'addexp 300 1000');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>learn [id]</code> – Learns the specified life skill.</li>
    <li><code>check [id] [level]</code> – Verifies if the player has the skill at that level. Level can be skipped to just check if learned.</li>
    <li><code>addexp [id] [amount]</code> – Adds EXP to that skill (triggers level-up if thresholds met).</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>check</code> before allowing delivery quests or crafting tasks.</li>
    <li>EXP gain can be from quests, farming, or delivery success. Use <code>addexp</code> after events.</li>
    <li><code>TrainReward</code> was supported in older engines but i dont know how it works.</li>
  </ul>
</details>


<details>
  <summary>💥 <strong>Check Current Number of Summoned Eudemons (Type 1515)</strong></summary>
  <br>

  <p><strong>Type 1515</strong> is used to check how many Eudemons the player currently has summoned. The action checks the value of <code>callout_num</code>, which represents the number of Eudemons currently summoned. Based on the value, the system can return different messages.</p>

  <h4>💡 Example SQL – Check Number of Summoned Eudemons</h4>
  <pre>
-- Check if the player has fewer than 1 summoned Eudemon
REPLACE INTO `cq_action` VALUES (1000, 1003, 1004, 1515, 0, 'callout_num < 1');

-- Check if the player has fewer than 2 summoned Eudemons
REPLACE INTO `cq_action` VALUES (1001, 1003, 1004, 1515, 0, 'callout_num < 2');

-- Check if the player has fewer than 3 summoned Eudemons
REPLACE INTO `cq_action` VALUES (1002, 1003, 1004, 1515, 0, 'callout_num < 3');

-- Player message if not all Eudemons are summoned
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'Please summon all eudemons first!');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><strong>callout_num</strong> – Represents the number of Eudemons currently summoned by the player.</li>
    <li><strong>Operators:</strong> <code><</code>, <code>==</code>, <code>></code></li>
    <li><strong>Message</strong> – Shows a message based on the number of summoned Eudemons.</li>
  </ul>

  <h4>📘 Old Engine Notes</h4>
  <ul>
    <li><strong>Old engine only supports:</strong> <code>callout_num == 1</code> to check if exactly 1 Eudemon is summoned.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>callout_num < 1</code> or similar checks to ensure that players have summoned the required number of Eudemons for an event or task.</li>
    <li>Make sure to show clear messages with <code>0126</code> to notify players if they need to summon additional Eudemons.</li>
    <li>Old engines won’t support the comparison with <code>callout_num < 2</code> or higher. Use <code>callout_num == 1</code> for such checks in older engines.</li>
  </ul>
</details>

<details>
  <summary>⏳ <strong>Experience Time for Level Up (Type 1518)</strong></summary>
  <br>

  <p>Type 1518 is used to add experience time for leveling up. The experience is calculated based on 432 seconds (1 minute) of monster killing experience multiplied by the monster's experience multiplier.</p>

  <p>The <code>data</code> parameter in this action i cant figure it out what is this:</p>

  <h4>💡 Example SQL – Old Engine (Values: 1, 2, 3)</h4>
  <pre>
-- Add 4500 seconds of experience (1 minute = 432 seconds)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1518, 3, 'uplevtime += 4500');

-- Add 304 seconds of experience
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1518, 2, 'uplevtime += 304');

-- Add 1800 seconds of experience
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1518, 1, 'uplevtime += 1800');
  </pre>

  <h4>💡 Example SQL – New Engine (Only Value 2)</h4>
  <pre>
-- Add 77760 seconds of experience in the new engine
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1518, 2, 'uplevtime += 77760');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>uplevtime</code> – The experience time added for leveling up. It is calculated in seconds, and can be modified based on the experience multiplier for the monster type.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Ensure that the time increments match your desired gameplay pacing. If you want faster leveling, adjust the time added in each <code>uplevtime</code> action accordingly.</li>
  </ul>
</details>

<details>
  <summary>💪 <strong>Fill Player's Attributes (Type 1002)</strong></summary>
  <br>

  <p><strong>Type 1002</strong> is used to fill up a player's attributes, such as their life and mana. You can specify which attribute to fill by passing <code>life</code> or <code>mana</code> as the parameter. This action can be used to instantly restore the player's life or mana to full.</p>

  <h4>💡 Example SQL – Restore Life</h4>
  <pre>
-- Restore player's life
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1002, 0, 'life');
  </pre>

  <h4>💡 Example SQL – Restore Mana</h4>
  <pre>
-- Restore player's mana
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1002, 0, 'mana');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>life</code> – Fills the player's life attribute to full.</li>
    <li><code>mana</code> – Fills the player's mana attribute to full.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>life</code> and <code>mana</code> to instantly restore the respective attributes during battle or events.</li>
    <li>Ensure to handle both attributes separately if you want to use them independently in different conditions.</li>
  </ul>
</details>


<details>
  <summary>🌍 <strong>Change Map (Type 1003)</strong></summary>
  <br>

  <p><strong>Type 1003</strong> is used to change the player's map. It requires the map ID, the X and Y coordinates, and optionally a flag to allow prison exit. By default, players cannot exit the prison, but setting the <code>bPrisonChk</code> parameter to 1 allows them to do so.</p>

  <h4>💡 Example SQL – Change Map</h4>
  <pre>
-- Change map to ID 1000, position (280, 411), allowing prison exit (bPrisonChk = 1)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1003, 0, '1000 280 411 1');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>idMap</code> – The ID of the target map to change to.</li>
    <li><code>nPosX</code> – The X-coordinate for the new position on the map.</li>
    <li><code>nPosY</code> – The Y-coordinate for the new position on the map.</li>
    <li><code>bPrisonChk</code> – Optional parameter:
      <ul>
        <li><code>0</code> – Cannot exit the prison (default behavior).</li>
        <li><code>1</code> – Allows the player to exit the prison.</li>
      </ul>
    </li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Ensure that the target map ID and coordinates are valid and accessible for the player.</li>
    <li>If you want to allow players to exit the prison, remember to set <code>bPrisonChk = 1</code> to enable this feature.</li>
  </ul>
</details>

<details>
  <summary>📍 <strong>Record Map Point (Type 1004)</strong></summary>
  <br>

  <p><strong>Type 1004</strong> is used to record the player's current position on the map, allowing them to return to this point later. The parameters include the map ID, X-coordinate, and Y-coordinate.</p>

  <h4>💡 Example SQL – Record Map Point</h4>
  <pre>
-- Record the player's position on map ID 1000 at coordinates (316, 467)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1004, 0, '1000 316 467');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>idMap</code> – The ID of the map where the point is being recorded.</li>
    <li><code>nMapX</code> – The X-coordinate for the recorded point.</li>
    <li><code>nMapY</code> – The Y-coordinate for the recorded point.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this action to create checkpoints for players, which they can return to later in the game.</li>
    <li>Ensure that the map ID and coordinates are valid and reachable for the player.</li>
  </ul>
</details>

<details>
  <summary>🌍 <strong>Change Map to Recorded Point (Type 1006)</strong></summary>
  <br>

  <p><strong>Type 1006</strong> is used to change the player's map to a previously recorded point. This action helps players quickly travel back to a saved location. Make sure to first set the checkpoint using <code>Type 1004</code> before attempting to use this action.</p>

  <h4>💡 Example SQL – Change Map to Recorded Point</h4>
  <pre>
-- Change to the recorded map point (ID 1000030)
REPLACE INTO `cq_action` VALUES (1000030, 0000, 0000, 1006, 0, '');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>idMap</code> – The map ID of the recorded point to travel to.</li>
  </ul>

  <h4>📘 Important Note</h4>
  <ul>
    <li>Make sure to set the checkpoint first using <code>Type 1004</code> before using <code>Type 1006</code> to change maps to that recorded point.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Ensure that the recorded map point ID is valid and that the player has access to that location.</li>
    <li>Use this action to quickly return to important locations within the game, such as dungeons, towns, or personal checkpoints.</li>
  </ul>
</details>



<details>
  <summary>👤 <strong>Change Player Avatar (Type 1008)</strong></summary>
  <br>

  <p><strong>Type 1008</strong> is used to change the player's avatar. The parameter for this action is the ID of the new avatar that the player wants to switch to.</p>

  <h4>💡 Example SQL – Change Player Avatar</h4>
  <pre>
-- Change the player's avatar to ID 84
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1008, 0, '84');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>id</code> – The avatar ID to change to. This ID corresponds to a specific avatar in the game.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Ensure that the avatar ID is valid and corresponds to an existing avatar.</li>
    <li>This action can be used for character customization or special events where avatars are changed.</li>
  </ul>
</details>


<details>
  <summary>🔎 <strong>Check Player Status (Type 1009)</strong></summary>
  <br>

  <p><strong>Type 1009</strong> is used to check whether the player is currently in a specific state. It simply checks whether a status exists (i.e. whether the <code>seconds</code> remaining is greater than 0), without needing to calculate time ranges or expiration.</p>

  <h4>💡 Example SQL – Check EXP Boost State</h4>
  <pre>
-- Check if the player is under EXP boost (state 158)
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1009, 0, '158 second > 0');

-- If true, show message
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You are currently under an EXP boost.');

-- If false, show alternate message
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You are not under an EXP boost.');
  </pre>

  <h4>💡 Example SQL – Check Drop Rate Boost (State 889)</h4>
  <pre>
-- Check if Drop Rate Boost (State 889) is active
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1009, 0, '889 second > 0');

-- If active
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You are already in Drop Rate Boost status.');

-- If not active, apply 10x drop boost for 20s
REPLACE INTO `cq_action` VALUES (1002, 1003, 0000, 4001, 0, '889 1000 20 0');
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'You have received 10x drop boost for 20 seconds.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>[state_id] second > 0</code> – Returns true if the player is in that status</li>
  </ul>

  <h4>📘 Common State IDs</h4>
  <ul>
    <li><code>158</code> – EXP Boost</li>
    <li><code>43</code> – EXP Boost</li>
    <li><code>164</code> – Dragon Morph</li>
    <li><code>184</code> – Flying</li>
    <li><code>166</code> – Delivery Log</li>
    <li><code>889</code> – Drop Rate Boost</li>
    <li><code>248</code>, <code>249</code>, <code>1014</code> – Unknown</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this to enable or restrict content (e.g. flying-only access, EXP boost bonuses).</li>
    <li>Works well with <code>0126</code> to provide user feedback.</li>
    <li>Can be used with <code>Type 4001</code> to apply statuses conditionally.</li>
  </ul>
</details>


<details>
  <summary>🧑 <strong>Check Player Avatar (Type 1014)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p>Type 1014 is used to check whether the player is currently wearing a specific avatar. This is useful for avatar-based conditions such as quest checks, special dialogue, or restricted access features.</p>

  <h4>💡 Example SQL – Check if Player is Wearing Avatar ID 45</h4>
  <pre>
-- Check if the player is equipped with avatar ID 45
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1014, 0, 'ChkFace 45');

-- If true, show message
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You equip this avatar.');

-- If false, show alternate message
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You are not equip this avatar.');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>ChkFace [id]</code> – Checks if the current player face (avatar) matches the given ID.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this check for unlocking avatar-only content, special access, or triggering rewards for wearing specific looks.</li>
    <li>Use with <code>0126</code> to show clear feedback to the player.</li>
    <li>Can be combined with avatar change logic from <code>Type 1008</code>.</li>
  </ul>
</details>



<details>
  <summary>✨ <strong>God Blessing Status (Type 1012)</strong></summary>
  <br>

  <p>Type 1012 is used to manage the "God Blessing" status on a player. You can check if the blessing is active, or apply it using either <code>add</code> or <code>addonly</code>. The second parameter is always <code>1</code> (purpose unknown, just required). The last parameter is the duration in hours.</p>

  <h4>💡 Example SQL – Addonly God Blessing (30 days)</h4>
  <pre>
-- Adds blessing only if the player does not already have it (720 hours = 30 days)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1012, 0, 'addonly 1 720');
  </pre>

  <h4>💡 Example SQL – Add God Blessing (1 day)</h4>
  <pre>
-- Adds blessing regardless of current status (24 hours = 1 day)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1012, 0, 'add 1 24');
  </pre>

  <h4>💡 Example SQL – Check Blessing Status</h4>
  <pre>
-- Check if player has God Blessing active
REPLACE INTO `cq_action` VALUES (1001, 1002, 1003, 1012, 0, 'check 1 0');

-- If not active
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You don\'t have blessing on you.');

-- If active
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0126, 0, 'You have blessing on you.');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>add</code> – Adds the blessing, even if one already exists. (just my theory)</li>
    <li><code>addonly</code> – Adds the blessing only if none currently exists. (just my theory)</li>
    <li><code>check</code> – Verifies if the blessing is active.</li>
    <li><code>1</code> – Always used in second position (actual meaning unknown, but required).</li>
    <li><code>duration</code> – Length of the blessing in hours (e.g. 24 = 1 day, 720 = 30 days).</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>addonly</code> if you want to avoid refreshing or overwriting an existing blessing.</li>
    <li>Use <code>add</code> if you want to forcibly apply or refresh the blessing regardless of status. </li>
    <li>Use <code>check</code> with branching to show different messages depending on whether the blessing is active.</li>
    <li>Combine with <code>0126</code> messages to notify players when blessings are applied or missing.</li>
  </ul>
</details>


<details>
  <summary>🌀 <strong>Skill Control (Type 1020)</strong></summary>
  <br>

  <p>Type 1020 is used to manage player skills. It supports checking, learning, leveling up, and adding EXP to specific magic skills. The skill type refers to the skill ID from <code>cq_magictype</code>.</p>

  <h4>💡 Example SQL – Learn Skill</h4>
  <pre>
-- Learn skill with ID 33001 (level 1)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1020, 0, 'learn 33001');

-- Learn skill 33001 at level 3 directly
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1020, 0, 'learn 33001 2');
  </pre>

  <h4>💡 Example SQL – Check Skill</h4>
  <pre>
-- Check if player has learned skill ID 1008
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1020, 0, 'check 1008');

-- If learned
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You already learn this skill');

-- If not learned
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You not learn this skill');
  </pre>

  <h4>💡 Example SQL – Add EXP to Skill</h4>
  <pre>
-- Add 5000 EXP to skill ID 5305
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1020, 0, 'addexp 5305 5000');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>check [type]</code> – Checks if the player has learned the skill ID.</li>
    <li><code>check [type] [level]</code> – Checks if the player has the skill at a specific level.</li>
    <li><code>learn [type]</code> – Teaches the player a new skill (defaults to level 0).</li>
    <li><code>learn [type] [level]</code> – Teaches a skill at the specified level.</li>
    <li><code>uplevel [type]</code> – Increases the level of an existing skill by 1.</li>
    <li><code>addexp [type] [exp]</code> – Adds experience points to the specified skill.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>check</code> before <code>learn</code> to avoid teaching duplicate skills.</li>
    <li>Use <code>uplevel</code> to simulate skill mastery over time or quests.</li>
    <li><code>addexp</code> is useful for skill training systems or battle rewards.</li>
  </ul>
</details>

<details>
  <summary>📝 <strong>GM Log with Player Info (Type 1022)</strong></summary>
  <br>

  <p>Type 1022 is used to write custom log entries to the GM log. This log is saved to the <code>MSGServer/gmlog</code> directory. The log message supports placeholders that automatically insert the triggering player's information such as name, ID, location, item, and NPC data.</p>

  <h4>💡 Example SQL – Log Player Action</h4>
  <pre>
-- Log player info, map, item, and NPC during action
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1022, 0, '%user_name[%user_id] Location(%user_map_id %user_map_x %user_map_y) ITEM(%item_type %target_name) NPC(%name %id %npc_x %npc_y) actionid = 930000070 param = e_money += 54000');
  </pre>

  <h4>📘 Placeholder Variables</h4>
  <ul>
    <li><code>%user_name</code> – Player's name</li>
    <li><code>%user_id</code> – Player's ID</li>
    <li><code>%user_map_id</code> – Current map ID</li>
    <li><code>%user_map_x</code> – Player's X position</li>
    <li><code>%user_map_y</code> – Player's Y position</li>
    <li><code>%item_type</code> – Item type ID</li>
    <li><code>%target_name</code> – Item or target name</li>
    <li><code>%name</code> – NPC name</li>
    <li><code>%id</code> – NPC ID</li>
    <li><code>%npc_x</code> – NPC X coordinate</li>
    <li><code>%npc_y</code> – NPC Y coordinate</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this for logging sensitive events like item rewards, purchases, quest completions, or suspicious actions.</li>
    <li>Placeholders are replaced automatically by the system at runtime based on the triggering player and context.</li>
    <li>The message is saved in <code>MSGServer/gmlog</code> and can be reviewed by server staff.</li>
  </ul>
</details>


<details>
  <summary>💍 <strong>Marriage & Divorce System (Type 1024 & 1025)</strong></summary>
  <br>

  <p>These action types are used to manage player marriage status in the game. Type 1025 checks if the player is married, while Type 1024 is used to perform a divorce action.</p>

  <h4>💡 Example SQL – Check Marriage Status (Type 1025)</h4>
  <pre>
-- Check if the player is married
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1025, 0, '');

-- If not married
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You not married');

-- If already married
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You already married');
  </pre>

  <h4>💡 Example SQL – Divorce (Type 1024)</h4>
  <pre>
-- Perform divorce
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1024, 0, '');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li>Type 1025 – Checks the marriage status of the player and branches based on result.</li>
    <li>Type 1024 – Executes a divorce and resets the marriage data of the player.</li>
    <li>No additional parameters are needed for either action.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>Type 1025</code> before allowing marriage-related actions like teleportation or gifting.</li>
    <li>Use <code>Type 1024</code> for NPC-based divorce options or special item effects.</li>
    <li>Pair with message actions (<code>0126</code>) to clearly inform the player of their status.</li>
  </ul>
</details>


<details>
  <summary>🚻 <strong>Gender Check (Type 1026)</strong></summary>
  <br>

  <p>Type 1026 is used to check the player's gender. It branches using <code>id_next</code> for male and <code>id_nextfail</code> for female. This is useful for displaying gender-specific messages or giving gender-specific items like casual outfits.</p>

  <h4>💡 Example SQL – Basic Gender Check</h4>
  <pre>
-- Check if player is male or female
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1026, 0, '');

-- Male
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You are male');

-- Female
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You are female');
  </pre>

  <h4>💡 New Engine Special Case – Give Gendered Casual by Class</h4>
  <p>In the new engine, the <code>Star Guardian</code> class (ID 110) uses different casual outfits. You need to check class first, then gender, before giving the appropriate item.</p>

  <pre>
-- Check if class is Star Guardian (profession 110)
REPLACE INTO `cq_action` VALUES (1000, 1001, 1010, 1001, 0, 'profession == 110');

-- Star Guardian gender check
REPLACE INTO `cq_action` VALUES (1001, 1002, 1003, 1026, 0, '0');

-- Male Star Guardian gets item 198500
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0501, 198500, 0);

-- Female Star Guardian gets item 198520
REPLACE INTO `cq_action` VALUES (1003, 0000, 0000, 0501, 198520, 0);

-- Other class gender check
REPLACE INTO `cq_action` VALUES (1010, 1011, 1012, 1026, 0, '0');

-- Male (other class) gets item 194600
REPLACE INTO `cq_action` VALUES (1011, 0000, 0000, 0501, 194600, 0);

-- Female (other class) gets item 194620
REPLACE INTO `cq_action` VALUES (1012, 0000, 0000, 0501, 194620, 0);
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li>No param needed or just <code>0</code> as a placeholder.</li>
    <li><code>id_next</code> → Male</li>
    <li><code>id_nextfail</code> → Female</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Always check <code>profession</code> before gender if giving class-specific items.</li>
    <li>Use <code>Type 0501</code> to give items based on gender.</li>
    <li>This action is useful for casuals, titles, class-locked visuals, and gender-exclusive quests.</li>
  </ul>
</details>


<details>
  <summary>🎆 <strong>Apply / Remove Visual Effects (Type 1027 & 1060)</strong></summary>
  <br>

  <p>Type 1027 applies a visual special effect to the player or target. The effect can be visible to everyone, only the target, or only the triggering player. The effect name must match one defined in <code>ini/3deffect.ini</code>.</p>

  <p>Type 1060 is used to remove effects applied by Type 1027. It uses the same format but clears the visual from the client. This is supported in <strong>new engine only</strong>.</p>

  <h4>💡 Example SQL – Apply Basic Effects</h4>
  <pre>
-- Show effect to yourself (others can see)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1027, 0, 'self skill307');

-- Show effect to target (others can see)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1027, 0, 'target flower2');

-- Show effect to target (only target sees it)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1027, 0, 'targetlone flower-red');
  </pre>

  <h4>💡 Example SQL – New Engine Only: Self-Only Advanced Effects</h4>
  <pre>
-- Show chained effect only to self
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1027, 0, 'selflone shfbqqskill_spexx100');
  </pre>

  <h4>💡 Example SQL – Remove Effect (New Engine Only)</h4>
  <pre>
-- Remove previously applied effect from self
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1060, 0, 'self shfbqqskill_spsxx100');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>opt effect</code> – Two-part value: visibility type and effect name</li>
    <li><code>opt</code> options:
      <ul>
        <li><code>self</code> – Player sees the effect (public)</li>
        <li><code>target</code> – Target sees the effect (public)</li>
        <li><code>targetlone</code> – Only target sees the effect</li>
        <li><code>selflone</code> – Only the triggering player sees the effect</li>
      </ul>
    </li>
    <li><code>effect</code> – The effect ID or name as listed in <code>ini/3deffect.ini</code></li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>Type 1060 is only supported in new engine.</li>
    <li>To remove effects, use the same format as Type 1027 but with script type 1060.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>self</code> or <code>target</code> for effects visible to others.</li>
    <li>Use <code>selflone</code> or <code>targetlone</code> for personal/secret visuals.</li>
    <li>Type 1060 is useful to clear buffs, transform visuals, or temporary animations.</li>
  </ul>
</details>


<details>
  <summary>🧠 <strong>Task Mask Operations (Type 1028)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p>Type 1028 is used to perform operations on task masks. Task masks are used to track task-related flags using bit values (0–31). The action supports checking, adding, and clearing specific mask bits.</p>

  <h4>💡 Example SQL – Check Task Mask</h4>
  <pre>
-- Check if task mask index 3 is active
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1028, 0, 'chk 3');
  </pre>

  <h4>💡 Example SQL – Add Task Mask</h4>
  <pre>
-- Set task mask index 4 to active
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1028, 0, 'add 4');
  </pre>

  <h4>💡 Example SQL – Clear Task Mask</h4>
  <pre>
-- Clear task mask index 4
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1028, 0, 'clr 4');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>chk idx</code> – Checks if the bit at index <code>idx</code> is set.</li>
    <li><code>add idx</code> – Sets the bit at index <code>idx</code> to 1 (mark as complete).</li>
    <li><code>clr idx</code> – Clears the bit at index <code>idx</code> to 0 (reset).</li>
    <li><code>idx</code> – The task mask index (range: 0 to 31).</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>chk</code> with <code>id_next</code> and <code>id_nextfail</code> to branch based on task progress.</li>
    <li>Task masks are ideal for tracking story flags, special rewards, one-time events, or hidden triggers.</li>
    <li>Be consistent with index usage to avoid conflict between unrelated systems.</li>
  </ul>
</details>


<details>
  <summary>🔊 <strong>Media Playback (Type 1029)</strong></summary>
  <br>

  <p>Type 1029 is used to trigger media playback on the client side. You can use it to play sound or video files that exist in the client directory. The param supports <code>play</code> or <code>broadcast</code> followed by the path to the media file.</p>

  <h4>💡 Example SQL – Play Sound File</h4>
  <pre>
-- Play a sound file (trap.wav) located in the "sound" folder of the client
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1029, 0, 'play sound/trap.wav 0 0 1');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>opt media_path [0 0 1]</code></li>
    <li><code>opt</code> – Action type:
      <ul>
        <li><code>play</code> – Play the media for the triggering player only.</li>
        <li><code>broadcast</code> – Broadcast the media to all nearby players (if supported).</li>
      </ul>
    </li>
    <li><code>media_path</code> – Path to the media file in the client folder (e.g. <code>sound/trap.wav</code>).</li>
    <li>The remaining <code>0 0 1</code> values are typically reserved or unused in most cases, but must be included for format compatibility.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Make sure the media file exists in the specified path on the client, or the effect won't play.</li>
    <li>Use <code>play</code> for personalized sound effects (e.g. when opening a chest).</li>
    <li>Use <code>broadcast</code> if you want the effect shared with other players in the area (new engine required).</li>
    <li>Best used for immersive effects, alert sounds, or cinematic cutscenes.</li>
  </ul>
</details>


<details>
  <summary>🃏 <strong>Checkout Eudemon Point Cards (Type 1032 & 1037)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p>These actions are used to convert Eudemon cards from the <code>cq_card</code> or <code>cq_card2</code> table into Eudemon Points (EP). When triggered, the system checks the player's card table, converts available entries to EP, sends the points to the player, and updates the card entry's <code>used</code> column to <code>1</code> (used).</p>

  <p><code>cq_card</code> cards are worth <strong>270 EP</strong> each.<br>
  <code>cq_card2</code> cards are worth <strong>1380 EP</strong> each.</p>

  <h4>💡 Example SQL – Type 1032 (cq_card: 270 EP each)</h4>
  <pre>
-- Checkout cq_card and convert to EP if exists
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1032, 0, '');

-- Success message
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You`ve taken out a Eudemon Card. Your Eudemon Points increased.');

-- Fail message
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You don`t have a Eudemon Card here.');
  </pre>

  <h4>💡 Example SQL – Type 1037 (cq_card2: 1380 EP each)</h4>
  <pre>
-- Checkout cq_card2 and convert to EP if exists
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1037, 0, '');

-- Success message
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You`ve taken out a Eudemon Card. Your Eudemon Points increased.');

-- Fail message
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You don`t have a Eudemon Card here.');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li>No additional param needed for either type.</li>
    <li>The system checks for valid cards in the player’s <code>cq_card</code> or <code>cq_card2</code> table.</li>
    <li>Each valid entry will:
      <ul>
        <li>Be converted into EP based on the card type (270 or 1380 EP)</li>
        <li>Be marked as used by setting <code>used = 1</code> in the database</li>
      </ul>
    </li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>1032</code> for standard card redemption (270 EP).</li>
    <li>Use <code>1037</code> for higher-tier card redemption (1380 EP).</li>
    <li>Always provide a clear message using <code>0126</code> to inform the player whether the card was redeemed.</li>
  </ul>
</details>

<details>
  <summary>🧹 <strong>Delete Multiple Skills (Type 1039)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>


  <p>Type 1039 is used to permanently delete one or more skills from the player. This type accepts up to 20 skill IDs in the param field. Once a skill is removed using this action, it cannot be restored at its previous level — if the player relearns it, the skill will start again from level 1.</p>

  <h4>💡 Example SQL – Forget Multiple Skills</h4>
  <pre>
-- Permanently remove multiple skills from the player
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1039, 0, '7010 7015 7002 7008 7000 7012 7013 7004 7003 6000 6004 6002 6001 6005 6003 6006 6040 6007 6008 6009');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>type1 type2 ... type20</code> – A space-separated list of up to 20 skill type IDs (from <code>cq_magictype</code>) to be permanently removed.</li>
  </ul>

  <h4>📘 Behavior Notes</h4>
  <ul>
    <li>Skills removed using this action are <strong>not recoverable</strong>.</li>
    <li>If the player relearns the skill using <code>learn</code>, it will restart at <code>level 0</code>.</li>
    <li>This is useful for skill resets, class transitions, or rebirth systems.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this type only when you want a clean skill wipe — no backups, no memory of previous levels.</li>
    <li>Combine with <code>0126</code> messages to alert the player before and after removal.</li>
    <li>Consider using <code>1020 uplevel</code> or <code>addexp</code> afterward if you want to selectively regrant skills at a different progression pace.</li>
  </ul>
</details>

<details>
  <summary>🌐 <strong>Open Web Page (Type 1041)</strong></summary>
  <br>

  <p>Type 1041 is used to notify the client to open a web page in the user's browser. This is useful for directing players to event pages, registration forms, donation portals, patch notes, or community links.</p>

  <h4>💡 Example SQL – Open External Web Page</h4>
  <pre>
-- Open Knightfall Online website
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1041, 0, 'https://knightfall-online.com');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>param</code> – The full URL of the web page to open, including <code>http://</code> or <code>https://</code>.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Make sure the link is properly formatted and accessible by the client browser.</li>
    <li>Ideal for use in NPCs, item interactions, or system announcements that lead players to web-based content.</li>
    <li>Can be used to open custom help guides, vote links, or real-time event announcements.</li>
  </ul>
</details>


<details>
  <summary>🧨 <strong>Drop Magic Skills (Type 1044)</strong></summary>
  <br>

  <p>Type 1044 is used to delete specific magic skills from a player, typically during rebirth, skill reset, or class change. You can specify up to 20 skill type IDs in the parameter (for new engine). Skills removed this way are permanent and will restart from level 0 if relearned.</p>

  <h4>💡 Example SQL – Drop Multiple Skills (New Engine)</h4>
  <pre>
-- Remove multiple skills during rebirth
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1044, 0, '17400 17100 17300 17000 18500');
  </pre>

  <h4>💡 Example SQL – Drop One Skill (Old Engine)</h4>
  <pre>
-- Old engine supports only 1 skill per line
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1044, 0, '7012');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>type1 type2 ... type20</code> – Space-separated list of skill type IDs (from <code>cq_magictype</code>).</li>
  </ul>

  <h4>📘 Engine Support</h4>
  <ul>
    <li><strong>Old engine:</strong> Only 1 skill type can be listed per action.</li>
    <li><strong>New engine:</strong> Supports up to 20 skill types per line.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use during rebirth, job reset, or special quests that wipe specific abilities.</li>
    <li>Always inform the player with <code>0126</code> messages when deleting important skills.</li>
    <li>Combine with <code>1020 learn</code> or <code>uplevel</code> to rebuild or replace skill sets.</li>
  </ul>
</details>




<details>
  <summary>🪟 <strong>Open UI Interface (Type 1046)</strong></summary>
  <br>

  <p>Type 1046 is used to open a client-side interface (UI dialog) by its ID. The <code>data</code> field determines which interface will appear. Different dialog IDs are supported depending on the engine version.</p>

  <h4>💡 Example SQL – Open Compose Interface</h4>
  <pre>
-- Open dialog ID 37 (Compose UI)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1046, 37, '');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li><code>data</code> – ID of the interface/dialog to open on the client.</li>
  </ul>

  <h4>📘 Supported Dialog IDs</h4>

  <p><strong>Old Engine:</strong></p>
  <pre>
301, 300, 71, 70, 62, 61, 60, 59, 58, 56, 55, 53, 51, 50, 49, 48, 47, 46, 45, 44, 43,
42, 41, 40, 39, 38, 37, 36, 35, 31, 30, 29, 28, 27, 25, 24, 23, 22, 21, 19, 17, 16,
15, 14, 13, 12, 11, 10, 8, 7, 6, 5, 3, 1
  </pre>

  <p><strong>New Engine:</strong></p>
  <pre>
10020, 10002, 10001, 10000, 1041, 927, 918, 917, 701, 602, 501, 421, 420, 419, 418,
415, 412, 410, 301, 300, 122, 121, 81, 80, 79, 74, 72, 60, 59, 58, 56, 55, 53, 49, 
47, 43, 41, 40, 39, 37, 36, 35, 29, 28, 27, 25, 24, 23, 22, 21, 19, 16, 13, 12, 11, 
10, 3, 1
  </pre>

  <h4>💡 Tips</h4>
  <ul>
    <li>Make sure the dialog ID is supported by your engine version.</li>
    <li>For custom UIs, use new engine IDs like <code>10000+</code>.</li>
  </ul>
</details>

<details>
  <summary>🗺️ <strong>Teleport to Main Map Respawn (Type 1052)</strong></summary>
  <br>

  <p>Type 1052 is used to teleport the player to the main map’s respawn point. This action will automatically use the map's default revival location, based on the fields <code>reborn_map</code>, <code>portal0_x</code>, and <code>portal0_y</code> set in <code>cq_map</code> for the current map.</p>

  <h4>💡 Example SQL – Teleport to Main Map Respawn</h4>
  <pre>
-- Teleport to the main map's configured respawn point
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1052, 0, '');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li>No param needed. The system will read <code>reborn_map</code>, <code>portal0_x</code>, and <code>portal0_y</code> from <code>cq_map</code> according to the current map ID.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>This is ideal for auto-respawn or emergency teleport after dungeon runs or failed events.</li>
    <li>Ensure the current map has valid values in <code>reborn_map</code> and portal coordinates in <code>cq_map</code>, or the teleport may fail.</li>
  </ul>
</details>

<details>
  <summary>🎲 <strong>Random Teleport on Current Map (Type 1053)</strong></summary>
  <br>

  <p>Type 1053 randomly teleports the player to any valid coordinate on the current map. This can be used for random warp scrolls, escape abilities, or event-based scattering.</p>

  <h4>💡 Example SQL – Random Teleport</h4>
  <pre>
-- Randomly teleport the player somewhere on the current map
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1053, 0, '');
  </pre>

  <h4>📘 Parameter Explanation</h4>
  <ul>
    <li>No parameters required. The system automatically selects a valid coordinate within the current map's bounds.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Useful for escape items, events like treasure hunts, or to create chaos in PvP zones.</li>
    <li>Ensure your map has enough walkable terrain, or teleport may land the player in blocked zones.</li>
    <li>Can be followed by visual effects using <code>Type 1027</code> to enhance the teleport animation.</li>
  </ul>
</details>



<details>
  <summary>📋 <strong>Task Manager (Type 1080)</strong></summary>
  <br>

  <p>Type 1080 is used to manage custom task entries for the player. It supports creating tasks, deleting them, and checking if a task already exists. The task number is stored in <code>cq_taskdetail</code>, where its state can also be tracked or modified manually.</p>

  <h4>💡 Example SQL – Create New Task</h4>
  <pre>
-- Create a new task with ID 1114
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1080, 1114, 'new');
  </pre>

  <h4>💡 Example SQL – Delete Task</h4>
  <pre>
-- Delete task ID 1114 from the player's task record
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1080, 1114, 'delete');
  </pre>

  <h4>💡 Example SQL – Check If Task Exists</h4>
  <pre>
-- Check if the player has task ID 1114
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1080, 1114, 'isexit');

-- If task exists
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You task is exist.');

-- If task does not exist
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You task not exist.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task number to manage (e.g. <code>1114</code>).</li>
    <li><code>param</code> – Operation type:
      <ul>
        <li><code>new</code> – Create a new task entry.</li>
        <li><code>delete</code> – Delete the task entry.</li>
        <li><code>isexit</code> – Check if the task exists for the player.</li>
      </ul>
    </li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>isexit</code> before giving or resetting a task to avoid duplicates.</li>
    <li>Combine with <code>0126</code> to notify the player if the task was already created or removed.</li>
    <li>Task entries created with this type are commonly used for quests, flags, or condition tracking outside of the standard quest system.</li>
  </ul>
</details>

<details>
  <summary>⚙️ <strong>Task Operations (Type 1081)</strong></summary>
  <br>

  <p>Type 1081 is used to operate on internal values of a task created with <code>Type 1080</code>. It can check or update task phase, completion count, or timing data. Useful for multi-stage quests, cooldowns, and advanced task conditions.</p>

  <h4>💡 Example SQL – Reset Task Start Time</h4>
  <pre>
-- Reset the start time of task 1114 to current server time
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1081, 1114, 'begintime reset');
  </pre>

  <h4>💡 Example SQL – Check Task Phase</h4>
  <pre>
-- Check if task 1114 is in phase 3
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1081, 1114, 'phase == 3');

-- If task is in phase 3
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You task currently in phase 3.');

-- If task is not in phase 3
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You task is not in phase 3.');
  </pre>

  <h4>💡 Example SQL – Add Completion Count</h4>
  <pre>
-- Add 1 to the completion count of task 1114
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1081, 1114, 'completenum += 1');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID to operate on. Use <code>-1</code> to apply on the task returned by <code>findnext</code>.</li>
    <li><code>param</code> – Format is: <code>ope opt value</code>
      <ul>
        <li><code>phase</code> – Operates on task phase.</li>
        <li><code>completenum</code> – Operates on completion count.</li>
        <li><code>begintime</code> – Operates on task start time.</li>
        <li><code>data1, data2, data3, data4</code> – Custom data fields for new engine.</li>
      </ul>
    </li>
    <li><code>opt</code> – Operator used in the operation:
      <ul>
        <li><code>==</code>, <code>>=</code> – Compare values</li>
        <li><code>=</code> – Assign values</li>
        <li><code>+=</code> – Increase value (or add seconds for begintime)</li>
        <li><code>reset</code> – Only for <code>begintime</code>, resets to current time</li>
      </ul>
    </li>
    <li><code>value</code> – Can be number (e.g. <code>3</code>), duration in seconds, or timestamp (<code>yyyy-mm-dd hh:mm:ss</code>)</li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>You can use this to build complex quest logic with cooldowns, progress stages, or repeatable limits.</li>
    <li>Values for <code>phase</code>, <code>completenum</code>, <code>data1–4</code> have no set limits.</li>
    <li>Use <code>data = -1</code> if you're using <code>findnext</code> and want to operate on its result.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use with <code>0126</code> to inform the player of their progress or cooldowns.</li>
    <li>Use <code>+=</code> to advance task phase or increment completion count.</li>
    <li>Use <code>reset</code> with <code>begintime</code> to track time-based cooldowns or delays between attempts.</li>
  </ul>
</details>

<details>
<summary>⏱️ <strong>Task Cooldown Timer Check (Type 1082)</strong></summary>
  <br>

  <p>Type 1082 is used to compare the current server time with the start time of a specific task. It checks whether the time passed since the task began exceeds the given number of seconds. This is useful for cooldowns, waiting periods, or time-limited quest progression.</p>

  <h4>💡 Example SQL – Check if 5 Minutes (300 seconds) Have Passed</h4>
  <pre>
-- Check if at least 300 seconds have passed since task 1114 started
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1082, 1114, '300');

-- If enough time has passed
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You are well-rested and may proceed with your mission.');

-- If not enough time has passed
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You need more rest before continuing. Please come back later.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID (set with Type 1080).</li>
    <li><code>param</code> – Number of seconds to compare. Returns true if the elapsed time since the task started is greater than this value.</li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>Set the task's start time using <code>1081 begintime reset</code> when the task begins.</li>
    <li>This action only checks elapsed time; it does not update or modify any task values.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Perfect for implementing cooldowns, delays between attempts, or real-time waiting mechanics.</li>
    <li>Display clear messages using <code>0126</code> so players understand when they can retry.</li>
    <li>Combine with <code>1080</code> and <code>1081</code> for full control over task state and timing.</li>
  </ul>
</details>



<details>
  <summary>🗂️ <strong>Write to Custom Log File (Type 1085)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Type 1085 is used to write custom logs from scripts into a text file. Although originally marked as unused in official sources, it works in practice and allows writing formatted lines into a specified log file. This can be useful for event tracking, race results, or custom admin tools.</p>

  <h4>💡 Example SQL – Write to Log</h4>
  <pre>
-- Write to gmlog/action_log.txt with formatted values
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1085, 0, 'gmlog/action_log 340,%user_name[%user_id],0,0,1,0,124,0,0,0');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>folder/filename</code> – Path to the log file, relative to <code>MSGServer</code> (e.g. <code>gmlog/action_log</code>).</li>
    <li><code>log content</code> – The string to be written. Can include variables like:
      <ul>
        <li><code>%user_name</code></li>
        <li><code>%user_id</code></li>
      </ul>
    </li>
    <li>Use <code>~</code> for spaces in the message (e.g. <code>%user_name~won~the~mount~race</code>).</li>
  </ul>

  <h4>💡 Customization Tips</h4>
  <ul>
    <li>You can replace <code>gmlog</code> with any folder name, such as <code>eventlog</code>, and change the filename like <code>mount_race</code> to create <code>eventlog/mount_race.txt</code>.</li>
    <li>The folder must exist manually beforehand. If the folder doesn't exist, the log will not be written.</li>
    <li>This action does not show any message to the player. It's purely for server-side logging.</li>
  </ul>

  <h4>💡 Usage Examples</h4>
  <pre>
-- Custom win message for event
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1085, 0, 'eventlog/mount_race %user_name~won~the~mount~race!');
  </pre>
</details>


<details>
<summary>📆 <strong>Task Day-Based Cooldown Check (Type 1086)</strong></summary>
  <br>

  <p>Type 1086 is used to compare the current date with the start time of a specific task. It checks whether the number of full days passed since the task was created is greater than or equal to the given value. This is typically used for daily/weekly reward cooldowns or time-locked progression.</p>

  <p><strong>Important:</strong> Make sure the task is created first using <code>Type 1080 (new)</code>, and its start time is set using <code>Type 1081 (begintime reset)</code>.</p>

  <h4>💡 Example SQL – Require 4 Days Since Task Start</h4>
  <pre>
-- Check if 4 days have passed since task 1114 started
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1086, 1114, '4');

-- If 4 days or more have passed
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You can receive reward now');

-- If not enough days have passed
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Sorry! You have received reward, please come after 4 days.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID created with Type 1080.</li>
    <li><code>param</code> – Number of days to compare. Returns true if current time is greater than or equal to the required days since task start.</li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>Use <code>Type 1080</code> to create the task.</li>
    <li>Use <code>Type 1081 begintime reset</code> to set the task start time.</li>
    <li>This action checks full calendar days, not just 24-hour differences.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this for weekly login rewards, time-gated events, or return-after missions.</li>
    <li>Combine with <code>0126</code> to inform the player if they can claim or must wait.</li>
    <li>Can be used with <code>1085</code> to log redemptions when the cooldown expires.</li>
  </ul>
</details>


<details>
  <summary>📢 <strong>Team Chat Broadcast (Type 1101)</strong></summary>
  <br>

  <p>Type 1101 is used to send a custom message to the team chat channel. It allows for dynamic player announcements, progress sharing, or event notifications among team members. Variables like <code>%user_name</code> can be used to personalize the broadcast.</p>

  <h4>💡 Example SQL – Broadcast Message to Team</h4>
  <pre>
-- Broadcast to team: "[PlayerName]: I have completed Fire Tomb 2 times today."
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1101, 0, '%user_name: I have completed Fire Tomb 2 times today.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>param</code> – The message to send. Can include player variables:
      <ul>
        <li><code>%user_name</code> – Player's name</li>
        <li><code>%user_id</code> – Player's ID (optional if needed)</li>
      </ul>
    </li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use <code>%user_name</code> to personalize messages automatically for the triggering player.</li>
    <li>Best used for quest completions, dungeon announcements, event wins, or custom group activities.</li>
    <li>This message will only be visible to the player’s current team members, not globally.</li>
  </ul>
</details>

<details>
  <summary>👥 <strong>Team Attribute Check (Type 1102)</strong></summary>
  <br>

  <p>Type 1102 is used to check or operate on attributes of a team. You can check the number of teammates, the level or god level of members, or if specific types of relations like mate or friend are present. If the player is not in a team, the check will return false immediately.</p>

  <h4>💡 Example SQL – Check if Teammate is Your Mate</h4>
  <pre>
-- Check if the player's mate is in the team
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1102, 0, 'mate');

-- If mate is found in the team
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your mate is in your team');

-- If mate is not found
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your mate is not in your team');
  </pre>

  <h4>💡 Example SQL – Check Team Size</h4>
  <pre>
-- Check if team size is less than 2 (need at least 1 teammate)
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1102, 0, 'count < 2');

-- If not enough teammates
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You need at least 1 teammate');

-- If requirement is met
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You can proceed now');
  </pre>

<h4>📘 Parameter Format</h4>
<ul>
  <li><code>param</code> – Format is <code>field opt data</code> depending on the field type.
    <ul>
      <li><code>level</code> – Compare team member levels. Supports operators: <code>&lt;</code>, <code>&gt;</code>, <code>==</code>.</li>
      <li><code>godlev</code> – Compare team member god levels (new engine only). Supports operators: <code>&lt;</code>, <code>&gt;</code>, <code>==</code>.</li>
      <li><code>count</code> – Check total number of players in the team (including captain). Supports operators: <code>&lt;</code>, <code>==</code>.</li>
      <li><code>count_near</code> – Check number of alive players on the same map. Supports operators: <code>&lt;</code>, <code>==</code>.</li>
      <li><code>mate</code> – Check if the player's mate is in the team and alive. No operator needed, only the field name is required (new engine only).</li>
      <li><code>friend</code> – Check if the player's friend is in the team and alive. No operator needed, only the field name is required (new engine only).</li>
    </ul>
  </li>
</ul>


  <h4>📘 Notes</h4>
  <ul>
    <li>If no team is formed, <code>count</code> and other checks will immediately return false.</li>
    <li>Fields like <code>mate</code> and <code>friend</code> are new engine features and require the players to be alive and near.</li>
    <li>Use <code>id_next</code> and <code>id_nextfail</code> properly to branch based on check results.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Good for event restrictions (e.g., need a team to enter) or special buffs (team with mate gets bonuses).</li>
    <li>Combine with <code>0126</code> to show clear reasons when blocking player actions.</li>
    <li>Can be stacked with team teleportation, buff sharing, or mission entry checks.</li>
  </ul>
</details>

<details>
  <summary>🧭 <strong>Team Map Teleport (Type 1107)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p>Type 1107 is used to teleport an entire team, including the team leader, to a new location. All team members must be alive, and the teleport must happen within the same map group (grouped maps only, not cross-world teleport).</p>

  <h4>💡 Example SQL – Teleport Team to a New Position</h4>
  <pre>
-- Move the entire team to Map ID 8901 at coordinates (35, 48)
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1107, 0, '8901 35 48');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>mapid</code> – Target map ID where the team will teleport.</li>
    <li><code>x</code> – Target X coordinate.</li>
    <li><code>y</code> – Target Y coordinate.</li>
  </ul>

  <h4>📘 Important Notes</h4>
  <ul>
    <li>All team members must be alive. Dead members will prevent the teleport.</li>
    <li>This only works within the same map group. Cannot teleport across unrelated maps.</li>
    <li>The team leader and members will arrive together at the specified position.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use for team dungeon entries, multi-player boss rooms, or synchronized event stages.</li>
    <li>Always verify that the team meets teleport conditions using checks like <code>1102 count</code> or <code>count_near</code> before calling this action.</li>
    <li>Combine with <code>1027</code> visual effects for dramatic entrance effects after teleport.</li>
  </ul>
</details>


<details>
<summary>👑 <strong>Team Leader Status Check (Type 1501)</strong></summary>
  <br>

  <p>Type 1501 is used to check whether the player is currently the team leader. No parameters are needed for this action. This is useful when you want only the team leader to trigger certain actions like entering dungeons or starting events.</p>

  <h4>💡 Example SQL – Check Leader Status</h4>
  <pre>
-- Check if the player is the team leader
REPLACE INTO `cq_action` VALUES (1000, 1002, 1001, 1501, 0, '');

-- If player is leader
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'You are team leader.');

-- If player is not leader
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'You are not team leader.');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li>No parameters are needed. Simply call the action to check leadership status.</li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>If the player is the team leader, <code>id_next</code> will execute.</li>
    <li>If the player is not the team leader, <code>id_nextfail</code> will execute.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Use this before starting team-only activities like boss fights, dungeon entries, or team rewards.</li>
    <li>Always combine with a clear <code>1010</code> or <code>0126</code> message to inform players why an action is blocked.</li>
    <li>You can also pair it with <code>1102 count</code> to further check if the team has enough members.</li>
  </ul>
</details>


<details>
  <summary>🎰 <strong>Lottery (Type 1508)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>data and param format is just my theory,cant find any reference.</strong>
  </div>
  <p>Type 1508 is used to trigger a Lottery event related to entries in the <code>cq_lottery</code> table. The action uses <code>data</code> and <code>param</code> fields to specify draw settings, although full behavior depends on server handling and table setup.</p>

  <h4>💡 Example SQL – Lottery Attempt</h4>
  <pre>
-- Trigger a Lottery with color = 1 and type = 5
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 1508, 1, '5');
  </pre>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Assumed to be the "color" field in <code>cq_lottery</code>.</li>
    <li><code>param</code> – Assumed to be the "type" field in <code>cq_lottery</code>.</li>
  </ul>

  <h4>📘 Notes</h4>
  <ul>
    <li>Both <code>color</code> and <code>type</code> must match entries inside <code>cq_lottery</code> for the draw to succeed.</li>
    <li>The server will search <code>cq_lottery</code> using the color and type values provided and select an appropriate reward randomly.</li>
    <li>Exact draw rate, reward item, and amount are controlled inside the <code>cq_lottery</code> database entries.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Ensure your <code>cq_lottery</code> table is correctly populated and balanced to avoid empty draws.</li>
  </ul>
</details>

<details>
  <summary>🧮 <strong>User Variable Control (Type 1521,1522,1523,1524)</strong></summary>
  <br>

  <p>Types 1521–1524 are used to manage temporary user variables (integer or string) during script execution. These variables are NOT saved into any database. They only exist during the current session and will be lost if the player logs out or if the server restarts. Useful for short-term event tracking, temporary counters, or dynamic scripting without needing database storage.</p>

  <h4>📘 Type 1521 – Initialize Integer Variable</h4>
  <p>Initialize an integer variable. You can directly set it to a fixed number or assign a random value within a range.</p>

  <h4>💡 Example SQL – Set Integer to 0</h4>
  <pre>
-- Initialize var(3) to 0
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1521, 0, 'var(3) set 0');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your Integer variable 3 is %iter_var_data3');

  </pre>

  <h4>💡 Example SQL – Random Assignment (New Engine)</h4>
  <pre>
-- Initialize var(2) to random between 10 and 50
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1521, 0, 'var(2) randset 10 50');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your Integer variable 3 randset is %iter_var_data2');
  </pre>

  <h4>📘 Type 1522 – Operate on Integer Variable</h4>
  <p>Perform operations on integer variables like adding, dividing, multiplying, or comparing values.</p>

  <h4>💡 Example SQL – Add 1 to a Variable</h4>
  <pre>
-- Increase var(3) by 1
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1522, 0, 'var(5) += 1');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your Integer variable 5 is %iter_var_data5');
  </pre>
<h4>💡 Example SQL – Modulo Operation (New Engine)</h4>
<pre>
-- Take var(5) and get remainder after division by 3
REPLACE INTO cq_action VALUES (1000, 1001, 0000, 1522, 0, 'var(5) $= 3');
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your Integer variable 5 after modulo 3 is %iter_var_data5');
</pre>

<h4>📘 Supported Operators for Integer Variables</h4>
<ul>
  <li><code>==</code> – Compare if equal</li>
  <li><code>+=</code> – Add a value</li>
  <li><code>/=</code> – Divide by a value (New Engine)</li>
  <li><code>*=</code> – Multiply by a value (New Engine)</li>
  <li><code>$=</code> – Modulo (remainder after division) operation (New Engine)</li>
</ul>


  <h4>📘 Type 1523 – String Variable Comparison</h4>
  <p>Compare text variables (not case sensitive). Useful for checking user input, answers, or choice selections.</p>

  <h4>💡 Example SQL – Compare String</h4>
  <pre>
-- Compare if var(8) equals 50
REPLACE INTO `cq_action` VALUES (1000, 1001, 1002, 1523, 0, 'var(8) == Untungla');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your String variable 8 is Untungla');
REPLACE INTO `cq_action` VALUES (1002, 0000, 0000, 0126, 0, 'Your String variable 8 is not Untungla');
  </pre>

  <h4>📘 Type 1524 – Initialize String Variables</h4>

  <h4>💡 Example SQL – Initialize Text List</h4>
  <pre>
-- Set var(3) to "Sembang~Lebat"
REPLACE INTO `cq_action` VALUES (1000, 1001, 0000, 1524, 0, 'var(3) set Sembang~Lebat');
REPLACE INTO `cq_action` VALUES (1001, 0000, 0000, 0126, 0, 'Your string variable 3 is %iter_var_str3');
  </pre>

  <h4>📘 Notes</h4>
  <ul>
    <li><code>var(n)</code> will link to system variables like <code>%iter_var_dataX</code> (integer) or <code>%iter_var_strX</code> (string).</li>
    <li><strong>New engine:</strong> 
      <ul>
        <li><code>%iter_var_data0</code> ~ <code>%iter_var_data99</code> (100 slots for integers)</li>
        <li><code>%iter_var_str0</code> ~ <code>%iter_var_str19</code> (20 slots for strings)</li>
      </ul>
    </li>
    <li><strong>Old engine:</strong> 
      <ul>
        <li><code>%iter_var_data0</code> ~ <code>%iter_var_data9</code> (10 slots for integers)</li>
        <li><code>%iter_var_str0</code> ~ <code>%iter_var_str9</code> (10 slots for strings)</li>
      </ul>
    </li>
    <li>Random assignment using <code>randset</code> is only available for integers (1521).</li>
    <li>String comparison (1523) is case-insensitive.</li>
    <li>All values are temporary and will disappear after player relog or server reboot.</li>
  </ul>

  <h4>💡 Tips</h4>
  <ul>
    <li>Good for temporary event stages, random rewards, guessing games, or timed puzzles.</li>
    <li>Use integer and string combinations for complex in-memory event flow without database load.</li>
    <li>Ideal for scripting quick mini-games inside NPC dialogs or event maps.</li>
  </ul>
</details>

<details>
  <summary>📦 <strong>Save Item Count to Variable (Type 1534)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1534</strong> is used to count how many specific items a player owns, and save the result into a player variable (<code>iter_var_data</code> field).</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = item_type_id</code></li>
    <li><code>szParam = "var_id bind_flag"</code></li>
    <li><strong>item_type_id:</strong> ID of the item (from <code>cq_itemtype</code>)</li>
    <li><strong>var_id:</strong> Which iter_var_data[] index to save the count (1-30)</li>
    <li><strong>bind_flag:</strong> 
      <ul>
        <li>0 = count both bound and non-bound items</li>
        <li>1 = count only non-bound items</li>
        <li>2 = count only bound items</li>
      </ul>
    </li>
  </ul>

  <h4>📜 Example - Save to var(6):</h4>
  <pre>
-- Count all 1111210 items (bound + non-bound), save result into var(6)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1534, 1111210, '6 0');
  </pre>

  <h4>📜 Example - Save only non-bound items to var(1):</h4>
  <pre>
-- Count only non-bound 1111210 items, save into var(1)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1534, 1111210, '1 1');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>var_id</strong> must match an available iter_var_data slot (commonly 1-30).</li>
    <li>Useful for checking if player has enough items for quest, upgrade, or event condition.</li>
    <li>Use binding flag to separate between bound/unbound inventory checks.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with conditional checks (Type 2003) after saving item counts to create item requirement logic!
  </div>

</details>

<details>
  <summary>🐲 <strong>Eudemon Attribute Check & Modify (Type 1533)</strong></summary>
  <br>

  <p><strong>Type 1533</strong> is used to check or modify Eudemon (pet) attributes such as stars, rebirth, luck, god level, and more.</p>

  <h4>📜 Old Engine Supported Attributes:</h4>
  <ul>
    <li><code>star</code> (supports <code>+=</code>, <code>&lt;</code>, <code>&gt;=</code>, <code>==</code>)</li>
    <li><code>uplevtime</code> (supports <code>+=</code>, <code>&lt;</code>, <code>&gt;=</code>, <code>==</code>)</li>
    <li><code>rebornlimitadd</code> (supports <code>+=</code>, <code>&lt;</code>, <code>&gt;=</code>, <code>==</code>)</li>
    <li><code>luck</code> (supports <code>+=</code>, <code>&lt;</code>, <code>&gt;=</code>, <code>==</code>)</li>
    <li><code>iscallout</code> (supports <code>==</code>) — Check if summoned or not</li>
    <li><code>alive</code> (supports <code>==</code>) — Check if Eudemon is dead or alive</li>
  </ul>

  <h4>🆕 New Engine Additional Supports:</h4>
  <ul>
    <li><code>star</code> (+=, <, >=, ==) – can directly increase/decrease stars</li>
    <li><code>rebornlimitadd</code> (+=, <, >=, ==)</li>
    <li><code>luck</code> (+=, <, >=, ==) – ⚠️ New engine: Adjusting <code>luck</code> does NOT affect stars automatically</li>
    <li><code>damagetype</code> (=, ==)</li>
    <li><code>alive</code> (==)</li>
    <li><code>iscallout</code> (==)</li>
    <li><code>egg</code> (==)</li>
    <li><code>godlev</code> (+=, <, >=, ==)</li>
    <li><code>upgodlevtime += value</code> – add divine EXP (multiplying 432)</li>
    <li><code>reborn</code> (+=, >=, ==) – auto updates score</li>
    <li><code>rebornandday</code> (+=) – rebirth limited to once per day</li>
  </ul>

  <h4>📖 Condition Syntax:</h4>
  <p><code>attribute operator value</code></p>
  <p>Example: <code>star >= 30</code> or <code>alive == 0</code></p>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚡ <strong>Only ONE condition is allowed per action 1533.</strong>
  </div>

  <h4>💬 Basic Example – Check if Eudemon is Dead:</h4>
  <pre>
-- Check if pet is dead
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1533, 0, 'alive == 0');

-- If dead
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your eudemon is dead.');

-- If alive
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Your eudemon is alive.');
  </pre>

  <h4>💡 Other Useful Examples:</h4>
  <pre>
-- Add +1 to eudemon rebirth (and auto update rating)
INSERT INTO cq_action VALUES (2000, 0000, 0000, 1533, 0, 'reborn += 1');
  </pre>

  <pre>
-- Check if God Level is at least 5
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1533, 0, 'luck >= 50');

-- If above
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your eudemon luck is greater than or equal to 50.');

-- If below 
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Your eudemon luck is less than or equal to 50.');
  </pre>



  <h4>📌 Tips:</h4>
  <ul>
    <li>Always use one attribute check or operation per 1533 action.</li>
    <li>Validate eudemon state (alive, summoned, reborn) before applying upgrades.</li>
    <li>Prefer using <code>reborn</code> and <code>rebornandday</code> for new systems instead of manual <code>rebornlimitadd</code> editing.</li>
  </ul>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🧠 <strong>Reminder:</strong><br>In the new engine, <code>luck += 1</code> does NOT increase pet stars automatically. Use <code>star +=</code> if needed.
  </div>
</details>

<details>
  <summary>⚖️ <strong>Event Attribute Comparison (Type 2003 & 2004)</strong></summary>
  <br>

  <p><strong>Type 2003</strong> and <strong>Type 2004</strong> are used to compare system or player attributes dynamically during script execution.</p>

  <h4>📘 Type Differences:</h4>
  <ul>
    <li><strong>Type 2003:</strong> Signed comparison (normal comparison, can use negative numbers)</li>
    <li><strong>Type 2004:</strong> Unsigned comparison (only compares positive values)</li>
  </ul>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data1 opt data2</code></li>
    <li><strong>data1, data2:</strong> Must use variable with % symbol (e.g., <code>%user_map_id</code>, <code>%user_map_x</code>, <code>%iter_var_data7</code>)</li>
    <li><strong>opt:</strong> ==, <, <=</li>
  </ul>

  <h4>📜 Basic Examples:</h4>
  <pre>
-- Type 2003 (Signed) - Allow negative number
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2003, 0, '%iter_var_data7 < -4');
  </pre>

  <pre>
-- Type 2004 (Unsigned) - Positive only
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2004, 0, '%user_map_id == 8971');
  </pre>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>Type 2003</code> when your variable might be negative. Use <code>Type 2004</code> when comparing IDs, positions, or timers that are always positive.
  </div>

</details>

<details>
  <summary>🐾 <strong>Create Monster (Type 2006)</strong></summary>
  <br>

  <p><strong>Type 2006</strong> is used to create a monster (or summon) dynamically in the map. You must specify at least 7 parameters. The created monster will exist based on the parameters set.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>nOwnerType idOwner idMap nPosX nPosY idGen idType [nData] [szName]</code></li>
    <li>Minimum 7 parameters are required</li>
    <li><strong>idOwner = 0</strong> → The monster will not be saved permanently (temporary summon)</li>
    <li>If <code>accept</code> parameter is used, it will rename monster based on accept string</li>
    <li>Otherwise, monster uses <code>szName</code> provided at the end</li>
  </ul>

  <h4>📜 Basic Example:</h4>
  <pre>
-- Create a monster without a custom name
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2006, 0, '0 0 %user_map_id 592 486 2013801 20138');
  </pre>

  <h4>📜 Example with Custom Name:</h4>
  <pre>
-- Create a Void Wraith monster with name
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2006, 0, '0 0 %user_map_id 465 637 9639001 12600 0 Void~Wraith');
  </pre>

  <h4>✅ Parameter Explanation:</h4>
  <ul>
    <li><strong>nOwnerType:</strong> Owner type (0 = no owner, 1 = player, etc.)</li>
    <li><strong>idOwner:</strong> Owner ID (if 0, monster not saved)</li>
    <li><strong>idMap:</strong> Map ID to spawn the monster</li>
    <li><strong>nPosX / nPosY:</strong> Coordinates (X, Y)</li>
    <li><strong>idGen:</strong> Generator ID (for controlling roaming)</li>
    <li><strong>idType:</strong> Monster type ID (link to <code>cq_monstertype</code>)</li>
    <li><strong>nData:</strong> (Optional) Extra data for special behaviors</li>
    <li><strong>szName:</strong> (Optional) Custom monster name (use <code>~</code> instead of spaces)</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>Void~Wraith</code> format if you want custom monster names (replace spaces with ~).
  </div>

</details>

<details>
  <summary>🏗️ <strong>Create New Dynamic NPC (Type 2007)</strong></summary>
  <br>

  <p><strong>Type 2007</strong> is used to dynamically create a brand new NPC in the map. The created NPC will be stored in <code>cq_dynanpc</code>, not <code>cq_npc</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>name type sort lookface ownertype ownerid mapid posx posy [life base linkid task0-task7 data0-data3 datastr]</code></li>
    <li>Minimum 9 parameters are required</li>
    <li>Maximum 25 parameters supported</li>
  </ul>

  <h4>📜 Example - Minimum (9 parameters):</h4>
  <pre>
-- Create basic blocking NPC
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2007, 0, 'Blocker 2 1 520410 0 0 %user_map_id 185 110');
  </pre>

  <h4>📜 Example - Full (25 parameters):</h4>
  <pre>
-- Create full custom NPC with tasks and extra data
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2007, 0, 'Mallos 154 01 762530 0 0 %user_map_id 64 71 0 0 0 20080250 0 0 0 0 0 0 0 0 0 0 0 0');
  </pre>

  <h4>✅ Parameter Breakdown:</h4>
  <ul>
    <li><strong>name:</strong> NPC name (use <code>~</code> instead of space)</li>
    <li><strong>type:</strong> NPC type ID</li>
    <li><strong>sort:</strong> NPC sort (behavior type)</li>
    <li><strong>lookface:</strong> Appearance ID</li>
    <li><strong>ownertype:</strong> Owner type (0 = none)</li>
    <li><strong>ownerid:</strong> Owner ID (0 = none)</li>
    <li><strong>mapid:</strong> Map ID to spawn</li>
    <li><strong>posx / posy:</strong> Coordinates (X, Y)</li>
    <li><strong>life, base, linkid:</strong> Optional extra settings</li>
    <li><strong>task0 - task7:</strong> Optional task links</li>
    <li><strong>data0 - data3:</strong> Optional extra data</li>
    <li><strong>datastr:</strong> Optional string field</li>
  </ul>

  <div style="border-left: 4px solid #03A9F4; background: #e1f5fe; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use minimum 9 parameters if you just need a simple static NPC. Use full 25 parameters if you want NPC linked with tasks, special events, or unique behaviors.
  </div>

  <hr>

  <h4>👀 NPC Lookface Direction (52041x)</h4>

  <p>When creating an NPC, the <code>lookface</code> field controls its appearance and facing direction. It uses the format <code>52041x</code> where <code>x</code> is the facing direction (0 to 7).</p>

  <ul>
    <li>Base lookface comes from <code>ini/npc.ini</code> (example: 52041)</li>
    <li>Last digit (0~7) decides the facing direction of the NPC</li>
    <li>Without correct direction, NPC might not show properly!</li>
  </ul>

  <h4>🧭 Basic Direction Guide:</h4>
  <p style="text-align:center;">
    <img src="assets/images/arah.png" alt="NPC Direction" style="max-width:300px; margin-top:10px; border-radius:8px;">
  </p>

  <h4>📸 In-Game Example:</h4>
  <p style="text-align:center;">
    <img src="assets/images/contoharah.png" alt="NPC Example" style="max-width:100%; border-radius:8px;">
  </p>


</details>

<details>
  <summary>🐉 <strong>Check Monster Count (Type 2008)</strong></summary>
  <br>

  <p><strong>Type 2008</strong> is used to check the number of monsters in the same map based on their generator ID or monster name. Useful for quests, event triggers, and hunting missions.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>idMap field data opt num</code></li>
    <li><strong>idMap:</strong> Map ID or %user_map_id</li>
    <li><strong>field:</strong> "gen_id" (by generator ID) or "name" (by monster name)</li>
    <li><strong>data:</strong> Generator ID or Monster Name to check</li>
    <li><strong>opt:</strong> == or &lt; (equal or less than)</li>
    <li><strong>num:</strong> Target number to compare</li>
  </ul>

  <h4>📜 Example - Check if Monster Count is Less Than Target:</h4>
  <pre>
-- Check if number of monster generator ID 1000500 is less than 30
INSERT INTO cq_action VALUES (1000, 1001, 1002, 2008, 0, '%user_map_id gen_id 1000500 < 30');

-- If less than 30
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Monster amount is less than 30.');

-- If not
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Monster amount is enough.');
  </pre>

  <h4>📜 Example - Check if Specific Monster Exists by Name:</h4>
  <pre>
-- Check if "Gory" monster is still alive
INSERT INTO cq_action VALUES (1000, 1001, 1002, 2008, 0, '%user_map_id name Gory == 0');

-- If no Gory left
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'No more Gory monsters on map.');

-- If Gory still exists
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Gory monsters still alive.');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>gen_id:</strong> Taken from <code>cq_generator</code> (generator type ID)</li>
    <li><strong>name:</strong> Taken from <code>cq_monstertype</code> (monster name)</li>
    <li>Use <code>id_next</code> and <code>id_nextfail</code> properly to handle success/fail path</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with <code>Type 126</code> to show friendly system messages after checking monster status!
  </div>

</details>

<details>
  <summary>🗑️ <strong>Delete Monster from Map (Type 2009)</strong></summary>
  <br>

  <p><strong>Type 2009</strong> is used to delete monsters from a specific map. You can delete based on monster type alone, or both type and monster name together.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>idMap type [data] [name]</code></li>
    <li><strong>idMap:</strong> Map ID to delete from</li>
    <li><strong>type:</strong> Monster type ID (from <code>cq_monstertype</code>)</li>
    <li><strong>data:</strong> (optional) Extra matching (usually 0)</li>
    <li><strong>name:</strong> (optional) Specific monster name</li>
  </ul>

  <h4>📜 Example - Recommended (By Type Only):</h4>
  <pre>
-- Delete all monsters of type 23105 from map 1990
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2009, 0, '1990 23105');
  </pre>

  <h4>📜 Example - By Type and Name:</h4>
  <pre>
-- Delete only "Amanyta" monsters of type 23105 from map 1990
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2009, 0, '1990 23105 0 Amanyta');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Using only <code>type</code> is faster and more recommended for large map cleanups</li>
    <li>Use <code>name</code> if you need to precisely target a specific monster</li>
    <li>Type ID is from <code>cq_monstertype</code> table</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Tip:</strong> Always check the monster's type ID correctly to avoid deleting wrong monsters!
  </div>

</details>

<details>
  <summary>🗑️ <strong>Delete Dynamic NPC by Type (Type 2011)</strong></summary>
  <br>

  <p><strong>Type 2011</strong> is used to delete all dynamic NPCs (<code>cq_dynanpc</code>) of a specific type in a given map. Once deleted, you cannot perform any further operations on them.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>idMap type</code></li>
    <li><strong>idMap:</strong> Target map ID or <code>%user_map_id</code> for current user's map</li>
    <li><strong>type:</strong> NPC type ID to delete</li>
  </ul>

  <h4>📜 Example - Delete from Specific Map:</h4>
  <pre>
-- Delete all NPCs of type 2 from map 1990
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2011, 0, '1990 2');
  </pre>

  <h4>📜 Example - Delete from Current User's Map:</h4>
  <pre>
-- Delete all NPCs of type 23 from current map
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2011, 0, '%user_map_id 23');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only works for <code>cq_dynanpc</code> (dynamic created NPCs)</li>
    <li>After deletion, do not attempt to interact with the deleted NPCs</li>
    <li>Recommended to use when clearing event NPCs, temporary guards, barriers, etc.</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Warning:</strong> Always ensure the correct type and map ID before running <code>2011</code> or you might remove unintended NPCs.
  </div>

</details>


<details>
  <summary>🌎 <strong>Run Script for All Players in Map (Type 2012)</strong></summary>
  <br>

  <p><strong>Type 2012</strong> is used to make every player inside a specific map execute a specified action script immediately. Useful for mass events, global messages, or sudden teleportation.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>idMap idAction -1</code></li>
    <li><strong>idMap:</strong> Target map ID or <code>%user_map_id</code> for current map</li>
    <li><strong>idAction:</strong> Action ID from <code>cq_action</code> to execute for each player</li>
    <li><strong>-1:</strong> Always put <code>-1</code> (purpose unknown but required)</li>
  </ul>

  <h4>📜 Example - Target Specific Map:</h4>
  <pre>
-- Run action 1002 for all players in map 1954
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2012, 0, '1954 1002 -1');
  </pre>

  <h4>📜 Example - Target Current User's Map:</h4>
  <pre>
-- Run action 1002 for all players in user's current map
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2012, 0, '%user_map_id 1002 -1');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Make sure the <code>idAction</code> exists in <code>cq_action</code> or nothing will happen.</li>
    <li><code>-1</code> is mandatory even though its function is unknown.</li>
    <li>Useful for teleporting players, sending announcements, or mass event handling.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with <code>Type 126</code> (system messages) or teleport actions for smooth mass events!
  </div>

</details>


<details>
  <summary>🧨 <strong>Create Trap (Type 2101)</strong></summary>
  <br>

  <p><strong>Type 2101</strong> is used to create a trap in the map. Traps are invisible or visible triggers that perform actions when players step into them. Trap types are based on entries from <code>cq_traptype</code> table.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>type look owner_id map_id pos_x pos_y data [pos_cx pos_cy]</code></li>
    <li><strong>type:</strong> Trap type ID (from <code>cq_traptype</code>)</li>
    <li><strong>look:</strong> Appearance (normally 0 if invisible)</li>
    <li><strong>owner_id:</strong> Owner (0 = no owner)</li>
    <li><strong>map_id:</strong> Map ID to spawn trap</li>
    <li><strong>pos_x / pos_y:</strong> Coordinates (X,Y)</li>
    <li><strong>data:</strong> Special trap data (usually 0)</li>
    <li><strong>pos_cx / pos_cy:</strong> (New Engine only) – Trap trigger center position offset</li>
  </ul>

  <h4>📜 Example - Old Engine (7 parameters):</h4>
  <pre>
-- Create trap at position (103, 144) on map 1000
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2101, 0, '11506 0 0 1000 103 144 0');
  </pre>

  <h4>📜 Example - New Engine Extension (9 parameters):</h4>
  <pre>
-- Create trap at (103, 144) with center offset (20, 20)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2101, 0, '11506 0 0 1000 103 144 0 20 20');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><code>type</code> must match an ID inside <code>cq_traptype</code></li>
    <li>Normally set <code>look = 0</code> for invisible traps (invisible teleport, triggers, etc.)</li>
    <li>Use <code>pos_cx</code> and <code>pos_cy</code> only if using New Engine features (larger trap trigger area)</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine traps with teleport actions, condition checks, or monster spawns for dynamic event maps!
  </div>

</details>


<details>
  <summary>🗑️ <strong>Delete Trap (Type 2102)</strong></summary>
  <br>

  <p><strong>Type 2102</strong> is used to delete (erase) a trap that was created. Once deleted, the trap is permanently removed and cannot be triggered or operated anymore.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li>No parameters needed (leave param empty)</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Delete the current trap
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2102, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only deletes the current trap that is executing the action.</li>
    <li>Once deleted, the trap cannot be activated, moved, or modified anymore.</li>
    <li>Best used after temporary traps, event triggers, or teleport traps are finished.</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Warning:</strong> Always make sure the trap has finished its purpose before calling <code>2102</code> because it cannot be recovered after deletion.
  </div>

</details>


<details>
  <summary>🧮 <strong>Check Trap Count (Type 2103)</strong></summary>
  <br>

  <p><strong>Type 2103</strong> is used to check the number of traps of a specific type within a defined area of the map. Useful for checking if traps have been activated, removed, or if an area is clear.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>map_id pos_x pos_y pos_cx pos_cy count type</code></li>
    <li><strong>map_id:</strong> Target map ID or <code>%user_map_id</code></li>
    <li><strong>pos_x / pos_y:</strong> Starting coordinate (X,Y)</li>
    <li><strong>pos_cx / pos_cy:</strong> Ending coordinate (X,Y)</li>
    <li><strong>count:</strong> Maximum number of traps allowed</li>
    <li><strong>type:</strong> Trap type ID (from <code>cq_traptype</code>)</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Check if number of trap ID 8839 in area (782,734) is less than 1
INSERT INTO cq_action VALUES (1000, 1001, 1002, 2103, 0, '%user_map_id 782 734 782 734 1 8839');

-- If trap count < 1
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'No traps found in the area.');

-- If trap count >= 1
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'There are still traps active.');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Good for checking if traps were triggered, destroyed, or missing after an event.</li>
    <li>Use accurate X/Y coordinates to limit the search area precisely.</li>
    <li><strong>count</strong> is the maximum number allowed. If actual is less, it returns success.</li>
    <li><strong>type</strong> must match ID inside <code>cq_traptype</code>.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine this with <code>trap erase</code> or <code>teleport traps</code> for smoother map event handling.
  </div>

</details>

<details>
  <summary>🗑️ <strong>Delete Trap by Type (Type 2105)</strong></summary>
  <br>

  <p><strong>Type 2105</strong> is used to delete all traps of a specific type in a specific map. Once deleted, the traps are permanently removed and cannot be triggered or operated anymore.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>map_id type</code></li>
    <li><strong>map_id:</strong> Map ID where traps will be deleted</li>
    <li><strong>type:</strong> Trap type ID (from <code>cq_traptype</code>)</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Delete all traps of type 9525 from map 1000
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2105, 0, '1000 9525');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only traps matching both <strong>map_id</strong> and <strong>type</strong> will be deleted.</li>
    <li>After deletion, you cannot interact with those traps anymore.</li>
    <li><strong>type</strong> must match an entry from <code>cq_traptype</code>.</li>
    <li>Useful for clearing event-based traps or cleaning maps after events.</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Warning:</strong> Be careful when deleting traps by type — it will remove <u>all</u> matching traps on the map at once!
  </div>

</details>


<details>
  <summary>🛡️ <strong>Set Special Status (Type 4001)</strong></summary>
  <br>

  <p><strong>Type 4001</strong> is used to apply a special status (buff, debuff, aura, etc.) to the player. Depending on engine version, you can set basic or extended parameters. The status will persist across relogin if saved into <code>cq_special_status</code> table, but will auto-remove after the timer expires.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li>Old Engine: <code>status power secs time</code></li>
    <li>New Engine: <code>status power secs time other savedb</code></li>
    <li><strong>status:</strong> Status ID</li>
    <li><strong>power:</strong> Strength or level of effect</li>
    <li><strong>secs:</strong> Duration in seconds</li>
    <li><strong>time:</strong> Timer field (normally 0)</li>
    <li><strong>other:</strong> Reserved (set 0)</li>
    <li><strong>savedb:</strong> Save into database (1 = yes, 0 = no)</li>
  </ul>

  <h4>📜 Example - Old Engine (4 Parameters Only):</h4>
  <pre>
-- Give status 245 for 86400 seconds (1 day) without database saving
INSERT INTO cq_action VALUES (1000, 0000, 0000, 4001, 0, '245 0 86400 0');
  </pre>

  <h4>📜 Example - New Engine (Save Status into Database):</h4>
  <pre>
-- Give status 245 for 86400 seconds (1 day) and save into cq_special_status
INSERT INTO cq_action VALUES (1000, 0000, 0000, 4001, 0, '245 0 86400 0 0 1');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>Old Engine:</strong> Only supports 4 parameters. Status will disappear after logout if not saved manually elsewhere.</li>
    <li><strong>New Engine:</strong> Supports up to 6 parameters. Setting <code>savedb = 1</code> stores the status in <code>cq_special_status</code>.</li>
    <li>When stored in database, the status will persist even after logout and relogin — but will <strong>auto-delete</strong> once <code>secs</code> expires.</li>
    <li><strong>status</strong> ID must match a valid entry in system settings.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>savedb=1</code> for buffs or event statuses that must survive relogin, but still expire automatically after time ends.
  </div>

</details>

<details>
  <summary>🧹 <strong>Delete Special Status (Type 4004)</strong></summary>
  <br>

  <p><strong>Type 4004</strong> is used to remove an active player status. If the status is stored in <code>cq_special_status</code>, it can also be deleted from the database by setting <code>DelDB = 1</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = unused (use 0)</li>
    <li><code>param</code> = <code>statusID DelDB</code></li>
  </ul>

  <h4>📘 Details:</h4>
  <ul>
    <li><code>statusID</code> = The ID of the state to remove</li>
    <li><code>DelDB</code> = <code>1</code> to delete from <code>cq_special_status</code>, <code>0</code> to remove from memory only</li>
  </ul>

  <h4>📜 Example – Remove status 2000 (from memory only):</h4>
  <pre>
REPLACE INTO cq_action VALUES (11000, 0000, 0000, 4004, 0, '2000 0');
  </pre>

  <h4>📜 Example – Remove status 2000 (from memory + cq_special_status):</h4>
  <pre>
REPLACE INTO cq_action VALUES (11001, 0000, 0000, 4004, 0, '2000 1');
  </pre>

  <h4>📛 Known Stackable Title Statuses:</h4>
  <ul>
    <li>2000 ~ 2012</li>
  </ul>

  <h4>⚠️ Warning:</h4>
  <p>Status effects can stack. Before applying a new title/status, always clear the previous ones to avoid visual or effect overlaps.</p>

  <h4>📦 Batch Clear Example (All 13 Title Statuses):</h4>
  <pre>
-- Clear all known title statuses (2000 to 2012) from cq_special_status and memory
REPLACE INTO cq_action VALUES (12000, 12001, 0000, 4004, 0, '2000 1');
REPLACE INTO cq_action VALUES (12001, 12002, 0000, 4004, 0, '2001 1');
REPLACE INTO cq_action VALUES (12002, 12003, 0000, 4004, 0, '2002 1');
REPLACE INTO cq_action VALUES (12003, 12004, 0000, 4004, 0, '2003 1');
REPLACE INTO cq_action VALUES (12004, 12005, 0000, 4004, 0, '2004 1');
REPLACE INTO cq_action VALUES (12005, 12006, 0000, 4004, 0, '2005 1');
REPLACE INTO cq_action VALUES (12006, 12007, 0000, 4004, 0, '2006 1');
REPLACE INTO cq_action VALUES (12007, 12008, 0000, 4004, 0, '2007 1');
REPLACE INTO cq_action VALUES (12008, 12009, 0000, 4004, 0, '2008 1');
REPLACE INTO cq_action VALUES (12009, 12010, 0000, 4004, 0, '2009 1');
REPLACE INTO cq_action VALUES (12010, 12011, 0000, 4004, 0, '2010 1');
REPLACE INTO cq_action VALUES (12011, 12012, 0000, 4004, 0, '2011 1');
REPLACE INTO cq_action VALUES (12012, 0000, 0000, 4004, 0, '2012 1');
  </pre>

  <div style="border-left: 4px solid #F44336; background: #ffebee; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🛡️ <strong>Tip:</strong> Always clear all title statuses before applying a new one to avoid visual bugs or duplicate effects.
  </div>
</details>

<details>
  <summary>🌀 <strong>Force Magic Attack (Type 4002)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>Old Engine Only</strong>
  </div>
  <p><strong>Type 4002</strong> is used to trigger a magic-type effect directly on a player or target. Only specific types of magic (Detach Status or Steal Status) are supported. It is not used for regular damage skills.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = magic type ID (from magictype table)</code></li>
    <li><code>szParam = magic level</code> (optional, usually 0)</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Use magic type 5025 (must be MAGICSORT_DETACHSTATUS or MAGICSORT_STEAL)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 4002, 5025, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>MAGICSORT_DETACHSTATUS:</strong> Removes buffs or debuffs from the player.</li>
    <li><strong>MAGICSORT_STEAL:</strong> Steals active buffs from a target.</li>
    <li><strong>data</strong> must match a valid entry in <code>magictype</code> table with correct magic sort type.</li>
    <li>Normal attack magic (damage skills) cannot be used with 4002.</li>
    <li><code>szParam</code> can specify magic level if needed (leave empty if default).</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use Type 4002 to forcibly clear buffs (detach) or steal buffs (steal) without needing player to manually use skill.
  </div>

</details>

<details>
  <summary>📚 <strong>Teach Pet Skill (Type 4003)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 4003</strong> is used to teach a specific skill to a Eudemon (pet). This allows controlling which pets can learn which skills and at what level they are allowed to learn.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = skill_id × 10</code></li>
    <li><code>szParam = "skill_id * level_requirement"</code></li>
    <li><strong>skill_id:</strong> ID of the skill from <code>cq_magic</code></li>
    <li><strong>level_requirement:</strong> Minimum pet level required to learn</li>
  </ul>

  <h4>📜 Example - Teach Skill 5003:</h4>
  <pre>
-- Pet will learn skill 5003 at level 20
INSERT INTO cq_action VALUES (1000, 0000, 0000, 4003, 50030, '5003 * 20');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>data:</strong> Must be skill ID multiplied by 10 (5003 × 10 = 50030)</li>
    <li><strong>szParam:</strong> Always format as "skill_id * required_level" (example: "5003 * 20")</li>
    <li>Use this method to restrict learning certain skills until pet reaches required level.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Useful for making pet skill progression more balanced and level-gated!
  </div>

</details>


<details>
  <summary>⏳ <strong>Start Countdown Timer (Type 8000)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <img src="assets/images/8000.png" alt="8000" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 8000</strong> is used to start a countdown timer for a player. When the countdown reaches zero, a specified script will execute automatically. If <code>data = 0</code>, it will close any existing countdown without executing anything.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = seconds</code> (duration of countdown)</li>
    <li><code>szParam = action_id</code> (script to execute when countdown ends)</li>
  </ul>

  <h4>📜 Example - Start 120 Seconds Countdown:</h4>
  <pre>
-- Start countdown for 120 seconds, run action 1001 after finish
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8000, 120, '1001');
  </pre>

  <h4>📜 Example - Stop Countdown Immediately:</h4>
  <pre>
-- Stop any existing countdown
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8000, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only one countdown can exist per player at a time.</li>
    <li>Always close previous countdown before starting a new one to avoid conflicts.</li>
    <li>If <code>data=0</code>, it simply closes the countdown without running any script.</li>
    <li>Good for time-limited quests, challenges, or event mechanics.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> You can combine with system messages (Type 126) to notify players when a countdown starts or finishes!
  </div>

</details>


<details>
  <summary>⏳ <strong>Advanced Countdown Timer (Type 8001)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <img src="assets/images/8001.png" alt="8001" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 8001</strong> is an advanced countdown for players, supporting visual effects and interruption controls. It will display a message, optionally show an animation, and execute a script when the countdown ends. Countdown can be interrupted if configured.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = seconds</code> (duration of countdown)</li>
    <li><code>szParam = "can_interrupt action_id effect_name display_text"</code></li>
    <li><strong>can_interrupt:</strong> 1 = can be interrupted by moving, clicking NPC, using skills; 0 = cannot interrupt</li>
    <li><strong>action_id:</strong> Action ID to run after countdown ends</li>
    <li><strong>effect_name:</strong> Visual effect name from <code>3deffect.ini</code> (use "null" if no effect)</li>
    <li><strong>display_text:</strong> Text shown during countdown</li>
  </ul>

  <h4>📜 Example - Simple Countdown:</h4>
  <pre>
-- 2 seconds countdown, can interrupt, no effect, shows "Collecting"
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8001, 2, '1 1001 null Collecting');
  </pre>

  <h4>📜 Example - Countdown with Effect:</h4>
  <pre>
-- 5 seconds countdown, cannot interrupt, shows "Pulling out the Holy Sword" with RichGod_Ground effect
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8001, 5, '0 1001 RichGod_Ground Pulling~out~the~Holy~Sword');
  </pre>

  <h4>📜 Example - Stop Countdown:</h4>
  <pre>
-- Stop any existing countdown immediately
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8001, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only one countdown can exist per player at a time.</li>
    <li>If player interrupts (move/use skill/talk to NPC) and <code>can_interrupt=1</code>, countdown will stop.</li>
    <li><code>effect_name</code> must exist in <code>3deffect.ini</code> if not set to "null".</li>
    <li><code>display_text</code> spaces must be replaced with <code>~</code> symbol.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine this with system messages or teleport scripts for immersive event mechanics!
  </div>

</details>


<details>
  <summary>⏳ <strong>Instance Map Countdown (Type 8002)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <img src="assets/images/8002.png" alt="8002" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 8002</strong> is used to start a countdown timer specific for players inside a duplicate (instance) map. If the player leaves the map before the countdown ends, the timer is automatically cancelled without executing anything.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = duplicate ID</code> (must be from <code>cq_duplicate</code>)</li>
    <li><code>szParam = action_id</code> (script to run after countdown ends)</li>
  </ul>

  <h4>📜 Example - Start Countdown:</h4>
  <pre>
-- Start a countdown linked to duplicate ID 303, execute action 1001 when finished
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8002, 303, '1001');
  </pre>

  <h4>✅ Important Notes:</h4>
  <ul>
    <li>Must be used only <strong>after teleporting</strong> the player into the instance map.</li>
    <li>The map ID must be <strong>4000000000+</strong> (special instance maps only).</li>
    <li>If player leaves the instance map before timer ends, the countdown will automatically stop without executing the script.</li>
    <li>Duplicate map settings are configured in <code>cq_duplicate</code> and <code>Duplicate.ini</code>.</li>
    <li><strong>x, y</strong> fields in <code>cq_duplicate</code> set the teleport arrival coordinates.</li>
    <li>Players already inside instance maps will not be invited again automatically.</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Tip:</strong> Always teleport players into the instance map before running Type 8002 countdown!
  </div>

</details>


<details>
  <summary>🕶️ <strong>Hidden Countdown Timer (Type 8003,8005,8006)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 8003,8005,8006</strong> is used to start a hidden countdown timer for a player. The countdown will not display any visible time or message on the client's screen. When the countdown reaches zero, a specified action script will execute. Useful for stealth events, traps, or silent triggers.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = seconds</code> (duration of countdown)</li>
    <li><code>szParam = action_id</code> (script to run after countdown ends)</li>
  </ul>

  <h4>📜 Example - Hidden Countdown:</h4>
  <pre>
-- Start hidden countdown for 300 seconds, then run action 1001
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8003, 300, '1001');
  </pre>

  <h4>📜 Example - Stop Countdown:</h4>
  <pre>
-- Stop any existing hidden countdown immediately
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8003, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Hidden countdown will not show any timer to the player.</li>
    <li>Only one countdown can exist per player at a time (shared with other countdown types).</li>
    <li>If <code>data = 0</code>, it closes the countdown without running the script.</li>
    <li>Useful for surprise triggers, timed missions, or background event controls.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine hidden countdown with teleport, spawn monster, or reward actions for surprise events!
  </div>

</details>



<details>
  <summary>🧍 <strong>Set Player Pose (Type 8010)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <img src="assets/images/8010.png" alt="8010" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 8010</strong> is used to change the player's pose (animation) based on the pose ID. The pose IDs are configured in the <code>3dmotion.ini</code> file.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = pose ID</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Set player pose to ID 335
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8010, 335, '');
  </pre>

  <h4>✅ Common Pose IDs:</h4>
  <table>
    <thead>
      <tr>
        <th>Pose ID</th><th>Pose ID</th><th>Pose ID</th><th>Pose ID</th><th>Pose ID</th><th>Pose ID</th><th>Pose ID</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>100</td><td>101</td><td>210</td><td>233</td><td>256</td><td>260</td><td>261</td></tr>
      <tr><td>265</td><td>335</td><td>351</td><td>532</td><td>533</td><td>534</td><td>535</td></tr>
      <tr><td>536</td><td>537</td><td>538</td><td>778</td><td>779</td><td>780</td><td>781</td></tr>
      <tr><td>782</td><td>783</td><td>784</td><td>851</td><td>853</td><td></td><td></td></tr>
    </tbody>
  </table>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use Type 8010 to trigger animations like resting, falling, celebration, or unique reactions during gameplay events!
  </div>

</details>

<details>
  <summary>🗺️ <strong>Across Map Auto Path (Type 8011)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <img src="assets/images/8011.png" alt="8011" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 8011</strong> is used to initiate an automatic pathfinding movement across different maps. The system will automatically calculate the route based on predefined path information.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = mapid x y</code></li>
    <li><strong>mapid:</strong> Target map ID</li>
    <li><strong>x, y:</strong> Target coordinate inside the target map</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Auto path to (388,476) on map 1000
INSERT INTO cq_action VALUES (1000, 0000, 0000, 8011, 0, '1000 388 476');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><strong>cq_acrossmappathinfo</strong> must be properly configured in the database for each map jump point.</li>
    <li><strong>AcrossMapPathInfo.ini</strong> must also be properly configured on the client side to support pathing.</li>
    <li>If either setting is missing, the auto-path will fail or the character will stop moving.</li>
    <li>This type is useful for quest auto-navigation, long travel auto-walks, or teleport redirection.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Always double-check <code>cq_acrossmappathinfo</code> and <code>AcrossMapPathInfo.ini</code> entries are correct for smooth auto-path transitions!
  </div>

</details>


<details>
  <summary>🏛️ <strong>Celebrity Hall Scripts (Type 230 & 231)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 230</strong> and <strong>Type 231</strong> are used exclusively for the Celebrity Hall system. They must be performed by Celebrity Hall related NPCs only (Type 100).</p>

  <h4>🧠 Type Overview:</h4>
  <ul>
    <li><strong>Type 230:</strong> Register for Celebrity Hall</li>
    <li><strong>Type 231:</strong> Sign up for Celebrity Hall actions</li>
    <li>Only used with NPCs in <code>cq_npc</code> ID range <strong>10200 to 11199</strong></li>
  </ul>

  <h4>📜 Example - Register (Type 230):</h4>
  <pre>
-- Register player into Celebrity Hall
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0230, 0, '');
  </pre>

  <h4>📜 Example - Sign Up (Type 231):</h4>
  <pre>
-- Sign up player for Celebrity Hall event
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0231, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only Celebrity Hall NPCs should use these types.</li>
    <li>General NPCs outside 10200-11199 range should not use these script types.</li>
  </ul>

  <div style="border-left: 4px solid #FF9800; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚡ <strong>Tip:</strong> For correct setup, always refer to <code>cq_npc</code> range 10200–11199 and official Celebrity Hall system flow.
  </div>

</details>

<details>
  <summary>🧿 <strong>Check Eudemon Celebrity Hall Registration (Type 1067)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1067</strong> is used to check if the player's currently summoned Eudemon has already been registered by another player into the Celebrity Hall for this week. If yes, it will block further registration.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li>No parameters needed (<code>param</code> empty)</li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Check if current summoned Eudemon is already registered
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1067, 0, '');

-- If already registered
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Eudemons that you are currently summon already register in other statues and cannot be re-registered this week!');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only affects the Eudemon currently summoned by the player.</li>
    <li>Validation is limited to this week's Celebrity Hall registration.</li>
    <li>If already registered, it blocks and shows system message (Type 126).</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this before allowing players to submit Eudemons into the Celebrity Hall!
  </div>

</details>


<details>
  <summary>🔥 <strong>Register Divine Fire Statue Ranking (Type 232)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 232</strong> is used to register the player's ranking into the Divine Fire Statue system. This script type must only be performed by Divine Fire Statue NPCs inside Cronus whic is using type 125.</p>

  <h4>🧠 Type Overview:</h4>
  <ul>
    <li>Designed only for Divine Fire Statue system registration.</li>
    <li>Must be used by specific NPCs:</li>
    <ul>
      <li>NPC ID 12116</li>
      <li>NPC ID 12117</li>
      <li>NPC ID 12118</li>
    </ul>
    <li>Located inside Cronus map.</li>
  </ul>

  <h4>📜 Example - Divine Fire Registration:</h4>
  <pre>
-- Register player into Divine Fire Statue ranking
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0232, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Do not use this type outside of the Divine Fire Statue NPCs.</li>
    <li>Normal NPCs should not use Type 232.</li>
    <li>Double-check NPC IDs to match official settings.</li>
  </ul>

  <div style="border-left: 4px solid #FF5722; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚡ <strong>Tip:</strong> Always verify that only Divine Fire Statue NPCs (ID 12116, 12117, 12118) are allowed to trigger Type 232!
  </div>

</details>



<details>
  <summary>📍 <strong>Run Script for Players in Area (Type 2020)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 2020</strong> is used to execute a script on players located inside a specific area on a map. You can define a rectangular zone and choose to affect all players or a limited number of players.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = mapid x y cx cy scriptid count</code></li>
    <li><strong>mapid:</strong> Target map ID</li>
    <li><strong>x, y:</strong> Starting coordinates</li>
    <li><strong>cx, cy:</strong> Area size (coordinate range)</li>
    <li><strong>scriptid:</strong> Action ID to execute</li>
    <li><strong>count:</strong> 
      <ul>
        <li>-1 = trigger for all players</li>
        <li>Other number = limit to specific number of players</li>
      </ul>
    </li>
  </ul>

  <h4>📜 Example - Affect All Players in Area:</h4>
  <pre>
-- Trigger action 10519091 for all players inside area (106,406) ~ (118,418) on map 1000
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2020, 0, '1000 106 406 12 12 10519091 -1');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>The selected area forms a square or rectangle based on <code>x,y</code> and <code>cx,cy</code>.</li>
    <li>Use <code>count = -1</code> if you want to trigger for all players in the zone.</li>
    <li>Useful for area events, auto-teleport, mass rewards, or trapping zones.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Good for dynamic events where players standing inside a zone are affected simultaneously!
  </div>

</details>

<details>
  <summary>🏆 <strong>PK Tournament Statue Update (Type 1602)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1602</strong> and <strong>NPC Type 126</strong> are used together to manage PK Tournament Statue behavior inside the server. These functions allow updating the PK statue's appearance or settings during PK events.</p>

  <h4>🧠 Type Overview:</h4>
  <ul>
    <li><strong>NPC Type 126:</strong> PK Tournament Statue type. Editable fields allowed in <code>cq_npc</code>.</li>
    <li><strong>Script Type 1602:</strong> Updates the PK statue's information if it exists.</li>
  </ul>

  <h4>📜 Example - Update PK Statue:</h4>
  <pre>
-- Update PK Statue info for NPC 12850
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1602, 0, '');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>This setup only applies to NPC ID <strong>12850</strong>.</li>
    <li>The PK statue must be created as <code>type = 126</code> inside <code>cq_npc</code>.</li>
    <li>If the statue does not exist when script 1602 runs, no action will be taken.</li>
  </ul>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚡ <strong>Tip:</strong> Always make sure PK Statue (NPC 12850) is spawned properly before using Type 1602 to avoid missing updates!
  </div>

</details>




<details>
  <summary>🐉 <strong>Delete All Monsters in Map (Type 2015)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 2015</strong> deletes all monsters currently existing in the specified map.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = mapid</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Delete all monsters in map 11052
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2015, 0, '11052');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This affects all monsters, regardless of how they were spawned (generator or script).</li>
    <li>Use carefully during events or map resets.</li>
  </ul>

</details>

<details>
  <summary>🗿 <strong>Delete All Dynamic NPCs in Map (Type 2016)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 2016</strong> removes all <code>cq_dynanpc</code> entries from the given map. This clears all scripted or event-based NPCs.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = mapid</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Delete all dynamic NPCs in map 11052
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2016, 0, '11052');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Does not affect permanent NPCs from <code>cq_npc</code>.</li>
    <li>Good for clearing temporary quest NPCs after completion.</li>
  </ul>

</details>


<details>
  <summary>🧨 <strong>Delete All Traps in Map (Type 2017)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 2017</strong> deletes all traps from the specified map, regardless of type or origin.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = mapid</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Delete all traps in map 11052
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2017, 0, '11052');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Removes everything placed with <code>Type 2101</code> or related trap scripts.</li>
    <li>Best used after time-based trap events or map resets.</li>
  </ul>

</details>


<details>
  <summary>🎁 <strong>Spawn Item on Map (Type 324)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 324</strong> is used to spawn one or more items on the map at a specified coordinate. This can be used as a reward drop, quest item spawn, or map event chest drop.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>Param = idItemType itemCount idMap x y nRange aliveSecs privSecs</code></li>
    <li><strong>idItemType:</strong> Item ID from <code>cq_itemtype</code></li>
    <li><strong>itemCount:</strong> Number of items to drop (max 100)</li>
    <li><strong>idMap:</strong> Map ID where item appears (can use <code>%user_map_id</code>)</li>
    <li><strong>x, y:</strong> Coordinates or dynamic vars like <code>%killmonster_x</code>, <code>%user_pos_x</code></li>
    <li><strong>nRange:</strong> Drop radius from center point (max 20)</li>
    <li><strong>aliveSecs:</strong> Time in seconds before the item disappears</li>
    <li><strong>privSecs:</strong> Protection time (only owner can pick up)</li>
  </ul>

  <h4>📜 Example - Drop Item at Monster Death Location:</h4>
  <pre>
-- Spawn 30x item 1042068 near kill location, radius 16, lasts 10 minutes, protected for 10 seconds
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0324, 0, '1042068 30 %user_map_id %killmonster_x %killmonster_y 16 600 10');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Item ID must exist in <code>cq_itemtype</code></li>
    <li>Max itemCount = 100</li>
    <li>Max nRange = 20 tiles</li>
    <li>After <code>privSecs</code>, item becomes free-for-all</li>
    <li>Use wisely to avoid server lag from mass spawning</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Perfect for dynamic event drops or rewarding players on monster kills or timed events!
  </div>

</details>



<details>
  <summary>👹 <strong>Create Advanced Monster (Type 2019)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 2019</strong> creates a monster with extended attributes such as HP, damage reduction, and magic power. It's an upgraded version of Type 2018 with more control over the monster's stats.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>nOwnerType idOwner idMap nPosX nPosY nDir idGen idType nHP nDefence2 nSealDevilVal nData szName</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Spawn Trial Judge monster with custom HP, defence, and power
INSERT INTO cq_action VALUES (1000, 0000, 0000, 2019, 0, '0 0 %user_map_id 71 67 7 0 43003 100000000 10000 0 0 Trial~Judge');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Must include at least the first 10 parameters</li>
    <li><strong>nHP:</strong> Min value is 1 — server will auto-correct anything lower</li>
    <li><strong>idOwner = 0:</strong> monster won't be saved</li>
    <li><strong>idGen:</strong> controls the movement range (from <code>cq_generator</code>, but <code>type</code> field is ignored)</li>
    <li>If using <code>accept</code>, monster name will follow accepted value, else use <code>szName</code></li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Great for boss events or scripted monster waves with specific stats and behavior.
  </div>

</details>

<details>
  <summary>🪟 <strong>Open UI Window (Type 187)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

    <img src="assets/images/187.png" alt="185" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 187</strong> is used to open a specific UI window on the client. This is typically used to trigger interfaces like Divine Furnace or other special panels. The <code>param</code> value is passed to the client and included in the packet, but its purpose is currently unknown.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = window ID</code></li>
    <li><code>param = client-side argument</code> (optional, usage unknown)</li>
  </ul>

  <h4>📜 Example – Open Divine Furnace:</h4>
  <pre>
-- Open Divine Furnace UI (Window ID 13047)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0187, 13047, '0');
  </pre>

  <h4>✅ Known Window IDs:</h4>
  <ul>
    <li><code>13047</code> – (Divine Furnace)</li>
    <!-- Add more if discovered -->
  </ul>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>This only affects the client (shows UI window).</li>
    <li>Used to trigger panels related to upgrades, events, or systems.</li>
    <li><code>param</code> is included in the sent packet but function unknown (can sniff via packet capture).</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this to create shortcut NPCs that trigger in-game upgrade panels or system UIs!
  </div>

</details>

<details>
  <summary>🏯 <strong>Open Divine Furnace Stages and Send Extra Info (Type 1601)</strong></summary>
  <br>
   <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1601</strong> is used to send stage unlock and player-specific data to the client, usually after opening the Divine Furnace UI using <code>Type 187</code>. After stage unlocking (1202), two additional info are sent: last dungeon level (1203) and remaining entries (1204).</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>szParam = ID data</code></li>
    <li><strong>1202:</strong> Stages unlocked (example: <code>1|2|3|4|5</code>)</li>
    <li><strong>1203:</strong> Last entered dungeon level (<code>%iter_taskdetail_1data800</code>)</li>
    <li><strong>1204:</strong> Remaining number of entries (<code>%iter_taskdetail_completenum800</code>)</li>
  </ul>

  <h4>📜 Full Example - Open UI and Send Data:</h4>
  <pre>
-- Step 1: Open Divine Furnace UI
INSERT INTO cq_action VALUES (1000, 1001, 0000, 0187, 13047, '0');

-- Step 2: Send unlocked stages (1 to 5)
INSERT INTO cq_action VALUES (1001, 1002, 0000, 1601, 0, '1202 1|2|3|4|5');

-- Step 3: Send last entered dungeon level
INSERT INTO cq_action VALUES (1002, 1003, 0000, 1601, 0, '1203 %iter_taskdetail_1data800');

-- Step 4: Send remaining entry times
INSERT INTO cq_action VALUES (1003, 0000, 0000, 1601, 0, '1204 %iter_taskdetail_completenum800');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li><code>1202</code> - Shows which stages are unlocked</li>
    <li><code>1203</code> - Displays last dungeon difficulty player entered</li>
    <li><code>1204</code> - Shows remaining allowed entries</li>
    <li>Always use <code>Type 1601</code> immediately after <code>Type 187</code> to sync the UI correctly.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Keep the order correct to avoid client UI desync issues!
  </div>

</details>

<details>
  <summary>🧠 <strong>Trigger and Check CAPTCHA (Type 1064 & 1065)</strong></summary>
  <br>
   <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

    <img src="assets/images/1065.png" alt="1065" style="border:1px solid #ccc; border-radius:6px;">

  <p>These types are used to implement a CAPTCHA-style quiz system. When triggered, a question will appear on the screen and the player must answer within the given time. You can check whether it's currently active and respond accordingly.</p>

  <h4>🧠 Script Type Overview:</h4>
  <ul>
    <li><strong>Type 1065:</strong> Trigger CAPTCHA UI, with countdown in seconds</li>
    <li><strong>Type 1064:</strong> Check if CAPTCHA is currently active</li>
  </ul>

  <h4>📜 Example – Trigger CAPTCHA:</h4>
  <pre>
-- Show CAPTCHA for 99 seconds
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1065, 99, '');
  </pre>

  <h4>📜 Example – Check CAPTCHA Result:</h4>
  <pre>
-- Check if CAPTCHA UI is active, and branch accordingly
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1064, 0, '');

-- If wrong
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You answer wrong');

-- If right
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You answer right');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>UI will display a basic math challenge with 20 answer buttons.</li>
    <li>Type 1065 defines the active countdown timer.</li>
    <li>Type 1064 checks the status and branches result depending on correctness.</li>
    <li>Client UI auto-handles input, you only manage logic.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Great for bot protection, AFK checks, or quick player interaction events!
  </div>

</details>

<details>
  <summary>🚪 <strong>Force Logout Player (Type 1066)</strong></summary>
  <br>
 <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1066</strong> is used to forcefully disconnect a player from the server. This script must always be used as the final line in an action chain to prevent unexpected behavior.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code> (unused)</li>
    <li><code>param = reason for disconnect</code></li>
  </ul>

  <h4>📜 Example – Force Disconnect:</h4>
  <pre>
-- Kick player with reason "Detected cheating behavior"
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1066, 0, 'Detected cheating behavior');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>This type immediately disconnects the player.</li>
    <li>Always place this as the <strong>final action</strong> in the sequence.</li>
  </ul>

  <div style="border-left: 4px solid #f44336; background: #ffebee; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Warning:</strong> Use responsibly and only when necessary. Always put as the last action line!
  </div>

</details>

<details>
  <summary>🎯 <strong>Run Script on Target (Type 134)</strong></summary>
  <br>
 <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 134</strong> is used to run a specific script (action ID) on a targeted player or guild. Only one target condition can be used per call. This is useful for personal rewards, guild triggers, or conditional execution based on name or ID.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>param = [action_id] [target_type] [value]</code></li>
    <li><strong>Target types:</strong></li>
    <ul>
      <li><code>user_id</code> – Player UID (unique account/player ID)</li>
      <li><code>user_name</code> – Exact player name (case-sensitive)</li>
      <li><code>syn_id</code> – Guild ID</li>
    </ul>
  </ul>

  <h4>📜 Example – Run on Player by ID:</h4>
  <pre>
-- Run action 1001 for player with user_id 100000
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0134, 0, '1001 user_id 100000');
  </pre>

  <h4>📜 Example – Run on Player by Name:</h4>
  <pre>
-- Run action 1001 for player named "DuaSelipar"
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0134, 0, '1001 user_name DuaSelipar');
  </pre>

  <h4>📜 Example – Run on Guild by ID:</h4>
  <pre>
-- Run action 1001 for all players in guild ID 10
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0134, 0, '1001 syn_id 10');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only one condition per script call is allowed.</li>
    <li>If the condition doesn’t match (e.g., wrong name), the script will not run.</li>
    <li>The target script (action ID) must already exist and be valid.</li>
    <li>Used in targeted event rewards, name-specific triggers, or guild-wide effects.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this to send private mail rewards, trigger scripts for VIPs, or launch guild-specific events!
  </div>

</details>

<details>
  <summary>⚡ <strong>Add Divine EXP to Summoned Divine Pets (Type 1063)</strong></summary>
  <br>
 <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1063</strong> grants divine experience to all currently summoned pets that have evolved into Divine form. The amount of divine experience added is based on a multiplier formula depending on the duration specified.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = minutes</code> (length of boost time)</li>
    <li><code>param = empty</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Add Divine EXP equivalent to 5 minutes to all active Divine pets
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1063, 5, '');
  </pre>

  <h4>✅ Calculation:</h4>
  <ul>
    <li>Divine EXP added = <code>divine EXP rate × 432 × minutes</code></li>
    <li><strong>432</strong> is the system multiplier constant.</li>
  </ul>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only affects summoned pets that have been upgraded to "Divine" status.</li>
    <li>Good for implementing divine EXP potions, events, or boss clear rewards.</li>
    <li>Setting <code>data = 0</code> does nothing.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Perfect for creating power-up items like Divine Energy Potions!
  </div>

</details>

<details>
  <summary>🤝 <strong>Team Task Manager (Type 1480)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1480</strong> checks whether team members have accepted a specific task. It operates based on the <code>isexit</code> condition, ensuring task requirements across the team.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [requirement] isexit</code></li>
    <ul>
      <li><code>1 isexit</code> – All team members must have the task.</li>
      <li><code>0 isexit</code> – Only one team member needs to have the task.</li>
    </ul>
  </ul>

  <h4>📜 Example – Check All Team Members for Task 1164:</h4>
  <pre>
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1480, 1164, '1 isexit');

-- If all have the task
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'All of your team members have accepted this task.');

-- If any member does not have the task
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Some of your team members have not accepted this task.');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Only supports <code>isexit</code> condition (task existence check).</li>
    <li>Very useful for team-based quests or dungeon entry requirements.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with team teleport or reward actions to control group quest flow!
  </div>

</details>

<details>
  <summary>👥 <strong>Team Task Operation (Type 1481)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1481</strong> is the team version of <code>Type 1081</code>, used to check internal task values (like phase or completion count) across party members. It only supports <code>==</code> and <code>>=</code> operations and must begin with a check mode flag.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [check_mode] field operator value</code></li>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID to operate on</li>
    <li><code>param</code> – Format: <code>[0/1] field operator value</code>
      <ul>
        <li><strong>0</strong> = only one member must meet the condition</li>
        <li><strong>1</strong> = all members must meet the condition</li>
      </ul>
    </li>
    <li><strong>field</strong> = task variable
      <ul>
        <li><code>phase</code>, <code>completenum</code>, <code>data1–data4</code></li>
      </ul>
    </li>
    <li><strong>operator</strong> = only <code>==</code> or <code>>=</code></li>
    <li><strong>value</strong> = comparison value</li>
  </ul>

  <h4>📜 Example – All Members Must Be in Phase 2:</h4>
  <pre>
-- Check if all team members have task 1200 in phase 2
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1481, 1200, '1 phase == 2');

-- If all match
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'All team members are in phase 2.');

-- If someone doesn't match
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team is not in phase 2.');
  </pre>

  <h4>📜 Example – At Least One Has completenum >= 5:</h4>
  <pre>
-- Check if at least one player has completed 5 times
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1481, 1200, '0 completenum >= 5');

-- If one or more match
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'Someone in your team has done this 5 or more times.');

-- If no one matches
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team has done this 5 times yet.');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Supports only <code>==</code> and <code>>=</code> operators.</li>
    <li>Good for team-based stage gating, reward eligibility, or boss challenge locks.</li>
    <li>Combine with <code>0126</code> to give feedback to players.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this when entering high-level instances where all team members must meet certain conditions.
  </div>

</details>


<details>
  <summary>⏱️ <strong>Team Task Cooldown Timer Check (Type 1482)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1482</strong> is the team-based version of <code>Type 1082</code>. It checks whether the cooldown period (in seconds) has passed since the start time of a specific task for one or all team members. Useful for team entry conditions, cooldown gates, and timed challenges.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [0/1] seconds</code></li>
    <ul>
      <li><code>0</code> – at least one member must satisfy the time condition</li>
      <li><code>1</code> – all members must satisfy the time condition</li>
    </ul>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID to check</li>
    <li><code>param</code> – Format: <code>[check_mode] seconds</code></li>
    <ul>
      <li><strong>check_mode</strong>: <code>0</code> = any member, <code>1</code> = all members</li>
      <li><strong>seconds</strong>: minimum time elapsed since task start</li>
    </ul>
  </ul>

  <h4>📜 Example – All Members Must Wait 5 Minutes (300s):</h4>
  <pre>
-- Check if all members' task 1200 started more than 5 minutes ago
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1482, 1200, '1 300');

-- All passed
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'All of your team may proceed.');

-- At least one did not wait long enough
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team must wait longer before proceeding.');
  </pre>

  <h4>📜 Example – Any One Member Waited Enough:</h4>
  <pre>
-- Check if any member passed cooldown
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1482, 1200, '0 300');

-- Success
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'At least one member is ready.');

-- Failure
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team is ready yet.');
  </pre>

  <h4>✅ Quick Notes:</h4>
  <ul>
    <li>Use <code>1081 begintime reset</code> to set the task start time.</li>
    <li>This only checks elapsed time. It doesn’t change task state.</li>
    <li>Perfect for time-gated events, group cooldowns, or retry limits.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Best used for team-based dungeon entry cooldowns or real-time wait logic!
  </div>

</details>


<details>
  <summary>📆 <strong>Team Task Day-Based Cooldown Check (Type 1486)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1486</strong> checks if a specified number of full days have passed since the <code>begintime</code> of a task, for all or part of a team. This is the team-based version of <code>Type 1086</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [0/1] days</code></li>
    <ul>
      <li><strong>0</strong> – At least one member must meet the requirement</li>
      <li><strong>1</strong> – All members must meet the requirement</li>
    </ul>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID assigned via <code>1080</code></li>
    <li><code>param</code> – Format: <code>[check_mode] [day_count]</code></li>
  </ul>

  <h4>📜 Example – All Members Must Wait 4 Days:</h4>
  <pre>
-- Check if 4 days have passed for all team members
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1486, 1114, '1 4');

-- If all okay
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Everyone in your team can now continue.');

-- If at least one hasn't waited enough
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team must wait longer before continuing.');
  </pre>

  <h4>📜 Example – Any Member Can Proceed:</h4>
  <pre>
-- Check if any member waited at least 4 days
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1486, 1114, '0 4');

-- If one or more passed
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'One of your teammates is ready to proceed.');

-- If no one passed
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'None of your teammates have reached the cooldown time.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>If the player has no party, the check is skipped completely.</li>
    <li>Use <code>1081 begintime reset</code> to initialize the cooldown start time.</li>
    <li>This script only checks cooldowns – it doesn’t update or modify task data.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Great for weekly events or team-locked progression that requires days of wait before retrying.
  </div>

</details>

<details>
  <summary>🆕 <strong>New Task Manager (Type 1180)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <img src="assets/images/1180.png" alt="1180" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 1180</strong> is used to manage new UI-based task entries. These tasks are displayed in the in-game panel (Story / Daily / Event), and stored in <code>cq_newtaskdetail</code>. It supports creating, deleting, and checking for task existence. It does not trigger any scripts from <code>cq_newtaskconfig</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = new task ID</code></li>
    <li><code>param = new | delete | isexit</code></li>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>new</code> – Creates a new task entry</li>
    <li><code>delete</code> – Deletes the task entry</li>
    <li><code>isexit</code> – Checks whether the task exists for the player</li>
  </ul>

  <h4>📜 Example – Create New Task:</h4>
  <pre>
-- Create task 2001 (e.g., Beast Hunting)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1180, 2001, 'new');
  </pre>

  <h4>📜 Example – Delete Task:</h4>
  <pre>
-- Remove task 2001
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1180, 2001, 'delete');
  </pre>

  <h4>📜 Example – Check If Task Exists:</h4>
  <pre>
-- Check if task 2001 exists
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1180, 2001, 'isexit');

-- If task exists
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'This task is already active.');

-- If task doesn't exist
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You don\'t have this task yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Task data is saved in <code>cq_newtaskdetail</code>.</li>
    <li>This type only manages data; it does NOT trigger scripts from <code>cq_newtaskconfig</code>.</li>
    <li>UI will reflect task immediately when created.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>Type 1180</code> to prepare new UI tasks, and combine with <code>Type 1181</code> to update their status and data.
  </div>

</details>


<details>
  <summary>🛠️ <strong>New Task Operations (Type 1181)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1181</strong> is used to manipulate or compare values within <code>cq_newtaskdetail</code>. This is the backbone for progressing new UI-based tasks. You can change phase, mark task complete, set custom counters (like kill count), and reset cooldown timers.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = field operator value</code></li>
  </ul>

  <h4>📘 Supported Fields:</h4>
  <ul>
    <li><code>phase</code> – Task phase or stage</li>
    <li><code>complete</code> – Task completion state</li>
    <li><code>begintime</code> – Time the task started</li>
    <li><code>data1 ~ data10</code> – Task-specific progress variables</li>
  </ul>

  <h4>📘 Supported Operators:</h4>
  <ul>
    <li><code>==</code> – Equal to (check only)</li>
    <li><code>>=</code> – Greater or equal (check only)</li>
    <li><code>+=</code> – Increment (add number)</li>
    <li><code>=</code> – Assign (set to specific value)</li>
    <li><code>reset</code> – Only for <code>begintime</code>, resets to current time</li>
  </ul>

  <h4>📜 Example – Increment Kill Count (data1 += 1):</h4>
  <pre>
-- Add 1 kill to Beast Hunting Common Beast (task 2001, data1)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1181, 2001, 'data1 += 1');
  </pre>

  <h4>📜 Example – Set Task to Complete:</h4>
  <pre>
-- Mark task 2001 as complete
INSERT INTO cq_action VALUES (1001, 0000, 0000, 1181, 2001, 'complete = 1');
  </pre>

  <h4>📜 Example – Set Start Time to Now:</h4>
  <pre>
-- Reset task 2001 start time
INSERT INTO cq_action VALUES (1002, 0000, 0000, 1181, 2001, 'begintime reset');
  </pre>

  <h4>📜 Example – Check If Phase >= 3:</h4>
  <pre>
-- Conditional check on task phase
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1181, 2001, 'phase >= 3');

-- If true
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'You have reached phase 3.');

-- If false
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'You have not reached phase 3 yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Used after <code>1180 new</code> to progress or update task state.</li>
    <li>All data is stored in <code>cq_newtaskdetail</code>.</li>
    <li><code>complete</code> should be set to <code>1</code> when task is done.</li>
    <li><code>phase</code> is often used for multi-stage growth tasks.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>data1~data10</code> for things like kill count, collection progress, etc. Phase is ideal for story or growth tasks!
  </div>

</details>



<details>
  <summary>⏱️ <strong>New Task Cooldown Timer Check (Type 1182)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1182</strong> checks whether a specified number of seconds has passed since the <code>begintime</code> of a new UI-based task stored in <code>cq_newtaskdetail</code>. This is useful for cooldown enforcement, retry restrictions, or time-locked progression.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = seconds</code> (e.g. <code>300</code> for 5 minutes)</li>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID that has a <code>begintime</code> set</li>
    <li><code>param</code> – Duration in seconds to check against current time</li>
  </ul>

  <h4>📜 Example – Check if 5 Minutes Have Passed:</h4>
  <pre>
-- Check if 300 seconds (5 minutes) have passed since task 2001 started
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1182, 2001, '300');

-- If enough time has passed
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You are well-rested and may proceed.');

-- If not enough time
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You need to wait a bit longer before continuing.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Use <code>1181 begintime = now</code> to set start time when task begins.</li>
    <li>This action only checks elapsed time — it doesn’t modify task state.</li>
    <li>Perfect for real-time delays or retry cooldown logic.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with <code>Type 1181</code> to manage cooldown windows and progression delays.
  </div>

</details>

<details>
  <summary>📆 <strong>New Task Day-Based Cooldown Check (Type 1186)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1186</strong> checks whether a specified number of full calendar days has passed since the <code>begintime</code> of a new UI task stored in <code>cq_newtaskdetail</code>. This is perfect for time-gated features such as daily rewards, long-term quests, or event re-entry limits.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = number of days</code> (e.g. <code>4</code> for 4 days)</li>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID assigned using <code>Type 1180</code></li>
    <li><code>param</code> – Integer number of days since task start</li>
  </ul>

  <h4>📜 Example – Require 4 Days Since Task 2001 Started:</h4>
  <pre>
-- Check if 4 days have passed since task 2001 started
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1186, 2001, '4');

-- If 4+ days have passed
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You are now eligible to proceed.');

-- If not enough days have passed
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You must wait a few more days before continuing.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Task must be initialized using <code>Type 1180</code></li>
    <li><code>begintime</code> should be set using <code>Type 1181 begintime reset</code></li>
    <li>This script only checks days – it does not modify any task state</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Best for login rewards, growth quests, or long-term task unlocks that require real waiting periods!
  </div>

</details>


<details>
  <summary>🐲 <strong>Check Eudemon Attributes (Type 1971)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1971</strong> is used to check the attributes of a player's eudemons. If <u>at least one</u> eudemon matches all conditions, the result is considered <strong>true</strong>. The scope of which eudemons are checked depends on the <code>data</code> value.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = Eudemon scope (0, 1, or 2)</li>
    <li><code>param</code> = comma-separated attribute checks</li>
  </ul>

  <h4>📘 Scope (data value):</h4>
  <ul>
    <li><code>0</code> – Only summoned and alive</li>
    <li><code>1</code> – Summoned (including dead)</li>
    <li><code>2</code> – All pets (summoned or not, alive or dead)</li>
  </ul>

  <h4>📘 Supported Conditions:</h4>
  <ul>
    <li><code>level</code> – Eudemon level</li>
    <li><code>starlev</code> – Star level</li>
    <li><code>callout</code> – 1 if summoned</li>
    <li><code>godlev</code> – Divine level</li>
  </ul>

  <h4>📜 Example – Check if Any Summoned Pet Has Level ≥ 20:</h4>
  <pre>
-- One of your summoned & alive pets must have level 20+
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1971, 0, 'level >= 20');

-- If condition true
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'At least one of your pets is level 20 or higher.');

-- If condition false
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'None of your pets meet the level requirement.');
  </pre>

  <h4>📜 Example – Check Combined Conditions:</h4>
  <pre>
-- Check if any summoned pet is called out AND star level ≥ 1000 AND not Divine
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1971, 0, 'callout == 1,starlev >= 1000,godlev == 0');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Multiple conditions are combined with AND logic.</li>
    <li>Only one pet needs to match all conditions to return true.</li>
    <li>Useful for unlocking actions based on pet training or Divine evolution.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with <code>Type 0126</code> to show messages depending on your pet’s power, star level, or evolution.
  </div>

</details>



<details>
  <summary>👥 <strong>New Task Team Manager (Type 1580)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1580</strong> is used to check whether a specified new task exists for players in a party. It works like <code>Type 1180</code> but supports team-based checking. It only supports the <code>isexit</code> operation.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [check_mode] isexit</code></li>
    <ul>
      <li><code>0 isexit</code> – At least one member must have the task</li>
      <li><code>1 isexit</code> – All members must have the task</li>
    </ul>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><strong>data:</strong> Task ID from <code>cq_newtaskdetail</code></li>
    <li><strong>param:</strong> One of:
      <ul>
        <li><code>0 isexit</code> – One member is enough</li>
        <li><code>1 isexit</code> – All members must have the task</li>
      </ul>
    </li>
  </ul>

  <h4>📜 Example – All Members Must Have Task 2001:</h4>
  <pre>
-- Check if all team members have task 2001
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1580, 2001, '1 isexit');

-- If true
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'All your team members have accepted the task.');

-- If false
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team has not accepted the task.');
  </pre>

  <h4>📜 Example – Only One Member Required:</h4>
  <pre>
-- Check if any member has task 2001
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1580, 2001, '0 isexit');

-- If true
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'One of your teammates has the required task.');

-- If false
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team has accepted the task yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Only works on tasks created with <code>Type 1180</code> (new UI quests)</li>
    <li>Use before entering team-based dungeons or challenge gates</li>
    <li>Combine with <code>0126</code> to notify player clearly</li>
  </ul>

  <div style="border-left: 4px solid #2196F3; background: #e3f2fd; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use <code>1580</code> to control access to multiplayer content that requires synchronized task progress!
  </div>

</details>


<details>
  <summary>👥 <strong>New Task Team Operations (Type 1581)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1581</strong> is the team-based version of <code>Type 1181</code>. It checks whether the values in <code>cq_newtaskdetail</code> for all or some members meet a specified condition. It only supports comparison operators <code>==</code> and <code>>=</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [check_mode] field operator value</code></li>
    <ul>
      <li><code>0</code> – Only one team member must meet the condition</li>
      <li><code>1</code> – All team members must meet the condition</li>
    </ul>
  </ul>

  <h4>📘 Supported Fields:</h4>
  <ul>
    <li><code>phase</code></li>
    <li><code>data1</code> ~ <code>data10</code></li>
  </ul>

  <h4>📘 Supported Operators:</h4>
  <ul>
    <li><code>==</code></li>
    <li><code>>=</code></li>
  </ul>

  <h4>📜 Example – All Members Must Be in Phase ≥ 2:</h4>
  <pre>
-- Check if all members have task 2001 at phase 2 or above
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1581, 2001, '1 phase >= 2');

-- If true
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your entire team has reached phase 2.');

-- If false
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team is still below phase 2.');
  </pre>

  <h4>📜 Example – At Least One Member Has Collected 5 Items (data1):</h4>
  <pre>
-- Check if any member has collected 5 or more
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1581, 2001, '0 data1 >= 5');

-- If true
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'One of your teammates has enough items.');

-- If false
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team has collected enough yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Only supports <code>==</code> and <code>>=</code> operators.</li>
    <li>Perfect for shared progress validation (e.g. dungeon entry, event boss eligibility).</li>
    <li>Combine with <code>Type 0126</code> to give user feedback.</li>
    <li>Only evaluates <strong>existing</strong> tasks created via <code>Type 1180</code>.</li>
  </ul>

  <div style="border-left: 4px solid #FF9800; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Good for multi-stage co-op quests where all team members must meet progress conditions!
  </div>

</details>



<details>
  <summary>⏱️ <strong>New Task Team Cooldown Timer Check (Type 1582)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1582</strong> is the team-based version of <code>Type 1182</code>. It checks whether the specified cooldown duration (in seconds) has passed since the task <code>begintime</code> for all or some members in the team. Useful for real-time co-op cooldown gates or retry control.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [check_mode] seconds</code></li>
    <ul>
      <li><code>0</code> – at least one member must pass</li>
      <li><code>1</code> – all members must pass</li>
    </ul>
  </ul>

  <h4>📘 Parameter Format</h4>
  <ul>
    <li><code>data</code> – Task ID assigned via <code>Type 1180</code></li>
    <li><code>param</code> – Format: <code>[0/1] seconds</code></li>
  </ul>

  <h4>📜 Example – All Members Must Wait 5 Minutes (300s):</h4>
  <pre>
-- Check if 5 minutes have passed for all members
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1582, 2001, '1 300');

-- If passed
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Your team is ready to continue.');

-- If failed
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team still needs to wait.');
  </pre>

  <h4>📜 Example – At Least One Member is Ready:</h4>
  <pre>
-- Check if any member waited 300 seconds
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1582, 2001, '0 300');

-- If success
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'Someone in your team is ready.');

-- If all failed
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team has finished the cooldown yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Make sure <code>begintime</code> is set using <code>1181 begintime reset</code> at task start.</li>
    <li>Only checks cooldown time; doesn’t update task values.</li>
    <li>Used in real-time retry systems or timed boss access.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Pair with <code>Type 1186</code> for calendar-day cooldowns and <code>Type 1581</code> for team-based phase/data checks!
  </div>

</details>


<details>
  <summary>📆 <strong>New Task Team Day-Based Cooldown Check (Type 1586)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>


  <p><strong>Type 1586</strong> checks if a specified number of full calendar days have passed since the <code>begintime</code> of a task (from <code>cq_newtaskdetail</code>) for some or all members in a team. It is the team-based version of <code>Type 1186</code>. If the player is not in a party, no check is performed.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = task ID</code></li>
    <li><code>param = [check_mode] days</code></li>
    <ul>
      <li><code>0</code> – at least one member must meet the condition</li>
      <li><code>1</code> – all members must meet the condition</li>
    </ul>
  </ul>

  <h4>📘 Parameter Format:</h4>
  <ul>
    <li><strong>data:</strong> Task ID (created using <code>Type 1180</code>)</li>
    <li><strong>param:</strong> Format: <code>[0/1] [day_count]</code></li>
  </ul>

  <h4>📜 Example – All Team Members Must Wait 4 Days:</h4>
  <pre>
-- Check if all team members have waited 4 days
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1586, 2001, '1 4');

-- All passed
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Everyone in your team is eligible.');

-- At least one didn’t wait enough
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Someone in your team still needs to wait.');
  </pre>

  <h4>📜 Example – Any Member Has Waited Enough:</h4>
  <pre>
-- At least one member waited long enough
INSERT INTO cq_action VALUES (2000, 2001, 2002, 1586, 2001, '0 4');

-- Passed
INSERT INTO cq_action VALUES (2001, 0000, 0000, 0126, 0, 'Someone in your team is ready.');

-- None passed
INSERT INTO cq_action VALUES (2002, 0000, 0000, 0126, 0, 'No one in your team is ready yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>If the player is not in a team, this check does nothing.</li>
    <li>Uses <code>cq_newtaskdetail.begintime</code> to compare full calendar days.</li>
    <li>Task must be initialized with <code>Type 1180</code> and <code>begintime</code> set via <code>1181 begintime reset</code>.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Best for weekly group quests, delayed event entries, or multi-day mission gating.
  </div>

</details>

<details>
  <summary>🔁 <strong>Run Script for All Team Members (Type 1412)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>

  <p><strong>Type 1412</strong> is used to trigger a specific action script for all members in the current team, including the one who initiated it. If the player is not in a team, the script will not be executed for anyone.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code></li>
    <li><code>param = action ID to execute (for all teammates)</code></li>
  </ul>

  <h4>📜 Example – Trigger Script 1001 for All Party Members:</h4>
  <pre>
-- Execute action ID 1001 for each teammate
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1412, 0, '1001');
  </pre>

  <h4>📜 Example – Script 1001 (Type 126):</h4>
  <pre>
-- Show message to each team member
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'This message was sent to every member of your team.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This is useful for team rewards, synchronized triggers, or team-based cutscenes.</li>
    <li>The target action ID (e.g., 1001) must exist and be valid for each player.</li>
    <li>Only executes if the player is currently in a team.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Combine with reward scripts or countdown triggers to sync events across all teammates!
  </div>

</details>

<details>
  <summary>📨 <strong>Invitation Dialog with Confirmation (Type 7001)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/7001.png" alt="7001" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 7001</strong> triggers an invitation-style dialog box for the player, using a message string from <code>strres.ini</code>. A countdown is shown, and if the player confirms before it ends, a predefined script will be executed. This script ID is set server-side via <code>邀请对话框确认执行脚本 = [ActionID]</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = message ID (from strres.ini)</code></li>
    <li><code>param = countdown time in seconds</code> (optional)</li>
  </ul>

  <h4>📜 Example – Show Dialog with 20 Seconds Countdown:</h4>
  <pre>
-- Show invitation dialog with message 5010010, wait 20 seconds
INSERT INTO cq_action VALUES (1000, 0000, 0000, 7001, 5010010, '20');
  </pre>

  <h4>✅ Server-Side Trigger:</h4>
  <pre>
邀请对话框确认执行脚本 = 1001
  </pre>

  <h4>📜 Example – Confirmation Script (Type 126):</h4>
  <pre>
-- Message after player accepts the dialog
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have accepted the invitation.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>If player presses “OK” before countdown ends, confirmation script will be triggered.</li>
    <li>If player ignores or closes dialog, no script is executed.</li>
    <li>Text is pulled from <code>strres.ini</code> using the ID in <code>data</code>.</li>
  </ul>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Tip:</strong> Use this for party invites, challenge prompts, entry confirmations, or real-time interactive choices!
  </div>

</details>

<details>
  <summary>🌍 <strong>Global Invitation Dialog (Type 197)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 197</strong> broadcasts an invitation dialog to all players in a specific map or globally. It uses a message from <code>strres.ini</code> and shows a countdown. If a player confirms before it expires, the server executes a predefined script via <code>邀请对话框确认执行脚本 = [ActionID]</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = message ID (from strres.ini)</code></li>
    <li><code>param = mapid countdown</code></li>
  </ul>

  <h4>📘 Examples:</h4>

  <pre>
-- Global invitation dialog (all maps), 300s countdown
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0197, 191123, '0 300');

-- Send dialog only to players in map 7000, countdown 300s
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0197, 191123, '7000 300');
  </pre>

  <h4>✅ Server-Side Setting:</h4>
  <pre>
邀请对话框确认执行脚本 = 1001
  </pre>

  <h4>📜 Example – Confirmation Script (Type 126):</h4>
  <pre>
-- Display confirmation message after player clicks "OK"
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You accepted the event invitation!');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Only players in the <code>mapid</code> specified will receive the dialog.</li>
    <li>If <code>mapid = 0</code>, all online players will receive it.</li>
    <li>Use this for real-time events, global raids, boss warnings, etc.</li>
    <li>Player must confirm within countdown seconds or script won't run.</li>
  </ul>

  <div style="border-left: 4px solid #03A9F4; background: #e1f5fe; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🌟 <strong>Tip:</strong> Use <code>197</code> to announce server-wide challenges or dungeon invites with real-time confirmation.
  </div>

</details>



<details>
  <summary>🧛‍♂️ <strong>Vampire Awakening Trigger (Type 1964)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1964</strong> is used to activate the Vampire Awakening form. It should only be triggered if the player is not already in the <code>status 1014</code>. This awakening grants enhanced power and visuals for the Vampire class.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code></li>
    <li><code>param = 0</code></li>
  </ul>

  <h4>📘 Prerequisite:</h4>
  <p>Check if player does NOT already have <code>status 1014</code> before triggering.</p>

  <h4>📜 Example – Check Status and Trigger Awakening:</h4>
  <pre>
-- If not awakened, trigger awakening
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1964, 0, '0');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Status ID <code>1014</code> indicates the player is already in awakening mode.</li>
    <li>Only use this for characters in the Vampire class (recommended to validate class externally).</li>
    <li>This script handles transformation and possible visual changes automatically.</li>
  </ul>

  <div style="border-left: 4px solid #673AB7; background: #ede7f6; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🧛 <strong>Tip:</strong> Use as part of storyline, reward, or after a PvE challenge to unlock Vampire transformation!
  </div>

</details>




<details>
  <summary>🐲 <strong>Custom Eudemon Creator (Type 549)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 549</strong> creates a new Eudemon directly in the player's Eudemon backpack with customized stats. You can define its level, divine level, star rating, binding status, and whether it is a necro-type pet.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = Eudemon Type ID (e.g. 1071960)</li>
    <li><code>param</code> = <code>level godlevel score [bind] [is_necro]</code></li>
  </ul>

  <h4>📘 Parameter Format:</h4>
  <ul>
    <li><strong>level</strong> – Eudemon level (e.g. 81)</li>
    <li><strong>godlevel</strong> – Divine level</li>
    <li><strong>score</strong> – Star rating (e.g. 2500 = 25 stars)</li>
    <li><strong>bind</strong> (optional) – Set to <code>1</code> to force bind the pet</li>
    <li><strong>is_necro</strong> (optional) – Set to <code>1</code> to make it a Necro-type Eudemon</li>
  </ul>

  <h4>📜 Example – Give a 25-Star Divine Eudemon (XO Type):</h4>
  <pre>
-- Create a 25-star, divine Level 1 XO-type Eudemon (unbound)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0549, 1071960, '81 1 2500');
  </pre>

  <h4>📜 Example – Give a Bound Necro-Type Eudemon:</h4>
  <pre>
-- Create a 25-star Necro-type Eudemon that is bound to the player
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0549, 1071960, '81 1 2500 1 1');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>The player must have at least 1 empty Eudemon slot available.</li>
    <li>Omitting the 4th param will allow the item pack to decide bind status.</li>
    <li>Setting the 5th param to <code>1</code> makes it a Necro Eudemon (compatible with "Spirit" fusion).</li>
  </ul>

  <div style="border-left: 4px solid #9C27B0; background: #f3e5f5; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this type for high-tier pet reward packs, starter bundles, or class-specific unlocks!
  </div>

</details>


<details>
  <summary>📍 <strong>Check If Player Is in Cronus Market (Type 1968)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1968</strong> checks if the player is currently located in the Cronus Market map (also known as 雷鸣市场). It requires no parameters and simply returns true if the player is in the correct map.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code></li>
    <li><code>param = (empty)</code></li>
  </ul>

  <h4>📜 Example – Allow Action Only in Cronus Market:</h4>
  <pre>
-- Check if player is in Cronus Market
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1968, 0, '');

-- If true
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You are currently in Cronus Market.');

-- If false
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'This feature is only available in Cronus Market.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This only checks the current map — no teleport or movement occurs.</li>
    <li>Use before market-exclusive actions, like NPC exchanges or events.</li>
    <li>Common Cronus Market Map ID is <code>1000</code>.</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this to restrict actions like pack opening, VIP shops, or trading to the market area only.
  </div>

</details>

<details>
  <summary>📱 <strong>Check Mobile Client Login (Type 1969)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1969</strong> checks if the current player is logged in from a mobile client. This is useful for triggering mobile-exclusive content, messages, or limiting features that are not supported on mobile devices.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code></li>
    <li><code>param = (empty)</code></li>
  </ul>

  <h4>📜 Example – Show Message for Mobile Players:</h4>
  <pre>
-- Check if player is using mobile client
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1969, 0, '');

-- If mobile
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Welcome mobile player! You are now connected via the mobile client.');

-- If not mobile (likely PC)
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You are currently using the PC client.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This only checks login source — no effect on gameplay.</li>
    <li>Can be used to disable or redirect certain features depending on client type.</li>
    <li>No parameters needed beyond <code>data = 0</code>.</li>
  </ul>

  <div style="border-left: 4px solid #FF9800; background: #fff3e0; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this to offer tailored rewards or simplified interactions for mobile users.
  </div>

</details>


<details>
  <summary>📩 <strong>Send Broadcast Message (Type 196)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 196</strong> sends a pigeon system message (normal or golden). It works like <code>Type 125</code>, but includes the player's name in the <code>param</code>. All spaces in the message must be replaced with <code>~</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = message type:
      <ul>
        <li><code>0</code> = normal pigeon</li>
        <li><code>1</code> = golden pigeon</li>
      </ul>
    </li>
    <li><code>param</code> = <code>[type] [player_name] [message_with_tildes]</code></li>
  </ul>

  <h4>📜 Example:</h4>
  <pre>
-- Send golden pigeon (VIP boradcast)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0196, 2017, '1 DuaSelipar[PM] Hello~World');
  </pre>

  <h4>📜 Example:</h4>
  <pre>
-- Send normal pigeon 
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0196, 2017, '0 DuaSelipar[PM] Hello~World');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li><strong>Use <code>~</code> instead of spaces</strong> in the message content to avoid script parsing errors.</li>
  </ul>

  <div style="border-left: 4px solid #2196F3; background: #e3f2fd; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    📢 <strong>Tip:</strong> Combine with rewards or events to directly notify individual players about rare drops, achievements, or wins.
  </div>

</details>


<details>
  <summary>🧭 <strong>Auto Pathfinding to NPC (Type 1532)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1532</strong> automatically moves the player to the specified NPC using pathfinding and opens its dialog. Commonly used in guided quests, tutorials, and main storyline scenes.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = 0</li>
    <li><code>param</code> = <code>x y npc_id map_id</code></li>
  </ul>

  <h4>📜 Example – Auto path to NPC 1034 at (335, 455) in map 1000:</h4>
  <pre>
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1532, 0, '335 455 1034 1000');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Performs auto pathfinding to the given coordinates and opens the NPC dialog.</li>
    <li>Primarily works with <strong>static NPCs</strong> defined in the <code>cq_npc</code> table.</li>
    <li><strong>Supports cross-map navigation</strong> — the player will auto-transfer to the target map if needed.</li>
    <li>Often used at the end of a cutscene or to guide players during storyline steps.</li>
  </ul>


</details>



<details>
  <summary>🐾 <strong>Summon Follow Pet / Auto-Loot Assistant (Type 1967)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1967</strong> is used to summon a follow pet (non-Eudemon) that follows the player around and typically auto-collects dropped items. The player must already own the follow pet before this script can successfully summon it.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = follow pet ID</li>
    <li><code>param</code> = leave empty</li>
  </ul>

  <h4>📜 Example – Summon Follow Pet ID 1:</h4>
  <pre>
-- Summon follow pet with ID 1 (must be owned)
INSERT INTO cq_action VALUES (1000, 0000, 0000, 1967, 1, '');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This is <strong>not an Eudemon</strong> — it's a cosmetic/helper pet like auto-collector.</li>
    <li>If a different follow pet is already active, it will be recalled before summoning the new one.</li>
    <li>The follow pet must be <strong>owned/unlocked</strong> by the player for this to work.</li>
  </ul>


</details>




<details>
  <summary>🎬 <strong>Play Animation / Visual Effect (Type 194)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 194</strong> is used to trigger animation or scripted visual effects from <code>luapartscript.ini</code>. It's typically used for cinematic effects, transformations, special skill triggers, or cutscene visuals.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data = 0</code></li>
    <li><code>param = type param1 param2</code></li>
  </ul>

  <h4>📘 Known Param Types:</h4>
  <ul>
    <li><code>0 0 0</code> – Likely stops all current effects (optional)</li>
    <li><code>10 0 LuaScriptName</code> – Runs a script from <code>luapartscript.ini</code></li>
    <li><code>1 effect_id duration</code> – Plays a single effect (e.g. stealth for 8s)</li>
  </ul>

  <h4>📜 Example – Run Lua Animation Script:</h4>
  <pre>
-- Plays Lua script from luapartscript.ini [10]
INSERT INTO cq_action VALUES (1000, 1001, 0000, 0194, 0, '10 0 PerformanceScript_10');
  </pre>

  <h4>📜 Example – Play Visual Effect (ID 52000) for 10s:</h4>
  <pre>
-- This will apply an invisible/stealth-like effect
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0194, 0, '1 52000 10');
  </pre>

  <h4>📜 (Optional) – Stop Current Effects:</h4>
  <pre>
-- Optional: Clear current effect layers
INSERT INTO cq_action VALUES (1000, 0000, 0000, 0194, 0, '0 0 0');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Script names like <code>PerformanceScript_10</code> must be defined inside <code>luapartscript.ini</code>.</li>
    <li>Effect IDs like <code>52000</code> are internal and engine-specific.</li>
    <li>Use to build cutscene sequences, intro effects, or skill unlocks.</li>
  </ul>

  <div style="border-left: 4px solid #673AB7; background: #ede7f6; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Chain multiple <code>194</code> actions for scripted cinematics or epic scene transitions!
  </div>

</details>

<details>
  <summary>🔊 <strong>Play Sound Effect (Type 195)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 195</strong> plays a sound from <code>SoundRes.ini</code>. It can be used in events, NPC interactions, transformations, or dramatic reveals.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = 0</li>
    <li><code>param</code> = <code>sound_id sound_name</code></li>
  </ul>

  <h4>📜 Example – Play BGM_State1 (ID 20015):</h4>
  <pre>
-- Sound must be defined in SoundRes.ini
INSERT INTO cq_action VALUES (20006403, 0000, 0000, 0195, 0, '20015 BGM_State1');
  </pre>

  <h4>📘 Example Entry in SoundRes.ini:</h4>
  <pre>
[20015]
Sound=sound/BGM_State1.wav
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Make sure the <code>sound_id</code> and <code>sound name</code> match <code>SoundRes.ini</code> exactly.</li>
    <li>This will play client-side and may vary by map or scene.</li>
    <li>Supports both music and short effects.</li>
  </ul>

  <div style="border-left: 4px solid #03A9F4; background: #e1f5fe; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    🎧 <strong>Tip:</strong> Combine with <code>Type 194</code> for full audiovisual cutscenes or boss intro effects.
  </div>

</details>


<details>
  <summary>🔥 <strong>Check Divine Fire Status (Type 1987)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1987.png" alt="1987" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 1987</strong> checks whether the player has access to the Divine Fire system. Returns true if it is unlocked and available.</p>

  <h4>📜 Example:</h4>
  <pre>
-- Check if Divine Fire is unlocked
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1987, 0, '');

-- If unlocked
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Divine Fire system is already unlocked.');

-- If locked
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'You have not unlocked the Divine Fire system yet.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>No parameters are needed for this check.</li>
    <li>Use this to gate access to Divine Fire-related content or upgrades.</li>
  </ul>
</details>

<details>
  <summary>🧩 <strong>Check Divine Fire Slot (Type 1988)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1988</strong> checks whether a specific Divine Fire slot is open (1–4). Each slot controls 2 grid positions in the interface.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = 1 ~ 4 (slot number)</li>
  </ul>

  <h4>📜 Example – Check if Slot 2 is Open:</h4>
  <pre>
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1988, 2, '');

-- If open
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'Slot 2 is already open.');

-- If not open
INSERT INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'Slot 2 is still locked.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Check before upgrades or gear placement in Divine Fire system.</li>
  </ul>
</details>


<details>
  <summary>🔓 <strong>Unlock Divine Fire Slot (Type 1989)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1989</strong> unlocks a specific Divine Fire slot. This is typically triggered via reward, quest, or item usage.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = 1 ~ 4 (slot number to unlock)</li>
  </ul>

  <h4>📜 Example – Unlock Slot 3:</h4>
  <pre>
-- Unlock slot 3
INSERT INTO cq_action VALUES (1000, 1001, 0000, 1989, 3, '');
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have unlocked Divine Fire Slot 3.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Each slot includes 2 grids — unlocking slot 3 enables grid 5 and 6.</li>
    <li>Only works if the Divine Fire system is already unlocked (check with <code>1987</code>).</li>
  </ul>
</details>


<details>
  <summary>🎖️ <strong>Check / Unlock / Equip Title (Type 1996, 1997, 1998)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1996.png" alt="1996" style="border:1px solid #ccc; border-radius:6px;">

  <p>These script types are used to manage player titles based on the <code>title.ini</code> file. Titles are stored in <code>cq_titleid</code> and can be unlocked, equipped, or checked using the following actions:</p>

  <h4>📘 Type Overview:</h4>
  <ul>
    <li><code>1996</code> – Check if a title is already unlocked</li>
    <li><code>1997</code> – Unlock a title for the player</li>
    <li><code>1998</code> – Equip an unlocked title</li>
  </ul>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = Title ID (must match <code>title.ini</code>)</li>
    <li><code>param</code> = (leave blank)</li>
  </ul>

  <h4>📜 Example – Full Chain (Check → Unlock → Equip Title 45):</h4>
  <pre>
-- Step 1: Check if title 45 is already unlocked
INSERT INTO cq_action VALUES (1000, 1001, 1002, 1996, 45, '');

-- If unlocked
INSERT INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have already unlocked this title,please equip in wardrobe.');

-- Step 2: If not unlocked → Unlock it first
INSERT INTO cq_action VALUES (1002, 1003, 0000, 1997, 45, '');

-- Step 3: Equip after unlocking
INSERT INTO cq_action VALUES (1003, 1004, 0000, 1998, 45, '');
  </pre>

  <h4>💬 Message Feedback (Optional with Type 126):</h4>
  <pre>
-- Title equipped
INSERT INTO cq_action VALUES (1004, 0000, 0000, 0126, 0, 'Title equipped successfully.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Title IDs must exist in <code>title.ini</code></li>
    <li>Unlocked titles are saved to <code>cq_titleid</code></li>
    <li>Use <code>1996</code> before equipping to avoid invalid equip attempts</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Perfect for achievements, ranking rewards, seasonal events, or permanent unlockable content.
  </div>

</details>




<details>
  <summary>📬 <strong>Send Mail to Player (Type 1980)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1980.png" alt="1980" style="border:1px solid #ccc; border-radius:6px;">

  <p><strong>Type 1980</strong> is used to send mail with attachments (items, EP, PP, Lunar Point) to players. The mail format uses a template from <code>cq_mailtemplate</code> and stores the sent data in <code>cq_mailinfo</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = Mail Template ID (from <code>cq_mailtemplate</code>)</li>
    <li><code>param</code> = Mail content (see breakdown below)</li>
  </ul>

  <h4>📜 Example SQL :</h4>
  <pre>
--INSERT INTO cq_action VALUES (1000, 0000, 0000, 1980, 1,'30 5000 4000 3000 754859 1 1 1025491 1 1 751022 1 1 2000 1000'
);
  </pre>
  <h4>📦 Param Format (in order):</h4>

  <table cellspacing="0" cellpadding="6">
    <thead>
      <tr>
        <th>Position</th>
        <th>Field</th>
        <th>Description</th>
        <th>Example</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>saveday</td><td>How many days the mail stays. Use <code>0</code> = permanent (100 days)</td><td>30</td></tr>
      <tr><td>2</td><td>money</td><td>Gold</td><td>5000</td></tr>
      <tr><td>3</td><td>emoney</td><td>EP</td><td>4000</td></tr>
      <tr><td>4</td><td>emoney2</td><td>PP</td><td>3000</td></tr>

      <tr><td>5</td><td>item1</td><td>First item ID to send</td><td>754859</td></tr>
      <tr><td>6</td><td>item1Amount</td><td>Amount (must be 1 for equipment)</td><td>1</td></tr>
      <tr><td>7</td><td>item1Lock</td><td><code>1</code> = Bound, <code>0</code> = Unbound</td><td>1</td></tr>

      <tr><td>8</td><td>item2</td><td>Second item ID</td><td>1025491</td></tr>
      <tr><td>9</td><td>item2Amount</td><td>Amount</td><td>1</td></tr>
      <tr><td>10</td><td>item2Lock</td><td>Bound status</td><td>1</td></tr>

      <tr><td>11</td><td>item3</td><td>Third item ID</td><td>751022</td></tr>
      <tr><td>12</td><td>item3Amount</td><td>Amount</td><td>1</td></tr>
      <tr><td>13</td><td>item3Lock</td><td>Bound status</td><td>1</td></tr>

      <tr><td>14</td><td>moonvalue</td><td>Lunar Point</td><td>2000</td></tr>
      <tr><td>15</td><td>starvalue</td><td>Star Point</td><td>1000</td></tr>
    </tbody>
  </table>



  <h4>✅ Notes:</h4>
  <ul>
    <li>Item amount must be <strong>1</strong> for non-stackable items (e.g. equipment).</li>
    <li>Use <code>lock = 1</code> to make items bound to the player.</li>
    <li>Only send valid, stackable items or pre-wrapped packs to prevent issues.</li>
    <li>All mail content is stored in <code>cq_mailinfo</code>.</li>
  </ul>

  <div style="border-left: 4px solid #009688; background: #e0f2f1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ✅ <strong>Tip:</strong> Use this for login rewards, compensation gifts, milestone rewards, or event prize delivery.
  </div>

</details>


<details>
  <summary>🆕 <strong>Character Rename (Rerole) Check & Action (Type 1990, 1991, 1992)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>This set of scripts is used to allow players to rename their characters, specifically those with default names like <code>rerole[123456]</code>. It includes validation, duplication checks, and rename request logic.</p>

  <h4>📘 Type Functions:</h4>
  <ul>
    <li><strong>1990</strong> – Check if player name starts with <code>rerole[</code> (e.g. <code>rerole[20345]</code>)</li>
    <li><strong>1991</strong> – Check if the player has already applied for rename</li>
    <li><strong>1992</strong> – Apply the rename request (takes effect after maintenance)</li>
  </ul>

  <h4>📜 Rename Flow – Example SQL Chain:</h4>
  <pre>
-- Step 1: Ask for new name input
REPLACE INTO cq_action VALUES (1000, 1001, 0000, 0101, 0, 'Please enter your new name.');
REPLACE INTO cq_action VALUES (1001, 1002, 0000, 0103, 0, '15 1003 New~name:');
REPLACE INTO cq_action VALUES (1002, 4000000, 0000, 0104, 0, '0 0 0');

-- Step 2: Check if current name is in rerole[xxxx] format
REPLACE INTO cq_action VALUES (1003, 1005, 1004, 1990, 0, '');
REPLACE INTO cq_action VALUES (1004, 0000, 0000, 0126, 0, 'Your current name is not a temporary name. Rename is not required.');

-- Step 3: Check if player already applied for rename
REPLACE INTO cq_action VALUES (1005, 1007, 1006, 1991, 0, '');
REPLACE INTO cq_action VALUES (1006, 0000, 0000, 0126, 0, 'You have already submitted a rename request. Please wait for it to be applied.');

-- Step 4: Store new name input into var(0)
REPLACE INTO cq_action VALUES (1007, 1008, 0000, 1524, 0, 'var(0) set %accept0');

-- Step 5: Ask for confirmation (re-type)
REPLACE INTO cq_action VALUES (1008, 1009, 0000, 0101, 0, 'Please enter your new name again.');
REPLACE INTO cq_action VALUES (1009, 1010, 0000, 0103, 0, '15 1011 Re-enter~new~name:');
REPLACE INTO cq_action VALUES (1010, 4000000, 0000, 0104, 0, '0 0 0');

-- Step 6: Compare both entries (var(0) vs second input)
REPLACE INTO cq_action VALUES (1011, 1012, 1014, 1523, 0, 'var(0) == %accept0');

-- Step 7A: If match, apply rename request
REPLACE INTO cq_action VALUES (1012, 1013, 10025049, 1992, 0, '%iter_var_str0');
REPLACE INTO cq_action VALUES (1013, 0000, 0000, 0126, 0, 'Your rename request has been submitted. It will take effect after the next maintenance.');

-- Step 7B: If mismatch
REPLACE INTO cq_action VALUES (1014, 0000, 0000, 0126, 0, 'The two name entries do not match. Please try again.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li><code>1990</code> only allows rename for characters named <code>rerole[xxxx]</code>.</li>
    <li><code>1991</code> prevents duplicate rename requests.</li>
    <li><code>1992</code> will not instantly rename — it marks the request to be applied after restart/maintenance.</li>
    <li>You must input and re-confirm the name to avoid mistakes.</li>
  </ul>


</details>

<details>
  <summary>🏷️ <strong>Legion Rename (Resyn) Check & Action (Type 1993, 1994, 1995)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>This script flow allows legion leaders with default names like <code>resyn[xxxxx]</code> to rename their guild. It checks for leader permission, validates if the name is still default, prevents duplicate requests, and requires confirmation of the new name.</p>

  <h4>📘 Script Types Used:</h4>
  <ul>
    <li><code>1993</code> – Check if current legion name is <code>resyn[xxx]</code></li>
    <li><code>1994</code> – Check if a rename request has already been submitted</li>
    <li><code>1995</code> – Submit the legion rename request (applies on next maintenance)</li>
  </ul>

  <h4>📜 SQL Example – Legion Rename Flow:</h4>
  <pre>
-- Ask for new legion name
REPLACE INTO cq_action VALUES (1000, 1001, 0000, 0101, 0, 'Please enter your new legion name.');
REPLACE INTO cq_action VALUES (1001, 1002, 0000, 0103, 0, '15 1003 New~legion~name:');
REPLACE INTO cq_action VALUES (1002, 4000000, 0000, 0104, 0, '0 0 0');

-- Only the leader can rename
REPLACE INTO cq_action VALUES (1003, 1005, 1004, 1001, 0, 'ranksshow == 1000');
REPLACE INTO cq_action VALUES (1004, 0000, 0000, 0126, 0, 'Only the legion leader can rename the legion.');

-- Check if name is still resyn[xxx]
REPLACE INTO cq_action VALUES (1005, 1007, 1006, 1993, 0, '');
REPLACE INTO cq_action VALUES (1006, 0000, 0000, 0126, 0, 'This legion already has a custom name. Rename is not required.');

-- Check if rename was already submitted
REPLACE INTO cq_action VALUES (1007, 1009, 1008, 1994, 0, '');
REPLACE INTO cq_action VALUES (1008, 0000, 0000, 0126, 0, 'A rename request has already been submitted. Please wait for the next maintenance.');

-- Save first input
REPLACE INTO cq_action VALUES (1009, 1010, 0000, 1524, 0, 'var(0) set %accept0');

-- Confirm re-entry
REPLACE INTO cq_action VALUES (1010, 1011, 0000, 0101, 0, 'Please enter your new legion name again.');
REPLACE INTO cq_action VALUES (1011, 1012, 0000, 0103, 0, '15 1013 Re-enter~legion~name:');
REPLACE INTO cq_action VALUES (1012, 4000000, 0000, 0104, 0, '0 0 0');

-- Compare both inputs
REPLACE INTO cq_action VALUES (1013, 1014, 1017, 1523, 0, 'var(0) == %accept0');

-- Attempt rename
REPLACE INTO cq_action VALUES (1014, 1015, 1016, 1995, 0, '%iter_var_str0');
REPLACE INTO cq_action VALUES (1015, 0000, 0000, 0126, 0, 'Your legion rename request has been submitted. The new name will take effect after the next maintenance.');
REPLACE INTO cq_action VALUES (1016, 0000, 0000, 0126, 0, 'That name is already taken or invalid. Please choose another.');

-- Name mismatch
REPLACE INTO cq_action VALUES (1017, 0000, 0000, 0126, 0, 'The two name entries do not match. Please try again.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Only the leader (<code>ranksshow == 1000</code>) can rename the legion.</li>
    <li>This flow is designed specifically for renaming default-format legions (<code>resyn[xxxxx]</code>).</li>
    <li>The rename will take effect after the next server maintenance.</li>
    <li>All name inputs are compared using <code>var(0)</code> and <code>1523</code>.</li>
  </ul>

  <div style="border-left: 4px solid #3F51B5; background: #e8eaf6; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> You can pair this system with an announcement or mail alert once the rename is successful.
  </div>
</details>


<details>
  <summary>⚔️ <strong>Check Legion War Active (Type 193)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 193</strong> is used to determine if a Legion War is currently ongoing. It behaves similarly to <code>Type 226</code>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = <code>0</code> (required)</li>
    <li><code>param</code> = (leave blank)</li>
  </ul>

  <h4>📜 Example – Check for active legion war:</h4>
  <pre>
-- If legion war is active
REPLACE INTO cq_action VALUES (1000, 1001, 1002, 193, 0, '');

-- If true
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'A legion war is currently in progress!');

-- If false
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'There is no legion war happening right now.');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This script returns <strong>true</strong> if a legion war is ongoing.</li>
    <li>Useful for conditionally enabling NPCs, zones, buffs, or messages related to the event.</li>
    <li>Can be combined with <code>Type 126</code> to inform players through dialog.</li>
  </ul>

  <div style="border-left: 4px solid #607D8B; background: #eceff1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use this script to lock certain features during war hours or activate special buffs/shops for participants only.
  </div>
</details>

<details>
  <summary>📢 <strong>Map Broadcast Messages (Type 320 & 321)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>These script types are used to send announcements to players within a specific map. Similar to <code>Type 303</code>.</p>

  <h4>📘 Script Types:</h4>
  <ul>
    <li><strong>320</strong> – Display message in the <strong>bottom-left corner</strong> of the screen</li>
    <li><strong>321</strong> – Display message in the <strong>center of the screen</strong></li>
  </ul>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = map ID where the message will be shown</li>
    <li><code>param</code> = message text</li>
  </ul>

  <h4>📜 Example – Bottom Left Message (Type 320):</h4>
  <pre>
-- Show message in lower-left of map 1000
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 320, 1000, 'Legion War has started! Join now!');
  </pre>

  <h4>📜 Example – Center Screen Message (Type 321):</h4>
  <pre>
-- Show center-screen message on map 1000
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 321, 1000, 'BOSS has appeared in the center of the battlefield!');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>The messages are visible only to players inside the map defined by <code>data</code>.</li>
    <li>Best used during war events, boss spawns, or timed PvP announcements.</li>
  </ul>

  <div style="border-left: 4px solid #2196F3; background: #e3f2fd; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use <code>320</code> for passive info (e.g., tips or alerts), and <code>321</code> for impactful messages (e.g., countdowns or boss spawn).
  </div>
</details>



<details>
  <summary>🔁 <strong>Recall All Summoned Eudemons (Type 1986)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1986</strong> is used to unsummon all currently <strong>summoned</strong> from the player. This is needed because <strong>modern engines no longer auto-recall summoned Eudemons when switching maps</strong>.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = <code>0</code></li>
    <li><code>param</code> = (leave blank)</li>
  </ul>

  <h4>📜 Example – Unsummon All Pets:</h4>
  <pre>
-- Recall all currently summoned (visible) Eudemons
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 1986, 0, '');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This only affects <strong>summoned Eudemons</strong> (main or secondary), not fused or deployed pets.</li>
    <li>Useful before teleport, dungeon entry, challenge mode, PvP gate, or cutscenes.</li>
    <li>Summon state will need to be reapplied manually if needed after this.</li>
  </ul>

  <div style="border-left: 4px solid #FFC107; background: #fff8e1; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Reminder:</strong> This does NOT cancel fighting pets or states like fusion — only removes the visual summoned Eudemons.
  </div>
</details>


<details>
  <summary>🧝‍♀️ <strong>Servant of the Goddess System (Types 1983, 1984, 1985)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1983.png" alt="1983" style="border:1px solid #ccc; border-radius:6px;">

  <p>These scripts are used to manage the <strong>Servants of the Goddess</strong> system. There are two servants: <strong>Gift Master</strong> (1) and <strong>Spirit Master</strong> (2). Scripts let you check if they’re activated, activate them, and give them EXP.</p>

  <h4>📘 Script Overview:</h4>
  <ul>
    <li><code>1983</code> – Check if a servant is activated (data = 1 or 2)</li>
    <li><code>1984</code> – Activate a servant (data = 1 or 2)</li>
    <li><code>1985</code> – Add EXP to a servant (data = 1 or 2, param = EXP)</li>
  </ul>

  <h4>🧪 Example – Check if Gift Master is Activated:</h4>
  <pre>
REPLACE INTO cq_action VALUES (16000, 16001, 16002, 1983, 1, '');

-- If activated
REPLACE INTO cq_action VALUES (16001, 0000, 0000, 0126, 0, 'Gift Master is already activated.');

-- If not
REPLACE INTO cq_action VALUES (16002, 0000, 0000, 0126, 0, 'Gift Master is not activated.');
  </pre>

  <h4>✨ Example – Activate Servants:</h4>
  <pre>
-- Activate Gift Master
REPLACE INTO cq_action VALUES (16010, 16011, 0000, 1984, 1, '');
REPLACE INTO cq_action VALUES (16011, 0000, 0000, 0126, 0, 'Gift Master has been activated.');

-- Activate Spirit Master
REPLACE INTO cq_action VALUES (16012, 16013, 0000, 1984, 2, '');
REPLACE INTO cq_action VALUES (16013, 0000, 0000, 0126, 0, 'Spirit Master has been activated.');
  </pre>

  <h4>📈 Example – Add EXP to Servants (Type 1985):</h4>
  <pre>
-- Add 1000 EXP to Gift Master
REPLACE INTO cq_action VALUES (16020, 0000, 0000, 1985, 1, '1000');

-- Add 2500 EXP to Spirit Master
REPLACE INTO cq_action VALUES (16021, 0000, 0000, 1985, 2, '2500');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Always check with <code>1983</code> before activating using <code>1984</code>.</li>
    <li>EXP via <code>1985</code> can be tied to quests, kill rewards, achievements, etc.</li>
    <li>You can show progress with <code>0126</code> messages or title unlocks.</li>
  </ul>

  <div style="border-left: 4px solid #8E24AA; background: #f3e5f5; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Use this for Goddess Trial modes, shrine blessings, or divine mission reward scaling.
  </div>
</details>


<details>
  <summary>⚔️ <strong>Set Player PK Mode (Type 1527)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1527</strong> sets the player's PK (player-kill) mode. This allows you to force or script PK mode changes for dungeon rules, arenas, PvP zones, or specific events. Each mode determines who the player can or cannot attack.</p>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><code>data</code> = unused (set as 0)</li>
    <li><code>param</code> = PK mode (0–5)</li>
  </ul>

  <h4>📘 PK Mode Reference:</h4>
  <table cellspacing="0" cellpadding="6">
    <thead>
      <tr><th>Param</th><th>Mode</th><th>Description</th></tr>
    </thead>
    <tbody>
      <tr><td><code>0</code></td><td>Peace Mode</td><td>Attack monsters only. Cannot attack any players.</td></tr>
      <tr><td><code>1</code></td><td>Team Mode</td><td>Attack monsters and players not in your team.</td></tr>
      <tr><td><code>2</code></td><td>Capture Mode</td><td>Attack monsters, red-named (PK) and blue-named (criminal) players only.</td></tr>
      <tr><td><code>3</code></td><td>Free PK Mode</td><td>Attack anyone freely without restriction.</td></tr>
      <tr><td><code>4</code></td><td>Legion Mode</td><td>Attack all except your own legion members.</td></tr>
      <tr><td><code>5</code></td><td>Ally Mode</td><td>Attack all except your legion and allied legions.</td></tr>
    </tbody>
  </table>

  <h4>📜 Example – Set Player to Legion Mode (4):</h4>
  <pre>
REPLACE INTO cq_action VALUES (17000, 0000, 0000, 1527, 0, '4');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>This change is immediate and affects combat behavior instantly.</li>
    <li>Only affects the local player using the script.</li>
    <li>Good for PvP dungeons, special zones, or safe areas (set to 0).</li>
  </ul>

  <div style="border-left: 4px solid #E53935; background: #ffebee; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    ⚠️ <strong>Reminder:</strong> For event maps or boss zones, always reset PK mode back to <code>0 (Peace)</code> after exit.
  </div>
</details>

<details>
  <summary>💇‍♂️ <strong>Hairstyle Unlock System (Type 1974 & 1975)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1974.png" alt="1974" style="border:1px solid #ccc; border-radius:6px;">

  <p>These scripts are used to check and unlock character hairstyles. Hair styles are defined in <code>HairData.ini</code> and <code>DressRoomItem.ini</code> (where <code>type = 2</code>).</p>

  <h4>🧠 Script Functions:</h4>
  <ul>
    <li><code>1974</code> – Check if a specific hairstyle is already unlocked</li>
    <li><code>1975</code> – Unlock a specific hairstyle</li>
  </ul>

  <h4>📘 Hairstyle Source:</h4>
  <ul>
    <li><strong>data</strong> = Hairstyle ID (from <code>HairData.ini</code>)</li>
    <li>These IDs must exist in <code>DressRoomItem.ini</code> as type <code>2</code></li>
  </ul>

  <h4>📜 Example – Check If Hair 225 Is Unlocked:</h4>
  <pre>
REPLACE INTO cq_action VALUES (18000, 18001, 18002, 1974, 225, '');

-- If already unlocked
REPLACE INTO cq_action VALUES (18001, 0000, 0000, 0126, 0, 'This hairstyle is already unlocked!');

-- If not yet unlocked
REPLACE INTO cq_action VALUES (18002, 0000, 0000, 0126, 0, 'This hairstyle is not yet unlocked.');
  </pre>

  <h4>📜 Example – Unlock Hair 225:</h4>
  <pre>
REPLACE INTO cq_action VALUES (18010, 18011, 0000, 1975, 225, '');
REPLACE INTO cq_action VALUES (18011, 0000, 0000, 0126, 0, 'You have unlocked hairstyle 225!');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Hairstyles must be listed in both <code>HairData.ini</code> and <code>DressRoomItem.ini</code> (with type 2).</li>
    <li>Script can be used as a cosmetic reward, fashion shop feature, or quest unlock.</li>
    <li>Use <code>0126</code> to notify player about success or already-unlocked status.</li>
  </ul>

  <div style="border-left: 4px solid #AB47BC; background: #f3e5f5; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Make sure the hair ID exists in <code>cq_hairinfotype</code>. Once unlocked, the data is stored in <code>cq_hairinfo</code>.
  </div>
</details>


<details>
  <summary>🖼️ <strong>Avatar Unlock System (Type 1976 & 1977)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1976.png" alt="1976" style="border:1px solid #ccc; border-radius:6px;">

  <p>These script types are used to check and unlock player avatar icons (also called head icons or portraits). Avatar data must exist in <code>cq_faceinfotype</code>, and unlocked data is stored in <code>cq_faceinfo</code>.</p>

  <h4>📘 Script Functions:</h4>
  <ul>
    <li><code>1976</code> – Check if a specific avatar is already unlocked</li>
    <li><code>1977</code> – Unlock a specific avatar</li>
  </ul>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><strong>data</strong> = Avatar ID (must match an entry in <code>cq_faceinfotype</code>)</li>
    <li><strong>param</strong> = (leave blank)</li>
  </ul>

  <h4>📜 Example – Check If Avatar ID 85 Is Unlocked:</h4>
  <pre>
REPLACE INTO cq_action VALUES (1000, 1001, 1002, 1976, 85, '');

-- If already unlocked
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'This avatar is already unlocked.');

-- If not yet unlocked
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'This avatar is not yet unlocked.');
  </pre>

  <h4>📜 Example – Unlock Avatar ID 85:</h4>
  <pre>
REPLACE INTO cq_action VALUES (1000, 1001, 0000, 1977, 85, '');
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have successfully unlocked avatar 85!');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Avatar IDs must exist in <code>cq_faceinfotype</code></li>
    <li>Unlocked avatars are stored in <code>cq_faceinfo</code></li>
    <li>Ideal for login rewards, battle pass, achievements, or title progression</li>
  </ul>

  <div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
    💡 <strong>Tip:</strong> Avatar icons are managed using <code>cq_faceinfotype</code> and saved per player in <code>cq_faceinfo</code>.
  </div>
</details>


<details>
  <summary>🐲 <strong>Eudemon Skin Unlock System (Type 1972 & 1973)</strong></summary>
  <br>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
    <img src="assets/images/1972.png" alt="1972" style="border:1px solid #ccc; border-radius:6px;">

  <p>These script types manage the unlocking of <strong>Eudemon Skins</strong>. Skins are defined in <code>cq_eudlookinfotype</code>, and once unlocked, are stored in <code>cq_eudlookinfo</code>.</p>

  <h4>📘 Script Overview:</h4>
  <ul>
    <li><strong>1972</strong> – Check if a specific Eudemon skin is unlocked</li>
    <li><strong>1973</strong> – Unlock a specific Eudemon skin</li>
  </ul>

  <h4>🧠 Syntax:</h4>
  <ul>
    <li><strong>data</strong> = Skin ID (from <code>cq_eudlookinfotype</code>)</li>
    <li><strong>param</strong> = (leave blank)</li>
  </ul>

  <h4>📜 Example – Check If Skin ID 22 Is Unlocked:</h4>
  <pre>
REPLACE INTO cq_action VALUES (1000, 1001, 1002, 1972, 22, '');

-- If unlocked
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have already unlocked this Eudemon skin.');

-- If not unlocked
REPLACE INTO cq_action VALUES (1002, 0000, 0000, 0126, 0, 'This Eudemon skin has not been unlocked yet.');
  </pre>

  <h4>📜 Example – Unlock Eudemon Skin ID 22:</h4>
  <pre>
REPLACE INTO cq_action VALUES (1000, 1001, 0000, 1973, 22, '');
REPLACE INTO cq_action VALUES (1001, 0000, 0000, 0126, 0, 'You have successfully unlocked Eudemon skin 22!');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Skin IDs must exist in <code>cq_eudlookinfotype</code></li>
    <li>Unlocks are saved in <code>cq_eudlookinfo</code></li>
    <li>Skins may be tied to limited-time events, promotions, or pet rankings</li>
  </ul>
</details>




<details>
  <summary>🌟 <strong>Divinize Eudemons (Type 1970)</strong></summary>
  <br>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p><strong>Type 1970</strong> is used to <strong>divinize</strong> Eudemons on your character. It targets pets that are <strong>summoned/unsummoned</strong> (visible or stored).</p>

  <h4>🧠 Parameters:</h4>
  <ul>
    <li><code>data</code> defines the scope of pets affected:</li>
    <ul>
      <li><strong>0</strong> — Only summoned and alive Eudemons</li>
      <li><strong>1</strong> — Summoned, including dead ones</li>
      <li><strong>2</strong> — All pets (summoned or not, alive or dead)</li>
    </ul>
    <li><code>param</code> = stat requirements, must match all to trigger:
      <ul>
        <li>Supports: <code>level</code> and <code>starlev</code></li>
        <li>Operators: <code>&lt;</code>, <code>&gt;</code>, <code>&gt;=</code>, <code>==</code></li>
        <li>Use comma to separate conditions</li>
      </ul>
    </li>
  </ul>

  <h4>📜 Example – Apply to All Pets with Level ≥ 1 and Star Level ≥ 4000:</h4>
  <pre>
REPLACE INTO cq_action VALUES (21000, 0000, 0000, 1970, 2, 'level >= 1,starlev >= 4000');
  </pre>

  <h4>✅ Notes:</h4>
  <ul>
    <li>Used for divine upgrade events, evolution quests, or system unlocks.</li>
  </ul>

</details>














---
## 📚 Understanding Variables

This section will explain about the types of variables available in the server. Some variables can be modified, while others are read-only and provided by the system.

These variables can be displayed or checked using dialog windows (Type 101), message boxes (Type 126), and other script actions depending on how they are used in the event system.




<details>
  <summary>🛠️ <strong>System Variables</strong></summary>
  <details style="margin-left: 20px;">
    <summary>📅 <strong>%datestamp (New Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the current server date in YYYYMMDD format (e.g., 20250427).</li>
      <li><strong>Usage:</strong> Useful for date-based rewards, daily event locks, or time-limited quests.</li>
      <li><strong>Important:</strong> Only available in the new engine. The format is intended for internal calculation, not for direct human-readable formatting.</li>
    </ul>

    <h4>💡 Example SQL – Display Current Date</h4>
    <pre>
-- Show the current server date using %datestamp
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%datestamp');
    </pre>

    <p>When displayed using a message box (Type 126), it will appear like this:</p>

    <img src="assets/images/datestamp.png" alt="%datestamp Example" style="max-width:200px; border:1px solid #ccc; border-radius:6px;">

    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
      ⚠️ <strong>New Engine Only</strong>
    </div>
  </details>

  <details style="margin-left: 20px;">
    <summary>⏰ <strong>%time</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the current server timestamp in seconds since Unix epoch (e.g., 1745688972).</li>
      <li><strong>Usage:</strong> Useful for time-based calculations, cooldown tracking, logging events, or precise timing comparisons.</li>
      <li><strong>Important:</strong> The timestamp is intended for internal calculations, not formatted for human reading directly.</li>
    </ul>

    <h4>💡 Example SQL – Display Current Timestamp</h4>
    <pre>
-- Show the current server time (timestamp) using %time
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%time');
    </pre>

    <p>When displayed using a message box (Type 126), it will appear like this:</p>

    <img src="assets/images/timestamp.png" alt="%time Example" style="max-width:200px; border:1px solid #ccc; border-radius:6px;">
  </details>

<details style="margin-left: 20px;">
  <summary>🌟 <strong>%maxeudemon_starlev (New Engine Only)</strong></summary>
  <br>
  <ul>
    <li><strong>Description:</strong> Returns the maximum Eudemon star limit setting. The value is stored as an integer, where 30000 means 300.00 stars.</li>
    <li><strong>Usage:</strong> Useful for comparing pet growth, setting evolution limits, or informing players about star caps in events or promotions.</li>
    <li><strong>Important:</strong> Only available in the new engine. The maximum star value can be modified in the server MSG settings.</li>
  </ul>

  <h4>💡 Example SQL – Display Maximum Eudemon Star Level</h4>
  <pre>
-- Show the maximum allowed Eudemon star level
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%maxeudemon_starlev');
  </pre>

  <p>When displayed using a message box (Type 126), it will appear like this:</p>

  <img src="assets/images/maxeudemon.png" alt="%maxeudemon_starlev Example" style="max-width:200px; border:1px solid #ccc; border-radius:6px;">

  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
</details>

  <details style="margin-left: 20px;">
    <summary>📆 <strong>%date_ywday1 ~ %date_ywday7</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> These variables return the current <strong>week number</strong> based on a custom week start setting. Counting starts from <code>2018-01-01</code>.</li>
      <li><strong>Usage:</strong> Useful for weekly events, reset timers, reward tiers, or weekly PVP rankings.</li>
      <li><strong>Format:</strong> The value returned is a <strong>positive integer</strong> indicating how many full weeks have passed since your chosen day-of-week reference point.</li>
    </ul>

    <h4>📘 Variable Explanation:</h4>
    <table cellspacing="0" cellpadding="6">
      <thead>
        <tr><th>Variable</th><th>Meaning</th><th>Week Start Day</th></tr>
      </thead>
      <tbody>
        <tr><td><code>%date_ywday1</code></td><td>Weekly index if your week starts on <strong>Monday</strong></td><td>Monday</td></tr>
        <tr><td><code>%date_ywday2</code></td><td>Weekly index if your week starts on <strong>Tuesday</strong></td><td>Tuesday</td></tr>
        <tr><td><code>%date_ywday3</code></td><td>Weekly index if your week starts on <strong>Wednesday</strong></td><td>Wednesday</td></tr>
        <tr><td><code>%date_ywday4</code></td><td>Weekly index if your week starts on <strong>Thursday</strong></td><td>Thursday</td></tr>
        <tr><td><code>%date_ywday5</code></td><td>Weekly index if your week starts on <strong>Friday</strong></td><td>Friday</td></tr>
        <tr><td><code>%date_ywday6</code></td><td>Weekly index if your week starts on <strong>Saturday</strong></td><td>Saturday</td></tr>
        <tr><td><code>%date_ywday7</code></td><td>Weekly index if your week starts on <strong>Sunday</strong></td><td>Sunday</td></tr>
      </tbody>
    </table>

    <h4>💡 Example SQL – Weekly Reset Check (Week Starts on Tuesday):</h4>
    <pre>
-- This shows how many weeks have passed since 2018-01-01, treating Tuesday as start of the week
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Week Index (Tuesday Start): %date_ywday2');
    </pre>

    <div style="border-left: 4px solid #03A9F4; background: #e1f5fe; padding: 12px 16px; margin-top: 16px; border-radius: 6px;">
      🧠 <strong>Tip:</strong> Use different <code>%date_ywdayX</code> depending on which day you want your server's week to begin (e.g., Monday for school ranking, Sunday for arena season).
    </div>
  </details>



</details>



<details>
  <summary>🎒 <strong>Item Variables</strong></summary>
  <details style="margin-left: 20px;">
    <summary>📦 <strong>%item_type</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the item type ID of the current item. Must be an item listed in <code>cq_itemtype</code>.</li>
      <li><strong>Usage:</strong> Used for identifying item categories, specific item checks, or triggering item-based events.</li>
      <li><strong>Important:</strong> When checking player-owned items, the value comes from the <code>TYPE</code> field in <code>cq_item</code> table.</li>
    </ul>

    <h4>💡 Example SQL – Display Item Type</h4>
    <pre>
-- Show the item type using %item_type
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%item_type');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🛠️ <strong>%item_data</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the item data value of the current item. Must be an item listed in <code>cq_itemtype</code>.</li>
      <li><strong>Usage:</strong> Used for basic item checks involving item data field.</li>
      <li><strong>Important:</strong> When checking player-owned items, the value comes from the <code>data</code> field in <code>cq_item</code> table.</li>
    </ul>

    <h4>💡 Example SQL – Display Item Data</h4>
    <pre>
-- Show the item data using %item_data
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%item_data');
    </pre>
  </details>

</details>


<details>
  <summary>🧙‍♂️ <strong>NPC Variables</strong></summary>

NPC Variables are used to retrieve information about static or dynamic NPCs in the server. The NPC must exist in <code>cq_npc</code> or <code>cq_dynanpc</code> to access these variables.

<div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin: 16px 0; border-radius:6px;">
  🔧 <strong>Tip:</strong> To modify or update NPC attributes, please refer to <strong>Check or Modify NPC Attributes (Type 201 & 206)</strong>.
</div>

  <details style="margin-left: 20px;">
    <summary>🏷️ <strong>%datastr</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the <code>datastr</code> field from the NPC record. It can contain letters and numbers.</li>
      <li><strong>Usage:</strong> Useful for storing text codes or custom NPC properties.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC DataStr</h4>
    <pre>
-- Show the datastr field using %datastr
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%datastr');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🔢 <strong>%data0 ~ %data3</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns numeric values from the <code>data0</code> to <code>data3</code> fields of the NPC.</li>
      <li><strong>Usage:</strong> Useful for custom configurations like NPC type behavior, event triggers, or dynamic settings.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Data0</h4>
    <pre>
-- Show data0 field using %data0
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%data0');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🧾 <strong>%name</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the name of the NPC.</li>
      <li><strong>Usage:</strong> Display the NPC name dynamically in messages or dialogs.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Name</h4>
    <pre>
-- Show NPC name using %name
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%name');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🆔 <strong>%id</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the unique ID of the NPC.</li>
      <li><strong>Usage:</strong> Useful for identification and targeting in custom scripts.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC ID</h4>
    <pre>
-- Show NPC ID using %id
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%id');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>📍 <strong>%npc_x and %npc_y</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the X and Y coordinates of the NPC on the map.</li>
      <li><strong>Usage:</strong> Useful for teleportation systems, event placement, or dynamic map interactions.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Coordinates</h4>
    <pre>
-- Show NPC X and Y positions
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'X: %npc_x Y: %npc_y');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>👤 <strong>%npc_ownerid</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player ID that owns the NPC (used for summoned or dynamic NPCs).</li>
      <li><strong>Usage:</strong> Useful for pet systems, summoned guards, or player-owned dynamic objects.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Owner ID</h4>
    <pre>
-- Show the owner ID using %npc_ownerid
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%npc_ownerid');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>❤️ <strong>%npc_maxlife</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the maximum life/HP value of the NPC.</li>
      <li><strong>Usage:</strong> Used in battle systems, pet management, or boss status displays.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Max Life</h4>
    <pre>
-- Show maximum life using %npc_maxlife
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%npc_maxlife');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🗺️ <strong>%npc_mapid (New Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the map ID where the NPC is located.</li>
      <li><strong>Important:</strong> Only available in the new engine version.</li>
    </ul>

    <h4>💡 Example SQL – Display NPC Map ID</h4>
    <pre>
-- Show NPC's current map ID
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%npc_mapid');
    </pre>

    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
      ⚠️ <strong>New Engine Only</strong>
    </div>
  </details>

<details style="margin-left: 20px;">
  <summary>🆕 <strong>%newdata1 ~ %newdata8 (New Engine Only)</strong></summary>
  <br>
  <ul>
    <li><strong>Description:</strong> Returns numeric values from extended dynamic NPC data fields. Available only in the new engine.</li>
    <li><strong>Usage:</strong> Useful for additional properties, event states, or advanced dynamic NPC behaviors.</li>
  </ul>

  <h4>💡 Example SQL – Display New Numeric Data</h4>
  <pre>
-- Show new numeric field using %newdata1
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%newdata1');
  </pre>

  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
</details>

<details style="margin-left: 20px;">
  <summary>🆕 <strong>%newdatastr1 ~ %newdatastr2 (New Engine Only)</strong></summary>
  <br>
  <ul>
    <li><strong>Description:</strong> Returns string values from extended dynamic NPC string fields. Available only in the new engine.</li>
    <li><strong>Usage:</strong> Useful for saving special string data like names, statuses, event codes, or owner information.</li>
  </ul>

  <h4>💡 Example SQL – Display New String Data</h4>
  <pre>
-- Show new string field using %newdatastr1
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%newdatastr1');
  </pre>

  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
</details>


</details>






<details>
  <summary>🧑‍💼 <strong>User Variables</strong></summary>

User Variables are used to retrieve information about the player character. This includes identity, position, name, legion information, and other player-specific attributes. Some variables are only available in the new engine version.

  <details style="margin-left: 20px;">
    <summary>🆔 <strong>%user_id</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player's unique user ID.</li>
      <li><strong>Usage:</strong> Useful for identification, tracking, and ownership systems.</li>
    </ul>

    <h4>💡 Example SQL – Display User ID</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%user_id');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🗺️ <strong>%user_map_id / %user_map_x / %user_map_y</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the current map ID and the player's X, Y coordinates on the map.</li>
      <li><strong>Usage:</strong> Useful for teleport systems, event zone checks, and movement tracking.</li>
    </ul>

    <h4>💡 Example SQL – Display Map and Coordinates</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Map: %user_map_id, X: %user_map_x, Y: %user_map_y');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🏠 <strong>%user_home_id (Old Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player's home map ID. Available only in old engine versions.</li>
    </ul>

    <h4>💡 Example SQL – Display Home Map ID</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%user_home_id');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🏰 <strong>%syn_id / %syn_name</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player's legion (guild) ID and name if they belong to one.</li>
    </ul>

    <h4>💡 Example SQL – Display Legion Info</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Legion: %syn_name (%syn_id)');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🧑 <strong>%user_name / %mate_name</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player’s character name and their mate’s name (if married).</li>
    </ul>

    <h4>💡 Example SQL – Display Character Name</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Name: %user_name / Mate: %mate_name');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>📍 <strong>%map_owner_id / %map_owner_type</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the owner ID and owner type of the current map.</li>
    </ul>

    <h4>💡 Example SQL – Display Map Ownership</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Map Owner ID: %map_owner_id, Type: %map_owner_type');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🤝 <strong>%ally_syn0 ~ %ally_syn4 / %enemy_syn0 ~ %enemy_syn4 (Old Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns allied or enemy legion IDs. Available only in the old engine.</li>
    </ul>

    <h4>💡 Example SQL – Display Alliance and Enemy Syn IDs</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Ally0: %ally_syn0, Enemy0: %enemy_syn0');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🎓 <strong>%tutor_exp / %student_exp (Old Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns mentor (tutor) and student contribution experience points.</li>
    </ul>

    <h4>💡 Example SQL – Display Mentor/Student EXP</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Tutor EXP: %tutor_exp, Student EXP: %student_exp');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🧿 <strong>%user_sealmagic (New Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player's current Seal Magic power.</li>
    </ul>

    <h4>💡 Example SQL – Display Seal Magic Power</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%user_sealmagic');
    </pre>

    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
      ⚠️ <strong>New Engine Only</strong>
    </div>
  </details>

  <details style="margin-left: 20px;">
    <summary>🎚️ <strong>%user_level (New Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player’s current level.</li>
    </ul>

    <h4>💡 Example SQL – Display User Level</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%user_level');
    </pre>

    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
      ⚠️ <strong>New Engine Only</strong>
    </div>
  </details>

  <details style="margin-left: 20px;">
    <summary>⚡ <strong>%user_bp (New Engine Only)</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the player’s Battle Power (BP) value.</li>
    </ul>

    <h4>💡 Example SQL – Display Battle Power</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, '%user_bp');
    </pre>

    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
      ⚠️ <strong>New Engine Only</strong>
    </div>
  </details>

 <details style="margin-left: 20px;">
    <summary>🏆 <strong>%halloffamerank</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Celebrity Hall ranking. Returns <code>0</code> if not ranked.</li>
    </ul>

    <h4>💡 Example SQL – Check HoF Rank</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Your Celebrity Hall Rank: %halloffamerank');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🔥 <strong>%godfirerank</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Rank among the 3 Divine Fire Statues.</li>
    </ul>

    <h4>💡 Example SQL – Check God Fire Rank</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'God Fire Rank: %godfirerank');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🟡 <strong>%currentfirepoint</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Current score of the Divine Fire equipped on the player.</li>
    </ul>

    <h4>💡 Example SQL – Show Current Divine Fire Score</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Current Fire Score: %currentfirepoint');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>💀 <strong>%killusertarget_name / %killusertarget_id</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong>Name and ID of the last player killed (or who killed you).</li>
    </ul>

    <h4>💡 Example SQL – Show Last PK Target Info</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'You last killed: %killusertarget_name (ID: %killusertarget_id)');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🔥 <strong>%firelev</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Divine Fire Realm Level.</li>
    </ul>

    <h4>💡 Example SQL – Check Fire Level</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Your Divine Fire Realm Level: %firelev');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🔥 <strong>%firepoint</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Player's historical highest Fire Score.</li>
    </ul>

    <h4>💡 Example SQL – Display Highest Fire Score</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Highest Fire Score: %firepoint');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🎯 <strong>%bonuspoint</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Displays the character’s bonus point total.</li>
    </ul>

    <h4>💡 Example SQL – Display Bonus Points</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Your Bonus Points: %bonuspoint');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>👑 <strong>%allgoddesslevel</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Total goddess tier level sum. Returns 0 if no goddess is unlocked.</li>
    </ul>

    <h4>💡 Example SQL – Display Goddess Total Tier</h4>
    <pre>
REPLACE INTO cq_action VALUES (1000, 0000, 0000, 0126, 0, 'Total Goddess Tier: %allgoddesslevel');
    </pre>
  </details>
</details>




















<details>
  <summary>⚔️ <strong>PK Event Variables</strong></summary>

 PK Event Variables are used specifically inside Daily, Weekly, or Monthly PK Tournament events. These variables help track player performance such as kill count and experience rewards during PK matches.

  <details style="margin-left: 20px;">
    <summary>⚔️ <strong>%pkgame_user_kill</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the number of enemy players the user has killed during a PK tournament match.</li>
      <li><strong>Usage:</strong> Useful for calculating rewards, ranking, or progression inside PK tournaments.</li>
    </ul>

    <h4>💡 Example SQL – Display PK Kill Count</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'PK Kills: %pkgame_user_kill');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🎖️ <strong>%pkgame_user_balance_exp</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the experience points rewarded to the player at the end of a PK tournament match.</li>
      <li><strong>Usage:</strong> Useful for balancing event rewards or displaying end-of-event results.</li>
    </ul>

    <h4>💡 Example SQL – Display PK Balance EXP</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'PK Balance EXP: %pkgame_user_balance_exp');
    </pre>
  </details>

</details>


<details>
  <summary>🛡️ <strong>Legion Variables</strong></summary>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
 Legion Variables are used inside the legion system. These variables allow checking and interacting with legion resources such as available funds, ranks, or contributions.
  <details style="margin-left: 20px;">
    <summary>💰 <strong>%available_fund</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the amount of legion fund available for allocation or distribution.</li>
      <li><strong>Usage:</strong> Useful for legion management systems where leaders or officers need to distribute resources among members.</li>
    </ul>

    <h4>💡 Example SQL – Display Available Legion Fund</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Available Legion Fund: %available_fund');
    </pre>
  </details>

</details>

<details>
  <summary>🗺️ <strong>Map Variables</strong></summary>
  <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
 Map Variables are used to manage map-specific data during gameplay. Some variables are temporary and exist only in server memory, while others are permanent based on database information.

  <details style="margin-left: 20px;">
    <summary>🧮 <strong>%map_iter_var_data0 ~ %map_iter_var_data19</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Temporary numeric values stored per map (index 0 to 19).</li>
      <li><strong>Important:</strong> These values are not saved into the database. They exist only inside MsgServer memory. After server restart, all values reset to 0. You must set the value manually. To modify these variables, use Map Variable Actions (Types 316, 317, 318, 319).</li>
    </ul>

    <h4>💡 Example SQL – Display Map Data Variable</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Map Data 0: %map_iter_var_data0');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>📝 <strong>%map_iter_var_str0 ~ %map_iter_var_str19</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Temporary string values stored per map (index 0 to 19).</li>
      <li><strong>Important:</strong> These values are not saved into the database. They exist only inside MsgServer memory. After server restart, all values reset to empty. You must set the value manually. To modify these variables, use Map Variable Actions (Types 316, 317, 318, 319).</li>
    </ul>

    <h4>💡 Example SQL – Display Map String Variable</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Map String 0: %map_iter_var_str0');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🧭 <strong>%backmapid / %backmap_x / %backmap_y</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the backup map ID and X, Y coordinates configured for the map.</li>
      <li><strong>Note:</strong> These values are stored inside <code>cq_map</code> database table.</li>
    </ul>

    <h4>💡 Example SQL – Display Backup Map Information</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Backmap ID: %backmapid, X: %backmap_x, Y: %backmap_y');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>🌍 <strong>%mapid / %map_name</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the current map ID and map name where the player or NPC is located.</li>
      <li><strong>Note:</strong> These values are stored inside <code>cq_map</code> database table.</li>
    </ul>

    <h4>💡 Example SQL – Display Map ID and Name</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Map ID: %mapid, Map Name: %map_name');
    </pre>
  </details>

</details>


<details>
  <summary>🌍 <strong>Global Variables</strong></summary>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Global Variables are used to store server-wide persistent values. These variables are shared across all players, maps, and events. Numeric variables use the <code>cq_vardata</code> table, while string variables use the <code>cq_varstrdata</code> table. To modify these variables, refer to <strong>Global Variable Control (Type 191 & 192)</strong>.</p>

  <details style="margin-left: 20px;">
    <summary>🧮 <strong>%public_var_data1 ~ %public_var_data99</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Global numeric variables stored in <code>cq_vardata</code>. Supports index 1 to 99.</li>
      <li><strong>Usage:</strong> Useful for server event tracking, counters, server-wide settings, or broadcast states.</li>
    </ul>

    <h4>💡 Example SQL – Display Global Numeric Variable</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Global Data 0: %public_var_data1');
    </pre>
  </details>

  <details style="margin-left: 20px;">
    <summary>📝 <strong>%public_var_str1 ~ %public_var_str99</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Global string variables stored in <code>cq_varstrdata</code>. Supports index 1 to 99.</li>
      <li><strong>Usage:</strong> Useful for server-wide flags, notices, event statuses, or admin messages.</li>
    </ul>

    <h4>💡 Example SQL – Display Global String Variable</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Global String 0: %public_var_str1');
    </pre>
  </details>

</details>


<details>
  <summary>👹 <strong>Monster Variables</strong></summary>
    <div style="background:#fff8e1; border-left:4px solid #FFC107; padding:12px 16px; margin-bottom:16px; border-radius:6px;">
    ⚠️ <strong>New Engine Only</strong>
  </div>
  <p>Monster Variables are used to retrieve the position (X, Y coordinates) where a monster was killed. These variables are only available during monster death event triggers. Must be placed on monster kill actions.</p>

  <details style="margin-left: 20px;">
    <summary>📍 <strong>%killmonster_x / %killmonster_y</strong></summary>
    <br>
    <ul>
      <li><strong>Description:</strong> Returns the X and Y coordinates where the monster was killed.</li>
      <li><strong>Usage:</strong> Useful for spawning rewards, teleport points, or triggering special effects at the kill location.</li>
      <li><strong>Important:</strong> Must be used inside scripts related to monster events (e.g., after monster killed).</li>
    </ul>

    <h4>💡 Example SQL – Display Monster Kill Coordinates</h4>
    <pre>
REPLACE INTO `cq_action` VALUES (1000, 0000, 0000, 0126, 0, 'Kill Position: %killmonster_x / %killmonster_y');
    </pre>
  </details>

</details>

---


<footer style="text-align:center; padding: 20px; font-size: 14px; color: #777;">
  🧠 Built with ❤️ by DuaSelipar • © 2025 • <a href="https://www.facebook.com/profile.php?id=61554036273018" target="_blank">facebook</a>
</footer>
