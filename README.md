# eudemons-action-guide


The cq_action table contains the following fields:
INSERT INTO `cq_action` VALUES (1000, 0000, 0000, 0501, 759100, '');
id: This is the unique identifier for each action. It must be unique.
id_next: By default, this is the next action ID that will be executed. If a condition is used, and it evaluates true, the flow continues to this ID.
id_nextfail: Mainly used when conditions are applied. If the condition is false, the flow continues to this ID.
type: The type of event. (See list of types below.)
data: This stores data related to the event type. For example, if the type is giving an item, this field holds the item ID.
param: Similar in use to data, but is generally used for descriptive text. For example, it could contain NPC dialog or item usage messages.
