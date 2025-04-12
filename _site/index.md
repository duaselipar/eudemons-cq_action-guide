# 🧠 Eudemons Online `cq_action` Table Guide

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

<div style="border-left: 4px solid #4CAF50; background: #e8f5e9; padding: 10px; margin: 10px 0;">
  <strong>✅ Tip:</strong> Helpful advice for doing things better or more easily.
</div>


 - [Create Dialog](type/101.md)
 - [Type : 200 - 299](type/101.md)
 - [Type : 300 - 399](type/101.md)
 - [Type : 400 - 499](type/101.md)
 - [Type : 500 - 599](type/101.md)
 - [Type : 600 - 699](type/101.md)
 - [Type : 700 - 799](type/101.md)
 - [Type : 800 - 899](type/101.md)
 - [Type : 1000 - 1099](type/101.md)
 - [Type : 1100 - 1199](type/101.md)
 - [Type : 5001 (VIP)](type/101.md)
 - [Type : 1500 - 1599](type/101.md)
 - [Type : 2000 - 2099](type/101.md)
 - [Type : 2100 - 2199](type/101.md)
 - [Type : 3000 - 3099](type/101.md)
 - [Type : 4000 - 4099](type/101.md)
 - [Type : 8000 - 8099](type/101.md)

  - [Color Font Generator](color-generator.html)

