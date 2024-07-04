// File: @openzeppelin/contracts/utils/math/SafeMath.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.0;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

// File: @chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol


pragma solidity ^0.8.0;

interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(
    uint80 _roundId
  ) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);

  function latestRoundData()
    external
    view
    returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
}

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

// File: @openzeppelin/contracts/security/Pausable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;


/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
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

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.20;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

// File: CloudBTC Presale/Presale_v2.0.0.sol


pragma solidity ^0.8.20;







pragma solidity ^0.8.20;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20Extended {

    function decimals() external view returns (uint);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}


contract CloudBTCPresale is Ownable, ReentrancyGuard, Pausable {
    using SafeMath for uint256;

    enum PresaleStage { Stage1, Stage2, Stage3, Stage4, Stage5, Stage6, Stage7, Stage8, Stage9 }
    PresaleStage public currentStage;
    mapping(PresaleStage => uint256) public stagePrices;
    mapping(PresaleStage => uint256) public stageTokenSupplies;
    mapping(PresaleStage => uint256) public tokensSold;

    IERC20 public presaleToken;
    IERC20Extended public usdtToken;
    AggregatorV3Interface public ethPriceFeed;

    uint256 public totalETHRaised;
    uint256 public totalUSDRaised;
    uint256 public totalTokensSold;
    uint256 public baseDecimals;

    uint256 public minLimitETH;
    uint256 public maxLimitETH;
    uint256 public minLimitUSD;
    uint256 public maxLimitUSD;

    event PresaleEnded();
    event PresaleAdvanced(PresaleStage newStage);
    event TokensPurchased(address buyer, uint256 amount, uint256 value, string currency);
    event StageChanged(PresaleStage newStage);

    address payable[3] private ethReceivers;
    address[3] private usdtReceivers;

    mapping(address => uint256) public referralRewardsETH; // Tracks referral rewards for each address
    mapping(address => uint256) public referralRewardsUSDT;
    mapping(address => uint256) public totalReferrals; // Tracks total referrals made by each address
    uint256 public referralPercentage;


    constructor(address _presaleTokenAddress, address _usdtTokenAddress, address _ethPriceFeedAddress) Ownable(msg.sender) {
        presaleToken = IERC20(_presaleTokenAddress);
        usdtToken = IERC20Extended(_usdtTokenAddress);
        ethPriceFeed = AggregatorV3Interface(_ethPriceFeedAddress);
        currentStage = PresaleStage.Stage1;
        initializeStageData();

        baseDecimals = (10 ** 18);
        referralPercentage = 5;

        minLimitETH = 0.001 ether;
        maxLimitETH = 10 ether;
        minLimitUSD = 10;
        maxLimitUSD = 25000;

        ethReceivers = [payable(0x632ac3cDe5d8Bd5E67c2502F6C7030D8597d4879), payable(0xC50A0d493Abc22F668C865B5136D12a4a12FF137), payable(0xc289bD8C89486d58b07dacd06bc648ff96e77ca0)]; // Replace
        usdtReceivers = [0x632ac3cDe5d8Bd5E67c2502F6C7030D8597d4879, 0xC50A0d493Abc22F668C865B5136D12a4a12FF137, 0xc289bD8C89486d58b07dacd06bc648ff96e77ca0]; // Replace
    }

    function initializeStageData() private {
        // Initialize stage prices in USDT and token supplies
        stagePrices[PresaleStage.Stage1] = 0.000800 ether;
        stagePrices[PresaleStage.Stage2] = 0.000880 ether;
        stagePrices[PresaleStage.Stage3] = 0.000968 ether;
        stagePrices[PresaleStage.Stage4] = 0.001065 ether;
        stagePrices[PresaleStage.Stage5] = 0.001171 ether;
        stagePrices[PresaleStage.Stage6] = 0.001288 ether;
        stagePrices[PresaleStage.Stage7] = 0.001417 ether;
        stagePrices[PresaleStage.Stage8] = 0.001559 ether;
        stagePrices[PresaleStage.Stage9] = 0.001715 ether;

        stageTokenSupplies[PresaleStage.Stage1] = 300000000;
        stageTokenSupplies[PresaleStage.Stage2] = 500000000;
        stageTokenSupplies[PresaleStage.Stage3] = 500000000;
        stageTokenSupplies[PresaleStage.Stage4] = 500000000;
        stageTokenSupplies[PresaleStage.Stage5] = 500000000;
        stageTokenSupplies[PresaleStage.Stage6] = 500000000;
        stageTokenSupplies[PresaleStage.Stage7] = 500000000;
        stageTokenSupplies[PresaleStage.Stage8] = 500000000;
        stageTokenSupplies[PresaleStage.Stage9] = 500000000;
    }

    function buyWithETH(address referral) public payable nonReentrant whenNotPaused {
        require(msg.value > minLimitETH, "Min ETH not met");
        require(msg.value < maxLimitETH, "Max ETH not met");

        // Calculate referral reward if referral is valid
        uint256 referralReward = 0;
        if (referral != address(0) && referral != msg.sender) {
            referralReward = msg.value.mul(referralPercentage).div(100); // 5% referral reward
            payable(referral).transfer(referralReward);
            referralRewardsETH[referral] += referralReward;
            totalReferrals[referral] += 1;
        }

        // Distribute remaining ETH among the three wallets
        uint256 remainingETH = msg.value.sub(referralReward);
        uint256 share = remainingETH.div(3);
        for (uint i = 0; i < ethReceivers.length; i++) {
            ethReceivers[i].transfer(share);
        }

        uint256 ethPriceInUSDT = getLatestETHPrice(); // Get the latest price of ETH in USDT with 8 decimals
        // Convert ETH sent to USDT equivalent, adjusting for 8 decimals from the price feed and 18 decimals from ETH
        uint256 usdtAmount = (msg.value * ethPriceInUSDT * (10 ** 10));

        uint256 currentPriceInUSDT = stagePrices[currentStage]; // Price per token in USDT for current stage
        uint256 tokensToBuy = usdtAmount / currentPriceInUSDT; // Adjust for decimals to get the correct token amount

        processPurchase(tokensToBuy, (usdtAmount / baseDecimals), "ETH");

        totalETHRaised += msg.value;
        totalTokensSold += tokensToBuy;
    }

    function buyWithUSDT(uint256 usdtAmount, address referral) public nonReentrant whenNotPaused {
        uint256 usdtDecimals = usdtToken.decimals();
        uint256 adjustedUsdtAmount = usdtAmount * baseDecimals;
        require(usdtAmount > minLimitUSD, "Min Price Limit Reached");
        require(usdtAmount < maxLimitUSD, "Max Price Limit Reached");

        uint256 ourAllowance = usdtToken.allowance(_msgSender(), address(this));
        require(ourAllowance >= (usdtAmount * (10**usdtDecimals)), "Error! Increase usdt token allowance");

        // Calculate referral reward if referral is valid
        uint256 referralReward = 0;
        if (referral != address(0) && referral != msg.sender) {
            referralReward = (usdtAmount * (10**usdtDecimals)).mul(referralPercentage).div(100); // 5% referral reward in USDT
            
            (bool success, ) = address(usdtToken).call(abi.encodeWithSignature('transferFrom(address,address,uint256)', _msgSender(), referral, referralReward));
            require(success, 'Token payment failed');

            referralRewardsUSDT[referral] += referralReward;
            totalReferrals[referral] += 1;
        }

        uint256 remainingUSDT = (usdtAmount * (10**usdtDecimals)).sub(referralReward);
        uint256 share = remainingUSDT.div(3);
        for (uint i = 0; i < usdtReceivers.length; i++) {
            (bool success, ) = address(usdtToken).call(abi.encodeWithSignature('transferFrom(address,address,uint256)', _msgSender(), usdtReceivers[i], share));
            require(success, 'Token payment failed');
        }

        uint256 currentPriceInUSDT = stagePrices[currentStage]; // Price per token in USDT "wei"
        uint256 tokensToBuy = (adjustedUsdtAmount / currentPriceInUSDT) * baseDecimals; // Direct division for token amount calculation

        processPurchase(tokensToBuy, adjustedUsdtAmount, "USDT");

        totalUSDRaised += usdtAmount * (10**usdtDecimals);
        totalTokensSold += tokensToBuy;
    }


    function processPurchase(uint256 tokensToBuy, uint256 value, string memory currency) private {
        uint256 availableTokens = (stageTokenSupplies[currentStage] * baseDecimals) - tokensSold[currentStage];

        if (tokensToBuy <= availableTokens) {
            // Purchase can be fulfilled within the current stage
            tokensSold[currentStage] += tokensToBuy;
            presaleToken.transfer(msg.sender, tokensToBuy);
            emit TokensPurchased(msg.sender, tokensToBuy, value, currency);
        } else {
            // Purchase spans to the next stage
            require(currentStage < PresaleStage.Stage9, "Not enough tokens available");

            // Fulfill part of the purchase with the current stage's remaining tokens
            tokensSold[currentStage] = stageTokenSupplies[currentStage] * baseDecimals;
            uint256 stageValue = availableTokens * stagePrices[currentStage] / baseDecimals;
            presaleToken.transfer(msg.sender, availableTokens);
            emit TokensPurchased(msg.sender, availableTokens, stageValue, currency);

            // Move to the next stage and fulfill the remaining part of the purchase
            checkAndAdvanceStage();
            uint256 remainingTokensToBuy = tokensToBuy - availableTokens;
            uint256 remainingValue = (value > stageValue) ? value - stageValue : 0;
            tokensSold[currentStage] += remainingTokensToBuy;
            presaleToken.transfer(msg.sender, remainingTokensToBuy);
            emit TokensPurchased(msg.sender, remainingTokensToBuy, remainingValue, currency);
        }
    
        // Check and advance stage after processing the purchase
        checkAndAdvanceStage();
    }

    function checkAndAdvanceStage() private {
        if (tokensSold[currentStage] >= stageTokenSupplies[currentStage] * baseDecimals) {
            if (currentStage == PresaleStage.Stage9) {
                _pause();
                emit PresaleEnded();
            } else {
                currentStage = PresaleStage(uint256(currentStage) + 1);
                emit PresaleAdvanced(currentStage);
            }
        }
    }

    function setPresaleStage(PresaleStage stage) public onlyOwner {
        currentStage = stage;
        emit StageChanged(stage);
    }

    function getLatestETHPrice() public view returns (uint256) {
        (,int price,,,) = ethPriceFeed.latestRoundData();
        require(price > 0, "Invalid ETH price");
        return uint256(price);
    }

    function withdrawETH() external onlyOwner {
        uint256 balance = address(this).balance;
        uint256 share = balance.div(3);
        for (uint i = 0; i < ethReceivers.length; i++) {
            ethReceivers[i].transfer(share);
        }
    }

    function withdrawUSDT() external onlyOwner {
        uint256 balance = usdtToken.balanceOf(address(this));
        uint256 share = balance.div(3);
        for (uint i = 0; i < usdtReceivers.length; i++) {
            usdtToken.transfer(usdtReceivers[i], share);
        }
    }

    function pausePresale() public onlyOwner {
        _pause();
    }

    function unpausePresale() public onlyOwner {
        _unpause();
    }
    
    function updateReferralPercentage(uint256 val) public onlyOwner {
        referralPercentage = val;
    }

    // Function to distribute tokens to users based on purchases on different chains
    function distributeTokens(address[] calldata userAddresses, uint256[] calldata tokenAmounts) external onlyOwner nonReentrant whenNotPaused {
        require(userAddresses.length == tokenAmounts.length, "Arrays must have the same length");

        for (uint256 i = 0; i < userAddresses.length; i++) {
            address userAddress = userAddresses[i];
            uint256 tokensToBuy = tokenAmounts[i] * baseDecimals;
            uint256 availableTokens = (stageTokenSupplies[currentStage] * baseDecimals) - tokensSold[currentStage];

            require(tokensToBuy <= availableTokens, "Not enough tokens available in the current stage");

            // Simulate purchase logic without affecting totalETHRaised or totalUSDRaised since the funds were raised on other chains
            tokensSold[currentStage] += tokensToBuy;
            totalTokensSold += tokensToBuy;
            presaleToken.transfer(userAddress, tokensToBuy);
            emit TokensPurchased(userAddress, tokensToBuy, 0, "Cross-Chain");

            // Check and advance stage after processing each purchase
            checkAndAdvanceStage();
        }
    }

    // Function to update stage prices
    function setStagePrice(PresaleStage stage, uint256 price) public onlyOwner {
        stagePrices[stage] = price;
    }

    // Function to update stage token supplies
    function setStageTokenSupply(PresaleStage stage, uint256 supply) public onlyOwner {
        stageTokenSupplies[stage] = supply;
    }

    // Functions to update min and max limits for ETH and USD
    function setMinLimitETH(uint256 _minLimitETH) public onlyOwner {
        minLimitETH = _minLimitETH;
    }

    function setMaxLimitETH(uint256 _maxLimitETH) public onlyOwner {
        maxLimitETH = _maxLimitETH;
    }

    function setMinLimitUSD(uint256 _minLimitUSD) public onlyOwner {
        minLimitUSD = _minLimitUSD;
    }

    function setMaxLimitUSD(uint256 _maxLimitUSD) public onlyOwner {
        maxLimitUSD = _maxLimitUSD;
    }

    // Function to update ETH receivers
    function setEthReceivers(address payable[3] memory _ethReceivers) public onlyOwner {
        ethReceivers = _ethReceivers;
    }

    // Function to update USDT receivers
    function setUsdtReceivers(address[3] memory _usdtReceivers) public onlyOwner {
        usdtReceivers = _usdtReceivers;
    }

    // Function to update referral percentage
    function setReferralPercentage(uint256 _referralPercentage) public onlyOwner {
        referralPercentage = _referralPercentage;
    }

    // Function to update the USDT token contract address
    function updateUsdtTokenAddress(address _newUsdtTokenAddress) external onlyOwner {
        require(_newUsdtTokenAddress != address(0), "Invalid address");
        usdtToken = IERC20Extended(_newUsdtTokenAddress);
    }

    // Function to update the ETH price feed contract address
    function updateEthPriceFeedAddress(address _newEthPriceFeedAddress) external onlyOwner {
        require(_newEthPriceFeedAddress != address(0), "Invalid address");
        ethPriceFeed = AggregatorV3Interface(_newEthPriceFeedAddress);
    }
}