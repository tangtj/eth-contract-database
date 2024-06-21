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

// File: contracts/XCashier.sol


pragma solidity ^0.8.20;


contract XCashier is Ownable {
    event DepositReceived(address indexed sender, address indexed recipient, uint256 id, uint256 amount);
    event Withdrawn(address, uint256);
    event MinAmountSet(uint256);
    event MaxAmountSet(uint256);
    event OperatorSet(address indexed operator, bool active);
    event Paused();
    event Unpaused();

    uint256 public maxAmount;
    uint256 public minAmount;
    uint256 private counter;
    bool private paused;

    mapping(address => bool) private operators;
    modifier onlyOperator {
        require(operators[msg.sender], "invalid operator");
        _;
    }

    constructor(uint256 _minAmount, uint256 _maxAmount) Ownable(msg.sender) {
        _setMaxAmount(_maxAmount);
        _setMinAmount(_minAmount);

        setOperator(msg.sender, true);
    }

    function _setMinAmount(uint256 _minAmount) internal {
        require(_minAmount <= maxAmount, "invalid min amount");
        minAmount = _minAmount;
    }

    function _setMaxAmount(uint256 _maxAmount) internal {
        require(_maxAmount >= minAmount, "invalid max amount");
        maxAmount = _maxAmount;
    }

    function setMinAmount(uint256 _minAmount) public onlyOperator {
        _setMinAmount(_minAmount);
    }

    function setMaxAmount(uint256 _maxAmount) public onlyOperator {
        _setMaxAmount(_maxAmount);
    }

    function pause() public onlyOperator {
        paused = true;
        emit Paused();
    }

    function unpause() public onlyOperator {
        paused = false;
        emit Unpaused();
    }

    function setOperator(address _operator, bool _active) public onlyOwner {
        operators[_operator] = _active;
        emit OperatorSet(_operator, _active);
    }

    function withdraw() public onlyOwner {
        payable(msg.sender).transfer(address(this).balance);
    }

    function _deposit(address _sender, address _recipient, uint256 _amount) internal {
        require(!paused, "invalid status");
        require(_amount >= minAmount && _amount <= maxAmount, "invalid amount");
        counter++;
        emit DepositReceived(_sender, _recipient, counter, _amount);
    }

    function deposit(address _recipient) public payable {
        _deposit(msg.sender, _recipient, msg.value);
    }

    receive() external payable {
        _deposit(msg.sender, msg.sender, msg.value);
    }
}