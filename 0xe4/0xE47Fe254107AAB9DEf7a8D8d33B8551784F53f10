// SPDX-License-Identifier: AGPL-3.0
pragma solidity ^0.8.21;


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
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
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
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
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
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
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
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
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)
/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */

interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/IERC20Permit.sol)
/**
 * @dev Interface of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in
 * https://eips.ethereum.org/EIPS/eip-2612[EIP-2612].
 *
 * Adds the {permit} method, which can be used to change an account's ERC20 allowance (see {IERC20-allowance}) by
 * presenting a message signed by the account. By not relying on {IERC20-approve}, the token holder account doesn't
 * need to send a transaction, and thus is not required to hold Ether at all.
 */
interface IERC20Permit {
    /**
     * @dev Sets `value` as the allowance of `spender` over ``owner``'s tokens,
     * given ``owner``'s signed approval.
     *
     * IMPORTANT: The same issues {IERC20-approve} has related to transaction
     * ordering also apply here.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `deadline` must be a timestamp in the future.
     * - `v`, `r` and `s` must be a valid `secp256k1` signature from `owner`
     * over the EIP712-formatted function arguments.
     * - the signature must use ``owner``'s current nonce (see {nonces}).
     *
     * For more information on the signature format, see the
     * https://eips.ethereum.org/EIPS/eip-2612#specification[relevant EIP
     * section].
     */
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    /**
     * @dev Returns the current nonce for `owner`. This value must be
     * included whenever a signature is generated for {permit}.
     *
     * Every successful call to {permit} increases ``owner``'s nonce by one. This
     * prevents a signature from being used multiple times.
     */
    function nonces(address owner) external view returns (uint256);

    /**
     * @dev Returns the domain separator used in the encoding of the signature for {permit}, as defined by {EIP712}.
     */
    // solhint-disable-next-line func-name-mixedcase
    function DOMAIN_SEPARATOR() external view returns (bytes32);
}

// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)
/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     *
     * Furthermore, `isContract` will also return true if the target contract within
     * the same transaction is already scheduled for destruction by `SELFDESTRUCT`,
     * which only has an effect at the end of a transaction.
     * ====
     *
     * [IMPORTANT]
     * ====
     * You shouldn't rely on `isContract` to protect against flash loan attacks!
     *
     * Preventing calls from contracts is highly discouraged. It breaks composability, breaks support for smart wallets
     * like Gnosis Safe, and does not provide security since it can be circumvented by calling from a contract
     * constructor.
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.8.0/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided one) in case of unsuccessful call or if target was not a contract.
     *
     * _Available since v4.8._
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or using the provided one.
     *
     * _Available since v4.3._
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}

library SafeERC20 {
    using Address for address;

    /**
     * @dev Transfer `value` amount of `token` from the calling contract to `to`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    /**
     * @dev Increase the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance + value));
    }

    /**
     * @dev Decrease the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance - value));
        }
    }

    /**
     * @dev Set the calling contract's allowance toward `spender` to `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful. Meant to be used with tokens that require the approval
     * to be set to zero before setting it to a non-zero value, such as USDT.
     */
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeWithSelector(token.approve.selector, spender, value);

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, 0));
            _callOptionalReturn(token, approvalCall);
        }
    }

    /**
     * @dev Use a ERC-2612 signature to set the `owner` approval toward `spender` on `token`.
     * Revert on invalid signature.
     */
    function safePermit(
        IERC20Permit token,
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal {
        uint256 nonceBefore = token.nonces(owner);
        token.permit(owner, spender, value, deadline, v, r, s);
        uint256 nonceAfter = token.nonces(owner);
        require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        require(returndata.length == 0 || abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     *
     * This is a variant of {_callOptionalReturn} that silents catches all reverts and returns a bool instead.
     */
    function _callOptionalReturnBool(IERC20 token, bytes memory data) private returns (bool) {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We cannot use {Address-functionCall} here since this should return false
        // and not revert is the subcall reverts.

        (bool success, bytes memory returndata) = address(token).call(data);
        return
            success && (returndata.length == 0 || abi.decode(returndata, (bool))) && Address.isContract(address(token));
    }
}

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

interface IBaseLeverage {
  enum FlashLoanType {
    AAVE,
    BALANCER
  }

  enum SwapType {
    NONE,
    DEPOSIT,
    WITHDRAW,
    UNISWAP,
    BALANCER,
    CURVE
  }

  struct MultipSwapPath {
    address[9] routes;
    uint256[3][4] routeParams;
    // uniswap/balancer/curve
    SwapType swapType;
    uint256 poolCount;
    address swapFrom;
    address swapTo;
    uint256 inAmount;
    uint256 outAmount;
  }

  struct BiDirectSwapInfo {
    MultipSwapPath[3] paths;
    MultipSwapPath[3] reversePaths;
    uint256 pathLength;
  }

  struct UniDirectSwapInfo {
    MultipSwapPath[3] paths;
    uint256 pathLength;
  }

  struct FlashLoanParams {
    bool isEnterPosition;
    uint256 minRequiredAmount;
    address user;
    address collateralAsset;
    address silo;
    BiDirectSwapInfo borrowAssetAndCollateral;
    BiDirectSwapInfo borrowAssetAndSiloAsset;
  }

  struct LeverageParams {
    address user;
    uint256 principal;
    uint256 leverage;
    address borrowAsset;
    address collateralAsset;
    address silo;
    FlashLoanType flashLoanType;
    BiDirectSwapInfo borrowAssetAndCollateral;
    BiDirectSwapInfo borrowAssetAndSiloAsset;
  }

  function enterPositionWithFlashloan(
    uint256 _principal,
    uint256 _leverage,
    address _borrowAsset,
    address _collateralAsset,
    address _silo,
    FlashLoanType _flashLoanType,
    BiDirectSwapInfo calldata _borrowAssetAndCollateral,
    BiDirectSwapInfo calldata _borrowAssetAndSiloAsset
  ) external;

  function withdrawWithFlashloan(
    uint256 _repayAmount,
    uint256 _requiredAmount,
    address _borrowAsset,
    address _collateralAsset,
    address _silo,
    FlashLoanType _flashLoanType,
    BiDirectSwapInfo calldata _borrowAssetAndSiloAsset,
    BiDirectSwapInfo calldata _borrowAssetAndCollateral
  ) external;
}

interface IFlashLoanReceiver {
  function executeOperation(
    address[] calldata assets,
    uint256[] calldata amounts,
    uint256[] calldata premiums,
    address initiator,
    bytes calldata params
  ) external returns (bool);
}

interface IFlashLoanRecipient {
  /**
   * @dev When `flashLoan` is called on the Vault, it invokes the `receiveFlashLoan` hook on the recipient.
   *
   * At the time of the call, the Vault will have transferred `amounts` for `tokens` to the recipient. Before this
   * call returns, the recipient must have transferred `amounts` plus `feeAmounts` for each token back to the
   * Vault, or else the entire flash loan will revert.
   *
   * `userData` is the same value passed in the `IVault.flashLoan` call.
   */
  function receiveFlashLoan(
    IERC20[] memory tokens,
    uint256[] memory amounts,
    uint256[] memory feeAmounts,
    bytes memory userData
  ) external;
}

interface IPoolAddressesProvider {
    /**
     * @dev Emitted when the market identifier is updated.
     * @param oldMarketId The old id of the market
     * @param newMarketId The new id of the market
     */
    event MarketIdSet(string indexed oldMarketId, string indexed newMarketId);

    /**
     * @dev Emitted when the pool is updated.
     * @param oldAddress The old address of the Pool
     * @param newAddress The new address of the Pool
     */
    event PoolUpdated(address indexed oldAddress, address indexed newAddress);

    /**
     * @dev Emitted when the pool configurator is updated.
     * @param oldAddress The old address of the PoolConfigurator
     * @param newAddress The new address of the PoolConfigurator
     */
    event PoolConfiguratorUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the price oracle is updated.
     * @param oldAddress The old address of the PriceOracle
     * @param newAddress The new address of the PriceOracle
     */
    event PriceOracleUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the ACL manager is updated.
     * @param oldAddress The old address of the ACLManager
     * @param newAddress The new address of the ACLManager
     */
    event ACLManagerUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the ACL admin is updated.
     * @param oldAddress The old address of the ACLAdmin
     * @param newAddress The new address of the ACLAdmin
     */
    event ACLAdminUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the price oracle sentinel is updated.
     * @param oldAddress The old address of the PriceOracleSentinel
     * @param newAddress The new address of the PriceOracleSentinel
     */
    event PriceOracleSentinelUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the pool data provider is updated.
     * @param oldAddress The old address of the PoolDataProvider
     * @param newAddress The new address of the PoolDataProvider
     */
    event PoolDataProviderUpdated(
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when a new proxy is created.
     * @param id The identifier of the proxy
     * @param proxyAddress The address of the created proxy contract
     * @param implementationAddress The address of the implementation contract
     */
    event ProxyCreated(
        bytes32 indexed id,
        address indexed proxyAddress,
        address indexed implementationAddress
    );

    /**
     * @dev Emitted when a new non-proxied contract address is registered.
     * @param id The identifier of the contract
     * @param oldAddress The address of the old contract
     * @param newAddress The address of the new contract
     */
    event AddressSet(
        bytes32 indexed id,
        address indexed oldAddress,
        address indexed newAddress
    );

    /**
     * @dev Emitted when the implementation of the proxy registered with id is updated
     * @param id The identifier of the contract
     * @param proxyAddress The address of the proxy contract
     * @param oldImplementationAddress The address of the old implementation contract
     * @param newImplementationAddress The address of the new implementation contract
     */
    event AddressSetAsProxy(
        bytes32 indexed id,
        address indexed proxyAddress,
        address oldImplementationAddress,
        address indexed newImplementationAddress
    );

    /**
     * @notice Returns the id of the Aave market to which this contract points to.
     * @return The market id
     **/
    function getMarketId() external view returns (string memory);

    /**
     * @notice Associates an id with a specific PoolAddressesProvider.
     * @dev This can be used to create an onchain registry of PoolAddressesProviders to
     * identify and validate multiple Aave markets.
     * @param newMarketId The market id
     */
    function setMarketId(string calldata newMarketId) external;

    /**
     * @notice Returns an address by its identifier.
     * @dev The returned address might be an EOA or a contract, potentially proxied
     * @dev It returns ZERO if there is no registered address with the given id
     * @param id The id
     * @return The address of the registered for the specified id
     */
    function getAddress(bytes32 id) external view returns (address);

    /**
     * @notice General function to update the implementation of a proxy registered with
     * certain `id`. If there is no proxy registered, it will instantiate one and
     * set as implementation the `newImplementationAddress`.
     * @dev IMPORTANT Use this function carefully, only for ids that don't have an explicit
     * setter function, in order to avoid unexpected consequences
     * @param id The id
     * @param newImplementationAddress The address of the new implementation
     */
    function setAddressAsProxy(
        bytes32 id,
        address newImplementationAddress
    ) external;

    /**
     * @notice Sets an address for an id replacing the address saved in the addresses map.
     * @dev IMPORTANT Use this function carefully, as it will do a hard replacement
     * @param id The id
     * @param newAddress The address to set
     */
    function setAddress(bytes32 id, address newAddress) external;

    /**
     * @notice Returns the address of the Pool proxy.
     * @return The Pool proxy address
     **/
    function getPool() external view returns (address);

    /**
     * @notice Updates the implementation of the Pool, or creates a proxy
     * setting the new `pool` implementation when the function is called for the first time.
     * @param newPoolImpl The new Pool implementation
     **/
    function setPoolImpl(address newPoolImpl) external;

    /**
     * @notice Returns the address of the PoolConfigurator proxy.
     * @return The PoolConfigurator proxy address
     **/
    function getPoolConfigurator() external view returns (address);

    /**
     * @notice Updates the implementation of the PoolConfigurator, or creates a proxy
     * setting the new `PoolConfigurator` implementation when the function is called for the first time.
     * @param newPoolConfiguratorImpl The new PoolConfigurator implementation
     **/
    function setPoolConfiguratorImpl(address newPoolConfiguratorImpl) external;

    /**
     * @notice Returns the address of the price oracle.
     * @return The address of the PriceOracle
     */
    function getPriceOracle() external view returns (address);

    /**
     * @notice Updates the address of the price oracle.
     * @param newPriceOracle The address of the new PriceOracle
     */
    function setPriceOracle(address newPriceOracle) external;

    /**
     * @notice Returns the address of the ACL manager.
     * @return The address of the ACLManager
     */
    function getACLManager() external view returns (address);

    /**
     * @notice Updates the address of the ACL manager.
     * @param newAclManager The address of the new ACLManager
     **/
    function setACLManager(address newAclManager) external;

    /**
     * @notice Returns the address of the ACL admin.
     * @return The address of the ACL admin
     */
    function getACLAdmin() external view returns (address);

    /**
     * @notice Updates the address of the ACL admin.
     * @param newAclAdmin The address of the new ACL admin
     */
    function setACLAdmin(address newAclAdmin) external;

    /**
     * @notice Returns the address of the price oracle sentinel.
     * @return The address of the PriceOracleSentinel
     */
    function getPriceOracleSentinel() external view returns (address);

    /**
     * @notice Updates the address of the price oracle sentinel.
     * @param newPriceOracleSentinel The address of the new PriceOracleSentinel
     **/
    function setPriceOracleSentinel(address newPriceOracleSentinel) external;

    /**
     * @notice Returns the address of the data provider.
     * @return The address of the DataProvider
     */
    function getPoolDataProvider() external view returns (address);

    /**
     * @notice Updates the address of the data provider.
     * @param newDataProvider The address of the new DataProvider
     **/
    function setPoolDataProvider(address newDataProvider) external;
}

library DataTypesV3 {
    struct ReserveData {
        //stores the reserve configuration
        ReserveConfigurationMap configuration;
        //the liquidity index. Expressed in ray
        uint128 liquidityIndex;
        //the current supply rate. Expressed in ray
        uint128 currentLiquidityRate;
        //variable borrow index. Expressed in ray
        uint128 variableBorrowIndex;
        //the current variable borrow rate. Expressed in ray
        uint128 currentVariableBorrowRate;
        //the current stable borrow rate. Expressed in ray
        uint128 currentStableBorrowRate;
        //timestamp of last update
        uint40 lastUpdateTimestamp;
        //the id of the reserve. Represents the position in the list of the active reserves
        uint16 id;
        //aToken address
        address aTokenAddress;
        //stableDebtToken address
        address stableDebtTokenAddress;
        //variableDebtToken address
        address variableDebtTokenAddress;
        //address of the interest rate strategy
        address interestRateStrategyAddress;
        //the current treasury balance, scaled
        uint128 accruedToTreasury;
        //the outstanding unbacked aTokens minted through the bridging feature
        uint128 unbacked;
        //the outstanding debt borrowed against this asset in isolation mode
        uint128 isolationModeTotalDebt;
    }

    struct ReserveConfigurationMap {
        //bit 0-15: LTV
        //bit 16-31: Liq. threshold
        //bit 32-47: Liq. bonus
        //bit 48-55: Decimals
        //bit 56: reserve is active
        //bit 57: reserve is frozen
        //bit 58: borrowing is enabled
        //bit 59: stable rate borrowing enabled
        //bit 60: asset is paused
        //bit 61: borrowing in isolation mode is enabled
        //bit 62-63: reserved
        //bit 64-79: reserve factor
        //bit 80-115 borrow cap in whole tokens, borrowCap == 0 => no cap
        //bit 116-151 supply cap in whole tokens, supplyCap == 0 => no cap
        //bit 152-167 liquidation protocol fee
        //bit 168-175 eMode category
        //bit 176-211 unbacked mint cap in whole tokens, unbackedMintCap == 0 => minting disabled
        //bit 212-251 debt ceiling for isolation mode with (ReserveConfiguration::DEBT_CEILING_DECIMALS) decimals
        //bit 252-255 unused

        uint256 data;
    }

    struct UserConfigurationMap {
        /**
         * @dev Bitmap of the users collaterals and borrows. It is divided in pairs of bits, one pair per asset.
         * The first bit indicates if an asset is used as collateral by the user, the second whether an
         * asset is borrowed by the user.
         */
        uint256 data;
    }

    struct EModeCategory {
        // each eMode category has a custom ltv and liquidation threshold
        uint16 ltv;
        uint16 liquidationThreshold;
        uint16 liquidationBonus;
        // each eMode category may or may not have a custom oracle to override the individual assets price oracles
        address priceSource;
        string label;
    }

    enum InterestRateMode {
        NONE,
        STABLE,
        VARIABLE
    }

    struct ReserveCache {
        uint256 currScaledVariableDebt;
        uint256 nextScaledVariableDebt;
        uint256 currPrincipalStableDebt;
        uint256 currAvgStableBorrowRate;
        uint256 currTotalStableDebt;
        uint256 nextAvgStableBorrowRate;
        uint256 nextTotalStableDebt;
        uint256 currLiquidityIndex;
        uint256 nextLiquidityIndex;
        uint256 currVariableBorrowIndex;
        uint256 nextVariableBorrowIndex;
        uint256 currLiquidityRate;
        uint256 currVariableBorrowRate;
        uint256 reserveFactor;
        ReserveConfigurationMap reserveConfiguration;
        address aTokenAddress;
        address stableDebtTokenAddress;
        address variableDebtTokenAddress;
        uint40 reserveLastUpdateTimestamp;
        uint40 stableDebtLastUpdateTimestamp;
    }

    struct ExecuteLiquidationCallParams {
        uint256 reservesCount;
        uint256 debtToCover;
        address collateralAsset;
        address debtAsset;
        address user;
        bool receiveAToken;
        address priceOracle;
        uint8 userEModeCategory;
        address priceOracleSentinel;
    }

    struct ExecuteSupplyParams {
        address asset;
        uint256 amount;
        address onBehalfOf;
        uint16 referralCode;
    }

    struct ExecuteBorrowParams {
        address asset;
        address user;
        address onBehalfOf;
        uint256 amount;
        InterestRateMode interestRateMode;
        uint16 referralCode;
        bool releaseUnderlying;
        uint256 maxStableRateBorrowSizePercent;
        uint256 reservesCount;
        address oracle;
        uint8 userEModeCategory;
        address priceOracleSentinel;
    }

    struct ExecuteRepayParams {
        address asset;
        uint256 amount;
        InterestRateMode interestRateMode;
        address onBehalfOf;
        bool useATokens;
    }

    struct ExecuteWithdrawParams {
        address asset;
        uint256 amount;
        address to;
        uint256 reservesCount;
        address oracle;
        uint8 userEModeCategory;
    }

    struct ExecuteSetUserEModeParams {
        uint256 reservesCount;
        address oracle;
        uint8 categoryId;
    }

    struct FinalizeTransferParams {
        address asset;
        address from;
        address to;
        uint256 amount;
        uint256 balanceFromBefore;
        uint256 balanceToBefore;
        uint256 reservesCount;
        address oracle;
        uint8 fromEModeCategory;
    }

    struct FlashloanParams {
        address receiverAddress;
        address[] assets;
        uint256[] amounts;
        uint256[] interestRateModes;
        address onBehalfOf;
        bytes params;
        uint16 referralCode;
        uint256 flashLoanPremiumToProtocol;
        uint256 flashLoanPremiumTotal;
        uint256 maxStableRateBorrowSizePercent;
        uint256 reservesCount;
        address addressesProvider;
        uint8 userEModeCategory;
        bool isAuthorizedFlashBorrower;
    }

    struct FlashloanSimpleParams {
        address receiverAddress;
        address asset;
        uint256 amount;
        bytes params;
        uint16 referralCode;
        uint256 flashLoanPremiumToProtocol;
        uint256 flashLoanPremiumTotal;
    }

    struct FlashLoanRepaymentParams {
        uint256 amount;
        uint256 totalPremium;
        uint256 flashLoanPremiumToProtocol;
        address asset;
        address receiverAddress;
        uint16 referralCode;
    }

    struct CalculateUserAccountDataParams {
        UserConfigurationMap userConfig;
        uint256 reservesCount;
        address user;
        address oracle;
        uint8 userEModeCategory;
    }

    struct ValidateBorrowParams {
        ReserveCache reserveCache;
        UserConfigurationMap userConfig;
        address asset;
        address userAddress;
        uint256 amount;
        InterestRateMode interestRateMode;
        uint256 maxStableLoanPercent;
        uint256 reservesCount;
        address oracle;
        uint8 userEModeCategory;
        address priceOracleSentinel;
        bool isolationModeActive;
        address isolationModeCollateralAddress;
        uint256 isolationModeDebtCeiling;
    }

    struct ValidateLiquidationCallParams {
        ReserveCache debtReserveCache;
        uint256 totalDebt;
        uint256 healthFactor;
        address priceOracleSentinel;
    }

    struct CalculateInterestRatesParams {
        uint256 unbacked;
        uint256 liquidityAdded;
        uint256 liquidityTaken;
        uint256 totalStableDebt;
        uint256 totalVariableDebt;
        uint256 averageStableBorrowRate;
        uint256 reserveFactor;
        address reserve;
        address aToken;
    }

    struct InitReserveParams {
        address asset;
        address aTokenAddress;
        address stableDebtAddress;
        address variableDebtAddress;
        address interestRateStrategyAddress;
        uint16 reservesCount;
        uint16 maxNumberReserves;
    }
}

interface IPool {
    /**
     * @dev Emitted on mintUnbacked()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address initiating the supply
     * @param onBehalfOf The beneficiary of the supplied assets, receiving the aTokens
     * @param amount The amount of supplied assets
     * @param referralCode The referral code used
     **/
    event MintUnbacked(
        address indexed reserve,
        address user,
        address indexed onBehalfOf,
        uint256 amount,
        uint16 indexed referralCode
    );

    /**
     * @dev Emitted on backUnbacked()
     * @param reserve The address of the underlying asset of the reserve
     * @param backer The address paying for the backing
     * @param amount The amount added as backing
     * @param fee The amount paid in fees
     **/
    event BackUnbacked(
        address indexed reserve,
        address indexed backer,
        uint256 amount,
        uint256 fee
    );

    /**
     * @dev Emitted on supply()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address initiating the supply
     * @param onBehalfOf The beneficiary of the supply, receiving the aTokens
     * @param amount The amount supplied
     * @param referralCode The referral code used
     **/
    event Supply(
        address indexed reserve,
        address user,
        address indexed onBehalfOf,
        uint256 amount,
        uint16 indexed referralCode
    );

    /**
     * @dev Emitted on withdraw()
     * @param reserve The address of the underlying asset being withdrawn
     * @param user The address initiating the withdrawal, owner of aTokens
     * @param to The address that will receive the underlying
     * @param amount The amount to be withdrawn
     **/
    event Withdraw(
        address indexed reserve,
        address indexed user,
        address indexed to,
        uint256 amount
    );

    /**
     * @dev Emitted on borrow() and flashLoan() when debt needs to be opened
     * @param reserve The address of the underlying asset being borrowed
     * @param user The address of the user initiating the borrow(), receiving the funds on borrow() or just
     * initiator of the transaction on flashLoan()
     * @param onBehalfOf The address that will be getting the debt
     * @param amount The amount borrowed out
     * @param interestRateMode The rate mode: 1 for Stable, 2 for Variable
     * @param borrowRate The numeric rate at which the user has borrowed, expressed in ray
     * @param referralCode The referral code used
     **/
    event Borrow(
        address indexed reserve,
        address user,
        address indexed onBehalfOf,
        uint256 amount,
        DataTypesV3.InterestRateMode interestRateMode,
        uint256 borrowRate,
        uint16 indexed referralCode
    );

    /**
     * @dev Emitted on repay()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The beneficiary of the repayment, getting his debt reduced
     * @param repayer The address of the user initiating the repay(), providing the funds
     * @param amount The amount repaid
     * @param useATokens True if the repayment is done using aTokens, `false` if done with underlying asset directly
     **/
    event Repay(
        address indexed reserve,
        address indexed user,
        address indexed repayer,
        uint256 amount,
        bool useATokens
    );

    /**
     * @dev Emitted on swapBorrowRateMode()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address of the user swapping his rate mode
     * @param interestRateMode The current interest rate mode of the position being swapped: 1 for Stable, 2 for Variable
     **/
    event SwapBorrowRateMode(
        address indexed reserve,
        address indexed user,
        DataTypesV3.InterestRateMode interestRateMode
    );

    /**
     * @dev Emitted on borrow(), repay() and liquidationCall() when using isolated assets
     * @param asset The address of the underlying asset of the reserve
     * @param totalDebt The total isolation mode debt for the reserve
     */
    event IsolationModeTotalDebtUpdated(
        address indexed asset,
        uint256 totalDebt
    );

    /**
     * @dev Emitted when the user selects a certain asset category for eMode
     * @param user The address of the user
     * @param categoryId The category id
     **/
    event UserEModeSet(address indexed user, uint8 categoryId);

    /**
     * @dev Emitted on setUserUseReserveAsCollateral()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address of the user enabling the usage as collateral
     **/
    event ReserveUsedAsCollateralEnabled(
        address indexed reserve,
        address indexed user
    );

    /**
     * @dev Emitted on setUserUseReserveAsCollateral()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address of the user enabling the usage as collateral
     **/
    event ReserveUsedAsCollateralDisabled(
        address indexed reserve,
        address indexed user
    );

    /**
     * @dev Emitted on rebalanceStableBorrowRate()
     * @param reserve The address of the underlying asset of the reserve
     * @param user The address of the user for which the rebalance has been executed
     **/
    event RebalanceStableBorrowRate(
        address indexed reserve,
        address indexed user
    );

    /**
     * @dev Emitted on flashLoan()
     * @param target The address of the flash loan receiver contract
     * @param initiator The address initiating the flash loan
     * @param asset The address of the asset being flash borrowed
     * @param amount The amount flash borrowed
     * @param interestRateMode The flashloan mode: 0 for regular flashloan, 1 for Stable debt, 2 for Variable debt
     * @param premium The fee flash borrowed
     * @param referralCode The referral code used
     **/
    event FlashLoan(
        address indexed target,
        address initiator,
        address indexed asset,
        uint256 amount,
        DataTypesV3.InterestRateMode interestRateMode,
        uint256 premium,
        uint16 indexed referralCode
    );

    /**
     * @dev Emitted when a borrower is liquidated.
     * @param collateralAsset The address of the underlying asset used as collateral, to receive as result of the liquidation
     * @param debtAsset The address of the underlying borrowed asset to be repaid with the liquidation
     * @param user The address of the borrower getting liquidated
     * @param debtToCover The debt amount of borrowed `asset` the liquidator wants to cover
     * @param liquidatedCollateralAmount The amount of collateral received by the liquidator
     * @param liquidator The address of the liquidator
     * @param receiveAToken True if the liquidators wants to receive the collateral aTokens, `false` if he wants
     * to receive the underlying collateral asset directly
     **/
    event LiquidationCall(
        address indexed collateralAsset,
        address indexed debtAsset,
        address indexed user,
        uint256 debtToCover,
        uint256 liquidatedCollateralAmount,
        address liquidator,
        bool receiveAToken
    );

    /**
     * @dev Emitted when the state of a reserve is updated.
     * @param reserve The address of the underlying asset of the reserve
     * @param liquidityRate The next liquidity rate
     * @param stableBorrowRate The next stable borrow rate
     * @param variableBorrowRate The next variable borrow rate
     * @param liquidityIndex The next liquidity index
     * @param variableBorrowIndex The next variable borrow index
     **/
    event ReserveDataUpdated(
        address indexed reserve,
        uint256 liquidityRate,
        uint256 stableBorrowRate,
        uint256 variableBorrowRate,
        uint256 liquidityIndex,
        uint256 variableBorrowIndex
    );

    /**
     * @dev Emitted when the protocol treasury receives minted aTokens from the accrued interest.
     * @param reserve The address of the reserve
     * @param amountMinted The amount minted to the treasury
     **/
    event MintedToTreasury(address indexed reserve, uint256 amountMinted);

    /**
     * @dev Mints an `amount` of aTokens to the `onBehalfOf`
     * @param asset The address of the underlying asset to mint
     * @param amount The amount to mint
     * @param onBehalfOf The address that will receive the aTokens
     * @param referralCode Code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     **/
    function mintUnbacked(
        address asset,
        uint256 amount,
        address onBehalfOf,
        uint16 referralCode
    ) external;

    /**
     * @dev Back the current unbacked underlying with `amount` and pay `fee`.
     * @param asset The address of the underlying asset to back
     * @param amount The amount to back
     * @param fee The amount paid in fees
     **/
    function backUnbacked(address asset, uint256 amount, uint256 fee) external;

    /**
     * @notice Supplies an `amount` of underlying asset into the reserve, receiving in return overlying aTokens.
     * - E.g. User supplies 100 USDC and gets in return 100 aUSDC
     * @param asset The address of the underlying asset to supply
     * @param amount The amount to be supplied
     * @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
     *   wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
     *   is a different wallet
     * @param referralCode Code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     **/
    function supply(
        address asset,
        uint256 amount,
        address onBehalfOf,
        uint16 referralCode
    ) external;

    /**
     * @notice Supply with transfer approval of asset to be supplied done via permit function
     * see: https://eips.ethereum.org/EIPS/eip-2612 and https://eips.ethereum.org/EIPS/eip-713
     * @param asset The address of the underlying asset to supply
     * @param amount The amount to be supplied
     * @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
     *   wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
     *   is a different wallet
     * @param deadline The deadline timestamp that the permit is valid
     * @param referralCode Code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     * @param permitV The V parameter of ERC712 permit sig
     * @param permitR The R parameter of ERC712 permit sig
     * @param permitS The S parameter of ERC712 permit sig
     **/
    function supplyWithPermit(
        address asset,
        uint256 amount,
        address onBehalfOf,
        uint16 referralCode,
        uint256 deadline,
        uint8 permitV,
        bytes32 permitR,
        bytes32 permitS
    ) external;

    /**
     * @notice Withdraws an `amount` of underlying asset from the reserve, burning the equivalent aTokens owned
     * E.g. User has 100 aUSDC, calls withdraw() and receives 100 USDC, burning the 100 aUSDC
     * @param asset The address of the underlying asset to withdraw
     * @param amount The underlying amount to be withdrawn
     *   - Send the value type(uint256).max in order to withdraw the whole aToken balance
     * @param to The address that will receive the underlying, same as msg.sender if the user
     *   wants to receive it on his own wallet, or a different address if the beneficiary is a
     *   different wallet
     * @return The final amount withdrawn
     **/
    function withdraw(
        address asset,
        uint256 amount,
        address to
    ) external returns (uint256);

    /**
     * @notice Allows users to borrow a specific `amount` of the reserve underlying asset, provided that the borrower
     * already supplied enough collateral, or he was given enough allowance by a credit delegator on the
     * corresponding debt token (StableDebtToken or VariableDebtToken)
     * - E.g. User borrows 100 USDC passing as `onBehalfOf` his own address, receiving the 100 USDC in his wallet
     *   and 100 stable/variable debt tokens, depending on the `interestRateMode`
     * @param asset The address of the underlying asset to borrow
     * @param amount The amount to be borrowed
     * @param interestRateMode The interest rate mode at which the user wants to borrow: 1 for Stable, 2 for Variable
     * @param referralCode The code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     * @param onBehalfOf The address of the user who will receive the debt. Should be the address of the borrower itself
     * calling the function if he wants to borrow against his own collateral, or the address of the credit delegator
     * if he has been given credit delegation allowance
     **/
    function borrow(
        address asset,
        uint256 amount,
        uint256 interestRateMode,
        uint16 referralCode,
        address onBehalfOf
    ) external;

    /**
     * @notice Repays a borrowed `amount` on a specific reserve, burning the equivalent debt tokens owned
     * - E.g. User repays 100 USDC, burning 100 variable/stable debt tokens of the `onBehalfOf` address
     * @param asset The address of the borrowed underlying asset previously borrowed
     * @param amount The amount to repay
     * - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
     * @param interestRateMode The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable
     * @param onBehalfOf The address of the user who will get his debt reduced/removed. Should be the address of the
     * user calling the function if he wants to reduce/remove his own debt, or the address of any other
     * other borrower whose debt should be removed
     * @return The final amount repaid
     **/
    function repay(
        address asset,
        uint256 amount,
        uint256 interestRateMode,
        address onBehalfOf
    ) external returns (uint256);

    /**
     * @notice Repay with transfer approval of asset to be repaid done via permit function
     * see: https://eips.ethereum.org/EIPS/eip-2612 and https://eips.ethereum.org/EIPS/eip-713
     * @param asset The address of the borrowed underlying asset previously borrowed
     * @param amount The amount to repay
     * - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
     * @param interestRateMode The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable
     * @param onBehalfOf Address of the user who will get his debt reduced/removed. Should be the address of the
     * user calling the function if he wants to reduce/remove his own debt, or the address of any other
     * other borrower whose debt should be removed
     * @param deadline The deadline timestamp that the permit is valid
     * @param permitV The V parameter of ERC712 permit sig
     * @param permitR The R parameter of ERC712 permit sig
     * @param permitS The S parameter of ERC712 permit sig
     * @return The final amount repaid
     **/
    function repayWithPermit(
        address asset,
        uint256 amount,
        uint256 interestRateMode,
        address onBehalfOf,
        uint256 deadline,
        uint8 permitV,
        bytes32 permitR,
        bytes32 permitS
    ) external returns (uint256);

    /**
     * @notice Repays a borrowed `amount` on a specific reserve using the reserve aTokens, burning the
     * equivalent debt tokens
     * - E.g. User repays 100 USDC using 100 aUSDC, burning 100 variable/stable debt tokens
     * @dev  Passing uint256.max as amount will clean up any residual aToken dust balance, if the user aToken
     * balance is not enough to cover the whole debt
     * @param asset The address of the borrowed underlying asset previously borrowed
     * @param amount The amount to repay
     * - Send the value type(uint256).max in order to repay the whole debt for `asset` on the specific `debtMode`
     * @param interestRateMode The interest rate mode at of the debt the user wants to repay: 1 for Stable, 2 for Variable
     * @return The final amount repaid
     **/
    function repayWithATokens(
        address asset,
        uint256 amount,
        uint256 interestRateMode
    ) external returns (uint256);

    /**
     * @notice Allows a borrower to swap his debt between stable and variable mode, or vice versa
     * @param asset The address of the underlying asset borrowed
     * @param interestRateMode The current interest rate mode of the position being swapped: 1 for Stable, 2 for Variable
     **/
    function swapBorrowRateMode(
        address asset,
        uint256 interestRateMode
    ) external;

    /**
     * @notice Rebalances the stable interest rate of a user to the current stable rate defined on the reserve.
     * - Users can be rebalanced if the following conditions are satisfied:
     *     1. Usage ratio is above 95%
     *     2. the current supply APY is below REBALANCE_UP_THRESHOLD * maxVariableBorrowRate, which means that too
     *        much has been borrowed at a stable rate and suppliers are not earning enough
     * @param asset The address of the underlying asset borrowed
     * @param user The address of the user to be rebalanced
     **/
    function rebalanceStableBorrowRate(address asset, address user) external;

    /**
     * @notice Allows suppliers to enable/disable a specific supplied asset as collateral
     * @param asset The address of the underlying asset supplied
     * @param useAsCollateral True if the user wants to use the supply as collateral, false otherwise
     **/
    function setUserUseReserveAsCollateral(
        address asset,
        bool useAsCollateral
    ) external;

    /**
     * @notice Function to liquidate a non-healthy position collateral-wise, with Health Factor below 1
     * - The caller (liquidator) covers `debtToCover` amount of debt of the user getting liquidated, and receives
     *   a proportionally amount of the `collateralAsset` plus a bonus to cover market risk
     * @param collateralAsset The address of the underlying asset used as collateral, to receive as result of the liquidation
     * @param debtAsset The address of the underlying borrowed asset to be repaid with the liquidation
     * @param user The address of the borrower getting liquidated
     * @param debtToCover The debt amount of borrowed `asset` the liquidator wants to cover
     * @param receiveAToken True if the liquidators wants to receive the collateral aTokens, `false` if he wants
     * to receive the underlying collateral asset directly
     **/
    function liquidationCall(
        address collateralAsset,
        address debtAsset,
        address user,
        uint256 debtToCover,
        bool receiveAToken
    ) external;

    /**
     * @notice Allows smartcontracts to access the liquidity of the pool within one transaction,
     * as long as the amount taken plus a fee is returned.
     * @dev IMPORTANT There are security concerns for developers of flashloan receiver contracts that must be kept
     * into consideration. For further details please visit https://developers.aave.com
     * @param receiverAddress The address of the contract receiving the funds, implementing IFlashLoanReceiver interface
     * @param assets The addresses of the assets being flash-borrowed
     * @param amounts The amounts of the assets being flash-borrowed
     * @param interestRateModes Types of the debt to open if the flash loan is not returned:
     *   0 -> Don't open any debt, just revert if funds can't be transferred from the receiver
     *   1 -> Open debt at stable rate for the value of the amount flash-borrowed to the `onBehalfOf` address
     *   2 -> Open debt at variable rate for the value of the amount flash-borrowed to the `onBehalfOf` address
     * @param onBehalfOf The address  that will receive the debt in the case of using on `modes` 1 or 2
     * @param params Variadic packed params to pass to the receiver as extra information
     * @param referralCode The code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     **/
    function flashLoan(
        address receiverAddress,
        address[] calldata assets,
        uint256[] calldata amounts,
        uint256[] calldata interestRateModes,
        address onBehalfOf,
        bytes calldata params,
        uint16 referralCode
    ) external;

    /**
     * @notice Allows smartcontracts to access the liquidity of the pool within one transaction,
     * as long as the amount taken plus a fee is returned.
     * @dev IMPORTANT There are security concerns for developers of flashloan receiver contracts that must be kept
     * into consideration. For further details please visit https://developers.aave.com
     * @param receiverAddress The address of the contract receiving the funds, implementing IFlashLoanSimpleReceiver interface
     * @param asset The address of the asset being flash-borrowed
     * @param amount The amount of the asset being flash-borrowed
     * @param params Variadic packed params to pass to the receiver as extra information
     * @param referralCode The code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     **/
    function flashLoanSimple(
        address receiverAddress,
        address asset,
        uint256 amount,
        bytes calldata params,
        uint16 referralCode
    ) external;

    /**
     * @notice Returns the user account data across all the reserves
     * @param user The address of the user
     * @return totalCollateralBase The total collateral of the user in the base currency used by the price feed
     * @return totalDebtBase The total debt of the user in the base currency used by the price feed
     * @return availableBorrowsBase The borrowing power left of the user in the base currency used by the price feed
     * @return currentLiquidationThreshold The liquidation threshold of the user
     * @return ltv The loan to value of The user
     * @return healthFactor The current health factor of the user
     **/
    function getUserAccountData(
        address user
    )
        external
        view
        returns (
            uint256 totalCollateralBase,
            uint256 totalDebtBase,
            uint256 availableBorrowsBase,
            uint256 currentLiquidationThreshold,
            uint256 ltv,
            uint256 healthFactor
        );

    /**
     * @notice Initializes a reserve, activating it, assigning an aToken and debt tokens and an
     * interest rate strategy
     * @dev Only callable by the PoolConfigurator contract
     * @param asset The address of the underlying asset of the reserve
     * @param aTokenAddress The address of the aToken that will be assigned to the reserve
     * @param stableDebtAddress The address of the StableDebtToken that will be assigned to the reserve
     * @param variableDebtAddress The address of the VariableDebtToken that will be assigned to the reserve
     * @param interestRateStrategyAddress The address of the interest rate strategy contract
     **/
    function initReserve(
        address asset,
        address aTokenAddress,
        address stableDebtAddress,
        address variableDebtAddress,
        address interestRateStrategyAddress
    ) external;

    /**
     * @notice Drop a reserve
     * @dev Only callable by the PoolConfigurator contract
     * @param asset The address of the underlying asset of the reserve
     **/
    function dropReserve(address asset) external;

    /**
     * @notice Updates the address of the interest rate strategy contract
     * @dev Only callable by the PoolConfigurator contract
     * @param asset The address of the underlying asset of the reserve
     * @param rateStrategyAddress The address of the interest rate strategy contract
     **/
    function setReserveInterestRateStrategyAddress(
        address asset,
        address rateStrategyAddress
    ) external;

    /**
     * @notice Sets the configuration bitmap of the reserve as a whole
     * @dev Only callable by the PoolConfigurator contract
     * @param asset The address of the underlying asset of the reserve
     * @param configuration The new configuration bitmap
     **/
    function setConfiguration(
        address asset,
        DataTypesV3.ReserveConfigurationMap calldata configuration
    ) external;

    /**
     * @notice Returns the configuration of the reserve
     * @param asset The address of the underlying asset of the reserve
     * @return The configuration of the reserve
     **/
    function getConfiguration(
        address asset
    ) external view returns (DataTypesV3.ReserveConfigurationMap memory);

    /**
     * @notice Returns the configuration of the user across all the reserves
     * @param user The user address
     * @return The configuration of the user
     **/
    function getUserConfiguration(
        address user
    ) external view returns (DataTypesV3.UserConfigurationMap memory);

    /**
     * @notice Returns the normalized income normalized income of the reserve
     * @param asset The address of the underlying asset of the reserve
     * @return The reserve's normalized income
     */
    function getReserveNormalizedIncome(
        address asset
    ) external view returns (uint256);

    /**
     * @notice Returns the normalized variable debt per unit of asset
     * @param asset The address of the underlying asset of the reserve
     * @return The reserve normalized variable debt
     */
    function getReserveNormalizedVariableDebt(
        address asset
    ) external view returns (uint256);

    /**
     * @notice Returns the state and configuration of the reserve
     * @param asset The address of the underlying asset of the reserve
     * @return The state and configuration data of the reserve
     **/
    function getReserveData(
        address asset
    ) external view returns (DataTypesV3.ReserveData memory);

    /**
     * @notice Validates and finalizes an aToken transfer
     * @dev Only callable by the overlying aToken of the `asset`
     * @param asset The address of the underlying asset of the aToken
     * @param from The user from which the aTokens are transferred
     * @param to The user receiving the aTokens
     * @param amount The amount being transferred/withdrawn
     * @param balanceFromBefore The aToken balance of the `from` user before the transfer
     * @param balanceToBefore The aToken balance of the `to` user before the transfer
     */
    function finalizeTransfer(
        address asset,
        address from,
        address to,
        uint256 amount,
        uint256 balanceFromBefore,
        uint256 balanceToBefore
    ) external;

    /**
     * @notice Returns the list of the underlying assets of all the initialized reserves
     * @dev It does not include dropped reserves
     * @return The addresses of the underlying assets of the initialized reserves
     **/
    function getReservesList() external view returns (address[] memory);

    /**
     * @notice Returns the address of the underlying asset of a reserve by the reserve id as stored in the DataTypesV3.ReserveData struct
     * @param id The id of the reserve as stored in the DataTypesV3.ReserveData struct
     * @return The address of the reserve associated with id
     **/
    function getReserveAddressById(uint16 id) external view returns (address);

    /**
     * @notice Returns the PoolAddressesProvider connected to this contract
     * @return The address of the PoolAddressesProvider
     **/
    function ADDRESSES_PROVIDER()
        external
        view
        returns (IPoolAddressesProvider);

    /**
     * @notice Updates the protocol fee on the bridging
     * @param bridgeProtocolFee The part of the premium sent to the protocol treasury
     */
    function updateBridgeProtocolFee(uint256 bridgeProtocolFee) external;

    /**
     * @notice Updates flash loan premiums. Flash loan premium consists of two parts:
     * - A part is sent to aToken holders as extra, one time accumulated interest
     * - A part is collected by the protocol treasury
     * @dev The total premium is calculated on the total borrowed amount
     * @dev The premium to protocol is calculated on the total premium, being a percentage of `flashLoanPremiumTotal`
     * @dev Only callable by the PoolConfigurator contract
     * @param flashLoanPremiumTotal The total premium, expressed in bps
     * @param flashLoanPremiumToProtocol The part of the premium sent to the protocol treasury, expressed in bps
     */
    function updateFlashloanPremiums(
        uint128 flashLoanPremiumTotal,
        uint128 flashLoanPremiumToProtocol
    ) external;

    /**
     * @notice Configures a new category for the eMode.
     * @dev In eMode, the protocol allows very high borrowing power to borrow assets of the same category.
     * The category 0 is reserved as it's the default for volatile assets
     * @param id The id of the category
     * @param config The configuration of the category
     */
    function configureEModeCategory(
        uint8 id,
        DataTypesV3.EModeCategory memory config
    ) external;

    /**
     * @notice Returns the data of an eMode category
     * @param id The id of the category
     * @return The configuration data of the category
     */
    function getEModeCategoryData(
        uint8 id
    ) external view returns (DataTypesV3.EModeCategory memory);

    /**
     * @notice Allows a user to use the protocol in eMode
     * @param categoryId The id of the category
     */
    function setUserEMode(uint8 categoryId) external;

    /**
     * @notice Returns the eMode the user is using
     * @param user The address of the user
     * @return The eMode id
     */
    function getUserEMode(address user) external view returns (uint256);

    /**
     * @notice Resets the isolation mode total debt of the given asset to zero
     * @dev It requires the given asset has zero debt ceiling
     * @param asset The address of the underlying asset to reset the isolationModeTotalDebt
     */
    function resetIsolationModeTotalDebt(address asset) external;

    /**
     * @notice Returns the percentage of available liquidity that can be borrowed at once at stable rate
     * @return The percentage of available liquidity to borrow, expressed in bps
     */
    function MAX_STABLE_RATE_BORROW_SIZE_PERCENT()
        external
        view
        returns (uint256);

    /**
     * @notice Returns the total fee on flash loans
     * @return The total fee on flashloans
     */
    function FLASHLOAN_PREMIUM_TOTAL() external view returns (uint128);

    /**
     * @notice Returns the part of the bridge fees sent to protocol
     * @return The bridge fee sent to the protocol treasury
     */
    function BRIDGE_PROTOCOL_FEE() external view returns (uint256);

    /**
     * @notice Returns the part of the flashloan fees sent to protocol
     * @return The flashloan fee sent to the protocol treasury
     */
    function FLASHLOAN_PREMIUM_TO_PROTOCOL() external view returns (uint128);

    /**
     * @notice Returns the maximum number of reserves supported to be listed in this Pool
     * @return The maximum number of reserves supported
     */
    function MAX_NUMBER_RESERVES() external view returns (uint16);

    /**
     * @notice Mints the assets accrued through the reserve factor to the treasury in the form of aTokens
     * @param assets The list of reserves for which the minting needs to be executed
     **/
    function mintToTreasury(address[] calldata assets) external;

    /**
     * @notice Rescue and transfer tokens locked in this contract
     * @param token The address of the token
     * @param to The address of the recipient
     * @param amount The amount of token to transfer
     */
    function rescueTokens(address token, address to, uint256 amount) external;

    /**
     * @notice Supplies an `amount` of underlying asset into the reserve, receiving in return overlying aTokens.
     * - E.g. User supplies 100 USDC and gets in return 100 aUSDC
     * @dev Deprecated: Use the `supply` function instead
     * @param asset The address of the underlying asset to supply
     * @param amount The amount to be supplied
     * @param onBehalfOf The address that will receive the aTokens, same as msg.sender if the user
     *   wants to receive them on his own wallet, or a different address if the beneficiary of aTokens
     *   is a different wallet
     * @param referralCode Code used to register the integrator originating the operation, for potential rewards.
     *   0 if the action is executed directly by the user, without any middle-man
     **/
    function deposit(
        address asset,
        uint256 amount,
        address onBehalfOf,
        uint16 referralCode
    ) external;
}

interface IBalancerVault {
  // Pools
  //
  // There are three specialization settings for Pools, which allow for cheaper swaps at the cost of reduced
  // functionality:
  //
  //  - General: no specialization, suited for all Pools. IGeneralPool is used for swap request callbacks, passing the
  // balance of all tokens in the Pool. These Pools have the largest swap costs (because of the extra storage reads),
  // which increase with the number of registered tokens.
  //
  //  - Minimal Swap Info: IMinimalSwapInfoPool is used instead of IGeneralPool, which saves gas by only passing the
  // balance of the two tokens involved in the swap. This is suitable for some pricing algorithms, like the weighted
  // constant product one popularized by Balancer V1. Swap costs are smaller compared to general Pools, and are
  // independent of the number of registered tokens.
  //
  //  - Two Token: only allows two tokens to be registered. This achieves the lowest possible swap gas cost. Like
  // minimal swap info Pools, these are called via IMinimalSwapInfoPool.

  enum PoolSpecialization {
    GENERAL,
    MINIMAL_SWAP_INFO,
    TWO_TOKEN
  }

  /**
   * @dev Returns a Pool's contract address and specialization setting.
   */
  function getPool(bytes32 poolId) external view returns (address, PoolSpecialization);

  // Swaps
  //
  // Users can swap tokens with Pools by calling the `swap` and `batchSwap` functions. To do this,
  // they need not trust Pool contracts in any way: all security checks are made by the Vault. They must however be
  // aware of the Pools' pricing algorithms in order to estimate the prices Pools will quote.
  //
  // The `swap` function executes a single swap, while `batchSwap` can perform multiple swaps in sequence.
  // In each individual swap, tokens of one kind are sent from the sender to the Pool (this is the 'token in'),
  // and tokens of another kind are sent from the Pool to the recipient in exchange (this is the 'token out').
  // More complex swaps, such as one token in to multiple tokens out can be achieved by batching together
  // individual swaps.
  //
  // There are two swap kinds:
  //  - 'given in' swaps, where the amount of tokens in (sent to the Pool) is known, and the Pool determines (via the
  // `onSwap` hook) the amount of tokens out (to send to the recipient).
  //  - 'given out' swaps, where the amount of tokens out (received from the Pool) is known, and the Pool determines
  // (via the `onSwap` hook) the amount of tokens in (to receive from the sender).
  //
  // Additionally, it is possible to chain swaps using a placeholder input amount, which the Vault replaces with
  // the calculated output of the previous swap. If the previous swap was 'given in', this will be the calculated
  // tokenOut amount. If the previous swap was 'given out', it will use the calculated tokenIn amount. These extended
  // swaps are known as 'multihop' swaps, since they 'hop' through a number of intermediate tokens before arriving at
  // the final intended token.
  //
  // In all cases, tokens are only transferred in and out of the Vault (or withdrawn from and deposited into Internal
  // Balance) after all individual swaps have been completed, and the net token balance change computed. This makes
  // certain swap patterns, such as multihops, or swaps that interact with the same token pair in multiple Pools, cost
  // much less gas than they would otherwise.
  //
  // It also means that under certain conditions it is possible to perform arbitrage by swapping with multiple
  // Pools in a way that results in net token movement out of the Vault (profit), with no tokens being sent in (only
  // updating the Pool's internal accounting).
  //
  // To protect users from front-running or the market changing rapidly, they supply a list of 'limits' for each token
  // involved in the swap, where either the maximum number of tokens to send (by passing a positive value) or the
  // minimum amount of tokens to receive (by passing a negative value) is specified.
  //
  // Additionally, a 'deadline' timestamp can also be provided, forcing the swap to fail if it occurs after
  // this point in time (e.g. if the transaction failed to be included in a block promptly).
  //
  // If interacting with Pools that hold WETH, it is possible to both send and receive ETH directly: the Vault will do
  // the wrapping and unwrapping. To enable this mechanism, the IAsset sentinel value (the zero address) must be
  // passed in the `assets` array instead of the WETH address. Note that it is possible to combine ETH and WETH in the
  // same swap. Any excess ETH will be sent back to the caller (not the sender, which is relevant for relayers).
  //
  // Finally, Internal Balance can be used when either sending or receiving tokens.

  enum SwapKind {
    GIVEN_IN,
    GIVEN_OUT
  }

  /**
   * @dev Performs a swap with a single Pool.
   *
   * If the swap is 'given in' (the number of tokens to send to the Pool is known), it returns the amount of tokens
   * taken from the Pool, which must be greater than or equal to `limit`.
   *
   * If the swap is 'given out' (the number of tokens to take from the Pool is known), it returns the amount of tokens
   * sent to the Pool, which must be less than or equal to `limit`.
   *
   * Internal Balance usage and the recipient are determined by the `funds` struct.
   *
   * Emits a `Swap` event.
   */
  function swap(
    SingleSwap memory singleSwap,
    FundManagement memory funds,
    uint256 limit,
    uint256 deadline
  ) external payable returns (uint256);

  /**
   * @dev Data for a single swap executed by `swap`. `amount` is either `amountIn` or `amountOut` depending on
   * the `kind` value.
   *
   * `assetIn` and `assetOut` are either token addresses, or the IAsset sentinel value for ETH (the zero address).
   * Note that Pools never interact with ETH directly: it will be wrapped to or unwrapped from WETH by the Vault.
   *
   * The `userData` field is ignored by the Vault, but forwarded to the Pool in the `onSwap` hook, and may be
   * used to extend swap behavior.
   */
  struct SingleSwap {
    bytes32 poolId;
    SwapKind kind;
    address assetIn;
    address assetOut;
    uint256 amount;
    bytes userData;
  }

  /**
   * @dev All tokens in a swap are either sent from the `sender` account to the Vault, or from the Vault to the
   * `recipient` account.
   *
   * If the caller is not `sender`, it must be an authorized relayer for them.
   *
   * If `fromInternalBalance` is true, the `sender`'s Internal Balance will be preferred, performing an ERC20
   * transfer for the difference between the requested amount and the User's Internal Balance (if any). The `sender`
   * must have allowed the Vault to use their tokens via `IERC20.approve()`. This matches the behavior of
   * `joinPool`.
   *
   * If `toInternalBalance` is true, tokens will be deposited to `recipient`'s internal balance instead of
   * transferred. This matches the behavior of `exitPool`.
   *
   * Note that ETH cannot be deposited to or withdrawn from Internal Balance: attempting to do so will trigger a
   * revert.
   */
  struct FundManagement {
    address sender;
    bool fromInternalBalance;
    address payable recipient;
    bool toInternalBalance;
  }

  /**
   * @dev Called by users to exit a Pool, which transfers tokens from the Pool's balance to `recipient`. This will
   * trigger custom Pool behavior, which will typically ask for something in return from `sender` - often tokenized
   * Pool shares. The amount of tokens that can be withdrawn is limited by the Pool's `cash` balance (see
   * `getPoolTokenInfo`).
   *
   * If the caller is not `sender`, it must be an authorized relayer for them.
   *
   * The `tokens` and `minAmountsOut` arrays must have the same length, and each entry in these indicates the minimum
   * token amount to receive for each token contract. The amounts to send are decided by the Pool and not the Vault:
   * it just enforces these minimums.
   *
   * If exiting a Pool that holds WETH, it is possible to receive ETH directly: the Vault will do the unwrapping. To
   * enable this mechanism, the IAsset sentinel value (the zero address) must be passed in the `assets` array instead
   * of the WETH address. Note that it is not possible to combine ETH and WETH in the same exit.
   *
   * `assets` must have the same length and order as the array returned by `getPoolTokens`. This prevents issues when
   * interacting with Pools that register and deregister tokens frequently. If receiving ETH however, the array must
   * be sorted *before* replacing the WETH address with the ETH sentinel value (the zero address), which means the
   * final `assets` array might not be sorted. Pools with no registered tokens cannot be exited.
   *
   * If `toInternalBalance` is true, the tokens will be deposited to `recipient`'s Internal Balance. Otherwise,
   * an ERC20 transfer will be performed. Note that ETH cannot be deposited to Internal Balance: attempting to
   * do so will trigger a revert.
   *
   * `minAmountsOut` is the minimum amount of tokens the user expects to get out of the Pool, for each token in the
   * `tokens` array. This array must match the Pool's registered tokens.
   *
   * This causes the Vault to call the `IBasePool.onExitPool` hook on the Pool's contract, where Pools implement
   * their own custom logic. This typically requires additional information from the user (such as the expected number
   * of Pool shares to return). This can be encoded in the `userData` argument, which is ignored by the Vault and
   * passed directly to the Pool's contract.
   *
   * Emits a `PoolBalanceChanged` event.
   */
  function exitPool(
    bytes32 poolId,
    address sender,
    address payable recipient,
    ExitPoolRequest memory request
  ) external;

  struct ExitPoolRequest {
    address[] assets;
    uint256[] minAmountsOut;
    bytes userData;
    bool toInternalBalance;
  }

  enum ExitKind {
    EXACT_BPT_IN_FOR_ONE_TOKEN_OUT,
    EXACT_BPT_IN_FOR_TOKENS_OUT,
    BPT_IN_FOR_EXACT_TOKENS_OUT
  }

  function joinPool(
    bytes32 poolId,
    address sender,
    address recipient,
    JoinPoolRequest memory request
  ) external payable;

  struct JoinPoolRequest {
    address[] assets;
    uint256[] maxAmountsIn;
    bytes userData;
    bool fromInternalBalance;
  }

  enum JoinKind {
    INIT,
    EXACT_TOKENS_IN_FOR_BPT_OUT,
    TOKEN_IN_FOR_EXACT_BPT_OUT
  }

  function getPoolTokenInfo(
    bytes32 poolId,
    IERC20 token
  )
    external
    view
    returns (uint256 cash, uint256 managed, uint256 lastChangeBlock, address assetManager);

  function getPoolTokens(
    bytes32 poolId
  )
    external
    view
    returns (address[] memory tokens, uint256[] memory balances, uint256 lastChangeBlock);

  struct BatchSwapStep {
    bytes32 poolId;
    uint256 assetInIndex;
    uint256 assetOutIndex;
    uint256 amount;
    bytes userData;
  }

  function batchSwap(
    SwapKind kind,
    BatchSwapStep[] memory swaps,
    address[] memory assets,
    FundManagement memory funds,
    int256[] memory limits,
    uint256 deadline
  ) external payable returns (int256[] memory);

  function flashLoan(
    address recipient,
    IERC20[] memory tokens,
    uint256[] memory amounts,
    bytes memory userData
  ) external;
}

library BalancerswapAdapter {
  using SafeERC20 for IERC20;

  struct Path {
    address[] tokens;
    bytes32[] poolIds;
  }

  error SW_PATH_LENGTH_INVALID();
  error SW_PATH_TOKEN_INVALID();
  error SW_MISMATCH_RETURNED_AMOUNT();

  address private constant BALANCER_VAULT = 0xBA12222222228d8Ba445958a75a0704d566BF2C8;

  function swapExactTokensForTokens(
    address assetToSwapFrom,
    address assetToSwapTo,
    uint256 amountToSwap,
    Path calldata path,
    uint256 minAmountOut
  ) external returns (uint256) {
    // Check path is valid
    uint256 length = path.tokens.length;
    if (length <= 1 || length - 1 != path.poolIds.length) revert SW_PATH_LENGTH_INVALID();
    if (path.tokens[0] != assetToSwapFrom || path.tokens[length - 1] != assetToSwapTo) revert SW_PATH_TOKEN_INVALID();

    // Approves the transfer for the swap. Approves for 0 first to comply with tokens that implement the anti frontrunning approval fix.
    IERC20(assetToSwapFrom).safeApprove(address(BALANCER_VAULT), 0);
    if (IERC20(assetToSwapFrom).allowance(address(this), address(BALANCER_VAULT)) == 0)
      IERC20(assetToSwapFrom).safeApprove(address(BALANCER_VAULT), amountToSwap);

    IBalancerVault.BatchSwapStep[] memory swaps = new IBalancerVault.BatchSwapStep[](length - 1);
    int256[] memory limits = new int256[](length);
    for (uint256 i; i < length - 1; ++i) {
      swaps[i] = IBalancerVault.BatchSwapStep({
        poolId: path.poolIds[i],
        assetInIndex: i,
        assetOutIndex: i + 1,
        amount: 0,
        userData: "0"
      });
    }
    swaps[0].amount = amountToSwap;
    limits[0] = int256(amountToSwap);
    unchecked {
      limits[length - 1] = int256(0 - minAmountOut);
    }

    IBalancerVault.FundManagement memory funds = IBalancerVault.FundManagement({
      sender: address(this),
      fromInternalBalance: false,
      recipient: payable(address(this)),
      toInternalBalance: false
    });

    int256[] memory receivedAmount = IBalancerVault(BALANCER_VAULT).batchSwap(
      IBalancerVault.SwapKind.GIVEN_IN,
      swaps,
      path.tokens,
      funds,
      limits,
      block.timestamp
    );

    uint256 receivedPositveAmount;
    unchecked {
      receivedPositveAmount = uint256(0 - receivedAmount[length - 1]);
    }

    if (receivedPositveAmount == 0) revert SW_MISMATCH_RETURNED_AMOUNT();
    if (IERC20(assetToSwapTo).balanceOf(address(this)) < receivedPositveAmount) revert SW_MISMATCH_RETURNED_AMOUNT();

    return receivedPositveAmount;
  }
}

interface IUniswapV3SwapCallback {
    /// @notice Called to `msg.sender` after executing a swap via IUniswapV3Pool#swap.
    /// @dev In the implementation you must pay the pool tokens owed for the swap.
    /// The caller of this method must be checked to be a UniswapV3Pool deployed by the canonical UniswapV3Factory.
    /// amount0Delta and amount1Delta can both be 0 if no tokens were swapped.
    /// @param amount0Delta The amount of token0 that was sent (negative) or must be received (positive) by the pool by
    /// the end of the swap. If positive, the callback must send that amount of token0 to the pool.
    /// @param amount1Delta The amount of token1 that was sent (negative) or must be received (positive) by the pool by
    /// the end of the swap. If positive, the callback must send that amount of token1 to the pool.
    /// @param data Any data passed through by the caller via the IUniswapV3PoolActions#swap call
    function uniswapV3SwapCallback(
        int256 amount0Delta,
        int256 amount1Delta,
        bytes calldata data
    ) external;
}

interface ISwapRouter is IUniswapV3SwapCallback {
    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 deadline;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another token
    /// @param params The parameters necessary for the swap, encoded as `ExactInputSingleParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInputSingle(
        ExactInputSingleParams calldata params
    ) external payable returns (uint256 amountOut);

    struct ExactInputParams {
        bytes path;
        address recipient;
        uint256 deadline;
        uint256 amountIn;
        uint256 amountOutMinimum;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another along the specified path
    /// @param params The parameters necessary for the multi-hop swap, encoded as `ExactInputParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInput(
        ExactInputParams calldata params
    ) external payable returns (uint256 amountOut);

    struct ExactOutputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 deadline;
        uint256 amountOut;
        uint256 amountInMaximum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps as little as possible of one token for `amountOut` of another token
    /// @param params The parameters necessary for the swap, encoded as `ExactOutputSingleParams` in calldata
    /// @return amountIn The amount of the input token
    function exactOutputSingle(
        ExactOutputSingleParams calldata params
    ) external payable returns (uint256 amountIn);

    struct ExactOutputParams {
        bytes path;
        address recipient;
        uint256 deadline;
        uint256 amountOut;
        uint256 amountInMaximum;
    }

    /// @notice Swaps as little as possible of one token for `amountOut` of another along the specified path (reversed)
    /// @param params The parameters necessary for the multi-hop swap, encoded as `ExactOutputParams` in calldata
    /// @return amountIn The amount of the input token
    function exactOutput(
        ExactOutputParams calldata params
    ) external payable returns (uint256 amountIn);

    // Taken from https://soliditydeveloper.com/uniswap3
    // Manually added to the interface
    function refundETH() external payable;
}

library UniswapAdapter {
  using SafeERC20 for IERC20;

  error SW_PATH_LENGTH_INVALID();
  error SW_PATH_TOKEN_INVALID();
  error SW_MISMATCH_RETURNED_AMOUNT();

  address private constant UNISWAP_ROUTER = 0xE592427A0AEce92De3Edee1F18E0157C05861564;

  struct Path {
    address[] tokens;
    uint256[] fees;
  }

  function swapExactTokensForTokens(
    address assetToSwapFrom,
    address assetToSwapTo,
    uint256 amountToSwap,
    Path calldata path,
    uint256 minAmountOut
  ) external returns (uint256) {
    // Check path is valid
    uint256 length = path.tokens.length;
    if (length <= 1 || length - 1 != path.fees.length) revert SW_PATH_LENGTH_INVALID();
    if (path.tokens[0] != assetToSwapFrom || path.tokens[length - 1] != assetToSwapTo) revert SW_PATH_TOKEN_INVALID();

    // Approves the transfer for the swap. Approves for 0 first to comply with tokens that implement the anti frontrunning approval fix.
    IERC20(assetToSwapFrom).safeApprove(address(UNISWAP_ROUTER), 0);
    IERC20(assetToSwapFrom).safeApprove(address(UNISWAP_ROUTER), amountToSwap);

    uint256 receivedAmount;
    if (length > 2) {
      bytes memory _path;

      for (uint256 i; i < length - 1; ++i) {
        _path = abi.encodePacked(_path, path.tokens[i], uint24(path.fees[i]));
      }
      _path = abi.encodePacked(_path, assetToSwapTo);

      ISwapRouter.ExactInputParams memory params = ISwapRouter.ExactInputParams({
        path: _path,
        recipient: address(this),
        deadline: block.timestamp,
        amountIn: amountToSwap,
        amountOutMinimum: minAmountOut
      });

      // Executes the swap.
      receivedAmount = ISwapRouter(UNISWAP_ROUTER).exactInput(params);
    } else {
      ISwapRouter.ExactInputSingleParams memory params = ISwapRouter.ExactInputSingleParams({
        tokenIn: assetToSwapFrom,
        tokenOut: assetToSwapTo,
        fee: uint24(path.fees[0]),
        recipient: address(this),
        deadline: block.timestamp,
        amountIn: amountToSwap,
        amountOutMinimum: minAmountOut,
        sqrtPriceLimitX96: 0
      });

      // Executes the swap.
      receivedAmount = ISwapRouter(UNISWAP_ROUTER).exactInputSingle(params);
    }

    if (receivedAmount == 0) revert SW_MISMATCH_RETURNED_AMOUNT();
    if (IERC20(assetToSwapTo).balanceOf(address(this)) < receivedAmount) revert SW_MISMATCH_RETURNED_AMOUNT();

    return receivedAmount;
  }
}

interface ICurveAddressProvider {
  function get_address(uint256 id) external view returns (address);
}

interface ICurveExchange {
  function exchange(
    address _pool,
    address _from,
    address _to,
    uint256 _amount,
    uint256 _expected,
    address _receiver
  ) external payable returns (uint256);

  function exchange_multiple(
    address[9] memory _route,
    uint256[3][4] memory _swap_params,
    uint256 _amount,
    uint256 _expected,
    address[4] memory _pools,
    address _receiver
  ) external payable returns (uint256);
}

library CurveswapAdapter {
  using SafeERC20 for IERC20;

  error SW_MISMATCH_RETURNED_AMOUNT();

  address private constant curveAddressProvider = 0x0000000022D53366457F9d5E68Ec105046FC4383;

  struct Path {
    address[9] routes;
    uint256[3][4] swapParams;
  }

  address constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

  function swapExactTokensForTokens(
    address assetToSwapFrom,
    address assetToSwapTo,
    uint256 amountToSwap,
    Path calldata path,
    uint256 minAmountOut
  ) external returns (uint256) {
    // Approves the transfer for the swap. Approves for 0 first to comply with tokens that implement the anti frontrunning approval fix.
    address curveExchange = ICurveAddressProvider(curveAddressProvider).get_address(2);

    IERC20(assetToSwapFrom).safeApprove(address(curveExchange), 0);
    IERC20(assetToSwapFrom).safeApprove(address(curveExchange), amountToSwap);

    address[4] memory pools;
    uint256 receivedAmount = ICurveExchange(curveExchange).exchange_multiple(
      path.routes,
      path.swapParams,
      amountToSwap,
      minAmountOut,
      pools,
      address(this)
    );

    if (receivedAmount == 0) revert SW_MISMATCH_RETURNED_AMOUNT();
    uint256 balanceOfAsset;
    if (assetToSwapTo == ETH) {
      balanceOfAsset = address(this).balance;
    } else {
      balanceOfAsset = IERC20(assetToSwapTo).balanceOf(address(this));
    }
    if (balanceOfAsset < receivedAmount - 2) revert SW_MISMATCH_RETURNED_AMOUNT();
    return balanceOfAsset;
  }
}

abstract contract BaseLeverage is IFlashLoanReceiver, IFlashLoanRecipient, ReentrancyGuard {
  using SafeERC20 for IERC20;

  error LV_INVALID_CONFIGURATION();
  error LV_AMOUNT_NOT_GT_0();
  error LV_SUPPLY_NOT_ALLOWED();
  error LV_SUPPLY_FAILED();

  address private constant AAVE_LENDING_POOL_ADDRESS = 0x87870Bca3F3fD6335C3F4ce8392D69350B4fA4E2;

  address private constant BALANCER_VAULT = 0xBA12222222228d8Ba445958a75a0704d566BF2C8;

  uint256 private constant PERCENTAGE_FACTOR = 100_00;

  //1 == not inExec
  //2 == inExec;
  //setting default to 1 to save some gas.
  uint256 private _balancerFlashLoanLock = 1;

  /**
   * This function is called after your contract has received the flash loaned amount
   * overriding executeOperation() in IFlashLoanReceiver
   */
  function executeOperation(
    address[] calldata assets,
    uint256[] calldata amounts,
    uint256[] calldata premiums,
    address initiator,
    bytes calldata params
  ) external override returns (bool) {
    if (initiator != address(this)) revert LV_INVALID_CONFIGURATION();
    if (msg.sender != AAVE_LENDING_POOL_ADDRESS) revert LV_INVALID_CONFIGURATION();
    if (assets.length != amounts.length) revert LV_INVALID_CONFIGURATION();
    if (assets.length != premiums.length) revert LV_INVALID_CONFIGURATION();
    if (amounts[0] == 0) revert LV_INVALID_CONFIGURATION();
    if (assets[0] == address(0)) revert LV_INVALID_CONFIGURATION();

    _executeOperation(assets[0], amounts[0], premiums[0], params);

    // approve the Aave LendingPool contract allowance to *pull* the owed amount
    IERC20(assets[0]).safeApprove(AAVE_LENDING_POOL_ADDRESS, 0);
    IERC20(assets[0]).safeApprove(AAVE_LENDING_POOL_ADDRESS, amounts[0] + premiums[0]);

    return true;
  }

  /**
   * This function is called after your contract has received the flash loaned amount
   * overriding receiveFlashLoan() in IFlashLoanRecipient
   */
  function receiveFlashLoan(
    IERC20[] memory tokens,
    uint256[] memory amounts,
    uint256[] memory feeAmounts,
    bytes memory userData
  ) external override {
    if (msg.sender != BALANCER_VAULT) revert LV_INVALID_CONFIGURATION();
    if (_balancerFlashLoanLock != 2) revert LV_INVALID_CONFIGURATION();
    if (tokens.length != amounts.length) revert LV_INVALID_CONFIGURATION();
    if (tokens.length != feeAmounts.length) revert LV_INVALID_CONFIGURATION();
    if (amounts[0] == 0) revert LV_INVALID_CONFIGURATION();
    if (address(tokens[0]) == address(0)) revert LV_INVALID_CONFIGURATION();

    _balancerFlashLoanLock = 1;

    _executeOperation(address(tokens[0]), amounts[0], feeAmounts[0], userData);

    // send tokens to Balancer vault contract
    IERC20(tokens[0]).safeTransfer(msg.sender, amounts[0] + feeAmounts[0]);
  }

  function _executeOperation(
    address asset,
    uint256 borrowAmount,
    uint256 fee,
    bytes memory params
  ) internal {
    // parse params
    IBaseLeverage.FlashLoanParams memory opsParams = abi.decode(
      params,
      (IBaseLeverage.FlashLoanParams)
    );
    if (opsParams.minRequiredAmount == 0) revert LV_INVALID_CONFIGURATION();
    if (opsParams.user == address(0)) revert LV_INVALID_CONFIGURATION();

    if (opsParams.isEnterPosition) {
      _enterPositionWithFlashloan(asset, borrowAmount, fee, opsParams);
    } else {
      _withdrawWithFlashloan(asset, borrowAmount, opsParams);
    }
  }

  /**
   * @param _principal - The amount of collateral
   * @param _leverage - Extra leverage value and must be greater than 0, ex. 300% = 300_00
   *                    _principal + _principal * _leverage should be used as collateral
   * @param _borrowAsset - The flashloan borrowing asset address when leverage works
   * @param _collateralAsset - The collateral asset address when leverage works
   * @param _silo - The silo address
   * @param _flashLoanType - 0 is Aave, 1 is Balancer
   * @param _borrowAssetAndCollateral - The uniswap/balancer/curve swap paths between borrowAsset and collateral
   * @param _borrowAssetAndSiloAsset - The uniswap/balancer/curve swap paths between borrowAsset and silo asset
   */
  function enterPositionWithFlashloan(
    uint256 _principal,
    uint256 _leverage,
    address _borrowAsset,
    address _collateralAsset,
    address _silo,
    IBaseLeverage.FlashLoanType _flashLoanType,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndCollateral,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndSiloAsset
  ) external nonReentrant {
    if (_principal == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_leverage == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_leverage >= 900_00) revert LV_INVALID_CONFIGURATION();
    if (_borrowAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_collateralAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_silo == address(0)) revert LV_INVALID_CONFIGURATION();
    if (IERC20(_collateralAsset).balanceOf(msg.sender) < _principal) revert LV_SUPPLY_NOT_ALLOWED();

    IERC20(_collateralAsset).safeTransferFrom(msg.sender, address(this), _principal);

    _leverageWithFlashloan(
      IBaseLeverage.LeverageParams(
        msg.sender,
        _principal,
        _leverage,
        _borrowAsset,
        _collateralAsset,
        _silo,
        _flashLoanType,
        _borrowAssetAndCollateral,
        _borrowAssetAndSiloAsset
      )
    );
  }

  /**
   * @param _repayAmount - The amount of repay
   * @param _requiredAmount - The amount of collateral
   * @param _borrowAsset - The flashloan borrowing asset address when leverage works
   * @param _collateralAsset - The collateral asset address when leverage works
   * @param _silo - The silo address
   * @param _flashLoanType - 0 is Aave, 1 is Balancer
   * @param _borrowAssetAndCollateral - The uniswap/balancer/curve swap paths between borrowAsset and collateral asset
   * @param _borrowAssetAndSiloAsset - The uniswap/balancer/curve swap paths between borrowAsset and silo asset
   */
  function withdrawWithFlashloan(
    uint256 _repayAmount,
    uint256 _requiredAmount,
    address _borrowAsset,
    address _collateralAsset,
    address _silo,
    IBaseLeverage.FlashLoanType _flashLoanType,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndCollateral,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndSiloAsset
  ) external nonReentrant {
    if (_repayAmount == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_requiredAmount == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_borrowAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_collateralAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_silo == address(0)) revert LV_INVALID_CONFIGURATION();

    uint256[] memory amounts = new uint256[](1);
    amounts[0] = _borrowAssetAndSiloAsset.paths[0].inAmount;
    if (amounts[0] == 0) {
      amounts[0] = _repayAmount;
    }

    bytes memory params = abi.encode(
      false /*leavePosition*/,
      _repayAmount,
      msg.sender,
      _collateralAsset,
      _silo,
      _borrowAssetAndCollateral,
      _borrowAssetAndSiloAsset
    );

    if (_flashLoanType == IBaseLeverage.FlashLoanType.AAVE) {
      // 0 means revert the transaction if not validated
      uint256[] memory modes = new uint256[](1);
      modes[0] = 0;

      address[] memory assets = new address[](1);
      assets[0] = _borrowAsset;
      IPool(AAVE_LENDING_POOL_ADDRESS).flashLoan(
        address(this),
        assets,
        amounts,
        modes,
        address(this),
        params,
        0
      );
    } else {
      if (_balancerFlashLoanLock != 1) revert LV_INVALID_CONFIGURATION();
      IERC20[] memory assets = new IERC20[](1);
      assets[0] = IERC20(_borrowAsset);
      _balancerFlashLoanLock = 2;
      IBalancerVault(BALANCER_VAULT).flashLoan(address(this), assets, amounts, params);
    }

    // remained borrow asset -> collateral
    _swapAsset(
      _borrowAsset,
      _collateralAsset,
      IERC20(_borrowAsset).balanceOf(address(this)),
      _borrowAssetAndCollateral.paths,
      _borrowAssetAndCollateral.pathLength,
      false
    );

    uint256 collateralAmount = IERC20(_collateralAsset).balanceOf(address(this));
    if (collateralAmount > _requiredAmount) {
      _supply(_collateralAsset, _silo, collateralAmount - _requiredAmount, msg.sender);
      collateralAmount = _requiredAmount;
    }

    // finally deliver the collateral to user
    IERC20(_collateralAsset).safeTransfer(msg.sender, collateralAmount);
  }

  /**
   * @param _principal - The amount of zapping asset
   * @param _borrowAmount - The amount of borrowing asset
   * @param _zappingAsset - The address which will zap into collateral asset
   * @param _collateralAsset - The collateral asset address
   * @param _silo - The silo address
   * @param _zapAssetToCollateral - The uniswap/balancer/curve swap paths from zappingAsset to collateral asset
   */
  function zapDeposit(
    uint256 _principal,
    uint256 _borrowAmount,
    address _zappingAsset,
    address _collateralAsset,
    address _silo,
    IBaseLeverage.UniDirectSwapInfo calldata _zapAssetToCollateral
  ) external nonReentrant {
    if (_principal == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_zappingAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_collateralAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_silo == address(0)) revert LV_INVALID_CONFIGURATION();
    if (IERC20(_zappingAsset).balanceOf(msg.sender) < _principal) revert LV_SUPPLY_FAILED();

    IERC20(_zappingAsset).safeTransferFrom(msg.sender, address(this), _principal);

    uint256 collateralAmount = _swapAsset(
      _zappingAsset, 
      _collateralAsset, 
      _principal, 
      _zapAssetToCollateral.paths, 
      _zapAssetToCollateral.pathLength, 
      true
    );
    
    // deposit collateral
    _supply(_collateralAsset, _silo, collateralAmount, msg.sender);

    //borrow
    if (_borrowAmount != 0) {
      _borrow(_silo, _borrowAmount, msg.sender, msg.sender);
    }
  }

  /**
   * @param _principal - The amount of the zapping asset
   * @param _leverage - Extra leverage value and must be greater than 0, ex. 300% = 300_00
   *                    principal + principal * leverage should be used as collateral
   * @param _zappingAsset - The address which will zap into collateral asset
   * @param _collateralAsset - The collateral asset address when leverage works
   * @param _borrowAsset - The flashloan borrowing asset address when leverage works
   * @param _silo - The silo address
   * @param _flashLoanType - 0 is Aave, 1 is Balancer
   * @param _zapAssetToCollateral - The uniswap/balancer/curve swap paths from zappingAsset to collateral asset
   * @param _borrowAssetAndCollateral - The uniswap/balancer/curve swap path length between borrowAsset and collateral asset
   * @param _borrowAssetAndSiloAsset - The uniswap/balancer/curve swap between borrowAsset and collateralAsset
   */
  function zapLeverageWithFlashloan(
    uint256 _principal,
    uint256 _leverage,
    address _zappingAsset,
    address _collateralAsset,
    address _borrowAsset,
    address _silo,
    IBaseLeverage.FlashLoanType _flashLoanType,
    IBaseLeverage.UniDirectSwapInfo calldata _zapAssetToCollateral,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndCollateral,
    IBaseLeverage.BiDirectSwapInfo calldata _borrowAssetAndSiloAsset
  ) external nonReentrant {
    if (_principal == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_leverage == 0) revert LV_AMOUNT_NOT_GT_0();
    if (_leverage >= 900_00) revert LV_INVALID_CONFIGURATION();
    if (_zappingAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_collateralAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_borrowAsset == address(0)) revert LV_INVALID_CONFIGURATION();
    if (_silo == address(0)) revert LV_INVALID_CONFIGURATION();
    if (IERC20(_zappingAsset).balanceOf(msg.sender) < _principal) revert LV_SUPPLY_FAILED();

    IERC20(_zappingAsset).safeTransferFrom(msg.sender, address(this), _principal);

    uint256 collateralAmount = _swapAsset(
      _zappingAsset, 
      _collateralAsset, 
      _principal, 
      _zapAssetToCollateral.paths, 
      _zapAssetToCollateral.pathLength, 
      true
    );

    _leverageWithFlashloan(
      IBaseLeverage.LeverageParams(
        msg.sender,
        collateralAmount,
        _leverage,
        _borrowAsset,
        _collateralAsset,
        _silo,
        _flashLoanType,
        _borrowAssetAndCollateral,
        _borrowAssetAndSiloAsset
      )
    );
  }

  function _leverageWithFlashloan(IBaseLeverage.LeverageParams memory _params) internal {
    uint256 minCollateralAmount = _params.principal * (PERCENTAGE_FACTOR + _params.leverage) / PERCENTAGE_FACTOR;

    bytes memory params = abi.encode(
      true /*enterPosition*/,
      minCollateralAmount,
      _params.user,
      _params.collateralAsset,
      _params.silo,
      _params.borrowAssetAndCollateral,
      _params.borrowAssetAndSiloAsset
    );

    uint256[] memory amounts = new uint256[](1);
    amounts[0] = _params.borrowAssetAndCollateral.paths[0].inAmount;
    if (_params.flashLoanType == IBaseLeverage.FlashLoanType.AAVE) {
      // 0 means revert the transaction if not validated
      uint256[] memory modes = new uint256[](1);
      address[] memory assets = new address[](1);
      assets[0] = _params.borrowAsset;
      IPool(AAVE_LENDING_POOL_ADDRESS).flashLoan(
        address(this),
        assets,
        amounts,
        modes,
        address(this),
        params,
        0
      );
    } else {
      if (_balancerFlashLoanLock != 1) revert LV_INVALID_CONFIGURATION();

      IERC20[] memory assets = new IERC20[](1);
      assets[0] = IERC20(_params.borrowAsset);
      _balancerFlashLoanLock = 2;
      IBalancerVault(BALANCER_VAULT).flashLoan(address(this), assets, amounts, params);
      _balancerFlashLoanLock = 1;
    }

    _afterLeverageWithFlashloan(
      _params.borrowAsset,
      _params
    );
  }

  function _swapAsset(
    address _fromAsset,
    address _toAsset,
    uint256 _amount,
    IBaseLeverage.MultipSwapPath[3] memory _paths,
    uint256 _pathLength,
    bool _checkOutAmount
  ) internal returns (uint256) {
    if (_pathLength == 0) revert LV_INVALID_CONFIGURATION();
    if (_paths[0].swapFrom != _fromAsset) revert LV_INVALID_CONFIGURATION();
    if (_paths[_pathLength - 1].swapTo != _toAsset) revert LV_INVALID_CONFIGURATION();

    uint256 amount = _amount;
    if (amount == 0) return 0;

    for (uint256 i; i < _pathLength; ++i) {
      if (_paths[i].swapType == IBaseLeverage.SwapType.NONE) continue;

      amount = _processSwap(amount, _paths[i], _checkOutAmount);
    }

    return amount;
  }

  function _swapByPath(
    uint256 _fromAmount,
    IBaseLeverage.MultipSwapPath memory _path,
    bool _checkOutAmount
  ) internal returns (uint256) {
    uint256 poolCount = _path.poolCount;
    uint256 outAmount = _checkOutAmount ? _path.outAmount : 0;
    if (poolCount == 0) revert LV_INVALID_CONFIGURATION();

    if (_path.swapType == IBaseLeverage.SwapType.BALANCER) {
      // Balancer Swap
      BalancerswapAdapter.Path memory path;
      path.tokens = new address[](poolCount + 1);
      path.poolIds = new bytes32[](poolCount);

      for (uint256 i; i < poolCount; ++i) {
        path.tokens[i] = _path.routes[i * 2];
        path.poolIds[i] = bytes32(_path.routeParams[i][0]);
      }
      path.tokens[poolCount] = _path.routes[poolCount * 2];

      return
        BalancerswapAdapter.swapExactTokensForTokens(
          _path.swapFrom,
          _path.swapTo,
          _fromAmount,
          path,
          outAmount
        );
    }

    if (_path.swapType == IBaseLeverage.SwapType.UNISWAP) {
      // UniSwap
      UniswapAdapter.Path memory path;
      path.tokens = new address[](poolCount + 1);
      path.fees = new uint256[](poolCount);

      for (uint256 i; i < poolCount; ++i) {
        path.tokens[i] = _path.routes[i * 2];
        path.fees[i] = _path.routeParams[i][0];
      }
      path.tokens[poolCount] = _path.routes[poolCount * 2];

      return
        UniswapAdapter.swapExactTokensForTokens(
          _path.swapFrom,
          _path.swapTo,
          _fromAmount,
          path,
          outAmount
        );
    }

    // Curve Swap
    return
      CurveswapAdapter.swapExactTokensForTokens(
        _path.swapFrom,
        _path.swapTo,
        _fromAmount,
        CurveswapAdapter.Path(_path.routes, _path.routeParams),
        outAmount
      );
  }

  function _enterPositionWithFlashloan(
    address _borrowAsset,
    uint256 _borrowedAmount,
    uint256 _fee,
    IBaseLeverage.FlashLoanParams memory _params
  ) internal virtual;

  function _afterLeverageWithFlashloan(
    address _borrowAsset,
    IBaseLeverage.LeverageParams memory _params
  ) internal virtual;

  function _withdrawWithFlashloan(
    address _borrowAsset,
    uint256 _borrowedAmount,
    IBaseLeverage.FlashLoanParams memory _params
  ) internal virtual;

  function _supply(
    address _collateralAsset, 
    address _silo, 
    uint256 _amount, 
    address _user
  ) internal virtual;

  function _remove(
    uint256 _amount, 
    address _silo, 
    uint256 _slippage, 
    address _user
  ) internal virtual;

  function _borrow(
    address _silo, 
    uint256 _amount, 
    address _borrower,
    address _receiver
  ) internal virtual;

  function _repay(
    address _silo, 
    uint256 _amount, 
    address _borrower
  ) internal virtual;

  function _processSwap(
    uint256 _amount,
    IBaseLeverage.MultipSwapPath memory _path,
    bool _checkOutAmount
  ) internal virtual returns (uint256);
}

struct VaultAccount {
    uint128 amount; // Total amount, analogous to market cap
    uint128 shares; // Total shares, analogous to shares outstanding
}

interface ISturdyPair {
    struct CurrentRateInfo {
        uint32 lastBlock;
        uint32 feeToProtocolRate; // Fee amount 1e5 precision
        uint64 lastTimestamp;
        uint64 ratePerSec;
        uint64 fullUtilizationRate;
    }

    function CIRCUIT_BREAKER_ADDRESS() external view returns (address);

    function COMPTROLLER_ADDRESS() external view returns (address);

    function DEPLOYER_ADDRESS() external view returns (address);

    function FRAXLEND_WHITELIST_ADDRESS() external view returns (address);

    function timelockAddress() external view returns (address);

    function addCollateral(uint256 _collateralAmount, address _borrower) external;
    
    function addInterest(
        bool _returnAccounting
    )
        external
        returns (
            uint256 _interestEarned,
            uint256 _feesAmount,
            uint256 _feesShare,
            CurrentRateInfo memory _currentRateInfo,
            VaultAccount memory _totalAsset,
            VaultAccount memory _totalBorrow
        );

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function approvedBorrowers(address) external view returns (bool);

    function approvedLenders(address) external view returns (bool);

    function approveBorrowDelegation(address _delegatee, uint256 _amount) external;

    function asset() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function borrowAsset(
        uint256 _borrowAmount,
        uint256 _collateralAmount,
        address _receiver
    ) external returns (uint256 _shares);

    function borrowAssetOnBehalfOf(
        uint256 _borrowAmount,
        address _onBehalfOf
    ) external returns (uint256 _shares);

    function borrowerWhitelistActive() external view returns (bool);

    function changeFee(uint32 _newFee) external;

    function cleanLiquidationFee() external view returns (uint256);

    function collateralContract() external view returns (address);

    function currentRateInfo()
        external
        view
        returns (
            uint32 lastBlock,
            uint32 feeToProtocolRate,
            uint64 lastTimestamp,
            uint64 ratePerSec,
            uint64 fullUtilizationRate
        );

    function decimals() external view returns (uint8);

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool);

    function deposit(uint256 _amount, address _receiver) external returns (uint256 _sharesReceived);

    function dirtyLiquidationFee() external view returns (uint256);

    function exchangeRateInfo() external view returns (address oracle, uint32 maxOracleDeviation, uint184 lastTimestamp, uint256 lowExchangeRate, uint256 highExchangeRate);

    function getConstants()
        external
        pure
        returns (
            uint256 _LTV_PRECISION,
            uint256 _LIQ_PRECISION,
            uint256 _UTIL_PREC,
            uint256 _FEE_PRECISION,
            uint256 _EXCHANGE_PRECISION,
            uint256 _DEVIATION_PRECISION,
            uint256 _RATE_PRECISION,
            uint256 _MAX_PROTOCOL_FEE
        );

    function getImmutableAddressBool()
        external
        view
        returns (
            address _assetContract,
            address _collateralContract,
            address _oracleMultiply,
            address _oracleDivide,
            address _rateContract,
            address _DEPLOYER_CONTRACT,
            address _COMPTROLLER_ADDRESS,
            address _FRAXLEND_WHITELIST,
            bool _borrowerWhitelistActive,
            bool _lenderWhitelistActive
        );

    function getImmutableUint256()
        external
        view
        returns (
            uint256 _oracleNormalization,
            uint256 _maxLTV,
            uint256 _cleanLiquidationFee,
            uint256 _maturityDate,
            uint256 _penaltyRate
        );

    function getPairAccounting()
        external
        view
        returns (
            uint128 _totalAssetAmount,
            uint128 _totalAssetShares,
            uint128 _totalBorrowAmount,
            uint128 _totalBorrowShares,
            uint256 _totalCollateral
        );

    function getUserSnapshot(
        address _address
    ) external view returns (uint256 _userAssetShares, uint256 _userBorrowShares, uint256 _userCollateralBalance);

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool);

    function lenderWhitelistActive() external view returns (bool);

    function leveragedPosition(
        address _swapperAddress,
        uint256 _borrowAmount,
        uint256 _initialCollateralAmount,
        uint256 _amountCollateralOutMin,
        address[] memory _path
    ) external returns (uint256 _totalCollateralBalance);

    function liquidate(
        uint128 _sharesToLiquidate,
        uint256 _deadline,
        address _borrower
    ) external returns (uint256 _collateralForLiquidator);

    function maturityDate() external view returns (uint256);

    function maxLTV() external view returns (uint256);

    function maxOracleDelay() external view returns (uint256);

    function name() external view returns (string memory);

    function oracleDivide() external view returns (address);

    function oracleMultiply() external view returns (address);

    function oracleNormalization() external view returns (uint256);

    function owner() external view returns (address);

    function pause() external;

    function paused() external view returns (bool);

    function penaltyRate() external view returns (uint256);

    function rateContract() external view returns (address);

    function redeem(uint256 _shares, address _receiver, address _owner) external returns (uint256 _amountToReturn);

    function removeCollateral(uint256 _collateralAmount, address _receiver) external;

    function removeCollateralFrom(
        uint256 _collateralAmount,
        address _receiver,
        address _borrower
    ) external;

    function setWhitelistedDelegators(address _delegator, bool _enabled) external;

    function renounceOwnership() external;

    function repayAsset(uint256 _shares, address _borrower) external returns (uint256 _amountToRepay);

    function repayAssetWithCollateral(
        address _swapperAddress,
        uint256 _collateralToSwap,
        uint256 _amountAssetOutMin,
        address[] memory _path
    ) external returns (uint256 _amountAssetOut);

    function setApprovedBorrowers(address[] memory _borrowers, bool _approval) external;

    function setApprovedLenders(address[] memory _lenders, bool _approval) external;

    function setMaxOracleDelay(uint256 _newDelay) external;

    function setSwapper(address _swapper, bool _approval) external;

    function setTimelock(address _newAddress) external;

    function swappers(address) external view returns (bool);

    function symbol() external view returns (string memory);

    function toAssetAmount(
        uint256 _shares,
        bool _roundUp,
        bool _previewInterest
    ) external view returns (uint256);

    function toAssetShares(
        uint256 _amount,
        bool _roundUp,
        bool _previewInterest
    ) external view returns (uint256);

    function toBorrowAmount(
        uint256 _shares,
        bool _roundUp,
        bool _previewInterest
    ) external view returns (uint256 _amount);

    function toBorrowShares(
        uint256 _amount,
        bool _roundUp,
        bool _previewInterest
    ) external view returns (uint256 _shares);

    function totalAsset() external view returns (uint128 amount, uint128 shares);

    function totalBorrow() external view returns (uint128 amount, uint128 shares);

    function totalCollateral() external view returns (uint256);

    function totalSupply() external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function transferFrom(address from, address to, uint256 amount) external returns (bool);

    function transferOwnership(address newOwner) external;

    function unpause() external;

    function updateExchangeRate()
        external
        returns (bool _isBorrowAllowed, uint256 _lowExchangeRate, uint256 _highExchangeRate);

    function userBorrowShares(address) external view returns (uint256);

    function userCollateralBalance(address) external view returns (uint256);

    function version() external pure returns (uint256 _major, uint256 _minor, uint256 _patch);

    function withdrawFees(uint128 _shares, address _recipient) external returns (uint256 _amountToTransfer);

    function isInterestPaused() external view returns (bool);
}

interface ISiloGateway {
    function borrowAsset(
        address _silo,
        uint256 _borrowAmount,
        uint256 _collateralAmount,
        address _collateralAsset,
        address _borrower,
        address _receiver
    ) external;
}

contract SturdyLeverage is Ownable, BaseLeverage {
  using SafeERC20 for IERC20;

  // silo -> silo gateway
  mapping (address => address) private _siloToGateway;

  error LV_REPAY_FAILED();

  /**
   * @dev Set the mapping between silos and gateways. 
   *      Gateways will be used to borrow asset for the Just-In-Time liquidity features.
   * @param _silos - The silo addresses
   * @param _gateways - The gateway addresses
   */
  function setGateways(
    address[] calldata _silos, 
    address[] calldata _gateways
  ) external payable onlyOwner {
    uint256 length = _silos.length;
    if (length != _gateways.length) revert LV_INVALID_CONFIGURATION();

    for (uint256 i; i < length; ++i) {
      _siloToGateway[_silos[i]] = _gateways[i];
    }
  }

  /**
   * @dev Get the gateway address for the silo
   * @param _silo - The silo address
   * @return The gateway address
   */
  function getGateway(
    address _silo
  ) external view returns (address) {
    return _siloToGateway[_silo];
  }

  function _enterPositionWithFlashloan(
    address _borrowAsset,
    uint256 _borrowedAmount,
    uint256 _fee,
    IBaseLeverage.FlashLoanParams memory _params
  ) internal override {
    //swap borrow asset -> collateral
    _swapAsset(
      _borrowAsset,
      _params.collateralAsset,
      _borrowedAmount,
      _params.borrowAssetAndCollateral.paths,
      _params.borrowAssetAndCollateral.pathLength,
      true
    );

    uint256 collateralAmount = IERC20(_params.collateralAsset).balanceOf(address(this));
    if (collateralAmount < _params.minRequiredAmount) revert LV_SUPPLY_FAILED();

    //deposit collateral
    _supply(_params.collateralAsset, _params.silo, collateralAmount, _params.user);

    ISturdyPair pair = ISturdyPair(_params.silo);
    if (pair.asset() == _borrowAsset) {
      //borrow
      _borrow(_params.silo, _borrowedAmount + _fee, _params.user, address(this));
    } else {
      ( uint256 LTV_PRECISION,,,, uint256 EXCHANGE_PRECISION,,,) = pair.getConstants();
      pair.addInterest(false);

      (,, uint256 exchangeRate) = pair.updateExchangeRate();
      uint256 borrowShares = pair.userBorrowShares(_params.user);
      uint256 borrowAmount =  pair.toBorrowAmount(borrowShares, true, false);
      uint256 collateralAmount = pair.userCollateralBalance(_params.user);
      borrowAmount = collateralAmount * EXCHANGE_PRECISION * pair.maxLTV() / exchangeRate / LTV_PRECISION - borrowAmount;

      //borrow
      _borrow(_params.silo, borrowAmount, _params.user, address(this));

      //swap asset -> borrow asset
      _swapAsset(
        pair.asset(),
        _borrowAsset,
        borrowAmount,
        _params.borrowAssetAndSiloAsset.reversePaths,
        _params.borrowAssetAndSiloAsset.pathLength,
        true
      );
    }
  }

  function _afterLeverageWithFlashloan(
    address _borrowAsset,
    IBaseLeverage.LeverageParams memory _params
  ) internal override {
    address asset = ISturdyPair(_params.silo).asset();

    if (asset == _borrowAsset) return;

    // remained borrow asset -> asset
    _swapAsset(
      _borrowAsset,
      asset,
      IERC20(_borrowAsset).balanceOf(address(this)),
      _params.borrowAssetAndSiloAsset.paths,
      _params.borrowAssetAndSiloAsset.pathLength,
      false
    );

    // repay
    _repay(_params.silo, IERC20(asset).balanceOf(address(this)), _params.user);
  }

  function _withdrawWithFlashloan(
    address _borrowAsset,
    uint256 _borrowedAmount,
    IBaseLeverage.FlashLoanParams memory _params
  ) internal override {
    ISturdyPair pair = ISturdyPair(_params.silo);
    address asset = pair.asset();

    if (asset == _borrowAsset) {
      // repay
      _repay(_params.silo, _borrowedAmount, _params.user);
    } else {
      //swap borrow asset -> asset
      _swapAsset(
        _borrowAsset,
        asset,
        _borrowedAmount,
        _params.borrowAssetAndSiloAsset.paths,
        _params.borrowAssetAndSiloAsset.pathLength,
        true
      );

      uint256 assetAmount = IERC20(asset).balanceOf(address(this));
      if (assetAmount < _params.minRequiredAmount) revert LV_REPAY_FAILED();

      // repay
      _repay(_params.silo, _params.minRequiredAmount, _params.user);

      //swap asset -> borrow asset
      _swapAsset(
        asset,
        _borrowAsset,
        assetAmount - _params.minRequiredAmount,
        _params.borrowAssetAndSiloAsset.reversePaths,
        _params.borrowAssetAndSiloAsset.pathLength,
        false
      );
    }

    // withdraw collateral    
    if (_params.collateralAsset != pair.collateralContract()) revert LV_INVALID_CONFIGURATION();

    ( uint256 LTV_PRECISION,,,, uint256 EXCHANGE_PRECISION,,,) = pair.getConstants();
    pair.addInterest(false);

    (,, uint256 exchangeRate) = pair.updateExchangeRate();
    uint256 borrowShares = pair.userBorrowShares(_params.user);
    uint256 borrowAmount =  pair.toBorrowAmount(borrowShares, true, false);
    uint256 collateralAmount = pair.userCollateralBalance(_params.user);
    uint256 withdrawalAmount = collateralAmount - (borrowAmount * exchangeRate * LTV_PRECISION / EXCHANGE_PRECISION / pair.maxLTV());

    _remove(withdrawalAmount, _params.silo, 0, _params.user);

    // collateral -> borrow asset
    _swapAsset(
      _params.collateralAsset, 
      _borrowAsset, 
      IERC20(_params.collateralAsset).balanceOf(address(this)),
      _params.borrowAssetAndCollateral.reversePaths, 
      _params.borrowAssetAndCollateral.pathLength,
      true
    );
  }

  function _supply(
    address _collateralAsset, 
    address _silo, 
    uint256 _amount, 
    address _user
  ) internal override {
    IERC20(_collateralAsset).safeApprove(_silo, 0);
    IERC20(_collateralAsset).safeApprove(_silo, _amount);
    ISturdyPair(_silo).addCollateral(_amount, _user);
  }

  function _remove(
    uint256 _amount, 
    address _silo, 
    uint256 _slippage, 
    address _user
  ) internal override {
    ISturdyPair(_silo).removeCollateralFrom(_amount, address(this), _user);
  }

  function _borrow(
    address _silo, 
    uint256 _amount, 
    address _borrower,
    address _receiver
  ) internal override {
    if (_siloToGateway[_silo] == address(0)) {
      ISturdyPair(_silo).borrowAssetOnBehalfOf(_amount, _borrower);
      
      if (_receiver != address(this)) {
        IERC20(ISturdyPair(_silo).asset()).safeTransfer(_receiver, _amount);
      }
    } else {
      ISiloGateway(_siloToGateway[_silo]).borrowAsset(
        _silo,
        _amount,
        0,
        ISturdyPair(_silo).collateralContract(),
        _borrower,
        _receiver
      );
    }

    
  }

  function _repay(
    address _silo, 
    uint256 _amount, 
    address _borrower
  ) internal override {
    IERC20 borrowAsset = IERC20(ISturdyPair(_silo).asset());

    ISturdyPair(_silo).addInterest(false);

    uint256 borrowShares =  ISturdyPair(_silo).toBorrowShares(_amount, false, false);

    borrowAsset.safeApprove(_silo, 0);
    borrowAsset.safeApprove(_silo, _amount);

    uint256 paybackAmount = ISturdyPair(_silo).repayAsset(borrowShares, _borrower);
    if (paybackAmount == 0) revert LV_REPAY_FAILED();
  }

  function _processSwap(
    uint256 _amount,
    IBaseLeverage.MultipSwapPath memory _path,
    bool _checkOutAmount
  ) internal virtual override returns (uint256) {
    return _swapByPath(_amount, _path, _checkOutAmount);
  }
}

interface IYearnVault {
    function pricePerShare() external view returns (uint256 price);

    function deposit(uint256 _amount, address recipient) external returns (uint256);

    function withdraw(
        uint256 maxShares,
        address recipient,
        uint256 maxLoss
    ) external returns (uint256);
}

interface ICurvePool {
    function get_virtual_price() external view returns (uint256 price);

    function price_oracle() external view returns (uint256);

    function price_oracle(uint256 i) external view returns (uint256);

    function balances(uint256 _id) external view returns (uint256);

    function calc_token_amount(
        uint256[2] memory _amounts,
        bool _is_deposit
    ) external view returns (uint256);

    function calc_token_amount(
        uint256[] memory _amounts,
        bool _is_deposit
    ) external view returns (uint256);

    function calc_withdraw_one_coin(uint256 _burn_amount, int128 i) external view returns (uint256);

    function get_dy(int128 i, int128 j, uint256 dx) external view returns (uint256);

    function coins(uint256 index) external view returns (address);

    function add_liquidity(uint256[2] memory amounts, uint256 _min_mint_amount) external;

    function add_liquidity(uint256[] memory amounts, uint256 _min_mint_amount) external returns (uint256);
}

/// @notice  Leverage contract for yearn vault of curve lp token
contract CrvYearnVaultLeverage2 is SturdyLeverage {
  using SafeERC20 for IERC20;

  address public immutable YEARN_VAULT;

  error LV_REQUIRE_MIN_AMOUNT();

  constructor(
      address _yearnVault
  ) {
    YEARN_VAULT = _yearnVault;
  }

  function _processSwap(
    uint256 _amount,
    IBaseLeverage.MultipSwapPath memory _path,
    bool _checkOutAmount
  ) internal override returns (uint256) {
    if (_path.swapType > IBaseLeverage.SwapType.WITHDRAW) {
      return _swapByPath(_amount, _path, _checkOutAmount);
    }

    if (_path.swapType == IBaseLeverage.SwapType.WITHDRAW) {
      if (!_checkOutAmount) revert LV_INVALID_CONFIGURATION();
      
      // Withdraw from Yearn Vault and receive Curve LP token
      uint256 outAmount = IYearnVault(_path.swapFrom).withdraw(_amount, address(this), 1);
      if (outAmount < _path.outAmount) revert LV_REQUIRE_MIN_AMOUNT();

      return outAmount;
    }

    address asset = _path.swapFrom;
    address vault = _path.swapTo;
    uint256 outAmount;

    IERC20(asset).safeApprove(vault, 0);
    IERC20(asset).safeApprove(vault, _amount);

    if (vault == YEARN_VAULT) {
      // Deposit Curve LP token to Yearn Vault and receive Yearn Vault LP token
      outAmount = IYearnVault(vault).deposit(_amount, address(this));
    } else {
      // Deposit asset to Curve pool and receive LP token.
      uint256[] memory amounts = new uint256[](2);
      for (uint256 i; i < 2; ++i) {
        if (ICurvePool(vault).coins(i) == asset) {
          amounts[i] = _amount;
          break;
        }
      }

      outAmount = ICurvePool(vault).add_liquidity(amounts, 0);
    }    

    if (_checkOutAmount && outAmount < _path.outAmount) {
      revert LV_REQUIRE_MIN_AMOUNT();
    }
  
    return outAmount;
  }
}