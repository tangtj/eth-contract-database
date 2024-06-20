// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

// Flattened OpenZeppelin Contracts and References
// IERC20 interface - Reference
interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
// SafeMath library - Reference
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }
    // be good wabits
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;
        return c;
    }
    
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    // divided wabits wont survive
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        return c;
    }
}
// Ownable Contract
abstract contract Ownable {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    constructor() {
        _owner = msg.sender;
        emit OwnershipTransferred(address(0), msg.sender);
    }
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    } // follow da black wabit...
    function owner() public view returns (address) {
        return _owner;
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
// ReentrancyGuard Contract
abstract contract ReentrancyGuard {
    uint256 private _guardCounter;
    constructor() {
        _guardCounter = 1;
    }
    modifier nonReentrant() {
        _guardCounter += 1;
        uint256 localCounter = _guardCounter;
        _;
        require(localCounter == _guardCounter, "ReentrancyGuard: reentrant call");
    }
}
// DaWabit ..welcomes you..
contract DaWabit is IERC20, Ownable, ReentrancyGuard {
    using SafeMath for uint256;
    // combining Chat GPT with creative human ingenuity..
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => uint256) private _lastTxTime;
    // strategically decentralized
    uint256 private _tTotal = 333e30; // 333 Trillion tokens with 18 decimals
    string private _name = "DaWabit";
    string private _symbol = "WABIT";
    uint8 private _decimals = 18;
    // there is only one WABIT..
    address public myWalletAddress; 
    address public constant BURN_ADDRESS = 0xdEAD000000000000000042069420694206942069; // DEAD WALLET
    uint256 public txCooldown = 30 seconds; // meant to be MEV DEFENSE, timing may need adjustment
    // truly deflationary
    constructor() {
        myWalletAddress = 0x981FB95d4101D1F6238456b680cCa1fa818e86a4; // DEV WALLET
        _balances[myWalletAddress] = _tTotal;
        emit Transfer(address(0), myWalletAddress, _tTotal);
    }
    // be free Wabits
    function name() external view returns (string memory) {
        return _name;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }
    // adventures Wabits
    function transfer(address recipient, uint256 amount) external override nonReentrant returns (bool) {
        require(block.timestamp > _lastTxTime[msg.sender] + txCooldown, "Cooldown time has not passed");
        _lastTxTime[msg.sender] = block.timestamp;
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override nonReentrant returns (bool) {
        require(block.timestamp > _lastTxTime[sender] + txCooldown, "Cooldown time has not passed");
        _lastTxTime[sender] = block.timestamp;
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount));
        return true;
    }
    // who DaWabit? You DaWabit..
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 feeAmount = amount.mul(1).div(10000);  // .01% fee
        uint256 transferAmount = amount.sub(feeAmount);
        // stay growing wabit..
        _balances[sender] = _balances[sender].sub(amount);
        _balances[recipient] = _balances[recipient].add(transferAmount);

        // Explicit send fee to burn address
        _balances[BURN_ADDRESS] = _balances[BURN_ADDRESS].add(feeAmount);

        emit Transfer(sender, recipient, transferAmount);
        emit Transfer(sender, BURN_ADDRESS, feeAmount);
    }
    // what dont kill you, make you stronga Wabit..
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}
// STAND UNITED WABITS