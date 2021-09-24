# Solidity

## 강의
* https://www.inflearn.com/course/%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-blockchain
* https://needjarvis.tistory.com/255?category=776493

## 실행
* https://remix.ethereum.org
* https://github.com/ethereum/browser-solidity/tree/gh-pages

## 투표하기
```sol
pragma solidity >=0.7.0 <0.9.0;

contract Vote {
    // struct
    struct Condidator {
        string name;
        uint upVote;
    }

    // variable
    address owner;
    Condidator[] public condidators;
    bool isLive;

    // mapping
    mapping(address => bool) Voted;

    // event
    event Voting(address owner);
    event AddCandidator(string name);
    event UpVote(string name, uint upVote);
    event FinishVote(bool isLive);

    // modifier (한정자: 실행 가능한 권한만 실행)
    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }

    // constructor
    constructor() {
        owner = msg.sender;
        isLive = true;

        // emit event
        emit Voting(owner);
    }

    function addCandidator(string memory _name) public onlyOwner {
        require(isLive == true);
        require(condidators.length < 5);
        condidators.push(Condidator(_name, 0));

        // emit event
        emit AddCandidator(_name);
    }

    function upVote(uint256 _index) public {
        require(isLive == true);
        require(condidators.length > _index);
        require(Voted[msg.sender] == false);
        Condidator memory condidator = condidators[_index];
        condidator.upVote++;

        // mapping
        Voted[msg.sender] = true;

        // emit event
        emit UpVote(condidator.name, condidator.upVote);
    }

    function finish() public onlyOwner {
        require(isLive == true);
        isLive = false;

        // emit event
        emit FinishVote(isLive);
    }
}
```
