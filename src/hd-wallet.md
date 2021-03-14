# HD wallet

[bitcoin dev guide의 hd wallet에 대한
글](https://developer.bitcoin.org/devguide/wallets.html#hierarchical-deterministic-key-creation)

하나의 키로 여러 키를 쉽게 관리하는 방식이다. 하나의 비밀 정보로부터
수 많은 키를 생성할 수 있다. 간단한 하나의 비밀정보만 보관하는 것으로
모든 지갑을 쉽게 관리할 수 있다.

## 비트코인에서 HD wallet이 의미 있는 이유

비트코인은 privacy를 신경을 많이 쓴다. 하나의 주소를 계속 사용하면
주소로 오가는 비트코인을 추적할 수 있게 된다. 따라서 여러 계정을
사용하거나 계정을 자주 바꾸는 것이 권장된다.

계정을 많이 생성하면 그만큼 많은 비밀키를 생성해야 해서 관리가
복잡해진다. HD wallet을 사용하면 적은 양의 비밀정보 만으로 수많은
계정을 쉽게 백업할 수 있다.

## 동작 방식

비밀키는 간단한 숫자다. 여기에 1, 2, 3 등을 더해서 새로운 비밀키를
만들 수 있다. 단순히 1, 2, 3을 더하는 건 쉽게 부모 키를 역추적할 수 있기 때문에
해시와 추가적인 정보를 넣어서 더 복잡하게 만든다.

HD Wallet는 비밀키 혹은 공개키와 Chain Code라는 값을 함께 관리한다.
부모 키와 부모의 Chain Code로부터 자식 키와 자식의 Chain Code를 만들어
낸다. 아래 수도코드 방식처럼 자식의 키와 자식의 Chain Code를 만든다.

```
(left_random, right_random) = Hash(parent_chain_code, parent_public_key, i)
child_private_key = parent_private_key + left_random
child_chain_code = right_random
```

## Extended key

키와 Chain Code를 함께 묶어서 Extended Key라고 부른다. Public Key와
Chain Code를 합치면 Extended Public Key라고 부른다. Private Key와
Chain Code를 합치면 Extended Private Key라고 부른다.

## Root Seed와 Master

Root Seed는 첫 번째 Private Key와 Chain Code를 만들기 위해 필요한
정보다. Root Seed를 한 번 해시하여 만들어낸 정보를 절반으로 잘라서
하나를 Private Key, 다른 하나를 Chain Code로 사용한다.

가장 먼저 만들어지고 모든 키의 부모가 되는 키를 Master 키라고 부른다.
비밀키와 공개키는 각각 Master Private Key와 Master Public Key라고
부른다.

## Hardened Key

부모의 Chain Code와 자식의 비밀 키가 노출되었을 때 부모의 private
키까지 노출되는 문제를 막는 방법이다.

키들 derive하는 방법을 둘로 나누어 하나는 normal 방식, 하나는 hardened
방식이라고 부른다. normal 방식은 derive할 때 부모의 공개키와 Chain
Code만 필요하다. hardened 방식은 derive할 때 부모의 *비밀키*와
ChainCode가 필요하다.

부모의 비밀키가 필요하게 만듬으로써 부모의 ChainCode와 자식의 비밀
키로 부모의 비밀키를 알아내는 게 불가능해 진다.

## key tree 노테이션

* Master Private Key는 `m`으로 지칭한다.
* Master Public Key는 `M`으로 지칭한다.
* Master Key의 `i`번째 normal derived private key는 `m/i`
* Master key의 `i`번째 hardened derived private key는 `m/i_H` 혹은 `m/i'`
* `m/44'/0'/0'/0/1`은 마스터의 44번째 hardened, 0번째 hardened, 0번째
  hardened, 0번째 normal, 1번째 normal derive된 비밀키다.
* `M/44'/0'/0'/0/1`은 마스터의 44번째 hardened, 0번째 hardened, 0번째
  hardened, 0번째 normal, 1번째 normal derive된 공개키다.

## 네모닉 코드로부터 Root Seed 생성하기

랜덤으로 생성한 단어들로부터 seed를 생성할 수 있다. 자세한 규칙은
BIP39에 명시되어 있다.
