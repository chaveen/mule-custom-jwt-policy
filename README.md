# MuleSoft Anypoint Platform Custom JWT Policy

## Overview

This Policy implements the following checks on a JSON Web Token contained in Authorization header of HTTP request:

- Issuer
- Audience
- Expiration
- Signature

If these checks are violated, a *401* response is given and the request is discarded.

## Support and Compatibility

### OAuth 2.0
OAuth 2.0 (JWT)
- To be applied to a protected resource 
- Token based on [rfc7519](https://tools.ietf.org/html/rfc7519)

This policy does **not** support opaque bearer (by reference) token, as this policy verifies the JWToken based on the signature rather than an introspection/validation endpoint.

### Cryptography Algorithms
Policy supports these algorithms for digital signature verification: 

#### Keys

- HmacSHA256, HmacSHA384 and HmacSHA512
- SHA256withRSA, SHA384withRSA and SHA512withRSA

#### Certificates

- HmacSHA256, HmacSHA384 and HmacSHA512

### Token Providers:
- Azure AD
- [jwt.io](https://jwt.io/) (For testing)

### Tested with MuleSoft Anypoint Platform

- Crowd Nov 2017 Release
- Mule 3.9.0

## Convention and Standards

### JWKS Service

- Azure AD [login.microsoftonline.com/common/discovery/keys](http://login.microsoftonline.com/common/discovery/keys)

### Bearer JWT

HTTP Authorization header must have following form:

    Bearer <jwt>

like this example:

    Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ

## Configuration

The policy configuration contains several input parameters:

### JWKS Service

Use if you want to obtain the secret or public key from a JWKS Service. The following parameter are required to configure the key service:

+ *keys_host* -
+ *keys_base_path* -
+ *keys_port* -

Keys will be cached, the following configure are required to configure the cache:

+ *cache_entries* -
+ *cache_ttl* -
+ *cache_interval* -

### secret or public key

+  *Secret* - (to be implemented) specifies a secret (in case of HmacSHA algorithms) or a public key (SHAwithRSA algorithms) for JWT digital signature algorithm. A public key is inserted without BEGIN and END parts, i.e.:

		MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArLrCUilXj+ggjh/kq/EhEsAXWhyF511k
		tdddFxuOts6ZEcq9u9OP1WnXO14qvZwsd4RBJ3RVm9bMQQzlproMlhkjihsz4ETQS8Ko3e3N0j6+
		is+jwX5hOVRu7WrD+iqE8AcoNtkTf8YwntHqWGMxzQSl57VQ7NSf4a/VSSBKW3oZy0tYQMZECZow
		aLlfjgPibrw9TGwYPceF0e203HuF9fSfqvlGSrr8QPDmwT3Tvp96yF3nwTDiTGdL1YTSUI8SFjzF
		STNVCTKc9P0e9MMdE28nZL9NDPWmi/DnYs6t32uolMc2erEd5OjSQ7Rry58Jt6IaURC93xuN9wir
		mxp8UQIDAQAB
	  
**Note**: A JWT token can be signed with any of the supported algorithms but the policy will use a parameter *Secret* in all cases so only one algorithm group will work.

### Token validations

+  *Issuer* - defines an issuer of JWT token
+  *Audience* - requested audience of JWT token.

Note: Tests need to be updated and are failing.


