# event & 반환변수

## event

- 외부 사용자에게 블록체인에서 어떤 일이 발생했는지 알리고 리턴 값을 사용 하는데 사용
- 테스트 블록체인 네트워크 <-> 실제 블록체인 네트워크
  - 실제에선 output이 없음 (출력 값이 없음)
  - 트랜잭션을 개시한 사람에게 return값을 적용 시킬 방법 => event
- 트리거로 사용 할 수 있음

```solidity
contract EventTest{
	mapping(address => uint) public tokenBalance;
	// 인자에 _를 넣는건 js와 solidity를 구분하기 위한것
	event TokensSent(address _from, address _to, uint _amount);
	
	functuion sendToken(address _to, uint _amount) public returns(bool) {
		require(tokenBalance[msg.sender] >= _amount, "Not enough tokens");
		assert(tokenBalance[_to] + _amount >= tokenBalance[_to]);
		assert(tokenBalance[msg.sender] - _amount <= tokenBalance[msg.sender]);
		tokenBalance[msg.sender] -= _amount;
		tokenBalance[_to] += _amount;
		
		emit TokensSent(msg.sender, _to, _amount);
		
		retrun true;
	}
}
```

- event는 주로 CapWords 스타일로 작성

- 최대 3개의 매개변수를 index로 사용 가능

- 체인에 저장된 데이터를 내보낼 수 있음