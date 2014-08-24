Digest::PSHA1
==============

Calculates Pseudorandom SHA1 as defined in http://tools.ietf.org/html/rfc5246
(5.  HMAC and the Pseudorandom Function)

This algorithm is used for signing XML documents when the RequestedProofToken
is either http://docs.oasis-open.org/ws-sx/ws-trust/200512/CK/PSHA1 or
http://schemas.xmlsoap.org/ws/2005/02/trust/CK/PSHA1.

Thanks to Leandro Boffi (http://leandrob.com/) for the nodejs version and a great blog.

## Example Usage ##

```perl6
    use Digest::PSHA1;
    use MIME::Base64;

    # Extract base64'd binary secret of a RequestSecurityToken request and a
    # RequestSecurityTokenResponse, like from such a structure:
    # <Entropy>
    #     <BinarySecret Type="http://schemas.xmlsoap.org/ws/2005/02/trust/Nonce">
    #         grrlUUfhuNwlvQzQ4bV6TT3wA8ieZPltIf4+H7nIvCE=
    #     </BinarySecret>
    # </Entropy>

    # Obtain the decoded versions
    my $client-secret = MIME::Base64.decode-str($client-binary-secret);
    my $server-secret = MIME::Base64.decode-str($server-binary-secret);

    # Build the key to sign an XML document
    my $key     = psha1($client-secret, $server-secret);
    my $key_b64 = MIME::Base64.encode($key, :oneline);

    # TODO: Show how to actually sign a document using the generated key
```

## Functions ##

 -  `sub psha1-hex($secret, $seed, $blocksize = 256 --> Str)`

    Returns the hex stringified output of psha1.

 -  `sub psha1($secret, $seed, $blocksize = 256 --> Buf)`

    Computes the PSHA1 of the passed information.

    `$secret` and `$seed` can either be Str or Blob objects; if they are Str they
    will be encoded as ascii.

