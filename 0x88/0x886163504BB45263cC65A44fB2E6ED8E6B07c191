// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts (last updated v5.0.1) (utils/Context.sol)

pragma solidity ^0.8.20;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function _contextSuffixLength() internal view virtual returns (uint256) {
        return 0;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v5.0.0) (access/Ownable.sol)

pragma solidity ^0.8.20;


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// File: frenkingdom.sol


pragma solidity ^0.8.20;

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


library IterableMapping {
    // Iterable mapping from address to uint;
    struct Map {
        address[] keys;
        mapping(address => uint256) values;
        mapping(address => uint256) indexOf;
        mapping(address => bool) inserted;
    }

    function exists(Map storage map, address key) internal view returns (bool) {
        return map.inserted[key];
    }

    function get(Map storage map, address key) internal view returns (uint256) {
        return map.values[key];
    }

    function getKeyAtIndex(Map storage map, uint256 index) internal view returns (address) {
        return map.keys[index];
    }

    function size(Map storage map) internal view returns (uint256) {
        return map.keys.length;
    }

    function set(Map storage map, address key, uint256 val) internal {
        if (map.inserted[key]) {
            map.values[key] = val;
        } else {
            map.inserted[key] = true;
            map.values[key] = val;
            map.indexOf[key] = map.keys.length;
            map.keys.push(key);
        }
    }

    function remove(Map storage map, address key) internal {
        if (!map.inserted[key]) {
            return;
        }

        delete map.inserted[key];
        delete map.values[key];

        uint256 index = map.indexOf[key];
        address lastKey = map.keys[map.keys.length - 1];

        map.indexOf[lastKey] = index;
        delete map.indexOf[key];

        map.keys[index] = lastKey;
        map.keys.pop();
    }
}

contract Frenlandia is IERC20, Ownable {

    mapping(address account => uint256) private _balances;
    mapping(address account => mapping(address spender => uint256)) private _allowances;
    mapping(address => bool) private _blacklist;

    uint256 constant private _totalSupply = 666666666666 * 10**18; // 666666666666 tokens with 18 decimals
    string constant private _name = "Frenlandia";
    string constant private _symbol = "Fren";
    uint8 constant private _decimals = 18;

    uint256 constant private _taxFee = 1; // 1% tax fee
    uint256 constant private _maxSends = 5; // maximum number of random holders who receive their reflection per transaction
    uint256 private _reflectionTotal;
    mapping(address => bool) private _excludedFromReflection;
    uint256 private _excludedSupply;

    using IterableMapping for IterableMapping.Map;
    IterableMapping.Map private _reflections;

    constructor() Ownable (_msgSender()) {
        _balances[_msgSender()] = _totalSupply;
        emit Transfer(address(0), _msgSender(), _totalSupply);
        _reflections.set(_msgSender(), _reflectionTotal);
        _excludedFromReflection[address(0)] = true;
        _excludedFromReflection[address(0x000000000000000000000000000000000000dEaD)] = true;
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        uint256 balance = _balances[account];
        return  balance;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()] - amount);
        return true;
    }

    function totalReflection() public view returns (uint256) {
        return _reflectionTotal;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        require(!_blacklist[owner], "Owner is blacklisted");
        require(!_blacklist[spender], "Spender is blacklisted");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);

        _excludeReflection(spender); // no reflections for router
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "ERC20: transfer amount must be greater than zero");
        require(!_blacklist[sender], "Sender is blacklisted");
        require(!_blacklist[recipient], "Recipient is blacklisted");

        uint256 taxAmount = amount * _taxFee / 100; // Calculate the tax amount
        uint256 transferAmount = amount - taxAmount; // Calculate the transfer amount after tax

        address owner = owner();
        if(sender != owner) { _checkAccount(sender); }
        if(recipient != owner) { _checkAccount(recipient); }
        if((sender == owner) || (recipient == owner) || (_excludedFromReflection[sender] && _excludedFromReflection[recipient])) {
            taxAmount = 0;
            transferAmount = amount;
        }

        if(taxAmount > 0) {
            // take fee and store in contract
            _balances[address(this)] = _balances[address(this)] + taxAmount;
            emit Transfer(sender, address(this), taxAmount);
            _reflectionTotal = _reflectionTotal + taxAmount;
        }
        _redistributeReflection(sender, recipient); // Distribute reflection on each transfer

        _balances[sender] = _balances[sender] - amount;
        if(_excludedFromReflection[sender]) {
            _excludedSupply-= amount;
        }
        _balances[recipient] = _balances[recipient] + transferAmount;
        if(_excludedFromReflection[recipient]) {
            _excludedSupply+= transferAmount;
        }
        emit Transfer(sender, recipient, transferAmount);
    }

    function _redistributeReflection(address sender, address recipient) internal {
        _redistributeReflectionFor(sender);
        _redistributeReflectionFor(recipient);

        uint256 size = _reflections.size();
        uint256 randomIndex = uint256(keccak256(abi.encodePacked(block.number, _msgSender(), _reflectionTotal))) % size;
        uint256 count = size;
        if(_reflections.exists(sender)) count--;
        if(_reflections.exists(recipient)) count--;
        uint256 max = (count > _maxSends) ? _maxSends : count;
        uint256 i = 0;
        while(i < max) {
            address holder = _reflections.getKeyAtIndex(randomIndex);
            if((holder != sender) && (holder != recipient)) { 
                if(!_redistributeReflectionFor(holder)) break;
                i++;
            }
            randomIndex = (randomIndex + 1) % size;
        }
    }

    function _redistributeReflectionFor(address holder) internal  returns (bool){
        if(_reflections.exists(holder)) {
            uint256 balance = _balances[holder];
            uint256 lastReflectionTotal = _reflections.get(holder);
            uint256 reflection_amount = balance * (_reflectionTotal - lastReflectionTotal) / (_totalSupply - _excludedSupply);
            if(reflection_amount > 0) {
                if(_balances[address(this)] >= (reflection_amount)) {
                    _balances[address(this)] = _balances[address(this)] - reflection_amount;
                    _balances[holder] = _balances[holder] + reflection_amount;
                    _reflections.set(holder, _reflectionTotal);
                    emit Transfer(address(this), holder, reflection_amount);
                    return true;
                } else return false;
            }
        }
        _reflections.set(holder, _reflectionTotal);
        return true;
    }

    event BlacklistUpdated(address indexed account, bool isBlacklisted);

    function blacklist(address account) public onlyOwner {
        require(!_blacklist[account], "Account is already blacklisted");
        _blacklist[account] = true;
        emit BlacklistUpdated(account, true);
    }

    function unBlacklist(address account) public onlyOwner {
        require(_blacklist[account], "Account is not blacklisted");
        delete(_blacklist[account]);
        emit BlacklistUpdated(account, false);
    }

    function isBlacklisted(address account) public view returns (bool) {
        return _blacklist[account];
    }
    
    event ExcludedUpdated(address indexed account, bool isExcluded, uint256 balance, uint256 excludedSupply);

    function _excludeReflection(address account) internal {
        if(!_excludedFromReflection[account]) {
            _excludedFromReflection[account] = true;
            _excludedSupply+= _balances[account];
            emit ExcludedUpdated(account, true, _balances[account], _excludedSupply);
        }
    }

    function _checkAccount(address account) internal {
        if(!_excludedFromReflection[account]) {
            if(_isPair(account)) {
                _excludeReflection(account);
            }
        }
    }

    function _isPair(address account) internal view returns (bool) {
        bool success;
        bytes memory data;
        (success, data) = account.staticcall(abi.encodeWithSelector(0x0dfe1681));//token0
        if(success && (data.length == 32)) {
            if(address(uint160(uint256(bytes32(data)))) == address(this)) return true;
        } else return false;
        (success, data) = account.staticcall(abi.encodeWithSelector(0xd21220a7));//token1
        if(success && (data.length == 32)) {
            if(address(uint160(uint256(bytes32(data)))) == address(this)) return true;
        }
        return false;
    }
}