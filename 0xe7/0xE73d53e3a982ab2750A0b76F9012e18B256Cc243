pragma solidity ^0.8.0;

/*

Some say the world will end in fire,
Some say in ice.
From what I’ve tasted of desire
I hold with those who favor fire.
But if it had to perish twice,
I think I know enough of hate
To say that for destruction ice
Is also great
And would suffice.

The following is a simple contract for a token, "N", with a bound supply curve.
Every block a new token is made available for minting.
Every 4 years a "doubling" occurs, wherein the number of blocks passed required to mint a new token doubles.
Following this schedule, in roughly 92 years it will take 4 years to mint a single token, at which point the supply will be roughly ~21,000,000.
The supply is technically uncapped, but after 92 years no more than a handful of tokens could ever be minted before the heat death of the universe.

Alea iacta est

*/

contract N {
    string public name = "N";
    string public symbol = "N";
    uint8 public decimals = 18;
    uint256 public totalSupply;
    uint8 public epoch = 0;
    uint256 public lastDoublingBlock;
    uint256 public nextDoublingBlock;
    uint256 public lastMintingBlock;
    

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        totalSupply = 1e18;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
        lastDoublingBlock = block.number;
        nextDoublingBlock = block.number + 10519200; //((86400*365*4)+86400)/12 (4 years and one day of blocks, with leap day)
        lastMintingBlock = block.number;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Insufficient allowance");
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function mint() public {
        uint256 numberToMint = mintable();
        totalSupply+= numberToMint;
        balanceOf[msg.sender]+= numberToMint;
        if(block.number >= nextDoublingBlock) {
            lastDoublingBlock = block.number;
            lastMintingBlock = block.number;
            nextDoublingBlock = block.number + 10519200;
            epoch++;
        }
    }

    function mintable() internal returns (uint256 numberToMint) {
        uint256 blocksBetweenMints = block.number-lastMintingBlock;
        if(blocksBetweenMints >= 2**epoch) {
            numberToMint = (blocksBetweenMints/(2**epoch))*1e18;
            lastMintingBlock = block.number - (blocksBetweenMints % (2**epoch));
            return (numberToMint);
        } else revert();
    }
}