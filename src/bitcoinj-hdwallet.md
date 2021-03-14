# bitcoinj HD wallet

bitcoinj 라이브러리는 HD wallet 기능을 제공한다. 이를 이더리움이나
클레이튼에서도 쓸 수 있다.

다만 HD wallet기능이 bitcoinj의 지갑 기능에 강하게 연결되어 있어서
제공되는 인터페이스가 친절하지 않다. 적절히 지갑의 기능을 다른
목적으로 감싸서 사용하자.

## 클래스들

DeterministicSeed


DeterministicKeyChain

// externalParentKey, internalParentKey
//issuedExternalKeys, issuedInternalKeys

isFollowing
watching


org/bitcoinj/wallet/DeterministicKeyChain.java

DeterministicHierarchy

DeterministicKey


