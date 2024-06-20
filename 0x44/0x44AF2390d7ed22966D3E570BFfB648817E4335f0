// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// ERC20 standard interface definition
interface IERC20 {
    // Token transfer event
    event Transfer(address indexed from, address indexed to, uint256 value);
    // Approval event
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // Function to get the total supply
    function totalSupply() external view returns (uint256);
    // Function to get the balance of an account
    function balanceOf(address account) external view returns (uint256);
    // Function to transfer tokens
    function transfer(address recipient, uint256 amount) external returns (bool);
    // Function to get the allowance
    function allowance(address owner, address spender) external view returns (uint256);
    // Function to approve spending
    function approve(address spender, uint256 amount) external returns (bool);
    // Function to transfer tokens on behalf of an owner
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
}

// Context abstract contract for the current message sender and data
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// Ownable contract managing ownership
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(address initialOwner) {
        _owner = initialOwner;
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    // Function to transfer ownership
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

// ERC20 token contract
contract ERC20 is Context, IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint8 private constant _decimals = 18;

    // Constructor with the ability to set the initial owner and token distribution
    constructor(string memory name_, string memory symbol_, uint256 totalSupply_, address initialOwner)
        Ownable(initialOwner) {
        require(initialOwner != address(0), "ERC20: initial owner is the zero address");

        _name = name_;
        _symbol = symbol_;
        _totalSupply = totalSupply_;
        // Assign the total supply to the initial owner
        _balances[initialOwner] = totalSupply_;
        emit Transfer(address(0), initialOwner, totalSupply_);
    }

    // Function to get the token name
    function name() public view returns (string memory) {
        return _name;
    }

    // Function to get the token symbol
    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual returns (uint8) {
        return _decimals;
    }

    // Function to get the total supply
    function totalSupply() public override view returns (uint256) {
        return _totalSupply;
    }

    // Function to get an account's balance
    function balanceOf(address account) public override view returns (uint256) {
        return _balances[account];
    }

    // Function to transfer tokens
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    // Function to get the allowance
    function allowance(address owner, address spender) public override view returns (uint256) {
        return _allowances[owner][spender];
    }

    // Function to approve spending
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    // Function for a delegate to transfer tokens
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        // Decrease the spender's allowance
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
        return true;
    }

    // Internal transfer logic
    function _transfer(address sender, address recipient, uint256 amount) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[sender] >= amount, "ERC20: transfer amount exceeds balance");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    // Internal approve logic
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}