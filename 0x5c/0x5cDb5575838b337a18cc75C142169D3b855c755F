/** 
 *  SourceUnit: /Users/kuldeep/ETHEREUM/dxsale/DxLock/contracts/LPLocker/SingleContractMode/LpLockerV2Lite.sol
*/
            
////// SPDX-License-Identifier-FLATTEN-SUPPRESS-WARNING: MIT
// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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




/** 
 *  SourceUnit: /Users/kuldeep/ETHEREUM/dxsale/DxLock/contracts/LPLocker/SingleContractMode/LpLockerV2Lite.sol
*/
            
////// SPDX-License-Identifier-FLATTEN-SUPPRESS-WARNING: MIT
// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)

pragma solidity ^0.8.1;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [////IMPORTANT]
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
     * ////IMPORTANT: because control is transferred to `recipient`, care must be
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




/** 
 *  SourceUnit: /Users/kuldeep/ETHEREUM/dxsale/DxLock/contracts/LPLocker/SingleContractMode/LpLockerV2Lite.sol
*/
            
////// SPDX-License-Identifier-FLATTEN-SUPPRESS-WARNING: MIT
// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

pragma solidity ^0.8.0;

////import "../utils/Context.sol";

/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
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




/** 
 *  SourceUnit: /Users/kuldeep/ETHEREUM/dxsale/DxLock/contracts/LPLocker/SingleContractMode/LpLockerV2Lite.sol
*/
            
////// SPDX-License-Identifier-FLATTEN-SUPPRESS-WARNING: MIT
// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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
     * ////IMPORTANT: Beware that changing an allowance with this method brings the risk
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


/** 
 *  SourceUnit: /Users/kuldeep/ETHEREUM/dxsale/DxLock/contracts/LPLocker/SingleContractMode/LpLockerV2Lite.sol
*/

////// SPDX-License-Identifier-FLATTEN-SUPPRESS-WARNING: MIT
/**
 * @title DXLock - Empowering Trust through Token Locking
 * @dev Secure, User-Friendly Smart Contract to Lock Liquidity and Regular Tokens.
 *
 * 🚀 Introduction:
 * Welcome to DXLock, the pinnacle of token locking on the blockchain! Developed with passion and precision,
 * DXLock stands out as a beacon of trust and commitment in the crypto world. This smart contract is meticulously
 * crafted to lock both liquidity tokens and regular ERC20 tokens, ensuring a secure and transparent environment
 * for your assets.
 *
 * 🌐 Visit DXLock:
 * Dive deeper into the world of DXLock by visiting our platform at [dx.app](https://dx.app). Discover a treasure trove of features,
 * tutorials, and support to elevate your token locking experience!
 *
 * 💡 Features:
 * 1. **Liquidity Locking**: Cement your project's credibility by locking liquidity tokens. Show the world that you're here to stay!
 * 2. **Token Locking**: Not just for liquidity! Lock any ERC20 tokens with ease and confidence.
 * 3. **Time-locked Security**: Your tokens are safe and sound until the predetermined unlock time hits the clock.
 * 4. **Transparent and Trustworthy**: Open-source and audited, DXLock is a fortress of reliability.
 *
 * 🛡️ Security:
 * Your trust is our top priority. DXLock is fortified with industry-leading security practices to shield your assets
 * and ensure a seamless experience. Though thorough audits have been conducted, we encourage users to do their own
 * research and verify the contract's integrity before engaging.
 *
 * 📖 How to Use:
 * Engaging with DXLock is a breeze! Simply deposit your tokens, set the lock duration, and rest easy knowing
 * your assets are in good hands. Once the lock period concludes, withdrawing is just a click away.
 *
 * 👥 Community and Support:
 * Join our vibrant community and connect with the DXLock team and fellow users! Your feedback and questions are invaluable
 * to us, as we continually strive to enhance DXLock’s functionality and user experience.
 *
 * 📜 License:
 * DXLock is proudly released under the MIT License. We believe in openness and the power of community-driven innovation.
 *
 * @author The DXLock Team
 * @notice Utilize DXLock at your own discretion. We’ve done everything to ensure its security, but the final responsibility lies with the user.
 */

pragma solidity 0.8.17;

////import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

////import "@openzeppelin/contracts/access/Ownable.sol";

////import "@openzeppelin/contracts/utils/Address.sol";

interface ILPLockStorage {
    struct UserLockedLP {
        address lpAddress;
        uint256 countID;
    }

    struct LockedLPData {
        uint256 createdOn;
        address lockOwner;
        address lockedLPTokens;
        uint256 lockTime;
        bool locked;
        string logo;
        uint256 lockedAmount;
        uint256 countID;
        // bool exists;
        address token0Addr;
        address token1Addr;
    }

    function addNewLock(
        address _user,
        address _lpAddress,
        uint256 _locktime,
        // address _lockContract,
        uint256 _tokenAmount,
        string memory _logo
    ) external;

    function extendLockerTime(
        address _user,
        uint256 _userLockerNumber,
        uint256 _newLockTime,
        uint256 _relockFees
    ) external;

    function transferLocker(
        address _user,
        address _newOwner,
        uint256 _userLockerNumber
    ) external;

    function unlockLocker(address _user, uint256 _userLockerNumber) external;

    function changeLogo(
        address _user,
        string memory _newLogo,
        uint256 _userLockerNumber
    ) external;

    function getPersonalLockerCount(address _owner) external returns (uint256);

    function burnContract() external view returns (address);

    function lpTokenToLockCount(address) external view returns (uint256);

    function getUserLockedLP(
        address user,
        uint countId
    ) external view returns (UserLockedLP memory);

    function getLockedLPToken(
        address token,
        uint countId
    ) external view returns (LockedLPData memory);
}

interface ReferralContract {
    function getDiscountedPrice(string memory _code) external returns (uint256);

    function validateCode(string memory _code) external returns (bool);

    function fetchCodeOwner(string memory _code) external returns (address);

    function fetchCodeOwnerPercentage(
        string memory _code
    ) external returns (uint256);

    function updateReferrerAmounts(
        address _referrer,
        uint256 _updateAmount
    ) external returns (bool);

    function updateCodeUseNumber(
        string memory _code,
        address _presaleAddress
    ) external returns (bool);
}

interface LP {
    function token0() external view returns (address);

    function token1() external view returns (address);
}

interface Factory {
    function getPair(address, address) external view returns (address);
}

contract LpLockerV2Lite is Ownable {
    event LPLockerCreated(
        address indexed lockingToken,
        address indexed lockerOwner,
        uint256 indexed lockerCount,
        uint256 lockerEndTimeStamp,
        uint256 lockingAmount,
        string referralCode
    );
    event LPUnlocked(
        address indexed lockingToken,
        address indexed lockerOwner,
        uint256 indexed lockerCount,
        uint256 lockerEndTimeStamp,
        uint256 lockingAmount,
        uint256 unlockTimestamp
    );
    event LPLockExtended(
        address indexed lockingToken,
        address indexed lockerOwner,
        uint256 indexed lockerCount,
        uint256 newLockerEndTimeStamp,
        uint256 lockingAmount,
        uint256 callTimestamp
    );

    uint256 public constant HUNDRED = 100;
    uint256 public constant THOUSAND = 1000;

    uint256 public lockFeePercent = 10; //Divider is 1000 so 1 is 0.1%
    uint256 public gracePeriod = 300;
    bool public referralDisabled;
    string public dappFeeName = "dxlockFeesTokenLP";
    ILPLockStorage public lockerStorage;
    ReferralContract public referralContract;
    uint256 public relockFeePercent = 10; //Divider is 1000 so 1 is 0.1%
    address public treasuryAddress = 0xb44ea272f317E379567Ce54Acd94a2891597024E;

    // factory => whitelisted
    mapping(address => bool) public whitelistedDEXes;
    // LP => whitelisted
    mapping(address => mapping(address => bool)) public whitelistedLPs;

    constructor(
        ILPLockStorage _lockerStorage,
        ReferralContract _referralContract
    ) {
        lockerStorage = _lockerStorage;
        referralContract = _referralContract;
    }

    function whitelistDEX(address _factory) external onlyOwner {
        require(_factory != address(0), "Address(0)");
        require(!whitelistedDEXes[_factory], "Whitelisted");
        whitelistedDEXes[_factory] = true;
    }

    function blacklistDEX(address _factory) external onlyOwner {
        require(_factory != address(0), "Address(0)");
        require(whitelistedDEXes[_factory], "Blacklisted");
        whitelistedDEXes[_factory] = false;
    }

    function createLPLocker(
        address _factory,
        address _lpToken,
        uint256 _lockerEndTimeStamp,
        string memory _logo,
        uint256 _lockingAmount,
        string memory _referralCode
    ) public returns (uint lockId) {
        require(whitelistedDEXes[_factory], "Invalid DEX");
        require(_lpToken != address(0), "Invalid token");
        require(_lockingAmount > 0, "Invalid amount");
        require(_lockerEndTimeStamp > block.timestamp, "Invalid time");
        require(bytes(_logo).length != 0, "Invalid logo");

        if (!whitelistedLPs[_factory][_lpToken]) {
            _verifyLPToken(_factory, _lpToken);
        }

        uint256 lpPerFee = (_lockingAmount * lockFeePercent) / THOUSAND;
        bytes32 referralCode = keccak256(abi.encodePacked(_referralCode));

        if (referralDisabled) {
            require(
                referralCode == keccak256(abi.encodePacked("default")),
                "R:Invalid code"
            );
        }

        IERC20 lockedToken = IERC20(_lpToken);

        if (referralCode != keccak256(abi.encodePacked("default"))) {
            require(
                referralContract.validateCode(_referralCode),
                "R:Invalid code"
            );

            uint256 referrerAmount = (lpPerFee *
                (referralContract.fetchCodeOwnerPercentage(_referralCode))) /
                HUNDRED;

            require(
                lockedToken.transferFrom(
                    msg.sender,
                    address(referralContract.fetchCodeOwner(_referralCode)),
                    referrerAmount
                ),
                "R:transfer failed"
            );

            require(
                lockedToken.transferFrom(
                    msg.sender,
                    treasuryAddress,
                    (lpPerFee - referrerAmount)
                ),
                "F:transfer failed"
            );

            require(
                referralContract.updateCodeUseNumber(
                    _referralCode,
                    address(this)
                ),
                "R:Code Update failed"
            );
        } else {
            require(
                lockedToken.transferFrom(msg.sender, treasuryAddress, lpPerFee),
                "F:transfer failed"
            );
        }
        lockId = lockerStorage.getPersonalLockerCount(msg.sender);

        require(
            lockedToken.transferFrom(
                msg.sender,
                address(this),
                _lockingAmount - lpPerFee
            ),
            "L:transfer failed"
        );

        ILPLockStorage(lockerStorage).addNewLock(
            msg.sender,
            _lpToken,
            _lockerEndTimeStamp,
            _lockingAmount - lpPerFee,
            _logo
        );

        // Emit the event
        emit LPLockerCreated(
            _lpToken,
            msg.sender,
            lockId,
            _lockerEndTimeStamp,
            _lockingAmount,
            _referralCode
        );

        return lockId;
    }

    function _verifyLPToken(address _factory, address _lpToken) internal {
        address token0 = LP(_lpToken).token0();
        address token1 = LP(_lpToken).token1();

        require(token0 != address(0) && token1 != address(0), "Invalid tokens");

        address pair = Factory(_factory).getPair(token0, token1);
        require(pair == _lpToken, "Invalid LP token");

        whitelistedLPs[_factory][_lpToken] = true;
    }

    function unlockLPTokens(uint256 _lockerCount) public {
        ILPLockStorage.UserLockedLP memory userData = lockerStorage
            .getUserLockedLP(msg.sender, _lockerCount);

        ILPLockStorage.LockedLPData memory lockData = lockerStorage
            .getLockedLPToken(userData.lpAddress, userData.countID);

        require(userData.lpAddress != address(0), "Lock doesn't exists");

        require(lockData.locked, "Already unlocked");

        require(block.timestamp > lockData.lockTime, "LP:not unlocked");

        uint256 _unlockAmount = lockData.lockedAmount;

        IERC20 token = IERC20(lockData.lockedLPTokens);
        ILPLockStorage(lockerStorage).unlockLocker(msg.sender, _lockerCount);
        require(token.transfer(msg.sender, _unlockAmount), "LP:failed unlock");

        // Emit the event
        emit LPUnlocked(
            lockData.lockedLPTokens,
            msg.sender,
            _lockerCount,
            lockData.lockTime,
            _unlockAmount,
            block.timestamp
        );
    }

    function extendLocker(uint256 _lockerCount, uint256 _newEndTime) public {
        ILPLockStorage.UserLockedLP memory userData = lockerStorage
            .getUserLockedLP(msg.sender, _lockerCount);

        ILPLockStorage.LockedLPData memory lockData = lockerStorage
            .getLockedLPToken(userData.lpAddress, userData.countID);

        require(userData.lpAddress != address(0), "Lock doesn't exists");

        require(lockData.locked, "Already unlocked");

        require(
            block.timestamp < lockData.lockTime + gracePeriod,
            "Use relock"
        );

        require(_newEndTime > block.timestamp, "N:Invalid time");
        require(_newEndTime > lockData.lockTime, "E:Invalid time");

        ILPLockStorage(lockerStorage).extendLockerTime(
            msg.sender,
            _newEndTime,
            _lockerCount,
            0
        );

        // Emit the event
        emit LPLockExtended(
            lockData.lockedLPTokens,
            msg.sender,
            _lockerCount,
            lockData.lockTime,
            lockData.lockedAmount,
            block.timestamp
        );
    }

    function relock(uint256 _lockerCount, uint256 _newEndTime) public {
        ILPLockStorage.UserLockedLP memory userData = lockerStorage
            .getUserLockedLP(msg.sender, _lockerCount);

        ILPLockStorage.LockedLPData memory lockData = lockerStorage
            .getLockedLPToken(userData.lpAddress, userData.countID);

        require(userData.lpAddress != address(0), "Lock doesn't exists");

        require(lockData.locked, "Already unlocked");

        require(_newEndTime > block.timestamp, "N:Invalid time");
        require(_newEndTime > lockData.lockTime, "R:Invalid time");

        uint256 relockFees = (lockData.lockedAmount * relockFeePercent) /
            THOUSAND;

        require(
            IERC20(lockData.lockedLPTokens).transfer(
                treasuryAddress,
                relockFees
            ),
            "R:transfer failed"
        );

        ILPLockStorage(lockerStorage).extendLockerTime(
            msg.sender,
            _newEndTime,
            _lockerCount,
            relockFees
        );
        // Emit the event
        emit LPLockExtended(
            lockData.lockedLPTokens,
            msg.sender,
            _lockerCount,
            lockData.lockTime,
            lockData.lockedAmount,
            block.timestamp
        );
    }

    function changeStorageContract(
        ILPLockStorage _lockerStorage
    ) external onlyOwner {
        lockerStorage = _lockerStorage;
    }

    function changeReferralContract(
        ReferralContract _newRefContract
    ) external onlyOwner {
        referralContract = _newRefContract;
    }

    function updateExtensionGracePeriod(
        uint256 _newGracePeriod
    ) public onlyOwner {
        gracePeriod = _newGracePeriod;
    }

    function disableReferral() external onlyOwner {
        require(!referralDisabled, "Disabled");
        referralDisabled = true;
    }

    function enableReferral() external onlyOwner {
        require(referralDisabled, "Enabled");
        referralDisabled = false;
    }

    function changeFeePerc(uint256 _feeAmount) external onlyOwner {
        require(_feeAmount <= 1000, "Over limit");
        lockFeePercent = _feeAmount;
    }

    function changeRelockFeePerc(uint256 _feeAmount) external onlyOwner {
        require(_feeAmount <= 1000, "Over limit");
        relockFeePercent = _feeAmount;
    }

    function changeTreasuryAddress(
        address _treasuryAddress
    ) external onlyOwner {
        require(_treasuryAddress != address(0), "Address(0)");
        treasuryAddress = _treasuryAddress;
    }

    function updateFeesName(string memory _newFeesName) external onlyOwner {
        dappFeeName = _newFeesName;
    }

    function withdrawETH() public onlyOwner {
        address payable burnContractAddress = payable(
            ILPLockStorage(lockerStorage).burnContract()
        );
        Address.sendValue(burnContractAddress, address(this).balance);
    }

    function withdrawERC20Token(address _token) external onlyOwner {
        require(
            lockerStorage.lpTokenToLockCount(_token) == 0,
            "Withdraw failed"
        );
        uint256 amount = IERC20(_token).balanceOf(address(this));
        IERC20(_token).transfer(msg.sender, amount);
    }
}