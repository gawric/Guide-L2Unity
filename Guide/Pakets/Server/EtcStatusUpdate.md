### The package sends additional information about the character[EtcStatusUpdate]

### EtcStatusUpdate OPCODE 0xF3

I haven’t fully disassembled the package yet, but I know that when you enter the game, this package sends information about Penalty
<blockquote>

		writeD(_player.getCharges());
		writeD(_player.getWeightPenalty().ordinal());
		writeD((_player.getBlockList().isBlockingAll() || _player.isChatBanned()) ? 1 : 0);
		writeD(_player.isInsideZone(ZoneId.DANGER_AREA) ? 1 : 0);
		writeD((_player.getWeaponGradePenalty() || _player.getArmorGradePenalty() > 0) ? 1 : 0);
		writeD(_player.isAffected(EffectFlag.CHARM_OF_COURAGE) ? 1 : 0);
		writeD(_player.getDeathPenaltyBuffLevel());
		
</blockquote>


_player.getDeathPenaltyBuffLevel()  //transmits only the level and does not transmit its ID number; it is in the client