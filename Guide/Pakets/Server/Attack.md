### The package is sent to the game, npc, monster, signaling that the blow has already occurred, indicating who is hitting and who is being hit, the coordinates of the one who hit, and general information about whether he missed or hit the amount of damage.[Attack]

### Attack OPCODE 0x05

<blockquote>

	protected final void writeImpl()
	{
		writeC(0x05);
		
		writeD(_attackerId);
		writeD(_hits[0]._targetId);
		writeD(_hits[0]._damage);
		writeC(_hits[0]._flags);
		writeD(_x);
		writeD(_y);
		writeD(_z);
		writeH(_hits.length - 1);
		
		if (_hits.length > 1)
		{
			for (int i = 1; i < _hits.length; i++)
			{
				writeD(_hits[i]._targetId);
				writeD(_hits[i]._damage);
				writeC(_hits[i]._flags);
			}
		}
	}
</blockquote>

General description of flags

<blockquote>
	public static final int HITFLAG_SS = 0x10;
	public static final int HITFLAG_CRIT = 0x20;
	public static final int HITFLAG_SHLD = 0x40;
	public static final int HITFLAG_MISS = 0x80;
</blockquote>