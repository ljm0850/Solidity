# Function

- setter function, getter function 함수 존재

## fallback function

```solidity
contract Functions {
	mapping(address => uint) public balanceReceived;
	
	function receiveMoney() public payable {
		assert(balanceReceived[msg.sender]+msg.value >= balanceReceived[msg.sender]);
		balanceReceived[msg.sender] += msg.value
	}

	function () external payable {
		receiveMoney();
		}
	}
```

- 매칭되는 함수가 없거나, 트랙잭션 인코딩 데이터 필드에 일치하는 함수가 없을때 사용

- 인수가 있던 없든 사용 가능
- 반드시 external
- 주 사용처
  - 함수와 상호 작용하지 않고 스마트 계약에 이더를 보내는 경우
  - 



## view function & pure function

```solidity
contract Functions {
	mapping(address => uint) public balanceReceived;
	address payable owner;
	
	constructor() public {
		owner = msg.sender;
	}
	
	function getOwner() public view returns(address){
		return owner;
	}
	
	function convertWeiToEther(uint _amountInWei) public pure returns(uint){
		return _amountInWei / 1 ether; // ether = 10^18
	}
}
```

- View의 경우 저장소 변수(위 예시에선 owner에 해당)와 상호작용
  - 상태를 읽고 다른 뷰 함수 호출 가능
  - 상태 수정 불가

- puer의 경우 저장소 변수와 상호작용하지 않음 
  - 상태를 읽거나 수정 불가
  - 다른 퓨어 함수 호출 가능



## Fucntion Visibility

### Public

- 계약 내부,외부 모두에서 호출 가능
- 상속으로 확장하는 모든 스마트 계약에서 호출 가능

### Private

- 스마트 계약 내에서만 호출 가능
- 파생된 스마트 계약(상속)과 상호 작용 불가

### External

- 외부에서 호출 가능
- 내부에서 외부에서 호출하듯 해야함 (this.함수이름)

### Internal

- Private에서 파생된 스마트 계약과 상호 작용이 가능하게 바뀐 것



## Constructor

- 배포 중 한 번만 호출됨
- Public 혹은 Internal로만 사용 가능