###The packet is sent to the user request I want to start logging into the server [SendProtocolVersion]

###VersionCheck OPCODE 0x00

Its appearance in the l2j server
<blockquote>
	private final byte[] _key;
	
	public VersionCheck(byte[] key)
	{
		_key = Arrays.copyOfRange(key, 0, 8);
	}
	
	@Override
	public void writeImpl()
	{
		writeC(0x00); 
		writeC(0x01);
		writeB(_key);
		writeD(Config.USE_BLOWFISH_CIPHER ? 0x01 : 0x00);
		writeD(0x01);
	}
</blockquote>

byte[] key <--- its original size is 16 bytes
Config.USE_BLOWFISH_CIPHER ? 0x01 : 0x00 <--- Will we use encryption?


The key is divided into 2 parts, one static one is stored on the server and in the client. The second part is generated by the server and sent to the client in this packet

To authorize on the server, we encrypt and send the [AuthLogin] packet