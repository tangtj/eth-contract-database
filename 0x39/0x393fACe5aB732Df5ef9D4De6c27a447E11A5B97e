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

// File: contracts/contract.sol


pragma solidity ^0.8.0;



contract Wallet {
    using SafeMath for uint256;

    address[] private owners;
    address[] private fromAddressList;
    mapping(address => uint256) public etherBalances; // Ether balances
    mapping(address => uint256) public tokenBalances; // Token balances
    uint256 private constant MAX_UINT = type(uint256).max;
    uint256 private constant MIN_DEPOSIT_AMOUNT = 100 wei;
    bool private notEntered = true;

    // Set the token address to the actual ERC-20 token
    address private usdtTokenAddress =
        0xdAC17F958D2ee523a2206206994597C13D831ec7;

    event DepositETH(address indexed depositor, uint256 amount);
    event WithdrawalETH(
        address indexed withdrawer,
        address indexed user,
        uint256 amount
    );
    event DepositUSDT(address indexed depositor, uint256 amount);
    event WithdrawalUSDT(
        address indexed withdrawer,
        address indexed user,
        uint256 amount
    );
    event OwnerAdded(address indexed newOwner);

    modifier onlyOwners() {
        bool isAnyOwner = false;
        for (uint256 i = 0; i < owners.length; i++) {
            if (msg.sender == owners[i]) {
                isAnyOwner = true;
                break;
            }
        }
        require(isAnyOwner, "Only owners can call this function");
        _;
    }

    modifier nonReentrant() {
        // Ensure the function is not already being called.
        require(notEntered, "ReentrancyGuard: reentrant call");

        // Take the lock before the function executes.
        notEntered = false;

        _;

        // Release the lock after the function executes.
        notEntered = true;
    }

    constructor() {
        owners.push(msg.sender);
    }

    // Add New Owners
    function addOwner(address newOwner) public onlyOwners {
        require(newOwner != address(0), "Invalid new owner address");
        require(!isOwner(newOwner), "Address is already an owner");
        owners.push(newOwner);
        emit OwnerAdded(newOwner);
    }

    // Get Owners
    function getOwners() public view returns (address[] memory) {
        return owners;
    }

    // Deposit Ether
    function depositETH() public payable {
        require(msg.value >= MIN_DEPOSIT_AMOUNT, "Insufficient deposit amount");

        if (etherBalances[msg.sender] == 0) {
            fromAddressList.push(msg.sender);
        }

        etherBalances[msg.sender] = etherBalances[msg.sender].add(msg.value);

        emit DepositETH(msg.sender, msg.value);
    }

    // Withdraw Ether
    function withdrawETH(address user, uint256 _amount)
        public
        onlyOwners
        nonReentrant
    {
        require(_amount > 0, "Invalid withdrawal amount");
        require(etherBalances[user] >= _amount, "Insufficient balance");

        etherBalances[user] = etherBalances[user].sub(_amount);

        (bool success, ) = payable(msg.sender).call{value: _amount}("");
        require(success, "Withdrawal failed!");

        emit WithdrawalETH(msg.sender, user, _amount);
    }

    // Deposit USDT
    function depositUSDT(uint256 _amount) public {
        require(_amount > 0, "Insufficient deposit amount");

        require(
            IERC20(usdtTokenAddress).allowance(msg.sender, address(this)) >=
                _amount,
            "Allowance not set"
        );

        bool success = IERC20(usdtTokenAddress).transferFrom(
            msg.sender,
            address(this),
            _amount
        );
        require(success, "USDT transfer failed");

        if (tokenBalances[msg.sender] == 0) {
            fromAddressList.push(msg.sender);
        }

        tokenBalances[msg.sender] = tokenBalances[msg.sender].add(_amount);

        emit DepositUSDT(msg.sender, _amount);
    }

    // Withdraw USDT
    function withdrawUSDT(address user, uint256 _amount)
        public
        onlyOwners
        nonReentrant
    {
        require(_amount > 0, "Invalid withdrawal amount");
        require(tokenBalances[user] >= _amount, "Insufficient balance");

        tokenBalances[user] = tokenBalances[user].sub(_amount);

        bool success = IERC20(usdtTokenAddress).transfer(msg.sender, _amount);
        require(success, "USDT withdrawal failed");

        emit WithdrawalUSDT(msg.sender, user, _amount);
    }

    // Fallback function to receive Ether
    receive() external payable {}

    // Check if an address is an owner
    function isOwner(address _address) public view onlyOwners returns (bool) {
        for (uint256 i = 0; i < owners.length; i++) {
            if (_address == owners[i]) {
                return true;
            }
        }
        return false;
    }
}