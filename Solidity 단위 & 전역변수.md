# Solidity 단위 & 전역변수

- https://solidity-kr.readthedocs.io/ko/latest/units-and-global-variables.html



## 블록 및 트랜잭션 속성

- `block.blockhash(uint blockNumber) returns (bytes32)`: 주어진 블록의 해시 - 현재 블록을 제외한 가장 최근 256개의 블록에 대해서만 작동함
- `block.coinbase` (`address`): 현재 블록 채굴자의 address
- `block.difficulty` (`uint`): 현재 블록 난이도
- `block.gaslimit` (`uint`): 현재 블록 gaslimit
- `block.number` (`uint`): 현재 블록 번호
- `block.timestamp` (`uint`): unix epoch 이후의 현재 블록 타임스탬프
- `gasleft() returns (uint256)`: 잔여 가스
- `msg.data` (`bytes`): 완전한 calldata
- `msg.gas` (`uint`): 잔여 가스 - 0.4.21버전에서 제거되었으며 `gasleft()` 로 대체됨
- `msg.sender` (`address`): 메세지 발신자 (현재 호출) 주소
- `msg.sig` (`bytes4`): calldata의 첫 4바이트(즉, 함수 식별자)
- `msg.value` (`uint`): 메세지와 함께 전송된 wei 수
- `now` (`uint`): 현재 블록 타임스탬프(`block.timestamp` 의 별칭)
- `tx.gasprice` (`uint`): 트랜잭션의 가스 가격
- `tx.origin` (`address`): 트랜잭션의 발신자 (full call chain)



## 그외

- `payable`

  - ```solidity
    function fund() public payable {}
    ```

  - 이더 전송을 나타냄

  - `msg.value`를 자주 사용하게 될것

- `require`

  - ```solidity
    function fund() public payable{
    	require(msg.value >= 조건, "에러 메시지")
    }
    ```

  - 유효성 체크 함수

  - 판별문이 true가 아닌 경우 에러 메시지 출력 후 함수 종료

- `address(this).balance`

  - ```solidity
    function currentCollection() public view
    return (uint256){
    	return address(this).balance
    }
    ```

  - 총 이더의 양 확인 가능

  - https://solidity-kr.readthedocs.io/ko/latest/units-and-global-variables.html?highlight=balance#address