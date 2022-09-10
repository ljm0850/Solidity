# Solidity intro

https://solidity-kr.readthedocs.io/ko/latest/index.html

- 솔리디티 파고들기-전역변수 쪽을 자주 보게 될 것 같다.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract Storage {

    uint256 number;

    function store(uint256 num) public {
        number = num;
    }

    function retrieve() public view returns (uint256){
        return number;
    }
}
```

- `SPDX-License-Identifier: GPL-3.0` : 소스코드의 라이선스 명시
- `pragma solidity >=0.7.0 <0.9.0;` : 소스코드가 이용하는 컴파일러 버전 명시
  - `^`(캐럿 연산자) : '이상'
  - 0.7.0 => major.minor.patch

- `uint256` :상태변수
  - 상태 변수의 접근 제어자 지정 가능
    - external
    - public
    - private

- `function`
  - 컨트랙트 단위 기능
  - 매개 변수, 제어자, 반환값 지정 가능

---

## 자료형

### 기본형 Primitives

- **논리형**: bool(true / false)

- **정수형**

  - 가스비를 고려하여 적정 범위 설정 필요
  - uint : unsigned integer
    - 8~ 256bit 범위 내에서 가능
    - default 는 `unit256`
    - `unit8` 이런 형식으로 정의
  - int : signed integer
    - `Int8` : -128 ~ 127

- Fixed Point Numbers(ex)1.1...)

  - https://solidity-kr.readthedocs.io/ko/latest/types.html#index-4
  - 가능하면 사용하지 말자
  - ufixedMxN or fixedMxN
    - ufixed128x18 => 128bits, 18 decimal points

- **주소형**

  - address : 이더리움의 주소를 표현함

- **바이트형**

  - `bytes#`,`byte[]` : 데이터를 바이트로 표현  
  - 원데이터를 보관하기 위함

- **String**

  - String과 Bytes는 special arrays

  - bytes와 같지만, length나 index-access가 불가

  - UTF8 데이터 저장용

  - 가스비가 비쌈..

  - ```solidity
    string public myString = "Hello World!";
        // 해당 인수가 있는 데이터 위치를 지정해줘야 함
        // 문자열이나 다른 참조 유형이 있으면 memory 키워드 입력 (이 인수가 스토리지 변수가 아닌 메모리에 저장될 것을 알려줌)
        function setMyString(string memory _myString) public {
            myString = _myString;
        }
    ```


#### 접근 제어자 Visibility

|                 | private                     | internal                                    | public                                                       | external                           |
| :-------------: | :-------------------------- | ------------------------------------------- | ------------------------------------------------------------ | ---------------------------------- |
|      접근       | 컨트랙트 내에서만 접근 가능 | 현재 컨트랙트와 자식 컨트랙트에서 접근 가능 | 현재 컨트랙트, 자식컨트랙트, 외부 컨트랙트 및 주소에서 접근 가능 | 외부 컨트랙트와 주소에서 접근 가능 |
| State Variables | o                           | x                                           | o                                                            | o                                  |
|    Functions    | o                           | o                                           | o                                                            | o                                  |

```solidity
contract Parent{}
contract Child is Parent{} // is 는 상속
```



### 참조형

#### 데이터 위치

- storage
  - 영구 데이터(permanet data) 영역에 데이터 저장
  - contract의 상태 변수를 주로 저장
  - 비용이 많이듬
- memory
  - 함수 안에서 사용되는 임시 데이터를 저장하는데 사용
- calldata
  - 함수에 전달되는 매개 변수에 주로 사용
  - 변경 불가하고 임시적인 데이터가 저장되는 영역



#### Array

- 고정 길이와 유동적 길이를 갖는 배열을 선언 가능
  - `uint[]`
  - `uint[10]`
- 자료구조 다루는 방법
  - index에 접근
  - push pop delete(default값으로 초기화)
- 함수 내에서 로컬 변수로 배열을 사용하기 위해서는 고정 길이로 선언해야함

#### Mapping

- 매핑형 선언 방법
- 접근,추가,삭제 방법
- 매핑에 저장된 key 목록을 얻을 수 있는 방법은 없음

```solidity
contract Mapping {
    mapping(address => uint) public addrToUint;

    function get(address _addr) public view returns (uint) {
        return addrToUint[_addr];
    }

    function set(address _addr, uint _i) public {
        addrToUint[_addr] = _i;
    }
    
    function reset(address _addr) public {
        delete addrToUint[_addr];
    }
}
```



### 사용자 선언 자료형 Struct

- 여러 자료형을 하나의 관점으로 묶어서 관리하고자 할 때 선언

```solidity
struct Todo {
	String text;
	bool completed;
}
```

- Array, Mapping 값으로 지정 가능



## 연산

### 함수

```solidity
contract Function {
    uint public num = 1;

    uint public a = 1;
    string public s = "hello solidity";
    bool public b = true;

    function addOne() public {
        num++;
    }

    function addNumber(uint x) public returns (uint) {
        num += x;

        return num;
    }
 	// view
    function addAndReturn(uint x) public view returns (uint) {
       return num + x;
    }
	// pure
    function add(uint x, uint y) public pure returns (uint) {
       return x + y;
    }

    function returnMany() public view returns (uint, string memory, bool) {
        return (a, s, b);
    }
}
```

- **view** : 값을 변경하지 않고 계산된 값만 가지고 옴
  - 가스 소모 x
- **pure**: 상태변수에 접근하지 않고 값을 계산할 때 사용



### 제어문

#### 반복문

 ```solidity
 contract Loop {
     function loop1() public pure {
         for (uint i = 0; i < 10; i++) {
             if (i == 3) {
                 continue;
             }
             if (i == 5) {
                 break;
             }
         }
     }
 
     function loop2() public pure {
         uint i;
         while (i < 10) {
             i++;
         }
     }
 }
 ```

- 비트코인의 경우 반복문을 제공 x (블록체인은 모든 컴퓨터에서 돌아가서 리스크가 너무 큼)
- 이더리움은 무한 반복 같은 상황에서 gas limit에 의해 실행 취소됨



## 연산자

- **NOT** : `!`
  - `myVar = !myVar`
- **AND** : `&&`
- **OR** : `||` 



- 2^8 : 2**8