### [ValidateLocation]

### ValidateLocation OPCODE 0x61

<blockquote>

		public ValidateLocation(Creature creature)
	{
		_objectId = creature.getObjectId();
		_x = creature.getX();
		_y = creature.getY();
		_z = creature.getZ();
		_heading = creature.getHeading();
		if(creature != null){
			if(creature.getName().equalsIgnoreCase("test1")){
				System.out.println("Validate location use Test1");
			}
		}
	}
</blockquote>

Sent by the server to the client to synchronize the position of the npc, monster, player  


So far I have only dealt with monsters:

If a monster receives this packet while moving, it will ignore it.

If the monster stands still and the desynchronization distance does not exceed 197 Units or 3.7 Meters, the monster will reach the synchronization point

If the distance exceeds 197 Units then he will move the monster like a teleport and the monster will also be moved if it is moving at that moment!!!! And the monster will continue to move towards the point if it is in its field of vision