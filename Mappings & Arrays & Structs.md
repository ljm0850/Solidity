# Mappings & Arrays & Structs

## Mapping

- `mapping (key type => value type) 이름` 
- Hash Maps과 비슷
- 모든 값(value)는 default 값으로 초기화
  - int는 0
  - boolean은 false
- getter함수가 자동으로 생성

```solidity
contract MappingEx {
	struct Payment {
		uint amount;
		uint timestamps;
	}
	
	struct Balance {
		uint totalBalance;
		uint numPayments;
		mapping(uint => Payment) payments;
	}
	
	mapping(address => Balance) public balanceReceived;
	}
}
```



## Structs

- custom 가능한 types
- `struct name { }`

```sol
struct Payment {
		uint amount;
		uint timestamps;
	}
```



## Arrays

- array[k] : k로 size가 fixed 됨
- array[] : size가 unfixed
- 가스 문제가 올 경우가 많으니 쓸 때 주의 

```solidity
uint balance[] = [1, 2, 3];
```



## Enum

- 사용자 정의 유형을 만드는 방법중 하나
- 사용은 잘 안함

```solidity
enum ActionChoices {GoLeft, GoRight, GoStraight}
ActionChoices choice;
ActionChoice constant defaultChoice = ActionChoices.GoStraight
```

