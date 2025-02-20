### Received by the client if the AuthLogin request was successful and the server offers to enter the lobby

### CharSelectInfo OPCODE 0x13

<blockquote>
final int size = _slots.length;
		
		writeC(0x13);
		writeD(size);
		
		long lastAccess = 0L;
		
		if (_activeId == -1)
		{
			for (int i = 0; i < size; i++)
				if (lastAccess < _slots[i].getLastAccess())
				{
					lastAccess = _slots[i].getLastAccess();
					_activeId = i;
				}
		}
		
		for (int i = 0; i < size; i++)
		{
			CharSelectSlot slot = _slots[i];
			
			writeS(slot.getName());
			writeD(slot.getCharId());
			writeS(_loginName);
			System.out.println("_loginName " + _sessionId);
			writeD(_sessionId);
			System.out.println("AccountId " + _sessionId);
			writeD(slot.getClanId());
			writeD(0x00); // Builder level
			
			writeD(slot.getSex());
			writeD(slot.getRace());
			writeD(slot.getBaseClassId());
			
			writeD(0x01); // active ??
			
			writeD(slot.getX());
			writeD(slot.getY());
			writeD(slot.getZ());
			
			writeF(slot.getCurrentHp());
			writeF(slot.getCurrentMp());
			
			writeD(slot.getSp());
			writeQ(slot.getExp());
			writeD(slot.getLevel());
			
			writeD(slot.getKarma());
			writeD(slot.getPkKills());
			writeD(slot.getPvpKills());
			
			writeD(0x00);
			writeD(0x00);
			writeD(0x00);
			writeD(0x00);
			writeD(0x00);
			writeD(0x00);
			writeD(0x00);
			
			writeD(slot.getPaperdollObjectId(Paperdoll.HAIRALL));
			writeD(slot.getPaperdollObjectId(Paperdoll.REAR));
			writeD(slot.getPaperdollObjectId(Paperdoll.LEAR));
			writeD(slot.getPaperdollObjectId(Paperdoll.NECK));
			writeD(slot.getPaperdollObjectId(Paperdoll.RFINGER));
			writeD(slot.getPaperdollObjectId(Paperdoll.LFINGER));
			writeD(slot.getPaperdollObjectId(Paperdoll.HEAD));
			writeD(slot.getPaperdollObjectId(Paperdoll.RHAND));
			writeD(slot.getPaperdollObjectId(Paperdoll.LHAND));
			writeD(slot.getPaperdollObjectId(Paperdoll.GLOVES));
			writeD(slot.getPaperdollObjectId(Paperdoll.CHEST));
			writeD(slot.getPaperdollObjectId(Paperdoll.LEGS));
			writeD(slot.getPaperdollObjectId(Paperdoll.FEET));
			writeD(slot.getPaperdollObjectId(Paperdoll.CLOAK));
			writeD(slot.getPaperdollObjectId(Paperdoll.RHAND));
			writeD(slot.getPaperdollObjectId(Paperdoll.HAIR));
			writeD(slot.getPaperdollObjectId(Paperdoll.FACE));
			
			writeD(slot.getPaperdollItemId(Paperdoll.HAIRALL));
			writeD(slot.getPaperdollItemId(Paperdoll.REAR));
			writeD(slot.getPaperdollItemId(Paperdoll.LEAR));
			writeD(slot.getPaperdollItemId(Paperdoll.NECK));
			writeD(slot.getPaperdollItemId(Paperdoll.RFINGER));
			writeD(slot.getPaperdollItemId(Paperdoll.LFINGER));
			writeD(slot.getPaperdollItemId(Paperdoll.HEAD));
			writeD(slot.getPaperdollItemId(Paperdoll.RHAND));
			writeD(slot.getPaperdollItemId(Paperdoll.LHAND));
			writeD(slot.getPaperdollItemId(Paperdoll.GLOVES));
			writeD(slot.getPaperdollItemId(Paperdoll.CHEST));
			writeD(slot.getPaperdollItemId(Paperdoll.LEGS));
			writeD(slot.getPaperdollItemId(Paperdoll.FEET));
			writeD(slot.getPaperdollItemId(Paperdoll.CLOAK));
			writeD(slot.getPaperdollItemId(Paperdoll.RHAND));
			writeD(slot.getPaperdollItemId(Paperdoll.HAIR));
			writeD(slot.getPaperdollItemId(Paperdoll.FACE));
			
			writeD(slot.getHairStyle());
			writeD(slot.getHairColor());
			writeD(slot.getFace());
			
			writeF(slot.getMaxHp());
			writeF(slot.getMaxMp());
			
			writeD((slot.getAccessLevel() > -1) ? ((slot.getDeleteTimer() > 0) ? (int) ((slot.getDeleteTimer() - System.currentTimeMillis()) / 1000) : 0) : -1);
			writeD(slot.getClassId());
			writeD((i == _activeId) ? 0x01 : 0x00);
			writeC(Math.min(127, slot.getEnchantEffect()));
			writeD(slot.getAugmentationId());
		}
		getClient().setCharSelectSlot(_slots);
</blockquote>


General information about created characters and their position in the lobby by key number in the array