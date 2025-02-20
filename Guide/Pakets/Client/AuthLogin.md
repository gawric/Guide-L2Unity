### I will not describe here how and where this cryptographic algorithm is used

### AuthLogin OPCODE 0x08

<blockquote>
@Override
	protected void readImpl()
	{
		_loginName = readS().toLowerCase();
		_playKey2 = readD();
		_playKey1 = readD();
		_loginKey1 = readD();
		_loginKey2 = readD();
	}
	

	protected void runImpl()
	{
		if (getClient().getAccountName() != null)
			return;
		
		getClient().setAccountName(_loginName);
		getClient().setSessionId(new SessionKey(_loginKey1, _loginKey2, _playKey1, _playKey2));
		
		// Add the client.
		LoginServerThread.getInstance().addClient(_loginName, getClient());
	}
</blockquote>

In l2j itself we receive the already decrypted data and check that there is such a user and his session keys