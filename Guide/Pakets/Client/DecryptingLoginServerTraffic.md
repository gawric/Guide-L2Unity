### The login server package decryptor is used in the Init server login package

The server sends packets in this order:  
1. it does not encrypt the first 2 bytes since it tells the client the packet size  
2. NewCrypt.encXORPass(raw, offset, size, Rnd.nextInt()); //The server uses the XOR operation  
3. _staticCrypt.crypt(raw, offset, size); //blowfish encryption  

The server uses blowfish encryption key for encryption, it takes a static one, that is, it is already known to both the client and the server in advance, this key:  

```
```private static final byte[] STATIC_BLOWFISH_KEY ={
		(byte) 0x6b,
		(byte) 0x60,
		(byte) 0xcb,
		(byte) 0x5b,
		(byte) 0x82,
		(byte) 0xce,
		(byte) 0x90,
		(byte) 0xb1,
		(byte) 0xcc,
		(byte) 0x2b,
		(byte) 0x6c,
		(byte) 0x55,
		(byte) 0x6c,
		(byte) 0x6c,
		(byte) 0x6c,
		(byte) 0x6c
	};
	
```
```

here is an example of an encryption block on the server  

```
```
private void encryptBlock(byte[] src, int srcIndex, byte[] dst, int dstIndex){
	int xl = bytesTo32bits(src, srcIndex);
	int xr = bytesTo32bits(src, srcIndex + 4);
    xl ^= P[0];
	for (int i = 1; i < ROUNDS; i += 2)
	{
		xr ^= func(xl) ^ P[i];
		xl ^= func(xr) ^ P[i + 1];
	}
	xr ^= P[ROUNDS + 1];
	bits32ToBytes(xr, dst, dstIndex);
	bits32ToBytes(xl, dst, dstIndex + 4);
}
```
```

</blockquote>

In order for us to decrypt this information, we need to do the reverse process: first decrypt and then remove xor    

XOR Example  

<blockquote>

public static bool decXORPass(byte[] packet) {
    int blen = packet.Length;

    if (blen < 1 || packet == null)
        return false; // TODO: Handle error or throw exception

    // Get XOR key
    int xorOffset = 8;
    uint xorKey = 0;
    xorKey |= packet[blen - xorOffset];
    xorKey |= (uint)(packet[blen - xorOffset + 1] << 8);
    xorKey |= (uint)(packet[blen - xorOffset + 2] << 16);
    xorKey |= (uint)(packet[blen - xorOffset + 3] << 24);

    // Decrypt XOR encrypted portion
    int offset = blen - xorOffset - 4;
    uint ecx = xorKey;
    uint edx = 0;

    while (offset > 2) // Adjust this condition if needed
    {
        edx = (uint)(packet[offset + 0] & 0xFF);
        edx |= (uint)(packet[offset + 1] & 0xFF) << 8;
        edx |= (uint)(packet[offset + 2] & 0xFF) << 16;
        edx |= (uint)(packet[offset + 3] & 0xFF) << 24;

        edx ^= ecx;
        ecx -= edx;

        packet[offset + 0] = (byte)((edx) & 0xFF);
        packet[offset + 1] = (byte)((edx >> 8) & 0xFF);
        packet[offset + 2] = (byte)((edx >> 16) & 0xFF);
        packet[offset + 3] = (byte)((edx >> 24) & 0xFF);
        offset -= 4;
    }
    return true;
}

</blockquote>

BLOWFISH  

```
```
private void decryptBlock(byte[] src, uint srcIndex, byte[] dst, uint dstIndex){
    uint xl = BytesTo32bits(src, srcIndex);
    uint xr = BytesTo32bits(src, srcIndex + 4);
    xl ^= P[ROUNDS + 1];
    for (int i = ROUNDS; i > 0; i -= 2)
    {
        xr ^= F(xl) ^ P[i];
        xl ^= F(xr) ^ P[i - 1];
    }
    xr ^= P[0];
    Bits32ToBytes(xr, dst, dstIndex);
    Bits32ToBytes(xl, dst, dstIndex + 4);
}
```


and now the package should be readable int,bytes,double  

![Client Class Decode](https://i.ibb.co/4gZL6Pj5/loginXor.png)