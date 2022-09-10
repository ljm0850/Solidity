# TEST CODE

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

