# TEST CODE

### 데이터 타입

```solidity
pragma solidity ^0.8.17;

contract WorkingWithVariables {
    // Int, Uint
    uint256 public myUint;
    function setMyUint(uint _myUint) public {
        myUint = _myUint;
    }
    
    uint8 public myUint8;
    function incrementUint() public {
        myUint8 ++;
    }
    function decrementUint() public {
        myUint8 --;
    }
    // Boolean
    bool public myBool;
    function setMyBool(bool _myBool) public {
        myBool = _myBool;
        // myBool = !myBool
    }
    // Address
    address public myAddress;
    function setAddress(address _address) public {
        myAddress = _address;
    }

    function getBalanceOfAddress() public view returns(uint) {
        return myAddress.balance;
    }
    // Dynamically sized byte arrays (String)
    string public myString = "Hello World!";
    // 해당 인수가 있는 데이터 위치를 지정해줘야 함
    // 문자열이나 다른 참조 유형이 있으면 memory 키워드 입력 (이 인수가 스토리지 변수가 아닌 메모리에 저장될 것을 알려줌)
    function setMyString(string memory _myString) public {
        myString = _myString;
    }
}
```

### 자금 송금

```solidity
pragma solidity ^0.8.17;

contract SendMoney {
    // 돈을 얼마나 받았는지 기록용, 잔고를 변수에 저장하는건 조심
	uint public balanceReceived;

	// smart contract 주소의 원장에게 보내짐 
	function receiveMoney() public payable {
	 	balanceReceived += msg.value
	}
	// 보내진 자금을 확인
	function getBalance() public view returns(uint) {
		return address(this).balance;
	}
	
	// 자금 받기
	function withdrawMoney() public {
		address payable to = msg.sender; // sender에는 계약 호출한 주소가 들어있음
		to.transfer(this.getBalance());
	}
	
	// 특정 주소에 자금 보내기 (receiveMoney -> withdrawMoneyTo 순서)
	function withdrawMoneyTo(address payable _to) public {
		_to.transfer(this.getBalance());
	}

}
```

### 자금 송금 control

```solidity
pragma solidity ^0.8.17;

contract control {
	address owner;
	
	bool paused; // 계약 정지용
	
	// 계약 배포 중 단 한번 호출
	constructor() public {
		owner = msg.sender;
	}
	function sendMoney() public payable {}
	
	
	function setPause(bool _paused) public {
		require(msg.sender == owner, "owner가 아닙니다")
		paused = _paused;
	}
	
	function withdrawMoneyTo(address payable _to) public {
		require(msg.sender == owner, "owner가 아닙니다")
		require(!paused, "계약이 정지 되었습니다")
		_to.transfer(this.getBalance());
	}
	
	// 계약 파기
	function destroySmartContract(address payable _to) public {
		require(msg.sender == owner, "owner가 아닙니다")
		selfdestruct(_to // 남은 잔고 받을 주소)
	}
}
```

