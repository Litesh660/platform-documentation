---
id: inventory
name: Inventory
title: Inventory
tags:
    - API
---

# Inventory

Inventory is a CoreObject that represents a container of InventoryItems. Items can be added directly to an inventory, picked up from an ItemObject in the world, or transferred between inventories. An Inventory may be assigned to a Player, and Players may have any number of Inventories.

## Properties

| Property Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `owner` | [`Player`](player.md) | The Player who currently owns the inventory. May be `nil`. Change owners with `Assign()` or `Unassign()`. | Read-Only |
| `slotCount` | `integer` | The number of unique inventory item stacks this inventory can hold. Zero or negative numbers indicate no limit. | Read-Only |
| `emptySlotCount` | `integer` | The number of slots in the inventory that do not contain an inventory item stack. | Read-Only |
| `occupiedSlotCount` | `integer` | The number of slots in the inventory that contain an inventory item stack. | Read-Only |

## Functions

| Function Name | Return Type | Description | Tags |
| -------- | ----------- | ----------- | ---- |
| `Assign(Player player)` | `None` | Sets the owner property of the inventory to the specified player. When a networked inventory is assigned to a player, only that player's client will be able to access the inventory contents. | None |
| `Unassign()` | `None` | Clears the owner property of the inventory. The given inventory will now be accessible to all clients. | None |
| `GetItem(integer slot)` | [`InventoryItem`](inventoryitem.md) | Returns the contents of the specified slot. Returns `nil` if the slot is empty. | None |
| `GetItems([table parameters])` | `Array`<[`InventoryItem`](inventoryitem.md)> | Returns a list of all items in the inventory. Returns an empty list if the inventory is empty. <br/>Supported parameters include: <br/>`itemAssetId (string)`: If specified, filters the results by the specified item asset reference. Useful for getting all inventory items of a specific type. | None |
| `ClearItems()` | `None` | Removes all items from the inventory. | None |
| `SortItems()` | `None` | Reorganizes inventory items into sequential slots starting with slot 1. Does not perform any consolidation of item stacks of the same type. Use `ConsolidateItems()` first if this behavior is desired. | None |
| `ConsolidateItems()` | `None` | Combines stacks of inventory items into as few slots as possible based on the `maximumStackCount` of each item. Slots may be emptied by this operation, but are otherwise not sorted or reorganized. | None |
| `CanResize(integer newSize)` | `boolean` | Checks if there are enough slots for all current contents of the inventory if the inventory were to be resized. Returns `true` if the inventory can be resized to the new size or `false` if it cannot. | None |
| `Resize(integer newSize)` | `boolean` | Changes the number of slots of the inventory. There must be enough slots to contain all of the items currently in the inventory or the operation will fail. This operation will move items into empty slots if necessary, but it will not consolidate item stacks, even if doing so would create sufficient space for the operation to succeed. Use `ConsolidateItems()` first if this behavior is desired. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. | None |
| `CanAddItem(string itemAssetId, [table parameters])` | `boolean` | Checks for room to add the specified item to this inventory. If the item can be added to the inventory, returns `true`. If the inventory is full or the item otherwise cannot be added, returns `false`. Supports the same parameters as `AddItem()`. | None |
| `AddItem(string itemAssetId, [table parameters])` | `boolean` | Attempts to add the specified item to this inventory. If the item was successfully added to the inventory, returns `true`. If the inventory was full or the item otherwise wasn't added, returns `false`. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of items to create and add to the inventory. Defaults to 1. <br/>`slot (integer)`: Attempts to create the item directly into the specified slot. If unspecified, the inventory will either look to stack this item with other items of the same `itemAssetId`, or will look to find the first empty slot. <br/>`customProperties (table)`: Applies initial property values to any dynamic properties on the item. Attempting to specify a property that does not exist will yield a warning. Attempting to specify a property that does exist and is not dynamic will raise an error. Providing a property value of the incorrect type will raise an error. | None |
| `CanPickUpItem(ItemObject item, [table parameters])` | `boolean` | Checks for room in an existing stack or free inventory slots to add the given item to the inventory. If the item can be added to the inventory, returns `true`. If the inventory is full or the item otherwise cannot be added, returns `false`. Supports the same parameters as `PickUpItem()`. | None |
| `PickUpItem(ItemObject item, [table parameters])` | `boolean` | Creates an inventory item from an ItemObject that exists in the world, taking 1 count from the ItemObject. Destroys the ItemObject if the inventory item is successfully created and the ItemObject count has been reduced to zero. Returns `true` if the item was picked up. Returns `false` and logs a warning if the item could not be picked up. <br/>Supported parameters include: <br/>`count (integer)`: Determines the number of items taken from the specified ItemObject. If the ItemObject still has a count greater than zero after this operation, it is not destroyed. Defaults to 1. <br/>`all (boolean)`: If `true`, picks up all of the given item's count instead of just 1. Overrides `count` if both are specified. | None |
| `CanMoveItem(integer fromSlot, integer toSlot, [table parameters])` | `boolean` | Checks if an item can be moved from one slot to another. If the item can be moved, returns `true`. Returns `false` if it cannot be moved. Supports the same parameters as `MoveItem()`. | None |
| `MoveItem(integer fromSlot, integer toSlot, [table parameters])` | `boolean` | Moves an inventory item and its entire count from one slot to another. If the target slot is empty, the stack is moved. If the target slot is occupied by a matching item, the stack will merge as much as it can up to its `maximumStackCount`. If the target slot is occupied by a non-matching item, the stacks will swap. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of items to move. When swapping with another stack containing a non-matching item, this operation will fail unless `count` is the entire stack. | None |
| `CanRemoveItem(string itemAssetId, [table parameters])` | `boolean` | Checks if an item can be removed from the inventory. If the item can be removed, returns `true`. Returns `false` if it cannot be removed. Supports the same parameters as `RemoveItem()`. | None |
| `RemoveItem(string itemAssetId, [table parameters])` | `boolean` | Deletes 1 item of the specified asset from the inventory. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be removed. Defaults to 1. <br/>`all (boolean)`: If `true`, removes all of the specified items instead of just 1. Overrides `count` if both are specified. | None |
| `CanRemoveSlot(integer slot, [table parameters])` | `boolean` | Checks if an item can be removed from an inventory slot. If the item can be removed, returns `true`. Returns `false` if it cannot be removed. Supports the same parameters as `RemoveSlot()`. | None |
| `RemoveSlot(integer slot, [table parameters])` | `boolean` | Deletes the inventory item and its entire count from the specified inventory slot. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be removed. Defaults to the total count in the specified slot. | None |
| `CanDropItem(string itemAssetId, [table parameters])` | `boolean` | Checks if an item can be dropped from the inventory. If the item can be dropped, returns `true`. Returns `false` if it cannot be dropped. Supports the same parameters as `DropItem()`. | None |
| `DropItem(string itemAssetId, [table parameters])` | `boolean` | Removes 1 item of the specified asset from the inventory and creates an ItemObject with the item's properties. Spawns the ItemObject at the position of the inventory in the world, or at the position of the owner player's feet if the inventory has been assigned to a player. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be dropped. Defaults to 1. <br/>`all (boolean)`: If `true`, drops all of the specified items instead of just 1. Overrides `count` if both are specified. <br/>`dropTo (ItemObject)`: Specifies a pre-existing ItemObject to drop the items onto. Doing this will add to that ItemObject's count. If the ItemObject's maximum stack count would be exceeded by this operation, it will fail, and this function will return false. <br/>`parent (CoreObject)`: Creates the new ItemObject as a child of the specified CoreObject. Can only be used if `dropTo` is not specified. <br/>`position (Vector3)`: Specifies the world position at which the ItemObject is spawned, or the relative position if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`rotation (Rotation or Quaternion)`: Specifies the world rotation at which the ItemObject is spawned, or the relative rotation if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`scale (Vector3)`: Specifies the world scale with which the ItemObject is spawned, or the relative scale if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`transform (Transform)`: Specifies the world transform at which the ItemObject is spawned, or the relative transform if a parent is specified. Can only be used if `dropTo` is not specified, and is mutually exclusive with `position`, `rotation`, and `scale`. <br/>Additional parameters supported by `World.SpawnAsset()` may also be supported here. | None |
| `CanDropSlot(integer slot, [table parameters])` | `boolean` | Checks if an item can be dropped from an inventory slot. If the item can be dropped, returns `true`. Returns `false` if it cannot be dropped. Supports the same parameters as `DropSlot()`. | None |
| `DropSlot(integer slot, [table parameters])` | `boolean` | Drops the entire contents of a specified slot, creating an ItemObject with the item's properties. Spawns the ItemObject at the position of the inventory in the world, or at the position of the owner player's feet if the inventory has been assigned to a player. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. If the full item count is successfully dropped, the slot will be left empty.<br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be dropped. If not specified, the total number of items in this slot will be dropped. <br/>`dropTo (ItemObject)`: Specifies a pre-existing ItemObject to drop the items onto. Doing this will add to that ItemObject's count. If the ItemObject's max stack count would be exceeded by this operation, it will fail, and this function will return false. <br/>`parent (CoreObject)`: Creates the new ItemObject as a child of the specified CoreObject. Can only be used if `dropTo` is not specified. <br/>`position (Vector3)`: Specifies the world position at which the ItemObject is spawned, or the relative position if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`rotation (Rotation or Quaternion)`: Specifies the world rotation at which the ItemObject is spawned, or the relative rotation if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`scale (Vector3)`: Specifies the world scale with which the ItemObject is spawned, or the relative scale if a parent is specified. Can only be used if `dropTo` is not specified. <br/>`transform (Transform)`: Specifies the world transform at which the ItemObject is spawned, or the relative transform if a parent is specified. Can only be used if `dropTo` is not specified, and is mutually exclusive with `position`, `rotation`, and `scale`. <br/>Additional parameters supported by `World.SpawnAsset()` may also be supported here. | None |
| `CanGiveItem(string itemAssetId, Inventory recipient, [table parameters])` | `boolean` | Checks if an item can be transferred to the specified recipient inventory. If the item can be transferred, returns `true`. Returns `false` if it cannot be transferred. Supports the same parameters as `GiveItem()`. | None |
| `GiveItem(string itemAssetId, Inventory recipient, [table parameters])` | `boolean` | Transfers an item, specified by item asset ID, from this inventory to the given recipient inventory. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be transferred. Defaults to 1. <br/>`all (boolean)`: If `true`, transfers all of the specified items instead of just 1. Overrides `count` if both are specified. | None |
| `CanGiveSlot(integer slot, Inventory recipient, [table parameters])` | `boolean` | Checks if a slot can be transferred to the specified recipient inventory. If the item can be transferred, returns `true`. Returns `false` if it cannot be transferred. Supports the same parameters as `GiveSlot()`. | None |
| `GiveSlot(integer slot, Inventory recipient, [table parameters])` | `boolean` | Transfers the entire stack of a given slot to the given recipient inventory. Returns `true` if the operation succeeded. Returns `false` and logs a warning if the operation failed. <br/>Supported parameters include: <br/>`count (integer)`: Specifies the number of the item to be transferred. If not specified, the total number of items in this slot will be transferred. | None |

## Events

| Event Name | Return Type | Description | Tags |
| ----- | ----------- | ----------- | ---- |
| `ownerChangedEvent` | [`Event`](event.md)<[`Inventory`](inventory.md) inventory> | Fired when the inventory's owner has changed. | None |
| `resizedEvent` | [`Event`](event.md)<[`Inventory`](inventory.md) inventory> | Fired when the inventory's size has changed. | None |
| `changedEvent` | [`Event`](event.md)<[`Inventory`](inventory.md) inventory, `integer` slot> | Fired when the contents of an inventory slot have changed. This includes when the item in that slot is added, given, received, dropped, moved, resized, or removed. | None |
| `itemPropertyChangedEvent` | [`Event`](event.md)<[`Inventory`](inventory.md) inventory, [`InventoryItem`](inventoryitem.md) item, `string` propertyName> | Fired when an inventory item's dynamic custom property value has changed. | None |