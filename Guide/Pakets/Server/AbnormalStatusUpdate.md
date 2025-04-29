### Sends data about what effects should be applied to the player [AbnormalStatusUpdate]

### AbnormalStatusUpdate OPCODE 0x7f

<blockquote>

		writeH(_effects.size() + _toggles.size());
		
		for (EffectHolder holder : _effects)
		{
			writeD(holder.getId());
			writeH(holder.getValue());
			writeD((holder.getDuration() == -1) ? -1 : holder.getDuration() / 1000);
		}
		
		for (EffectHolder holder : _toggles)
		{
			writeD(holder.getId());
			writeH(holder.getValue());
			writeD(-1);
		}
		
</blockquote>


Transmits information to the client what effects should be applied. In my case, I get information about Herbs after killing an npc. The information includes skillid/skill_level/duration (sec)