# require & assert(예외 처리)

- 예외처리에 걸리면 실행 자체가 일어나지 않은 일로 처리

## require

```solidity
contract Exception {
	mapping(address => uint) public balanceReceived;
	
	fuction receiveMoney() public payable {
		balanceReceived[msg.sender] += msg.value
	}
	
	fuction withdrawMoney(address payable _to, uint _amount) public {
		// 인출하려는 돈이 저금액보다 같거나 작아야함
		require(_amount <= balanceReceived[msg.sender],"not enough moneyy")
		balanceReceived[msg.sender] -= _amount;
		_to.transfer(_to)
	}
}
```

- 사용자의 입력값을 평가
  - 입력 유효성 검사에 사용
- require 조건이 맞지 않은 경우 
  - 두번째로 받는 인자(String)를 피드백 보냄 
  - 트랜잭션을 종료 시킴
  - 사용하지 않은 남은 가스를 사용자에게 반환

- 계약이 view 함수나 getter 함수에서 에테르를 받거나 전송 실패, 주소 전송 실패에 작동되는 예외 형식 

## assert

```solidity
contract Exception {
	mapping(address => uint64) public balanceReceived;
	
	fuction receiveMoney() public payable {
		//balanceReceived에 매핑된 값이 uint64인데, uint64를 넘는 범위가 될 경우 발생하는 문제 방지
		assert(balanceReceive[msg.sender] + uint64(msg.value) >= balanceReceived[msg.sender] )
		balanceReceived[msg.sender] += msg.value
	}
	
	fuction withdrawMoney(address payable _to, uint _amount) public {
		require(_amount <= balanceReceived[msg.sender],"not enough moneyy")
		assert(balanceReceived[msg.sender] >= balanceReceived[msg.sender] - _amount)
		balanceReceived[msg.sender] -= _amount;
		_to.transfer(_to)
	}
}
```

- assert 조건이 맞지 않을 경우
  - 트랜잭션을 종료 시킴
  - 사용자의 입력값과 관계 없이
  - 남은 모든 가스를 소비하여 반환하지 않음
- zero division error등에 사용되는 기본 예외 형식

- 내부 상태나 불변량 검사를 할 때 사용

## Revert

- 트랜잭션을 되돌림

```solidity
if (amount > msg.value){
	revert("Not enough")
}
// require(amiunt <= msg.value, "Not enough")와 동일
```

