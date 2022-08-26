# Web3와 이더리움 상호작용

- web3 문서
  - https://web3js.readthedocs.io/

- cdn
  - https://cdnjs.com/libraries/web3

# DApp (Decentralized Application)

- 탈중앙화된 P2P 네트워크 상에 백엔드 로직이 구동되는 응용프로그램
- DApp = Frontend + Smart Contracts on Blockchain
  - Backend의 역할의 대부분을 Smart Contracts에서 처리해버림
    - 단점으로는 가스비, 속도
    - 장점으로는 신뢰성
  - 필요에 따라 Backend를 추가하기도 함

### DApp 구성 요소

- Smart Contract
  - 서비스 로직이 구현된 이더리움 네트워크에 배포된 바이트 코드
- 사용자 인터페이스
  - Front End

- Web3 API for JavaScript
  - 이더리움 스마트 컨트랙트와 JavaScript 코드 간의 상호작용 지원



### web3 객체 생성

```javascript
window.addEventListener('load', () => {
    if( typeof web3 !== 'undefined') {
        window.web3 = new Web3(web3.currentProvider);

    } else {
        window.web3 = new Web3(new Web3.providers.HttpProvider(환경 URL))
    }
});
```

- 환경 url은 Metamask setting에서 확인
- MetaMask에서 export private key

- `contract.methods.retrieve().call({from: address}).then()`
  - 비용이 소모되지 않는 호출 call
  - `retrieve()`는 함수 이름
  - call의 인자
    - from : 보내는 주소
    - gasPrice (옵션)
    - gas(옵션)

- `contract.methods.store(value).send(params)`
  - 트랜잭션을 생성하는 send
  - `store` 는 함수이름
  - `value` 는 파라미터
  - `params`
    - {from:address,gas:gasLimit,gasPrice:gasPrice}