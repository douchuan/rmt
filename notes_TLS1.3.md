HTTP/2


### [Taking Transport Layer Security (TLS) to the next level with TLS 1.3](https://www.microsoft.com/security/blog/2020/08/20/taking-transport-layer-security-tls-to-the-next-level-with-tls-1-3/)

TLS 1.3 is the latest version of the internet’s most deployed security protocol, which encrypts data to provide a secure communication channel between two endpoints. TLS 1.3 eliminates obsolete cryptographic algorithms, enhances security over older versions, and aims to encrypt as much of the handshake as possible.

TLS 1.3 now uses just 3 cipher suites, all with perfect forward secrecy (PFS), authenticated encryption and additional data (AEAD), and modern algorithms. This addresses challenges with the IANA TLS registry defining hundreds of cipher suite code points, which often resulted in uncertain security properties or broken interoperability.

The new TLS version also improves privacy by using a minimal set of cleartext protocol bits on the wire, which helps prevent protocol ossification and will facilitate the deployment of future TLS versions. In addition, in TLS 1.3, content length hiding is enabled by a minimal set of cleartext protocol bits. This means that less user information is visible on the network.

In previous TLS versions, client authentication exposed client identity on the network unless it was accomplished via renegotiation, which entailed extra round trips and CPU costs. In TLS 1.3, client authentication is always confidential.


### [An Overview of TLS 1.3 – Faster and More Secure](https://kinsta.com/blog/tls-1-3/)

[RFC8446](https://tools.ietf.org/html/rfc8446)

Speed Benefits of TLS 1.3

TLS and encrypted connections have always added a slight overhead when it comes to web performance. HTTP/2 definitely helped with this problem, but TLS 1.3 helps speed up encrypted connections even more with features such as TLS false start and Zero Round Trip Time (0-RTT).

To put it simply, with TLS 1.2, two round-trips have been needed to complete the TLS handshake. With 1.3, it requires only one round-trip, which in turn cuts the encryption latency in half. This helps those encrypted connections feel just a little bit snappier than before.

Another advantage of is that in a sense, it remembers! On sites you have previously visited, you can now send data on the first message to the server. This is called a “zero round trip.” (0-RTT). And yes, this also results in improved load time times.

![](res/tls-1.3-handshake-performance.png)


Improved Security With TLS 1.3

A big problem with TLS 1.2 is that it’s often not configured properly it leaves websites vulnerable to attacks. TLS 1.3 now removes obsolete and insecure features from TLS 1.2, including the following:

- SHA-1
- RC4
- DES
- 3DES
- AES-CBC
- MD5
- Arbitrary Diffie-Hellman groups — CVE-2016-0701
- EXPORT-strength ciphers – Responsible for FREAK and LogJam

Because the protocol is in a sense more simplified, this makes it less likely for administrators and developers to misconfigure the protocol. 


### [wolfSSL](https://www.wolfssl.com/)


