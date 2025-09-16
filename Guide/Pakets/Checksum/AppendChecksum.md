### Today I got a shock when I tried to connect to the PTS through the Unity client   

### AppendChecksum or why it didn't work in my client   

Today I tried to connect to the PTS server and was surprised that even the login and password verification could not pass the verification on the PTS server   
I opened the debugger and looked at what the official client sends that the Unity client does not send   
Here is a sample of the original client   

ReuqestServerList - Original Packet   

0 = 34 size packet   
1 = 0 size packet   
2 = 5 id packet   
   
3 = -72 int   
4 = 98   
5 = -23   
6 = 97   
   
7 = 66 int    
8 = -86   
9 = -94   
10 = -77   
   
11 = 0   
12 = 0   
13 = 0   
14 = 0   
15 = 0   
16 = 0   
17 = 0   
18 = -41 appendCheckSum   
19 = -2   
20 = -56   
21 = 75   
22 = 0   
23 = 0   
24 = 0   
25 = 0   
26 = 0   
27 = 0   
28 = 0   
29 = 0   

ReuqestServerList - Unity Packet   

0 = 34 size packet   
1 = 0 size packet   
2 = 5 id packet   

3 = -72 int   
4 = 98   
5 = -23   
6 = 97   

7 = 66 int    
8 = -86   
9 = -94   
10 = -77   

22 = 0   
23 = 0   
24 = 0   
25 = 0   
18 = -41 appendCheckSum   
19 = -2   
20 = -56   
21 = 75   

as you can see there are no zero bytes, this is of course not a problem. But I was very surprised that the checksum is placed absolutely differently.   
The original uses in-band checksum   
The Unity client uses out‑of‑band/trailer checksum   
If you use java servers it can skip both options but PTS works only with 1 option in-band checksum   
   
<blockquote>   
public static boolean verifyChecksum(byte[] raw, final int offset, final int size)   
	{   
		// check if size is multiple of 4 and if there is more then only the checksum  
		if ((size & 3) != 0 || size <= 4)   
			return false;   
		
		long chksum = 0;
		int count = size - 4;
		long check = -1;
		int i;
		
		for (i = offset; i < count; i += 4)
		{
			check = raw[i] & 0xff;
			check |= raw[i + 1] << 8 & 0xff00;
			check |= raw[i + 2] << 0x10 & 0xff0000;
			check |= raw[i + 3] << 0x18 & 0xff000000;
			
			chksum ^= check;
		}
		
		check = raw[i] & 0xff;
		check |= raw[i + 1] << 8 & 0xff00;
		check |= raw[i + 2] << 0x10 & 0xff0000;
		check |= raw[i + 3] << 0x18 & 0xff000000;
		
		return check == chksum;
	}
</blockquote>
   
here is an example of code and when the client sends a packet it always comes to the values check = 0 , chksum = 0   
the unity client check = -222202 , chksum = -222202   


Here is my solution for Unity I remove the old appenChecksum and add a new one that will result in check and checksum == 0   

<blockquote>
public static void AppendChecksumWord(List<byte> buf, int offset = 2, int step = 4, bool pad = false)   
{
    try
    {
        if (buf == null) throw new ArgumentNullException(nameof(buf));
        if (offset < 0) throw new ArgumentOutOfRangeException(nameof(offset));
        if (step <= 0 || step > 8) throw new ArgumentOutOfRangeException(nameof(step), "step must be in range 1..8");

        int n = buf.Count;

   
        if (n < offset)
        {
            if (pad)
            {
                for (int i = 0; i < offset - n; i++) buf.Add(0);
                n = buf.Count;
            }
            else
            {
                throw new ArgumentException("Buffer too small for offset; use pad=true to auto-extend");
            }
        }

    
        int rem = (n - offset) % step;
        if (rem != 0)
        {
            if (pad)
            {
                int need = step - rem;
                for (int i = 0; i < need; i++) buf.Add(0);
                n = buf.Count;
            }
            else
            {
                throw new ArgumentException("length-offset is not multiple of step; use pad=true to auto-extend");
            }
        }


        ulong xorAcc = 0UL;
        ulong mask = (step == 8) ? ulong.MaxValue : ((1UL << (8 * step)) - 1UL);

        for (int i = offset; i < n; i += step)
        {
            ulong word = 0;
            for (int b = 0; b < step; b++)
            {
                word |= ((ulong)buf[i + b]) << (8 * b); // little-endian
            }
            xorAcc ^= word;
        }


        ulong appendValue = xorAcc & mask;


        for (int b = 0; b < step; b++)
        {
            buf.Add((byte)((appendValue >> (8 * b)) & 0xFF));
        }
     
    }
    catch (Exception ex)
    {
        Debug.LogWarning("AppendChecksumWord НЕ Сработал Ошибка! " + ex.ToString());

    }
}
</blockquote>

An example of its use   

<blockquote>
 protected void BuildPacketExperimental(int addZeroBytes)
 {
     try
     {
         _buffer.Insert(0, _packetType);

         //Padding for balance checksum equls 0
         WriteCheckSum();

         // Padding for checksum
         WriteI(0);

         for(int i=0; i < addZeroBytes; i++)
         {
             WriteB((byte)0);
         }

         PadBuffer();

         byte[] array = _buffer.ToArray();
         SetData(array);
     }
     catch (Exception ex)
     {
         Debug.LogError("ClientPacket->BuildPacketExperimental: " + ex.Message);
     }

 }
 </blockquote>