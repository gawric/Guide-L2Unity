### The server sends information to the client that the inventory has changed.

### InventoryUpdate OPCODE 0x27

```
	@Override
	protected final void writeImpl()
	{
		writeC(0x27);
		writeH(_items.size());
		for (ItemInfo temp : _items)
		{
			Item item = temp.getItem();
			writeH(temp.getChange().ordinal()); <---- ItemState
			writeH(item.getType1());
			writeD(temp.getObjectId());
			writeD(item.getItemId());
			writeD(temp.getCount());
			writeH(item.getType2());
			writeH(temp.getCustomType1());
			writeH(temp.getEquipped());
			writeD(item.getBodyPart());
			writeH(temp.getEnchant());
			writeH(temp.getCustomType2());
			writeD(temp.getAugmentationBoni());
			writeD(temp.getMana());
		}
	}
```
```
public enum ItemState
{
	UNCHANGED,
	ADDED,
	MODIFIED,
	REMOVED
}
```


The item has 4 states. Which state to send depends on the java server, here is an example:  
When purchasing a product in a sell/buy store, the l2jAcis server sends InventoryUpdate first, and then sends a complete list of all items, it looks like this:  
InventoryUpdate  
InventoryUpdate  
ItemList  
Second example:  
When farming mobs, the server sends InventoryUpdate with the ->MODIFIED or ADD flag. But it does not send a list of all items  