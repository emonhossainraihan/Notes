```js
//supported encoding:  `ascii`, `utf8`, `base64`, `binary`, and `hex`
// encoding 
function b64(str) {
    return new Buffer(str).toString('base64')
        .replace(/=/g, '')
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
}

function encode(h, p) {
    const headerEnc = b64(JSON.stringify(h))
    const payloadEnc = b64(JSON.stringify(p))
    return `${headerEnc}.${payloadEnc}`
}

console.log(encode(`{alg: sha}`, `{data:iloveyou}`))

// decoding
function decode(jwt) {
    const [headerB64, payloadB64] = jwt.split('.');
    // These supports parsing the URL safe variant of Base64 as well.
    const headerStr = new Buffer(headerB64, 'base64').toString();
    const payloadStr = new Buffer(payloadB64, 'base64').toString();
    return {
        header: JSON.parse(headerStr),
        payload: JSON.parse(payloadStr)
    };
}

console.log(decode('InthbGc6IHNoYX0i.IntkYXRhOmlsb3ZleW91fSI'))

```

## Signing and Validating Tokens

```js
import jwt from 'jsonwebtoken';
const payload = {
    sub: "1234567890",
    name: "Emon Hossain",
    admin: true
};
```

### HS256: HMAC + SHA-256

HMAC signatures require a shared secret. Any string will do

```js
const secret = 'my-secret';
const signed = jwt.sign(payload, secret, {
    algorithm: 'HS256',
    expiresIn: '5s' // if ommited, the token will not expire
});

console.log({ signed })

// avoid crashing
try {
    const decoded = jwt.verify(signed, secret, {
        // Never forget to make this explicit to prevent
        // signature stripping attacks
        algorithms: ['HS256'],
    });
    console.log({ decoded })
} catch (e) {
    console.log(e)
}

```

### RS256: RSASSA + SHA256

Signing and verifying RS256 signed tokens is just as easy. The only difference lies in the use of
a private/public key pair rather than a shared secret.

**Generate a private key:**

`openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048`

**Derive the public key from the private key:**

`openssl rsa -pubout -in private_key.pem -out public_key.pem`

```js
import fs from 'fs';

const privateRsaKey = await fs.readFileSync('./private_key.pem');
const publicRsaKey = await fs.readFileSync('./public_key.pem');




const signedRsa = jwt.sign(payload, privateRsaKey, {
    algorithm: 'RS256',
    expiresIn: '5s'
});

console.log({ signedRsa });

const decodedRsa = jwt.verify(signedRsa, publicRsaKey, {
    algorithms: ['RS256']
});

console.log({ decodedRsa });
```

### ES256: ECDSA using P-256 and SHA-256

The math behind the algorithm is different,
though, so the steps to generate the keys are different as well. The `P-256` in the name of this
algorithm tells us exactly which version of the algorithm to use

**Generate a private key (prime256v1 is the name of the parameters used
to generate the key, this is the same as P-256 in the JWA spec):**

`openssl ecparam -name prime256v1 -genkey -noout -out ecdsa_private_key.pem`

**Derive the public key from the private key:**

`openssl ec -in ecdsa_private_key.pem -pubout -out ecdsa_public_key.pem`
