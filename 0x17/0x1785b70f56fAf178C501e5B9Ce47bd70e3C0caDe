// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/**
 * @title ERC20 Token
 * @dev Standard ERC20 token with blacklist, renounce ownership, and liquidity addition features.
 */
contract JokerToken {
    string public name = "JOKER";
    string public symbol = "JOKER";
    uint8 public decimals = 15;
    uint256 private _totalSupply = 420690000000000 * (10 ** uint256(decimals));

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _blacklist;
    address private _owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event BlacklistUpdated(address indexed account, bool isBlacklisted);

    constructor() {
        _balances[msg.sender] = _totalSupply;
        _owner = msg.sender;
        _blacklist[address(0)] = true; // Adding the zero address to blacklist by default
        emit Transfer(address(0), msg.sender, _totalSupply);
        emit OwnershipTransferred(address(0), msg.sender);
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }

    function setBlacklist(address account, bool isBlacklisted) external {
        require(msg.sender == _owner, "ERC20: only owner can set blacklist");
        _blacklist[account] = isBlacklisted;
        emit BlacklistUpdated(account, isBlacklisted);
    }

    function renounceOwnership() external {
        require(msg.sender == _owner, "ERC20: only owner can renounce ownership");
        _blacklist[_owner] = true; // Add owner to blacklist upon renouncing ownership
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(!_blacklist[sender], "ERC20: sender is blacklisted");
        require(!_blacklist[recipient], "ERC20: recipient is blacklisted");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}