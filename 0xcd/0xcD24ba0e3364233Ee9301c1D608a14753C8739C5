/**
 * @dev This smart contract was developed based on the general
 * OpenZeppelin Contracts guidelines where functions revert instead of
 * returning `false` on failure.
 */

// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

/********************************************************************************************
  LIBRARY
********************************************************************************************/

/**
 * @title Address Library
 *
 * @notice Collection of functions providing utility for interacting with addresses.
 */
library Address {
    // ERROR

    /**
     * @notice Error indicating insufficient balance while performing an operation.
     *
     * @param account Address where the balance is insufficient.
     */
    error AddressInsufficientBalance(address account);

    /**
     * @notice Error indicating an attempt to interact with a contract having empty code.
     *
     * @param target Address of the contract with empty code.
     */
    error AddressEmptyCode(address target);

    /**
     * @notice Error indicating a failed internal call.
     */
    error FailedInnerCall();

    // FUNCTION

    /**
     * @notice Calls a function on a specified address without transferring value.
     *
     * @param target Address on which the function will be called.
     * @param data Encoded data of the function call.
     *
     * @return returndata Result of the function call.
     *
     * @dev The `target` must be a contract address and this function must be calling
     * `target` with `data` not reverting.
     */
    function functionCall(
        address target,
        bytes memory data
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0);
    }

    /**
     * @notice Calls a function on a specified address with a specified value.
     *
     * @param target Address on which the function will be called.
     * @param data Encoded data of the function call.
     * @param value Value to be sent in the call.
     *
     * @return returndata Result of the function call.
     *
     * @dev This function ensure that the calling contract actually have Ether balance
     * of at least `value` and that the called Solidity function is a `payable`. Should
     * throw if caller does have insufficient balance.
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value
    ) internal returns (bytes memory) {
        if (address(this).balance < value) {
            revert AddressInsufficientBalance(address(this));
        }
        (bool success, bytes memory returndata) = target.call{value: value}(
            data
        );
        return verifyCallResultFromTarget(target, success, returndata);
    }

    /**
     * @notice Verifies the result of a function call and handles errors if any.
     *
     * @param target Address on which the function was called.
     * @param success Boolean indicating the success of the function call.
     * @param returndata Result data of the function call.
     *
     * @return Result of the function call or reverts with an appropriate error.
     *
     * @dev This help to verify that a low level call to smart-contract was successful
     * and will reverts if the target was not a contract. For unsuccessful call, this
     * will bubble up the revert reason (falling back to {FailedInnerCall}). Should
     * throw if both the returndata and target.code length are 0 when `success` is true.
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata
    ) internal view returns (bytes memory) {
        if (!success) {
            _revert(returndata);
        } else {
            if (returndata.length == 0 && target.code.length == 0) {
                revert AddressEmptyCode(target);
            }
            return returndata;
        }
    }

    /**
     * @notice Reverts with decoded revert data or FailedInnerCall if no revert
     * data is available.
     *
     * @param returndata Result data of a failed function call.
     *
     * @dev Should throw if returndata length is 0.
     */
    function _revert(bytes memory returndata) private pure {
        if (returndata.length > 0) {
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert FailedInnerCall();
        }
    }
}

/**
 * @title SafeERC20 Library
 *
 * @notice Collection of functions providing utility for safe operations with
 * ERC20 tokens.
 *
 * @dev This is mainly for the usage of token that throw on failure (when the
 * token contract returns false). Tokens that return no value (and instead revert
 * or throw on failure) are also supported where non-reverting calls are assumed
 * to be a successful transaction.
 */
library SafeERC20 {
    // LIBRARY

    using Address for address;

    // ERROR

    /**
     * @notice Error indicating a failed operation during an ERC-20 token transfer.
     *
     * @param token Address of the token contract.
     */
    error SafeERC20FailedOperation(address token);

    // FUNCTION

    /**
     * @notice Safely transfers tokens.
     *
     * @param token ERC20 token interface.
     * @param to Address to which the tokens will be transferred.
     * @param value Amount of tokens to be transferred.
     *
     * @dev Transfer `value` amount of `token` from the calling contract to `to` where
     * non-reverting calls are assumed to be successful if `token` returns no value.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeCall(token.transfer, (to, value)));
    }

    /**
     * @notice Calls a function on a token contract and reverts if the operation fails.
     *
     * @param token ERC20 token interface.
     * @param data Encoded data of the function call.
     *
     * @dev This imitates a Solidity high-level call such as a regular function call to
     * a contract while relaxing the requirement on the return value.
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        bytes memory returndata = address(token).functionCall(data);
        if (returndata.length != 0 && !abi.decode(returndata, (bool))) {
            revert SafeERC20FailedOperation(address(token));
        }
    }
}

/********************************************************************************************
  INTERFACE
********************************************************************************************/

/**
 * @title Router Interface
 *
 * @notice Interface of the Router contract, providing functions to interact with
 * Router contract that is derived from Uniswap V2 Router.
 *
 * @dev See https://docs.uniswap.org/contracts/v2/reference/smart-contracts/router-02
 */
interface IRouter {

    // FUNCTION

    /**
     * @notice Get the address of the Wrapped Ether (WETH) token.
     *
     * @return The address of the WETH token.
     */
    function WETH() external pure returns (address);

    /**
     * @notice Get the address of the linked Factory contract.
     *
     * @return The address of the Factory contract.
     */
    function factory() external pure returns (address);

    /**
     * @notice Swaps an exact amount of tokens for ETH, supporting
     * tokens that implement fee-on-transfer mechanisms.
     *
     * @param amountIn The exact amount of input tokens for the swap.
     * @param amountOutMin The minimum acceptable amount of ETH to receive in the swap.
     * @param path An array of token addresses representing the token swap path.
     * @param to The recipient address that will receive the swapped ETH.
     * @param deadline The timestamp by which the transaction must be executed to be
     * considered valid.
     *
     * @dev This function swaps a specific amount of tokens for ETH on a specified path,
     * ensuring a minimum amount of output ETH.
     */
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint256 amountIn, uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external;

    /**
     * @notice Swaps a precise amount of ETH for tokens, supporting tokens with fee-on-transfer mechanisms.
     *
     * @param amountOutMin The minimum acceptable amount of output tokens expected from the swap.
     * @param path An array of token addresses representing the token swap path.
     * @param to The recipient address that will receive the swapped tokens.
     * @param deadline The timestamp by which the transaction must be executed to be considered valid.
     *
     * @dev This function performs a direct swap of a specified amount of ETH for tokens based on the provided
     * path and minimum acceptable output token amount.
     */
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint256 amountOutMin, address[] calldata path, address to, uint256 deadline) external payable;
}

/**
 * @title Factory Interface
 *
 * @notice Interface of the Factory contract, providing functions to interact with
 * Factory contract that is derived from Uniswap V2 Factory.
 *
 * @dev See https://docs.uniswap.org/contracts/v2/reference/smart-contracts/factory
 */
interface IFactory {

    // FUNCTION

    /**
     * @notice Create a new token pair for two given tokens on Uniswap V2-based factory.
     *
     * @param tokenA The address of the first token.
     * @param tokenB The address of the second token.
     *
     * @return pair The address of the created pair for the given tokens.
     */
    function createPair(address tokenA, address tokenB) external returns (address pair);

    /**
     * @notice Get the address of the pair for two tokens on the decentralized exchange.
     *
     * @param tokenA The address of the first token.
     * @param tokenB The address of the second token.
     *
     * @return pair The address of the pair corresponding to the provided tokens.
     */
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

/**
 * @title Pair Interface
 *
 * @notice Interface of the Pair contract in a decentralized exchange based on the
 * Pair contract that is derived from Uniswap V2 Pair.
 *
 * @dev See https://docs.uniswap.org/contracts/v2/reference/smart-contracts/pair
 */
interface IPair {

    // FUNCTION

    /**
     * @notice Get the address of the first token in the pair.
     *
     * @return The address of the first token.
     */
    function token0() external view returns (address);

    /**
     * @notice Get the address of the second token in the pair.
     *
     * @return The address of the second token.
     */
    function token1() external view returns (address);
}

/**
 * @title ERC20 Token Standard Interface
 *
 * @notice Interface of the ERC-20 standard token as defined in the ERC.
 *
 * @dev See https://eips.ethereum.org/EIPS/eip-20
 */
interface IERC20 {

    // EVENT

    /**
     * @notice Emitted when `value` tokens are transferred from
     * one account (`from`) to another (`to`).
     *
     * @param from The address tokens are transferred from.
     * @param to The address tokens are transferred to.
     * @param value The amount of tokens transferred.
     *
     * @dev The `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @notice Emitted when the allowance of a `spender` for an `owner`
     * is set by a call to {approve}.
     *
     * @param owner The address allowing `spender` to spend on their behalf.
     * @param spender The address allowed to spend tokens on behalf of `owner`.
     * @param value The allowance amount set for `spender`.
     *
     * @dev The `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    // FUNCTION

    /**
     * @notice Returns the value of tokens in existence.
     *
     * @return The value of the total supply of tokens.
     *
     * @dev This should get the total token supply.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @notice Returns the value of tokens owned by `account`.
     *
     * @param account The address to query the balance for.
     *
     * @return The token balance of `account`.
     *
     * @dev This should get the token balance of a specific account.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @notice Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * @param to The address to transfer tokens to.
     * @param value The amount of tokens to be transferred.
     *
     * @return A boolean indicating whether the transfer was successful or not.
     *
     * @dev This should transfer tokens to a specified address and emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @notice Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}.
     *
     * @param owner The address allowing `spender` to spend on their behalf.
     * @param spender The address allowed to spend tokens on behalf of `owner`.
     *
     * @return The allowance amount for `spender`.
     *
     * @dev The return value should be zero by default and
     * changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @notice Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
     *
     * @param spender The address allowed to spend tokens on behalf of the sender.
     * @param value The allowance amount for `spender`.
     *
     * @return A boolean indicating whether the approval was successful or not.
     *
     * @dev This should approve `spender` to spend a specified amount of tokens
     * on behalf of the sender and emits an {Approval} event.
     */
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @notice Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's allowance.
     *
     * @param from The address to transfer tokens from.
     * @param to The address to transfer tokens to.
     * @param value The amount of tokens to be transferred.
     *
     * @return A boolean indicating whether the transfer was successful or not.
     *
     * @dev This should transfer tokens from one address to another after
     * spending caller's allowance and emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

/**
 * @title ERC20 Token Metadata Interface
 *
 * @notice Interface for the optional metadata functions of the ERC-20 standard as defined in the ERC.
 *
 * @dev It extends the IERC20 interface. See https://eips.ethereum.org/EIPS/eip-20
 */
interface IERC20Metadata is IERC20 {

    // FUNCTION

    /**
     * @notice Returns the name of the token.
     *
     * @return The name of the token as a string.
     */
    function name() external view returns (string memory);

    /**
     * @notice Returns the symbol of the token.
     *
     * @return The symbol of the token as a string.
     */
    function symbol() external view returns (string memory);

    /**
     * @notice Returns the number of decimals used to display the token.
     *
     * @return The number of decimals as a uint8.
     */
    function decimals() external view returns (uint8);
}

/**
 * @title ERC20 Token Standard Error Interface
 *
 * @notice Interface of the ERC-6093 custom errors that defined common errors
 * related to the ERC-20 standard token functionalities.
 *
 * @dev See https://eips.ethereum.org/EIPS/eip-6093
 */
interface IERC20Errors {

    // ERROR

    /**
     * @notice Error indicating that the `sender` has inssufficient `balance` for the operation.
     *
     * @param sender Address whose tokens are being transferred.
     * @param balance Current balance for the interacting account.
     * @param needed Minimum amount required to perform a transfer.
     *
     * @dev The `needed` value is required to inform user on the needed amount.
     */
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);

    /**
     * @notice Error indicating that the `sender` is invalid for the operation.
     *
     * @param sender Address whose tokens are being transferred.
     */
    error ERC20InvalidSender(address sender);

    /**
     * @notice Error indicating that the `receiver` is invalid for the operation.
     *
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC20InvalidReceiver(address receiver);

    /**
     * @notice Error indicating that the `spender` does not have enough `allowance` for the operation.
     *
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     * @param allowance Amount of tokens a `spender` is allowed to operate with.
     * @param needed Minimum amount required to perform a transfer.
     *
     * @dev The `needed` value is required to inform user on the needed amount.
     */
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);

    /**
     * @notice Error indicating that the `approver` is invalid for the approval operation.
     *
     * @param approver Address initiating an approval operation.
     */
    error ERC20InvalidApprover(address approver);

    /**
     * @notice Error indicating that the `spender` is invalid for the allowance operation.
     *
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC20InvalidSpender(address spender);
}

/**
 * @title Common Error Interface
 *
 * @notice Interface of the common errors not specific to ERC-20 functionalities.
 */
interface ICommonError {

    // ERROR

    /**
     * @notice Error indicating that the `current` address cannot be used in this context.
     *
     * @param current Address used in the context.
     */
    error CannotUseCurrentAddress(address current);

    /**
     * @notice Error indicating that the `current` value cannot be used in this context.
     *
     * @param current Value used in the context.
     */
    error CannotUseCurrentValue(uint256 current);

    /**
     * @notice Error indicating that the `current` state cannot be used in this context.
     *
     * @param current Boolean state used in the context.
     */
    error CannotUseCurrentState(bool current);

    /**
     * @notice Error indicating that the `invalid` address provided is not a valid address for this context.
     *
     * @param invalid Address used in the context.
     */
    error InvalidAddress(address invalid);

    /**
     * @notice Error indicating that the `invalid` value provided is not a valid value for this context.
     *
     * @param invalid Value used in the context.
     */
    error InvalidValue(uint256 invalid);
}

/********************************************************************************************
  ACCESS
********************************************************************************************/

/**
 * @title Ownable Contract
 *
 * @notice Abstract contract module implementing ownership functionality through
 * inheritance as a basic access control mechanism, where there is an owner account
 * that can be granted exclusive access to specific functions.
 *
 * @dev The initial owner is set to the address provided by the deployer and can
 * later be changed with {transferOwnership}.
 */
abstract contract Ownable {

    // DATA

    address private _owner;

    // MODIFIER

    /**
     * @notice Modifier that allows access only to the contract owner.
     *
     * @dev Should throw if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    // ERROR

    /**
     * @notice Error indicating that the `account` is not authorized to perform an operation.
     *
     * @param account Address used to perform the operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @notice Error indicating that the provided `owner` address is invalid.
     *
     * @param owner Address used to perform the operation.
     *
     * @dev Should throw if called by an invalid owner account such as address(0) as an example.
     */
    error OwnableInvalidOwner(address owner);

    // CONSTRUCTOR

    /**
     * @notice Initializes the contract setting the `initialOwner` address provided by
     * the deployer as the initial owner.
     *
     * @param initialOwner The address to set as the initial owner.
     *
     * @dev Should throw an error if called with address(0) as the `initialOwner`.
     */
    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
    }

    // EVENT

    /**
     * @notice Emitted when ownership of the contract is transferred.
     *
     * @param previousOwner The address of the previous owner.
     * @param newOwner The address of the new owner.
     */
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    // FUNCTION

    /**
     * @notice Get the address of the smart contract owner.
     *
     * @return The address of the current owner.
     *
     * @dev Should return the address of the current smart contract owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @notice Checks if the caller is the owner and reverts if not.
     *
     * @dev Should throw if the sender is not the current owner of the smart contract.
     */
    function _checkOwner() internal view virtual {
        if (owner() != msg.sender) {
            revert OwnableUnauthorizedAccount(msg.sender);
        }
    }

    /**
     * @notice Allows the current owner to renounce ownership and make the
     * smart contract ownerless.
     *
     * @dev This function can only be called by the current owner and will
     * render all `onlyOwner` functions inoperable.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @notice Allows the current owner to transfer ownership of the smart contract
     * to `newOwner` address.
     *
     * @param newOwner The address to transfer ownership to.
     *
     * @dev This function can only be called by the current owner and will render
     * all `onlyOwner` functions inoperable to him/her. Should throw if called with
     * address(0) as the `newOwner`.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    /**
     * @notice Internal function to transfer ownership of the smart contract
     * to `newOwner` address.
     *
     * @param newOwner The address to transfer ownership to.
     *
     * @dev This function replace current owner address stored as _owner with
     * the address of the `newOwner`.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

/********************************************************************************************
  TOKEN
********************************************************************************************/

/**
 * @title DeepSouth AI Token Contract
 *
 * @notice DeepSouth AI is an extended version of ERC-20 standard token that
 * includes additional functionalities for ownership control, trading enabling,
 * and token management.
 *
 * @dev Implements ERC20Metadata, ERC20Errors, and CommonError interfaces, and
 * extends Ownable contract.
 */
contract DeepSouthAI is Ownable, IERC20Metadata, IERC20Errors, ICommonError {

    // LIBRARY

    using SafeERC20 for IERC20;
    using Address for address;

    // DATA

    struct Fee {
        uint256 marketing;
        uint256 total;
    }

    Fee public buyFee = Fee(1000, 1000);
    Fee public sellFee = Fee(1000, 1000);
    Fee public transferFee = Fee(0, 0);
    Fee public collectedFee = Fee(0, 0);
    Fee public redeemedFee = Fee(0, 0);

    IRouter public router = IRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);

    string private constant NAME = "DeepSouth AI";
    string private constant SYMBOL = "SOUTH";

    uint8 private constant DECIMALS = 18;

    uint256 public constant FEEDENOMINATOR = 10_000;

    uint256 public immutable deployTime;

    uint256 private _totalSupply;

    uint256 public tradeStartTime = 0;
    uint256 public tradeStartBlock = 0;
    uint256 public minSwap = 1_000 ether;
    uint256 public maxWalletLimit = 200;

    address public projectOwner = 0x8683b156e02a61924f36E1a88dD1D30964FeB7c5;
    address public marketingReceiver = 0xFFb44e038Cd5bfAD4807FF6ace10793ec9d6330e;

    address public pair;

    bool public tradeEnabled = false;
    bool public isFeeActive = false;
    bool public isFeeLocked = false;
    bool public isReceiverLocked = false;
    bool public isFailsafeLocked = false;
    bool public isWalletLimitLocked = false;
    bool public isWalletLimitActive = false;
    bool public isSwapEnabled = false;
    bool public inSwap = false;

    // MAPPING

    mapping(address account => uint256) private _balances;
    mapping(address account => mapping(address spender => uint256)) private _allowances;

    mapping(address pair => bool) public isPairLP;
    mapping(address account => bool) public isExemptFee;
    mapping(address account => bool) public isExemptWalletLimit;

    // MODIFIER

    /**
     * @notice Modifier to mark the start and end of a swapping operation.
     */
    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }

    /**
     * @notice Modifier that allows access only to the project owner or current
     * smart contract owner.
     *
     * @dev Should throw if called by any account other than the project owner or
     * smart contract owner.
     */
    modifier onlyOwnerFailsafe() {
        _checkOwnerFailsafe();
        _;
    }

    // ERROR

    /**
     * @notice Error indicating that an action is attempted before the cooldown period ends.
     *
     * @param cooldownEnd The timestamp when the cooldown period ends.
     * @param timeLeft The time remaining in the cooldown period.
     *
     * @dev The `timeLeft` is required to inform user of the waiting period.
     */
    error WaitForCooldownTimer(uint256 cooldownEnd, uint256 timeLeft);

    /**
     * @notice Error indicating that trading has already been enabled at a specific `timestamp`.
     *
     * @param currentState The current state of trading.
     * @param timestamp The timestamp when trading was enabled.
     *
     * @dev The `currentState` is required to inform user of the current state of trading.
     */
    error TradeAlreadyEnabled(bool currentState, uint256 timestamp);

    /**
     * @notice Error indicating an invalid total fee compared to the maximum allowed.
     *
     * @param current The current total fee.
     * @param max The maximum allowed total fee.
     *
     * @dev The `max` is required to inform user of the maximum value allowed.
     */
    error InvalidTotalFee(uint256 current, uint256 max);

    /**
     * @notice Error indicating an invalid max wallet limit used compared to
     * the minimum value allowed.
     *
     * @param current The current limit used.
     * @param min The minimum allowed wallet limit.
     *
     * @dev The `min` is required to inform user of the minimum value allowed.
     */
    error InvalidWalletLimit(uint256 current, uint256 min);

    /**
     * @notice Error indicating an invalid amount compared to the maximum allowed.
     *
     * @param current The current amount.
     * @param max The maximum allowed wallet limit.
     *
     * @dev The `max` is required to inform user of the maximum value allowed.
     */
    error ExceedWalletLimit(uint256 current, uint256 max);

    /**
     * @notice Error indicating an invalid amount compared to the maximum allowed.
     *
     * @param current The current amount used.
     * @param max The maximum amount allowed to be used.
     *
     * @dev The `max` is required to inform user of the maximum value allowed.
     */
    error CannotRedeemMoreThanAllowedTreshold(uint256 current, uint256 max);

    /**
     * @notice Error indicating that trading has not been enabled yet.
     */
    error TradeNotYetEnabled();

    /**
     * @notice Error indicating that the native token cannot be withdrawn from the smart contract.
     */
    error CannotWithdrawNativeToken();

    /**
     * @notice Error indicating that lock is active for given state and cannot be modified.
     */
    error Locked(string lockType);

    /**
     * @notice Error indicating that the receiver cannot initiate transfer of Ether.
     *
     * @dev Should throw if called by the receiver address.
     */
    error ReceiverCannotInitiateTransferEther();

    /**
     * @notice Error indicating that only a wallet address is allowed to perform the action.
     *
     * @dev Should throw if called to use an address that is not believed to be a wallet.
     */
    error OnlyWalletAddressAllowed();

    // CONSTRUCTOR

    /**
     * @notice Constructs the DeepSouth AI contract and initializes both owner
     * and project owner addresses. Deployer will receive 1,000,000 tokens after
     * the smart contract was deployed.
     *
     * @dev If deployer is not the project owner, then deployer will be exempted
     * from fees along with the project owner and router.
     *
     * IMPORTANT: Project owner should be aware that {enableTrade} function and
     * feature usually would significantly impact the audit score since these
     * functions/features possess the potential for malicious exploitation,
     * which might affect the received score.
     */
    constructor() Ownable (msg.sender) {
        isExemptFee[projectOwner] = true;
        isExemptFee[address(router)] = true;
        isExemptWalletLimit[projectOwner] = true;
        isExemptWalletLimit[address(this)] = true;

        if (projectOwner != msg.sender) {
            isExemptFee[msg.sender] = true;
            isExemptWalletLimit[msg.sender] = true;
        }

        deployTime = block.timestamp;
        _mint(msg.sender, 1_000_000 * 10**DECIMALS);

        pair = IFactory(router.factory()).createPair(address(this), router.WETH());
        isPairLP[pair] = true;
        isExemptWalletLimit[pair] = true;
    }

    // EVENT

    /**
     * @notice Emits when an automatic or manual redemption occurs, distributing fees
     * and redeeming a specific amount.
     *
     * @param marketingFeeDistribution The amount distributed for marketing fees.
     * @param amountToRedeem The total amount being redeemed.
     * @param caller The address that triggered the redemption.
     * @param timestamp The timestamp at which the redemption event occurred.
     */
    event AutoRedeem(uint256 marketingFeeDistribution, uint256 amountToRedeem, address caller, uint256 timestamp);

    /**
     * @notice Emitted when the router address is updated.
     *
     * @param oldRouter The address of the old router.
     * @param newRouter The address of the new router.
     * @param caller The address that triggered the router update.
     * @param timestamp The timestamp when the update occurred.
     */
    event UpdateRouter(address oldRouter, address newRouter, address caller, uint256 timestamp);

    /**
     * @notice Emitted upon setting the status of a specific address type.
     *
     * @param addressType The type of address status being modified.
     * @param account The address of the account whose status is being updated.
     * @param oldStatus The previous exemption status.
     * @param newStatus The new exemption status.
     * @param caller The address that triggered the status update.
     * @param timestamp The timestamp when the update occurred.
     */
    event SetAddressState(string addressType, address account, bool oldStatus, bool newStatus, address caller, uint256 timestamp);

    /**
     * @notice Emitted when trading is enabled for the contract.
     *
     * @param caller The address that triggered the trading enablement.
     * @param timestamp The timestamp when trading was enabled.
     */
    event TradeEnabled(address caller, uint256 timestamp);

    /**
     * @notice Emitted when a lock is applied.
     *
     * @param lockType The type of lock applied.
     * @param caller The address of the caller who applied the lock.
     * @param timestamp The timestamp when the lock was applied.
     */
    event Lock(string lockType, address caller, uint256 timestamp);

    /**
     * @notice Emitted when the minimum swap value is updated.
     *
     * @param oldMinSwap The old minimum swap value before the update.
     * @param newMinSwap The new minimum swap value after the update.
     * @param caller The address of the caller who updated the minimum swap value.
     * @param timestamp The timestamp when the update occurred.
     */
    event UpdateMinSwap(uint256 oldMinSwap, uint256 newMinSwap, address caller, uint256 timestamp);

    /**
     * @notice Emitted when the state of a feature is updated.
     *
     * @param stateType The type of state being updated.
     * @param oldStatus The previous status before the update.
     * @param newStatus The new status after the update.
     * @param caller The address of the caller who updated the state.
     * @param timestamp The timestamp when the update occurred.
     */
    event UpdateState(string stateType, bool oldStatus, bool newStatus, address caller, uint256 timestamp);

    /**
     * @notice Emitted when the value of a limit is updated.
     *
     * @param limitType The type of fee being updated.
     * @param oldLimit The previous marketing fee value before the update.
     * @param newLimit The new marketing fee value after the update.
     * @param caller The address of the caller who updated the fee.
     * @param timestamp The timestamp when the fee update occurred.
     */
    event UpdateLimit(string limitType, uint256 oldLimit, uint256 newLimit, address caller, uint256 timestamp);

    /**
     * @notice Emitted when the value of a fee is updated.
     *
     * @param feeType The type of fee being updated.
     * @param oldMarketingFee The previous marketing fee value before the update.
     * @param newMarketingFee The new marketing fee value after the update.
     * @param caller The address of the caller who updated the fee.
     * @param timestamp The timestamp when the fee update occurred.
     */
    event UpdateFee(string feeType, uint256 oldMarketingFee, uint256 newMarketingFee, address caller, uint256 timestamp);

    /**
     * @notice Emitted upon updating a receiver address.
     *
     * @param receiverType The type of receiver being updated.
     * @param oldReceiver The previous receiver address before the update.
     * @param newReceiver The new receiver address after the update.
     * @param caller The address of the caller who updated the receiver address.
     * @param timestamp The timestamp when the receiver address was updated.
     */
    event UpdateReceiver(string receiverType, address oldReceiver, address newReceiver, address caller, uint256 timestamp);

    // FUNCTION

    /* General */

    /**
     * @notice Allows the contract to receive Ether.
     *
     * @dev This is a required feature to have in order to allow the smart contract
     * to be able to receive ether from the swap.
     */
    receive() external payable {}

    /**
     * @notice Withdraws tokens or Ether from the contract to a specified address.
     *
     * @param tokenAddress The address of the token to withdraw.
     * @param amount The amount of tokens or Ether to withdraw.
     *
     * @dev You need to use address(0) as `tokenAddress` to withdraw Ether and
     * use 0 as `amount` to withdraw the whole balance amount in the smart contract.
     * Anyone can trigger this function to send the fund to the `marketingReceiver`.
     * Only `marketingReceiver` address will not be able to trigger this function to
     * withdraw Ether from the smart contract by himself/herself. Should throw if try
     * to withdraw any amount of native token from the smart contract. Distribution
     * of native token can only be done through autoRedeem function.
     */
    function wTokens(address tokenAddress, uint256 amount) external {
        uint256 toTransfer = amount;
        address receiver = marketingReceiver;

        if (tokenAddress == address(this)) {
            revert CannotWithdrawNativeToken();
        } else if (tokenAddress == address(0)) {
            if (amount == 0) {
                toTransfer = address(this).balance;
            }
            if (msg.sender == receiver) {
                revert ReceiverCannotInitiateTransferEther();
            }
            payable(receiver).transfer(toTransfer);
        } else {
            if (amount == 0) {
                toTransfer = IERC20(tokenAddress).balanceOf(address(this));
            }
            IERC20(tokenAddress).safeTransfer(receiver, toTransfer);
        }
    }

    /**
     * @notice Enables trading functionality for the token contract.
     *
     * @dev Should trade is not enabled, if ownership is set and the sender is not the owner,
     * users can trigger it 30 days after deployment. If ownership is not set or contract
     * has been renounced before enable trade, users can trigger it 15 days after deployment.
     * Other than these, it validates the sender's authorization based on the contract
     * deployment time and ownership status and should throw if trading already enabled.
     * Can only be triggered once and emits a TradeEnabled event upon successful transaction.
     */
    function enableTrading() external {
        if (tradeEnabled) {
            revert TradeAlreadyEnabled(tradeEnabled, tradeStartTime);
        }
        if (
            owner() != address(0) &&
            owner() != msg.sender &&
            deployTime + 30 days > block.timestamp
        ) {
            revert OwnableUnauthorizedAccount(msg.sender);
        }
        if (
            owner() == address(0) &&
            owner() != msg.sender &&
            deployTime + 15 days > block.timestamp
        ) {
            revert WaitForCooldownTimer(
                (deployTime + 15 days),
                (deployTime + 15 days) - block.timestamp
            );
        }

        if (!isWalletLimitActive) {
            isWalletLimitActive = true;
        }
        if (!isFeeActive) {
            isFeeActive = true;
        }
        if (!isSwapEnabled) {
            isSwapEnabled = true;
        }

        tradeEnabled = true;
        tradeStartTime = block.timestamp;
        tradeStartBlock = block.number;

        emit TradeEnabled(msg.sender, block.timestamp);
    }

    /**
     * @notice Calculates the circulating supply of the token.
     *
     * @return The circulating supply of the token.
     *
     * @dev This should only return the token supply that is in circulation,
     * which excluded the potential balance that could be in both address(0)
     * and address(0xdead) that are already known to not be out of circulation.
     */
    function circulatingSupply() public view returns (uint256) {
        return totalSupply() - balanceOf(address(0xdead)) - balanceOf(address(0));
    }

    /* Check */

    /**
     * @notice Checks if the caller is the project owner and reverts if not.
     *
     * @dev Should throw if the sender is not the current project owner.
     */
    function _checkOwnerFailsafe() internal view {
        _checkFailsafeLock();
        if (projectOwner != msg.sender && owner() != msg.sender) {
            revert OwnableUnauthorizedAccount(msg.sender);
        }
    }

    /**
     * @notice Checks if using current value.
     *
     * @dev Should throw if using current value.
     */
    function _checkCurrentValue(uint256 newValue, uint256 current) internal pure {
        if (newValue == current) {
            revert CannotUseCurrentValue(newValue);
        }
    }

    /**
     * @notice Checks if using current state.
     *
     * @dev Should throw if using current state.
     */
    function _checkCurrentState(bool newState, bool current) internal pure {
        if (newState == current) {
            revert CannotUseCurrentState(newState);
        }
    }

    /**
     * @notice Checks if using current address.
     *
     * @dev Should throw if using current address.
     */
    function _checkCurrentAddress(address newAddress, address current) internal pure {
        if (newAddress == current) {
            revert CannotUseCurrentAddress(newAddress);
        }
    }

    /**
     * @notice Checks if the failsafe is already locked.
     *
     * @dev Should throw if the failsafe locked.
     */
    function _checkFailsafeLock() internal view {
        if (isFailsafeLocked) {
            revert Locked("Failsafe");
        }
    }

    /**
     * @notice Checks if the receiver is already locked.
     *
     * @dev Should throw if the receiver locked.
     */
    function _checkReceiverLock() internal view {
        if (isReceiverLocked) {
            revert Locked("Receiver");
        }
    }

    /**
     * @notice Checks if the fee is already locked.
     *
     * @dev Should throw if the fee locked.
     */
    function _checkFeeLock() internal view {
        if (isFeeLocked) {
            revert Locked("Fee");
        }
    }

    /**
     * @notice Checks if the wallet limit is already locked.
     *
     * @dev Should throw if the wallet limit locked.
     */
    function _checkWalletLimitLock() internal view {
        if (isWalletLimitLocked) {
            revert Locked("WalletLimit");
        }
    }

    /* Redeem */

    /**
     * @notice Initiates an automatic redemption process by distributing a specific
     * amount of tokens for marketing purposes, swapping a portion for ETH. Limited
     * to a maximum of 10% of circulating supply per transaction.
     *
     * @param amountToRedeem The amount of tokens to be redeemed and distributed
     * for marketing.
     *
     * @dev This function calculates the distribution of tokens for marketing,
     * redeems the specified amount, and triggers a swap for ETH. This function can
     * be used for both auto and manual redeem of the specified amount. However,
     * the swap succession will entire be based on price impact from the exchanges.
     */
    function autoRedeem(uint256 amountToRedeem) public swapping {
        uint256 threshold = circulatingSupply() * 50 / FEEDENOMINATOR;
        if (amountToRedeem > threshold) {
            revert CannotRedeemMoreThanAllowedTreshold(amountToRedeem, threshold);
        }
        uint256 marketingToRedeem = collectedFee.marketing - redeemedFee.marketing;
        uint256 totalToRedeem = collectedFee.total - redeemedFee.total;

        uint256 marketingFeeDistribution = amountToRedeem * marketingToRedeem / totalToRedeem;

        redeemedFee.marketing += marketingFeeDistribution;
        redeemedFee.total += amountToRedeem;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();

        _approve(address(this), address(router), amountToRedeem);

        emit AutoRedeem(marketingFeeDistribution, amountToRedeem, msg.sender, block.timestamp);

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            marketingFeeDistribution,
            0,
            path,
            marketingReceiver,
            block.timestamp
        );
    }

    /* Update */

    /**
     * @notice Locks the fee mechanism, preventing further changes once locked.
     *
     * @dev This function will emits the Lock event.
     */
    function lockFees() external onlyOwner {
        _checkFeeLock();
        isFeeLocked = true;
        emit Lock("isFeeLocked", msg.sender, block.timestamp);
    }

    /**
     * @notice Locks the receivers, preventing further changes once locked.
     *
     * @dev This function will emits the Lock event.
     */
    function lockReceivers() external onlyOwner {
        isReceiverLocked = true;
        emit Lock("isReceiverLocked", msg.sender, block.timestamp);
    }

    /**
     * @notice Locks the failsafe feature, preventing access control once locked.
     *
     * @dev This function will emits the Lock event.
     */
    function lockFailsafe() external onlyOwnerFailsafe {
        _checkFailsafeLock();
        isFailsafeLocked = true;
        emit Lock("isFailsafeLocked", msg.sender, block.timestamp);
    }

    /**
     * @notice Locks the wallet limit feature, preventing further changes once locked.
     *
     * @dev This function will emits the Lock event.
     */
    function lockWalletLimit() external onlyOwnerFailsafe {
        _checkWalletLimitLock();
        isWalletLimitLocked = true;
        emit Lock("isWalletLimitLocked", msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the minimum swap value, ensuring it doesn't exceed
     * a certain threshold.
     *
     * @param newMinSwap The new minimum swap value to be set.
     *
     * @dev This function will emits the UpdateMinSwap event.
     */
    function updateMinSwap(uint256 newMinSwap) external onlyOwnerFailsafe {
        if (newMinSwap > circulatingSupply() * 10 / FEEDENOMINATOR) {
            revert InvalidValue(newMinSwap);
        }
        _checkCurrentValue(newMinSwap, minSwap);
        uint256 oldMinSwap = minSwap;
        minSwap = newMinSwap;
        emit UpdateMinSwap(oldMinSwap, newMinSwap, msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the status of fee activation, allowing toggling the fee mechanism.
     * a certain threshold.
     *
     * @param newStatus The new status for fee activation.
     *
     * @dev This function will emits the UpdateState event.
     */
    function updateFeeActive(bool newStatus) external onlyOwnerFailsafe {
        _checkFeeLock();
        _checkCurrentState(newStatus, isFeeActive);
        bool oldStatus = isFeeActive;
        isFeeActive = newStatus;
        emit UpdateState("isFeeActive", oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the status of limit activation, allowing toggling the wallet limit mechanism.
     * a certain threshold.
     *
     * @param newStatus The new status for limit activation.
     *
     * @dev This function will emits the UpdateState event.
     */
    function updateWalletLimitActive(bool newStatus) external onlyOwnerFailsafe {
        _checkWalletLimitLock();
        _checkCurrentState(newStatus, isWalletLimitActive);
        bool oldStatus = isWalletLimitActive;
        isWalletLimitActive = newStatus;
        emit UpdateState("isWalletLimitActive", oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the status of swap enabling, allowing toggling the swap mechanism.
     *
     * @param newStatus The new status for swap enabling.
     *
     * @dev This function will emits the UpdateState event.
     */
    function updateSwapEnabled(bool newStatus) external onlyOwnerFailsafe {
        _checkCurrentState(newStatus, isSwapEnabled);
        bool oldStatus = isSwapEnabled;
        isSwapEnabled = newStatus;
        emit UpdateState("isSwapEnabled", oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to modify max wallet limit allowed.
     *
     * @param newLimit The new limit allowed for a wallet.
     *
     * @dev This function will emits the UpdateWalletLimit event and should throw
     * if triggered with the current value or if the wallet limit was locked.
     */
    function updateMaxWalletLimit(uint256 newLimit) external onlyOwnerFailsafe {
        if (newLimit < 100) {
            revert InvalidWalletLimit(newLimit, 100);
        }
        _checkWalletLimitLock();
        _checkCurrentValue(newLimit, maxWalletLimit);
        uint256 oldLimit = maxWalletLimit;
        maxWalletLimit = newLimit;
        emit UpdateLimit("maxWalletLimit", oldLimit, newLimit, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to modify marketing fee for buy transactions.
     *
     * @param newMarketingFee The new marketing fee percentage for buy transactions.
     *
     * @dev This function will emits the UpdateFee event and should throw if triggered
     * with the current value or if the fee was locked.
     */
    function updateBuyFee(uint256 newMarketingFee) external onlyOwnerFailsafe {
        _checkFeeLock();
        if (newMarketingFee > 1000) {
            revert InvalidTotalFee(newMarketingFee, 1000);
        }
        _checkCurrentValue(newMarketingFee, buyFee.marketing);
        uint256 oldMarketingFee = buyFee.marketing;
        buyFee.marketing = newMarketingFee;
        buyFee.total = newMarketingFee;
        emit UpdateFee("buyFee", oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to modify marketing fee for sell transactions.
     *
     * @param newMarketingFee The new marketing fee percentage for sell transactions.
     *
     * @dev This function will emits the UpdateFee event and should throw if triggered
     * with the current value or if the fee was locked.
     */
    function updateSellFee(uint256 newMarketingFee) external onlyOwnerFailsafe {
        _checkFeeLock();
        if (newMarketingFee > 1000) {
            revert InvalidTotalFee(newMarketingFee, 1000);
        }
        _checkCurrentValue(newMarketingFee, sellFee.marketing);
        uint256 oldMarketingFee = sellFee.marketing;
        sellFee.marketing = newMarketingFee;
        sellFee.total = newMarketingFee;
        emit UpdateFee("sellFee", oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to modify marketing fee for transfer transactions.
     *
     * @param newMarketingFee The new marketing fee percentage for transfer transactions.
     *
     * @dev This function will emits the UpdateFee event and should throw if triggered
     * with the current value or if the fee was locked.
     */
    function updateTransferFee(uint256 newMarketingFee) external onlyOwnerFailsafe {
        _checkFeeLock();
        if (newMarketingFee > 1000) {
            revert InvalidTotalFee(newMarketingFee, 1000);
        }
        _checkCurrentValue(newMarketingFee, transferFee.marketing);
        uint256 oldMarketingFee = transferFee.marketing;
        transferFee.marketing = newMarketingFee;
        transferFee.total = newMarketingFee;
        emit UpdateFee("transferFee", oldMarketingFee, newMarketingFee, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to change the address receiving marketing fees.
     *
     * @param newMarketingReceiver The new address to receive marketing fees.
     *
     * @dev This function will emits the UpdateReceiver event and should throw
     * if triggered with the current address or if the receiver was locked.
     */
    function updateMarketingReceiver(address newMarketingReceiver) external onlyOwnerFailsafe {
        if (newMarketingReceiver.code.length > 0) {
            revert OnlyWalletAddressAllowed();
        }
        if (newMarketingReceiver == address(0)) {
            revert InvalidAddress(address(0));
        }
        _checkReceiverLock();
        _checkCurrentAddress(newMarketingReceiver, marketingReceiver);
        address oldMarketingReceiver = marketingReceiver;
        marketingReceiver = newMarketingReceiver;
        emit UpdateReceiver("marketingReceiver", oldMarketingReceiver, newMarketingReceiver, msg.sender, block.timestamp);
    }

    /**
     * @notice Allow the owner to set the status of a specified LP pair.
     *
     * @param lpPair The LP pair address.
     * @param newStatus The new status of the LP pair.
     *
     * @dev This function will emits the SetAddressState event and should throw
     * if triggered with the current state for the address or if the lpPair
     * address is not a valid pair address.
     */
    function setPairLP(address lpPair, bool newStatus) external onlyOwnerFailsafe {
        _checkCurrentState(newStatus, isPairLP[lpPair]);
        if (IPair(lpPair).token0() != address(this) && IPair(lpPair).token1() != address(this)) {
            revert InvalidAddress(lpPair);
        }
        if (!isPairLP[pair]) {
            isExemptWalletLimit[pair] = true;
        }
        bool oldStatus = isPairLP[lpPair];
        isPairLP[lpPair] = newStatus;
        emit SetAddressState("isPairLP", lpPair, oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the router address used for token swaps.
     *
     * @param newRouter The address of the new router contract.
     *
     * @dev This should also generate the pair address using the factory of the `newRouter` if
     * the address of the pair on the new router's factory is address(0). If the new pair address's
     * isPairLP status is not yet set to true, this function will automatically set it to true.
     */
    function updateRouter(address newRouter) external onlyOwnerFailsafe {
        if (newRouter == address(0)) {
            revert InvalidAddress(newRouter);
        }
        _checkCurrentAddress(newRouter, address(router));

        address oldRouter = address(router);
        router = IRouter(newRouter);

        emit UpdateRouter(oldRouter, newRouter, msg.sender, block.timestamp);

        if (address(IFactory(router.factory()).getPair(address(this), router.WETH())) == address(0)) {
            pair = IFactory(router.factory()).createPair(address(this), router.WETH());

            if (!isExemptWalletLimit[pair]) {
                isExemptWalletLimit[pair] = true;
            }
            if (!isPairLP[pair]) {
                isPairLP[pair] = true;
            }
        }
    }

    /**
     * @notice Updates the exemption status for fee on a specific account.
     *
     * @param user The address of the account.
     * @param newStatus The new exemption status.
     *
     * @dev Should throw if the `newStatus` is the exact same state as the current state
     * for the `user` address.
     */
    function updateExemptFee(address user, bool newStatus) external onlyOwnerFailsafe {
        _checkCurrentState(newStatus, isExemptFee[user]);
        bool oldStatus = isExemptFee[user];
        isExemptFee[user] = newStatus;
        emit SetAddressState("isExemptFee", user, oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /**
     * @notice Updates the exemption status for wallet limit on a specific account.
     *
     * @param user The address of the account.
     * @param newStatus The new exemption status.
     *
     * @dev Should throw if the `newStatus` is the exact same state as the current state
     * for the `user` address.
     */
    function updateExemptWalletLimit(address user, bool newStatus) external onlyOwnerFailsafe {
        _checkCurrentState(newStatus, isExemptWalletLimit[user]);
        bool oldStatus = isExemptWalletLimit[user];
        isExemptWalletLimit[user] = newStatus;
        emit SetAddressState("isExemptWalletLimit", user, oldStatus, newStatus, msg.sender, block.timestamp);
    }

    /* Fee */

    /**
     * @notice Takes the buy fee from the specified address and amount, and distribute
     * the fees accordingly.
     *
     * @param from The address from which the fee is taken.
     * @param amount The amount from which the fee is taken.
     *
     * @return The new amount after deducting the fee.
     */
    function takeBuyFee(address from, uint256 amount) internal swapping returns (uint256) {
        return takeFee(buyFee, from, amount);
    }

    /**
     * @notice Takes the sell fee from the specified address and amount, and distribute
     * the fees accordingly.
     *
     * @param from The address from which the fee is taken.
     * @param amount The amount from which the fee is taken.
     *
     * @return The new amount after deducting the fee.
     */
    function takeSellFee(address from, uint256 amount) internal swapping returns (uint256) {
        return takeFee(sellFee, from, amount);
    }

    /**
     * @notice Takes the transfer fee from the specified address and amount, and distribute
     * the fees accordingly.
     *
     * @param from The address from which the fee is taken.
     * @param amount The amount from which the fee is taken.
     *
     * @return The new amount after deducting the fee.
     */
    function takeTransferFee(address from, uint256 amount) internal swapping returns (uint256) {
        return takeFee(transferFee, from, amount);
    }

    /**
     * @notice Takes the transfer fee from the specified address and amount, and distribute
     * the fees accordingly.
     *
     * @param feeType The type of fee being taken.
     * @param from The address from which the fee is taken.
     * @param amount The amount from which the fee is taken.
     *
     * @return The new amount after deducting the fee.
     */
    function takeFee(Fee memory feeType, address from, uint256 amount) internal swapping returns (uint256) {
        uint256 feeTotal = feeType.marketing;
        if (block.number <= tradeStartBlock + 0) {
            feeTotal = 9900;
        }
        uint256 feeAmount = amount * feeTotal / FEEDENOMINATOR;
        uint256 newAmount = amount - feeAmount;
        if (feeAmount > 0) {
            tallyFee(feeType, from, feeAmount, feeTotal);
        }
        return newAmount;
    }

    /**
     * @notice Tally the collected fee for a given fee type and address,
     * based on the amount and fee provided.
     *
     * @param feeType The type of fee being tallied.
     * @param from The address from which the fee is collected.
     * @param amount The total amount being collected as a fee.
     * @param fee The total fee being collected.
     */
    function tallyFee(Fee memory feeType, address from, uint256 amount, uint256 fee) internal swapping {
        uint256 collectMarketing = amount * feeType.marketing / fee;
        tallyCollection(collectMarketing, amount);

        _update(from, address(this), amount);
    }

    /**
     * @notice Tally the collected fee for marketing based on
     * provided amounts.
     *
     * @param collectMarketing The amount collected for marketing fees.
     * @param amount The total amount collected as a fee.
     */
    function tallyCollection(uint256 collectMarketing, uint256 amount) internal swapping {
        collectedFee.marketing += collectMarketing;
        collectedFee.total += amount;
    }

    /* Override */

    /**
     * @notice Overrides the {transferOwnership} function to update project owner.
     *
     * @param newOwner The address of the new owner.
     *
     * @dev Should throw if the `newOwner` is set to the current owner address or address(0xdead).
     * This overrides function is just an extended version of the original {transferOwnership}
     * function. See {Ownable-transferOwnership} for more information.
     */
    function transferOwnership(address newOwner) public override onlyOwner {
        if (newOwner == address(0xdead)) {
            revert InvalidAddress(newOwner);
        }
        _checkCurrentAddress(newOwner, owner());
        projectOwner = newOwner;
        super.transferOwnership(newOwner);
    }

    /* ERC20 Standard */

    /**
     * @notice Returns the name of the token.
     *
     * @return The name of the token.
     *
     * @dev This is usually a longer version of the name.
     */
    function name() public view virtual returns (string memory) {
        return NAME;
    }

    /**
     * @notice Returns the symbol of the token.
     *
     * @return The symbol of the token.
     *
     * @dev This is usually a shorter version of the name.
     */
    function symbol() public view virtual returns (string memory) {
        return SYMBOL;
    }

    /**
     * @notice Returns the number of decimals used for token display purposes.
     *
     * @return The number of decimals.
     *
     * @dev This is purely used for user representation of the amount and does not
     * affect any of the arithmetic of the smart contract including, but not limited
     * to {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual returns (uint8) {
        return DECIMALS;
    }

    /**
     * @notice Returns the total supply of tokens.
     *
     * @return The total supply of tokens.
     *
     * @dev See {IERC20-totalSupply} for more information.
     */
    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    /**
     * @notice Returns the balance of tokens for a given account.
     *
     * @param account The address of the account to check.
     *
     * @return The token balance of the account.
     *
     * @dev See {IERC20-balanceOf} for more information.
     */
    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }

    /**
     * @notice Transfers tokens from the sender to a specified recipient.
     *
     * @param to The address of the recipient.
     * @param value The amount of tokens to transfer.
     *
     * @return A boolean indicating whether the transfer was successful or not.
     *
     * @dev See {IERC20-transfer} for more information.
     */
    function transfer(address to, uint256 value) public virtual returns (bool) {
        address provider = msg.sender;
        _transfer(provider, to, value);
        return true;
    }

    /**
     * @notice Returns the allowance amount that a spender is allowed to spend on behalf of a provider.
     *
     * @param provider The address allowing spending.
     * @param spender The address allowed to spend tokens.
     *
     * @return The allowance amount for the spender.
     *
     * @dev See {IERC20-allowance} for more information.
     */
    function allowance(address provider, address spender) public view virtual returns (uint256) {
        return _allowances[provider][spender];
    }

    /**
     * @notice Approves a spender to spend a certain amount of tokens on behalf of the sender.
     *
     * @param spender The address allowed to spend tokens.
     * @param value The allowance amount for the spender.
     *
     * @return A boolean indicating whether the approval was successful or not.
     *
     * @dev See {IERC20-approve} for more information.
     */
    function approve(address spender, uint256 value) public virtual returns (bool) {
        address provider = msg.sender;
        _approve(provider, spender, value);
        return true;
    }

    /**
     * @notice Transfers tokens from one address to another on behalf of a spender.
     *
     * @param from The address to transfer tokens from.
     * @param to The address to transfer tokens to.
     * @param value The amount of tokens to transfer.
     *
     * @return A boolean indicating whether the transfer was successful or not.
     *
     * @dev See {IERC20-transferFrom} for more information.
     */
    function transferFrom(address from, address to, uint256 value) public virtual returns (bool) {
        address spender = msg.sender;
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    }

    /**
     * @notice Internal function to handle token transfers with additional checks.
     *
     * @param from The address tokens are transferred from.
     * @param to The address tokens are transferred to.
     * @param value The amount of tokens to transfer.
     *
     * @dev This internal function is equivalent to {transfer}, and thus can be used for other functions
     * such as implementing automatic token fees, slashing mechanisms, etc. Since this function is not
     * virtual, {_update} should be overridden instead. This function can only be called if the address
     * for `from` and `to` are not address(0) and the sender should at least have a balance of `value`.
     * It also enforces various conditions including validations for trade status, fees, exemptions,
     * and redemption.
     *
     * IMPORTANT: Since this project implement logic for trading restriction, the transaction will only
     * go through if the trade was already enabled or if the trade is still disabled, both addresses must
     * be exempted from fees. Please note that this feature could significantly impact the audit score as
     * since it possesses the potential for malicious exploitation, which might affect the received score.
     */
    function _transfer(address from, address to, uint256 value) internal {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        if (!tradeEnabled) {
            if (!isExemptFee[from] && !isExemptFee[to]) {
                revert TradeNotYetEnabled();
            }
        }

        if (inSwap || isExemptFee[from]) {
            return _update(from, to, value);
        }
        if (from != pair && isSwapEnabled && collectedFee.total - redeemedFee.total >= minSwap && balanceOf(address(this)) >= minSwap) {
            uint256 swapAmount = minSwap;

            if (isFailsafeLocked && owner() == address(0)) {
                uint256 failsafeAmount = circulatingSupply() * 10 / FEEDENOMINATOR;
                swapAmount = failsafeAmount <= swapAmount ? failsafeAmount : swapAmount;
            }

            autoRedeem(swapAmount);
        }

        uint256 newValue = value;

        if (isFeeActive && !isExemptFee[from] && !isExemptFee[to]) {
            newValue = _beforeTokenTransfer(from, to, value);
        }

        _update(from, to, newValue);
    }

    /**
     * @notice Internal function called before token transfer, applying fee mechanisms
     * based on transaction specifics.
     *
     * @param from The address from which tokens are being transferred.
     * @param to The address to which tokens are being transferred.
     * @param amount The amount of tokens being transferred.
     *
     * @return The modified amount after applying potential fees.
     *
     * @dev This function calculates and applies fees before executing token transfers
     * based on the transaction details and address types.
     */
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal swapping virtual returns (uint256) {
        if (isPairLP[from] && (buyFee.marketing > 0)) {
            return takeBuyFee(from, amount);
        }
        if (isPairLP[to] && (sellFee.marketing > 0)) {
            return takeSellFee(from, amount);
        }
        if (!isPairLP[from] && !isPairLP[to] && (transferFee.marketing > 0)) {
            return takeTransferFee(from, amount);
        }
        return amount;
    }

    /**
     * @notice Internal function to update token balances during transfers.
     *
     * @param from The address tokens are transferred from.
     * @param to The address tokens are transferred to.
     * @param value The amount of tokens to transfer.
     *
     * @dev This function is used internally to transfer a `value` amount of token from
     * `from` address to `to` address. This function is also used for mints if `from`
     * is the zero address and for burns if `to` is the zero address.
     *
     * IMPORTANT: All customizations that are required for transfers, mints, and burns
     * should be done by overriding this function.

     */
    function _update(address from, address to, uint256 value) internal virtual {
        uint256 newBalance = balanceOf(to) + value;
        uint256 limit = circulatingSupply() * maxWalletLimit / FEEDENOMINATOR;

        if (newBalance > limit && !isExemptWalletLimit[to]) {
            revert ExceedWalletLimit(newBalance, limit);
        }

        if (from == address(0)) {
            _totalSupply += value;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < value) {
                revert ERC20InsufficientBalance(from, fromBalance, value);
            }
            unchecked {
                _balances[from] = fromBalance - value;
            }
        }

        if (to == address(0)) {
            unchecked {
                _totalSupply -= value;
            }
        } else {
            unchecked {
                _balances[to] += value;
            }
        }

        emit Transfer(from, to, value);
    }

    /**
     * @notice Internal function to mint tokens and update the total supply.
     *
     * @param account The address to mint tokens to.
     * @param value The amount of tokens to mint.
     *
     * @dev The `account` address cannot be address(0) because it does not make any sense to mint to it.
     * Since this function is not virtual, {_update} should be overridden instead for customization.
     */
    function _mint(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(address(0), account, value);
    }

    /**
     * @notice Internal function to set an allowance for a `spender` to spend a specific `value` of tokens
     * on behalf of a `provider`.
     *
     * @param provider The address allowing spending.
     * @param spender The address allowed to spend tokens.
     * @param value The allowance amount for the spender.
     *
     * @dev This internal function is equivalent to {approve}, and thus can be used for other functions
     * such as setting automatic allowances for certain subsystems, etc.
     *
     * IMPORTANT: This function internally calls {_approve} with the emitEvent parameter set to `true`.
     */
    function _approve(address provider, address spender, uint256 value) internal {
        _approve(provider, spender, value, true);
    }

    /**
     * @notice Variant of {_approve} with an optional flag to enable or disable the {Approval} event.
     *
     * @param provider The address allowing spending.
     * @param spender The address allowed to spend tokens.
     * @param value The allowance amount for the spender.
     * @param emitEvent A boolean indicating whether to emit the Approval event.
     *
     * @dev This internal function is equivalent to {approve}, and thus can be used for other functions
     * such as setting automatic allowances for certain subsystems, etc. This function can only be called
     * if the address for `provider` and `spender` are not address(0). If `emitEvent` is set to `true`,
     * this function will emits the Approval event.
     */
    function _approve(address provider, address spender, uint256 value, bool emitEvent) internal virtual {
        if (provider == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[provider][spender] = value;
        if (emitEvent) {
            emit Approval(provider, spender, value);
        }
    }

    /**
     * @notice Internal function to decrease allowance when tokens are spent.
     *
     * @param provider The address allowing spending.
     * @param spender The address allowed to spend tokens.
     * @param value The amount of tokens spent.
     *
     * @dev If the allowance value for the `spender` is infinite/the max value of uint256,
     * this function will notupdate the allowance value. Should throw if not enough allowance
     * is available. On all occasion, this function will not emit an Approval event.
     */
    function _spendAllowance(address provider, address spender, uint256 value) internal virtual {
        uint256 currentAllowance = allowance(provider, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(provider, spender, currentAllowance - value, false);
            }
        }
    }
}