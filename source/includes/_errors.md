# Errors

<aside class="notice">Each response form the Enigma Bridge service will contain a numerical code, as well as a short text with explanation of the error. 

The response code for successful processiong of a request is 36864 (0x9000).
</aside>

Clients using Enigma Bridge API may encounter the following error codes:


Error Code | Meaning
---------- | -------
4101 (0x1005) | object capacity reached
4102 (0x1006) | timeout
4111 (0x100F) | service shutting down
4213 (0x1075)  | object processing limit reached
4262 (0x10A6)  | processing queues full
4304 (0x10D0)  | no authorization tokens left
8364 (0x20AC)  | integrity error / attack detected
8380 (0x20BC)  | request not authorized
16504 (0x4078)  | object already exists
16512 (0x4080)  | public key not available
32769 (0x8001)  | unknown operation
32772 (0x8004)  | operation not available
32818 (0x8032)  | input data length incorrect
32819 (0x8033)  | invalid key provided
32820 (0x8034)  | invalid handle/APIkey
32822 (0x8036)  | missing or invalid argument
32823 (0x8037)  | request JSON parsing failed
32824 (0x8038)  | request JSON version not supported
32825 (0x8039)  | request JSON data invalid
32826 (0x803A)  | request JSON missing nonce
32827 (0x803B)  | invalid object
32828 (0x803C)  | unsupported operation
32829 (0x803D)  | incorrect data padding
32830 (0x803E)  | object format error
32833 (0x8041)  | object format error
32835 (0x8043)  | object format error
32839 (0x8047)  | data is incorrect
32840 (0x8048)  | object format error
32841 (0x8049)  | MAC length incorrect
32842 (0x804A)  | object format error
32844 (0x804C)  | object format error
32846 (0x804E)  | encrypt data padding error
32847 (0x804F)  | encrypt data padding error
32851 (0x8053)  | incorrect key length
32852 (0x8054)  | data too long
32853 (0x8055)  | malformed data
32856 (0x8058)  | authorization cards required
32866 (0x8062)  | invalid user context
32871 (0x8067)  | object classification is incorrect
32872 (0x8068)  | invalid API key
32873 (0x8069)  | unrecognized client
32899 (0x8083)  | invalid operation type
32909 (0x808D)  | key component length incorrect
32910 (0x808E)  | key component index incorrect
32911 (0x808F)  | key component already set
32912 (0x8090)  | key component not set
32918 (0x8096)  | length of application key is incorrect
32919 (0x8097)  | length of communication key is incorrect
32921 (0x8099)  | incorrect usage flags or keys not generated
32943 (0x80AF)  | data encryption error
32948 (0x80B4)  | OTP code length request incorrect
32953 (0x80B9)  | authentication method not allowed
36864 (0x9000)  | Success
41046 (0xA056)  | incorrect length of OTP key
41059 (0xA063)  | too many password tries
41060 (0xA064)  | incorrect password length
41061 (0xA065)  | incorrect password
41062 (0xA066)  | too many HOTP tries
41121 (0xA0A1)  | wrong MAC on data
41136 (0xA0B0)  | incorrect HOTP code
41137 (0xA0B1)  | too many account tries
41138 (0xA0B2)  | keys stored in the user object are incorrect
41139 (0xA0B3)  | OTP counter overflow
41142 (0xA0B6)  | incorrect user ID / malicious request
41146 (0xA0BA)  | unknown authentication method
41170(0xA0D2)  | no authorization credits left
61458(0xF012)  | processor failure
61465(0xF019)  | server not ready
61488(0xF030)  | invalid authorization token
61489(0xF031)  | input data too long
61493(0xF035)  | invalid object

