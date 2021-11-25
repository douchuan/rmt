[EPID](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-enhanced-privacy-id-epid-security-technology.html)

EPID: Intel Enhanced Privacy ID

EPID 解决的问题是：用组签名的方式，可以安全的认证设备，并授予相应level的权限。对于一批IoT设备来说，认证
很重要，但个性不重要。

A second limitation with PKI is the inability to remain anonymous while still being granted access.  Because one public certificate contains the key owner’s name and information, the ownership of the secured information is inherently known, and if the same device is verified multiple times, its activity could be tracked. Usage of PKI for signed and encrypted emails is useful in this scenario where it is desired for the users to be identified.  The recipient installs the public certificate of the sender, and when opening the message has a level of trust that the sender signed these emails using a protected matching private key. 

As devices increasingly play roles in requesting authentication to systems, there is a greater need for both devices and users to be anonymous.  While a valid attestation is required, it is not a requirement that the device be individually identified or that the device provide any additional details other than the minimum amount of information necessary to prove that they are a genuine member of a trusted device family.  Taking this approach allows devices to access a system based on their approved level of access and not any personal information like a MACID or who they are. In other words, in the Intel® EPID scheme, if a hundred authentic signatures are verified, the verifier would not be able to determine whether one hundred devices were authenticated, or if the same device was authenticated one hundred times.

Authentication vs Identification

Gaining access to a system should not always require user identification.  The intent behind requesting access to a system is to obtain access; providing only a minimal, certifiable proof that access has been granted.  Each user might require a certain level of anonymity based on specific use-cases.  Take, as an example, a medical wristband device that monitors sleep habits of someone that is experiencing insomnia.  For the individual, it is important to ensure that the data is provided to the doctors for analysis without allowing anyone else to potentially identify them or their private medical data.

Identification 能识别用户身份，是一种涉及隐私的强认证机制。

For users accessing services, the authentication process is owned by the access provider, which unfortunately often ties access rights directly to an account identifier, which is usually then linked directly to additional account details that the user may want to hold private.  Unfortunately, most systems today require a user or device to actually identify themselves in a way that can in fact be traced back to the original user with every transaction.  An example in software would be a username.  For a device, it might be a MACID or even a public key provided that was stored on secure storage.  To prevent this from occurring, the ability must exist by which a user can effectively and rightfully use a system without being required to provide any information that can be immediately linked to themselves.   One example would be a toll both.  A user should be able to pass through the booth because they were previously issued a valid RFID tag, however no personal information should be required, and the user is not directly known in any transaction or tracking on that device.  If the requirement is to trace whom is travelling through the toll booth(道路收费), then that right is reserved by the access provider, however there are instances when authentication should be separated from identification.

Authentication vs Identication: 比如银行，用户提供给银行Identification, 银行用一套Authentication process验证Identification

What is EPID?
Enhanced Privacy Identification (EPID) is an implementation ISO 20008 from Intel that addresses two of the problems with PKI Security Scheme:  anonymity and membership revocation.  Intel includes EPID keys in many of its processors, starting with chipset series 5 in 2008 which includes all Intel® Core™ processor family products.  In 2016, Intel as a certified EPID Key Generation Facility, announced that it has distributed over 4.5 billion EPID keys since 2008. 

todo: memership revocation，是吊销了这一组的全部设备吗？还是吊销了组中的某个独立设备，如果是吊销了组中
的某个独立设备，说明EPID还是有Identification的属性。

The first improvement over PKI provides a user or device “Direct Anonymous Attestation” which provides the ability to authenticate a device for a given level of access while allowing the device to remain anonymous.  This is accomplished by introducing the concept of a Group level membership authentication scheme.  Instead of a 1:1 public to private key assignment for an individual user, EPID allows a group of membership private keys to be associated together, and linked to one public group key.  This public EPID group public key can be used to verify the signature produced by any EPID member private key in the group.  Most importantly, no one, including the issuer, has any way to know the identity of the user.  Only the member device has access to the private key, and will validate only using a properly provisioned EPID signature.

todo：是一个组共享一个private key吗？这风险很大

The second security improvement that EPID provides is the ability to revoke an individual device by detecting a compromised signature or key.  If the private key used by a device has been compromised or stolen, allowing the EPID ecosystem to recognize this allows the device to be revoked as well as prevent any future forgery.  During a security exchange, the EPID protocol requires that members perform mathematical proofs to indicate that they could not have created any of the signatures that have been flagged on a signature revocation list.  This built in revocation feature allows devices or even entire groups of devices to be instantly flagged for revocation, instantly being denied service. It allows anonymous devices to be revoked on the basis of a signature alone, which allows an issuer to ban a device from a group without ever knowing which device was banned.

可以理解为清晰数据的过程：把独立设备信息抹去，只留下组信息；而对于不安全设备，设置黑明单。

Intel® EPID Roles

There are three roles in the EPID security ecosystem.  Firstly, the Issuer is the authority group that assigns or issues EPID Group IDs and Keys to individual platforms; similar devices that should be grouped together from an access level perspective.  The issuer manages group membership and maintains current versions of all revocation lists. Using a newly generated private key for the group, the issuer generates one group public key, and as many EPID member private keys as requested, all of which are paired with the one group public key.  The Member role is an end device, and represents one individual member in a group of many members, all sharing the same level of access.  Finally, the Verifier role serves as the gatekeeper:  checking and verifying EPID signatures generated by platforms ultimately ensuring they belong to the correct group.  Using the EPID group public key, the verifier is able to validate the EPID signature of any member in the group with no knowledge of membership identity.

Issuer 知道一切，device Identificaiton
Verifier 只知道组
Memeber 知道组，和private key

Issuer相当于操作系统，知道一切事情；Memeber相当于一个独立的文件；Verifer只有组权限



