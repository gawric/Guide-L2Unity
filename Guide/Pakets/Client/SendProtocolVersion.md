### Sent by the client to obtain an encryption key so that it can connect to the gameserver [VersionCheck]

### SendProtocolVersion OPCODE 0x00


<blockquote>
	protected void readImpl()
	{
		_version = readD(); Contains the client protocol number
	}
	
	protected void runImpl()
	{
		switch (_version)
		{
			case 737:
			case 740:
			case 744:
			case 746:
				break;
			default:
				getClient().close((L2GameServerPacket) null);
				break;
		}
	}
</blockquote>



We receive the bytes and extract from them the number of the client who wants to connect. If we find it in the allowed list, we send a  [VersionCheck] package