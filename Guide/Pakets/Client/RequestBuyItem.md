### Sent by the client when he clicks the buy button in the store. For example, in a weapons store [RequestBuyItem]

### RequestBuyItem OPCODE 0x1f

```
```private static final int BATCH_LENGTH = 8; // length of the one item
@Override
	protected void readImpl()
	{

		_listId = readD();
		int count = readD();
		if (count <= 0 || count > Config.MAX_ITEM_IN_PACKET || count * BATCH_LENGTH != _buf.remaining())
			return;
		
		_items = new IntIntHolder[count];
		for (int i = 0; i < count; i++)
		{
			int itemId = readD();
			int cnt = readD();
			
			if (itemId < 1 || cnt < 1)
			{
				_items = null;
				return;
			}
			
			_items[i] = new IntIntHolder(itemId, cnt);
		}
	}
```
```

The server also has a limitation in this place; it asks that all products be exactly 8 bytes. id and quantity and there is no alignment of packets by 8 or 4!

