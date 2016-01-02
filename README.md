# H.248 Parser
NOTICE: This is **NOT** an open-source project. It's a simple tutorial explaining how to generate a C# H.248 coder/decoder.

I've been looking around the internet for a binary H.248 parser for a while but failed to find one.
So I tried the best next thing which is generate a parser based on the ASN.1 specification for the protocol.
To do so I used OSS Nokalva's "ASN.1/C# Compile". Note that this is a comercial software but offers a 30-days trial.

##Create your own H.248 parser
1. Grab the relevant .asn files from this repository ( In my case it was the H248v3.asn)
2. Download and install OSS Nokalva's compiler from here:

	http://www.oss.com/asn1/products/asn1-download.html
3. Locate the installed asn1cs.exe. Usually found at

	C:\Program Files (x86)\OSS Nokalva\asn1csharp\win32.trial\4.2.0\bin\asn1cs.exe
4. Run the next command in CMD:

	asn1cs.exe -ber h248v3.asn

  _*replace "h238v3.asn" with the full path if the files aren't both in the same directory_
5. Locate the newly created "H248v3" folder in the working directory. This contains all H.248 classes. Add those to your solution.
6. Also add the asn1csrt.dll from 
	C:\Program Files (x86)\OSS Nokalva\asn1csharp\win32.trial\4.2.0\bin\asn1csrt.dll
as the decoder depends on it.

##Usage
Parsing bytes of an H.248 packet into a "MegacoMessage":
```C#
byte[] packetBytes = new byte[]{....};
MegacoMessage parsedMessage = new MegacoMessage();
BerCodec codec = new BerCodec();
using (MemoryStream memStream = new MemoryStream(packetBytes))
{
	codec.Decode(memStream, parsedMessage);
}
```
You can also create a h.248 message from scrate using the MegacMessage and it's propeties then encode it using codec.Encode method.

###Additional links
I grabbed the .asn files from wiresharks repository at

https://github.com/wireshark/wireshark/tree/master/asn1/h248

The example pcap I've included in the repository, named mm_2010_11_15.pcap, is from pcapr:

http://www.pcapr.com/view/nos/2011/6/2/13/mm_2010_11_15.pcap.html

Not that OSS Nokalva  offers other compilers as well - Java, C++.
I Assume you can use them to create H.248 parsers just as easily.
