# ECDSA 공개키 인코딩

ECDSA 알고리즘에서 공개키는 이차원에서 표현되는 타원곡선 위의 한
점이다. 따라서 X좌표와 Y좌표를 가진다. 비트코인과 클레이튼은 SEC
인코딩을 따르며, 이더리움은 살짝 다른 자체 인코딩 방식을 사용한다.

## 비트코인과 SEC 인코딩

Standards of Efficient Cryptography (SEC)는 Elliptic Curve에서
공개키의 인코딩 방식을 정의한다.[^sec-public-encoding] 비트코인은
SEC의 방법을 따라서 공개키를 인코딩한다.

[^sec-public-encoding]: <http://www.secg.org/sec1-v2.pdf>

SEC 인코딩은 압축의 유무에 따라 두가지 방식으로 공개키를 인코딩한다.
하나는 압축하는 것이고 다른 하나는 압축하지 않는 방법이다.

* 압축하지 않는 경우 맨 처음에 16진수 04를 붙이고 그 뒤에 X 좌표, 그
  뒤에 Y 좌표를 붙인다.
* 압축하는 경우 y 값이 짝수면 16진수 02를, 홀수면 16진수 03을 앞에
  붙이고 뒤에 X 좌표를 붙인다.[^programming-bitcoin-serialization]

[^programming-bitcoin-serialization]: [책 Programming Bitcoin의
    Serialization
    챕터](https://learning.oreilly.com/library/view/programming-bitcoin/9781492031482/ch04.html)참조.
    SEC 인코딩 문서를 보면 인코딩 방법이 수학적으로 정리되어 있지만
    이해하기 힘들다. Programming Bitcoin 책에 수학적인 배경지식이
    부족해도 이해할 수 있게 설명되어 있다.

### BitcoinJ

BitcoinJ는 Address를 만들 때 SEC로 인코딩 되어 있는 공개키 값을
받는다. `Address`를 만들 때 `ECKey`타입을 받는다. `byte[]` 로부터
`ECKey`를 만들 때, `ECKey.isPubKeyCompressed`를 호출하여 첫 번째
`byte`를 검사한다.[^bitcoinj-fromKey]

[^bitcoinj-fromKey]: <https://github.com/bitcoinj/bitcoinj/blob/388ca037ef5e6211d5f8ba4ec5b58f4514c7b4fa/core/src/main/java/org/bitcoinj/core/Address.java#L82>

## Ethereum의 공개키 인코딩

이더리움은 자체 방식을 써서 공개키를 인코딩한다. 공식적인 레퍼런스를
찾진 못했다. [Ethereum의 YellowPaper](http://gavwood.com/paper.pdf)를
보면 매우 짧게 공개키는 두 숫자를 이어붙인 64바이트 어레이라고
적혀 있다.[^public-key-in-yellow-paper]

[^public-key-in-yellow-paper]: Where `p_u` is the public key, assumed
    to be a byte array of size 64 (formed from the concatenation of
    two positiveintegers each<`2^256`)

Ethereum 관련 라이브러리들에는 각각 이에 대한 설명이 적혀있다.

아래는 Web3J에 달린 주석이다. [GitHub link](https://github.com/web3j/web3j/blob/116539fff875a083c896b2d569d17416dfeb8a6f/crypto/src/main/java/org/web3j/crypto/ECKeyPair.java#L68-L70)

```
// Ethereum does not use encoded public keys like bitcoin - see
// https://en.bitcoin.it/wiki/Elliptic_Curve_Digital_Signature_Algorithm for details
// Additionally, as the first bit is a constant prefix (0x04) we ignore this value
```

아래는 `eth-keys` 라이브러리의
[README.md](https://github.com/ethereum/eth-keys#keyapipublickeypublic_key_bytes)에
적힌 내용이다.

> Note that there are two other common formats for public keys: 65
> bytes with a leading \x04 byte and 33 bytes starting with either
> \x02 or \x03. To use the former with the PublicKey object, remove
> the first byte. For the latter, refer to
> PublicKey.from_compressed_bytes.

### Web3J

Web3J의 코드는 `byte[]`로 들어온 public key가 유효한 Ethereum Public
Key 인코딩이라고 가정하고 유효성 검사를 하지 않는다.[^web3j-getAddress]

[^web3j-getAddress]: <https://github.com/web3j/web3j/blob/116539fff875a083c896b2d569d17416dfeb8a6f/crypto/src/main/java/org/web3j/crypto/Keys.java#L113>

## KLAYTN과 공개키 인코딩

KLAYTN의 공식적인 공개키 인코딩 관련 문서를 찾지 못했다. caver의
소스코드를 봤을 때 SEC의 인코딩을 사용한다.

아래는 caver-java의 `AccountKeyPublic`에서 복사한 주석이다.

```
/**
 * ECC Public Key value with "SECP-256k1" curve.
 * This String has following format.
 * 1. Uncompressed format : 0x{Public Key X point}||{Public Y point}
 * 2. Compressed format : 0x{02 or 03 || Public Key X}
 */
```

PrivateKey#getPublicKey 함수도 compressed를 인자로 받은 뒤 적절하게
0x02~0x04의 prefix를 붙여준다.[^caver-java-privatekey-getpublickey]

```java
public String getPublicKey(boolean compressed) {
    BigInteger publicKey = Sign.publicKeyFromPrivate(Numeric.toBigInt(privateKey));

    if(compressed) {
        return Utils.compressPublicKey(Numeric.toHexStringWithPrefixZeroPadded(publicKey, LEN_UNCOMPRESSED_PUBLIC_KEY_STRING));
    }

    return Numeric.toHexStringNoPrefixZeroPadded(publicKey, LEN_UNCOMPRESSED_PUBLIC_KEY_STRING);
}
```

[^caver-java-privatekey-getpublickey]: [getPublicKey 소스코드](https://github.com/klaytn/caver-java/blob/b5b1d39dc1a1c7b1b1a83fc581c9de842bdf57e4/core/src/main/java/com/klaytn/caver/wallet/keyring/PrivateKey.java#L116)

### 클레이튼의 getDerivedAddress

Caver의 `PrivateKey` `class`에는 `getDerivedAddress`라는 함수가 있다.
이 함수는 Address를 만들기 위해서 Ethereum 방식의 공개키 인코딩을
사용한 뒤 이더리움의 포맷의 주소를 사용해서 리턴한다. 내부 코드를 보면
Web3j의 `Keys.getAddress` 함수를 사용한다.

이더리움 방식의 공개키 인코딩이 외부에 노출된 건 아니지만 라이브러리
소스코드를 읽을 때 헷갈릴 수 있으니 주의하자.

[getDerivedAddress GitHub
링크](https://github.com/klaytn/caver-java/blob/b5b1d39dc1a1c7b1b1a83fc581c9de842bdf57e4/core/src/main/java/com/klaytn/caver/wallet/keyring/PrivateKey.java#L130)
