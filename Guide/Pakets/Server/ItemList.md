### The server sends this packet in different situations. But it is believed that the package is not simple and is sent when there are no active actions [ItemList]

### ItemList OPCODE 0x1b

```
	@Override
	protected final void writeImpl()
	{
		writeC(0x1b);
		writeH(_showWindow ? 0x01 : 0x00);
		writeH(_items.size());
		
		for (ItemInstance temp : _items)
		{
			Item item = temp.getItem();
			
			writeH(item.getType1());
			writeD(temp.getObjectId());
			writeD(temp.getItemId());
			writeD(temp.getCount());
			writeH(item.getType2());
			writeH(temp.getCustomType1());
			writeH(temp.isEquipped() ? 0x01 : 0x00);
			writeD(item.getBodyPart());
			writeH(temp.getEnchantLevel());
			writeH(temp.getCustomType2());
			writeD((temp.isAugmented()) ? temp.getAugmentation().getId() : 0x00);
			writeD(temp.getMana());
		}
	}
```




The package is sent to update the entire player interface. Sends a list of all his belongings, as well as the things he is wearing.   
The list does not contain cell position numbers, so it is added randomly to the inventory.  
You can also send a flag to open the inventory after receiving and processing the package  