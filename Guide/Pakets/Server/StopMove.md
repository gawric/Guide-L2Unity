### This is not a simple package, although it seems quite small.[StopMove]

### StopMove OPCODE 0x47

<blockquote>
	protected final void writeImpl()
	{
		writeC(0x47);
		writeD(_objectId);
		writeD(_x);
		writeD(_y);
		writeD(_z);
		writeD(_heading);
	}
</blockquote>

General description

After sending, I assumed that it should stop any movement or attack, but unfortunately this is not true. Even if this packet is sent, the attack will not stop until the monster is dead, only after that the packet will be able to completely stop the movement of the attack. It stops movement towards the target, there is no attack on the target
This packet sends information about where the player or monster's ending point should be before stopping. But often the client does not reach this point if he does not have time, he turns on the teleport or speeds up the movement in order to get to the point. If this does not help, it stops where it can and then gets ValidatePosition