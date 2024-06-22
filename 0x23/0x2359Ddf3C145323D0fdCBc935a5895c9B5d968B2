// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        return a / b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }
}

contract Ownable {
    address public owner;
    address private _previousOwner;
    uint256 private _lockTime;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        require(msg.sender == owner, "Ownable: caller is not the owner");
        _;
    }

    function waiveOwnership() public onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }

    function getUnlockTime() public view returns (uint256) {
        return _lockTime;
    }

    function getTime() public view returns (uint256) {
        return block.timestamp;
    }

    function lock(uint256 time) public onlyOwner {
        _previousOwner = owner;
        owner = address(0);
        _lockTime = block.timestamp + time;
        emit OwnershipTransferred(owner, address(0));
    }

    function unlock() public {
        require(_previousOwner == msg.sender, "Ownable: caller is not the previous owner");
        require(block.timestamp > _lockTime, "Ownable: contract is locked, time is not up");
        emit OwnershipTransferred(owner, _previousOwner);
        owner = _previousOwner;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }
}


interface ERC20Basic {
    function balanceOf(address who) external view returns (uint);
    function transfer(address to, uint value) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
}

interface ERC20 is ERC20Basic {
    function allowance(address owner, address spender) external view returns (uint);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function approve(address spender, uint value) external returns (bool);

    event Approval(address indexed owner, address indexed spender, uint value);
}


contract StandardToken is ERC20 {
    using SafeMath for uint256;

    uint256 public txFee;
    uint256 public burnFee;
    address public FeeAddress;
    uint256 public totalSupply;
    mapping (address => bool) public isExcludedFromFee;
    mapping (address => mapping (address => uint256)) internal allowed;
    mapping(address => bool) public  tokenBlacklist;
    event Blacklist(address indexed blackListed, bool value);
    bool public paused;
    mapping(address => uint256) balances;


    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(!tokenBlacklist[sender], "StandardToken: sender is blacklisted");
        require(recipient != address(0), "StandardToken: transfer to the zero address");
        require(amount <= balances[sender], "StandardToken: transfer amount exceeds balance");
        if((!isExcludedFromFee[sender] && !isExcludedFromFee[recipient]) ){
            require(!paused, "not start");
        }

        balances[sender] = balances[sender].sub(amount);
        uint256 finalAmount = (isExcludedFromFee[sender] || isExcludedFromFee[recipient]) ?
                                        amount : takeFee(sender, amount);


        balances[recipient] = balances[recipient].add(finalAmount);

        emit Transfer(sender, recipient, finalAmount);
        return true;
    }
    function takeFee(address sender, uint256 amount) internal returns (uint256) {

        uint256 feeAmount = 0;
        uint256 destAmount = 0;
        if(burnFee > 0) {
            destAmount = amount.mul(burnFee).div(100);
        }
        
        if(txFee > 0) {
            feeAmount = amount.mul(txFee).div(100);
        }

        if(feeAmount > 0) {
            balances[FeeAddress] = balances[FeeAddress].add(feeAmount);
            emit Transfer(sender, FeeAddress, feeAmount);
        }
        if(destAmount >0){
            balances[address(0)] = balances[address(0)].add(destAmount);
            emit Transfer(sender, address(0), destAmount);
        }

        return amount.sub(feeAmount.add(destAmount));
    }


    function transfer(address _to, uint256 _value) public virtual override returns (bool) {
        _transfer(msg.sender, _to, _value);
        return true;
    }

    function balanceOf(address _owner) public view virtual override returns (uint256 balance) {
        return balances[_owner];
    }

    function transferFrom(address _from, address _to, uint256 _value) public virtual override returns (bool) {
        _transfer(_from,_to,_value);
        allowed[_from][msg.sender] = allowed[_from][msg.sender].sub(_value);
        return true;
    }

    function approve(address _spender, uint256 _value) public virtual override returns (bool) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function allowance(address _owner, address _spender) public view virtual override returns (uint256) {
        return allowed[_owner][_spender];
    }

    function increaseApproval(address _spender, uint256 _addedValue) public virtual returns (bool) {
        allowed[msg.sender][_spender] = allowed[msg.sender][_spender].add(_addedValue);
        emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
        return true;
    }

    function decreaseApproval(address _spender, uint256 _subtractedValue) public virtual returns (bool) {
        uint256 oldValue = allowed[msg.sender][_spender];
        if (_subtractedValue > oldValue) {
            allowed[msg.sender][_spender] = 0;
        } else {
            allowed[msg.sender][_spender] = oldValue.sub(_subtractedValue);
        }
        emit Approval(msg.sender, _spender, allowed[msg.sender][_spender]);
        return true;
    }

   
}


contract CoinToken is StandardToken,Ownable {
    string public name;
    string public symbol;
    uint public decimals;

    event Mint(address indexed from, address indexed to, uint256 value);
    event Burn(address indexed burner, uint256 value);

    constructor(string memory _name, string memory _symbol, uint256 _decimals, uint256 _supply, uint256 _txFee, uint256 _burnFee, address _FeeAddress, address tokenOwner, address service) payable {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _supply * 10**_decimals;
        balances[tokenOwner] = totalSupply;
        owner = tokenOwner;
        txFee = _txFee;
        burnFee = _burnFee;
        FeeAddress = _FeeAddress;
        isExcludedFromFee[tokenOwner] = true;
        isExcludedFromFee[address(this)] = true;
        payable(service).transfer(msg.value);
        emit Transfer(address(0), tokenOwner, totalSupply);
    }

    function burn(uint256 _value) public {
        _burn(msg.sender, _value);
    }

    function updateFee(uint256 _txFee, uint256 _burnFee, address _FeeAddress) onlyOwner public {
        txFee = _txFee;
        burnFee = _burnFee;
        FeeAddress = _FeeAddress;
    }

    function _burn(address _who, uint256 _value) internal virtual {
        require(_value <= balances[_who], "CoinToken: burn amount exceeds balance");
        balances[_who] -= _value;
        totalSupply -= _value;
        emit Burn(_who, _value);
        emit Transfer(_who, address(0), _value);
    }
    
    function excludeMultipleAccountsFromFees(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            isExcludedFromFee[accounts[i]] = excluded;
        }
    }
    
    function pause() onlyOwner public {
        paused = true;
    }

    function unpause() onlyOwner public {
        paused = false;
    }

    function blackListAddress(address listAddress,  bool isBlackListed) public onlyOwner {
        tokenBlacklist[listAddress] = isBlackListed;

    }

}