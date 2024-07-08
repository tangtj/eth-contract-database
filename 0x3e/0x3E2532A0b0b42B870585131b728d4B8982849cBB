//
//
// @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
// @@                                                                                                @@
// @@   This token was launched using software provided by Metadrop. To learn more or to launch      @@
// @@   your own token, visit: https://metadrop.com. See legal info at the end of this file.         @@
// @@                                                                                                @@
// @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
//
// SPDX-License-Identifier: BUSL-1.1
// Metadrop Contracts (v2.1.0)
//
// Sources flattened with hardhat v2.20.1 https://hardhat.org

// File @openzeppelin/contracts/utils/Context.sol@v5.0.1

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

// File @openzeppelin/contracts/access/Ownable.sol@v5.0.1

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

// File @openzeppelin/contracts/interfaces/draft-IERC6093.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (interfaces/draft-IERC6093.sol)
pragma solidity ^0.8.20;

/**
 * @dev Standard ERC20 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC20 tokens.
 */
interface IERC20Errors {
    /**
     * @dev Indicates an error related to the current `balance` of a `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param balance Current balance for the interacting account.
     * @param needed Minimum amount required to perform a transfer.
     */
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC20InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC20InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `spender`’s `allowance`. Used in transfers.
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     * @param allowance Amount of tokens a `spender` is allowed to operate with.
     * @param needed Minimum amount required to perform a transfer.
     */
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC20InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `spender` to be approved. Used in approvals.
     * @param spender Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC20InvalidSpender(address spender);
}

/**
 * @dev Standard ERC721 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC721 tokens.
 */
interface IERC721Errors {
    /**
     * @dev Indicates that an address can't be an owner. For example, `address(0)` is a forbidden owner in EIP-20.
     * Used in balance queries.
     * @param owner Address of the current owner of a token.
     */
    error ERC721InvalidOwner(address owner);

    /**
     * @dev Indicates a `tokenId` whose `owner` is the zero address.
     * @param tokenId Identifier number of a token.
     */
    error ERC721NonexistentToken(uint256 tokenId);

    /**
     * @dev Indicates an error related to the ownership over a particular token. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param tokenId Identifier number of a token.
     * @param owner Address of the current owner of a token.
     */
    error ERC721IncorrectOwner(address sender, uint256 tokenId, address owner);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC721InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC721InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `operator`’s approval. Used in transfers.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     * @param tokenId Identifier number of a token.
     */
    error ERC721InsufficientApproval(address operator, uint256 tokenId);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC721InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `operator` to be approved. Used in approvals.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC721InvalidOperator(address operator);
}

/**
 * @dev Standard ERC1155 Errors
 * Interface of the https://eips.ethereum.org/EIPS/eip-6093[ERC-6093] custom errors for ERC1155 tokens.
 */
interface IERC1155Errors {
    /**
     * @dev Indicates an error related to the current `balance` of a `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     * @param balance Current balance for the interacting account.
     * @param needed Minimum amount required to perform a transfer.
     * @param tokenId Identifier number of a token.
     */
    error ERC1155InsufficientBalance(address sender, uint256 balance, uint256 needed, uint256 tokenId);

    /**
     * @dev Indicates a failure with the token `sender`. Used in transfers.
     * @param sender Address whose tokens are being transferred.
     */
    error ERC1155InvalidSender(address sender);

    /**
     * @dev Indicates a failure with the token `receiver`. Used in transfers.
     * @param receiver Address to which tokens are being transferred.
     */
    error ERC1155InvalidReceiver(address receiver);

    /**
     * @dev Indicates a failure with the `operator`’s approval. Used in transfers.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     * @param owner Address of the current owner of a token.
     */
    error ERC1155MissingApprovalForAll(address operator, address owner);

    /**
     * @dev Indicates a failure with the `approver` of a token to be approved. Used in approvals.
     * @param approver Address initiating an approval operation.
     */
    error ERC1155InvalidApprover(address approver);

    /**
     * @dev Indicates a failure with the `operator` to be approved. Used in approvals.
     * @param operator Address that may be allowed to operate on tokens without being their owner.
     */
    error ERC1155InvalidOperator(address operator);

    /**
     * @dev Indicates an array length mismatch between ids and values in a safeBatchTransferFrom operation.
     * Used in batch transfers.
     * @param idsLength Length of the array of token identifiers
     * @param valuesLength Length of the array of token amounts
     */
    error ERC1155InvalidArrayLength(uint256 idsLength, uint256 valuesLength);
}

// File @openzeppelin/contracts/token/ERC20/IERC20.sol@v5.0.1

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

// File @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.20;

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
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

// File @openzeppelin/contracts/token/ERC20/ERC20.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.20;

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 */
abstract contract ERC20 is Context, IERC20, IERC20Metadata, IERC20Errors {
    mapping(address account => uint256) private _balances;

    mapping(address account => mapping(address spender => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `value`.
     */
    function transfer(address to, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, value);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `value` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, value);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `value`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `value`.
     */
    function transferFrom(address from, address to, uint256 value) public virtual returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    }

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead.
     */
    function _transfer(address from, address to, uint256 value) internal {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(from, to, value);
    }

    /**
     * @dev Transfers a `value` amount of tokens from `from` to `to`, or alternatively mints (or burns) if `from`
     * (or `to`) is the zero address. All customizations to transfers, mints, and burns should be done by overriding
     * this function.
     *
     * Emits a {Transfer} event.
     */
    function _update(address from, address to, uint256 value) internal virtual {
        if (from == address(0)) {
            // Overflow check required: The rest of the code assumes that totalSupply never overflows
            _totalSupply += value;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < value) {
                revert ERC20InsufficientBalance(from, fromBalance, value);
            }
            unchecked {
                // Overflow not possible: value <= fromBalance <= totalSupply.
                _balances[from] = fromBalance - value;
            }
        }

        if (to == address(0)) {
            unchecked {
                // Overflow not possible: value <= totalSupply or value <= fromBalance <= totalSupply.
                _totalSupply -= value;
            }
        } else {
            unchecked {
                // Overflow not possible: balance + value is at most totalSupply, which we know fits into a uint256.
                _balances[to] += value;
            }
        }

        emit Transfer(from, to, value);
    }

    /**
     * @dev Creates a `value` amount of tokens and assigns them to `account`, by transferring it from address(0).
     * Relies on the `_update` mechanism
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead.
     */
    function _mint(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(address(0), account, value);
    }

    /**
     * @dev Destroys a `value` amount of tokens from `account`, lowering the total supply.
     * Relies on the `_update` mechanism.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * NOTE: This function is not virtual, {_update} should be overridden instead
     */
    function _burn(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        _update(account, address(0), value);
    }

    /**
     * @dev Sets `value` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     *
     * Overrides to this logic should be done to the variant with an additional `bool emitEvent` argument.
     */
    function _approve(address owner, address spender, uint256 value) internal {
        _approve(owner, spender, value, true);
    }

    /**
     * @dev Variant of {_approve} with an optional flag to enable or disable the {Approval} event.
     *
     * By default (when calling {_approve}) the flag is set to true. On the other hand, approval changes made by
     * `_spendAllowance` during the `transferFrom` operation set the flag to false. This saves gas by not emitting any
     * `Approval` event during `transferFrom` operations.
     *
     * Anyone who wishes to continue emitting `Approval` events on the`transferFrom` operation can force the flag to
     * true using the following override:
     * ```
     * function _approve(address owner, address spender, uint256 value, bool) internal virtual override {
     *     super._approve(owner, spender, value, true);
     * }
     * ```
     *
     * Requirements are the same as {_approve}.
     */
    function _approve(address owner, address spender, uint256 value, bool emitEvent) internal virtual {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[owner][spender] = value;
        if (emitEvent) {
            emit Approval(owner, spender, value);
        }
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `value`.
     *
     * Does not update the allowance value in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Does not emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 value) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(owner, spender, currentAllowance - value, false);
            }
        }
    }
}

// File @openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/extensions/ERC20Burnable.sol)

pragma solidity ^0.8.20;

/**
 * @dev Extension of {ERC20} that allows token holders to destroy both their own
 * tokens and those that they have an allowance for, in a way that can be
 * recognized off-chain (via event analysis).
 */
abstract contract ERC20Burnable is Context, ERC20 {
    /**
     * @dev Destroys a `value` amount of tokens from the caller.
     *
     * See {ERC20-_burn}.
     */
    function burn(uint256 value) public virtual {
        _burn(_msgSender(), value);
    }

    /**
     * @dev Destroys a `value` amount of tokens from `account`, deducting from
     * the caller's allowance.
     *
     * See {ERC20-_burn} and {ERC20-allowance}.
     *
     * Requirements:
     *
     * - the caller must have allowance for ``accounts``'s tokens of at least
     * `value`.
     */
    function burnFrom(address account, uint256 value) public virtual {
        _spendAllowance(account, _msgSender(), value);
        _burn(account, value);
    }
}

// File @openzeppelin/contracts/token/ERC20/extensions/IERC20Permit.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/extensions/IERC20Permit.sol)

pragma solidity ^0.8.20;

/**
 * @dev Interface of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in
 * https://eips.ethereum.org/EIPS/eip-2612[EIP-2612].
 *
 * Adds the {permit} method, which can be used to change an account's ERC20 allowance (see {IERC20-allowance}) by
 * presenting a message signed by the account. By not relying on {IERC20-approve}, the token holder account doesn't
 * need to send a transaction, and thus is not required to hold Ether at all.
 *
 * ==== Security Considerations
 *
 * There are two important considerations concerning the use of `permit`. The first is that a valid permit signature
 * expresses an allowance, and it should not be assumed to convey additional meaning. In particular, it should not be
 * considered as an intention to spend the allowance in any specific way. The second is that because permits have
 * built-in replay protection and can be submitted by anyone, they can be frontrun. A protocol that uses permits should
 * take this into consideration and allow a `permit` call to fail. Combining these two aspects, a pattern that may be
 * generally recommended is:
 *
 * ```solidity
 * function doThingWithPermit(..., uint256 value, uint256 deadline, uint8 v, bytes32 r, bytes32 s) public {
 *     try token.permit(msg.sender, address(this), value, deadline, v, r, s) {} catch {}
 *     doThing(..., value);
 * }
 *
 * function doThing(..., uint256 value) public {
 *     token.safeTransferFrom(msg.sender, address(this), value);
 *     ...
 * }
 * ```
 *
 * Observe that: 1) `msg.sender` is used as the owner, leaving no ambiguity as to the signer intent, and 2) the use of
 * `try/catch` allows the permit to fail and makes the code tolerant to frontrunning. (See also
 * {SafeERC20-safeTransferFrom}).
 *
 * Additionally, note that smart contract wallets (such as Argent or Safe) are not able to produce permit signatures, so
 * contracts should have entry points that don't rely on permit.
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
     *
     * CAUTION: See Security Considerations above.
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

// File @openzeppelin/contracts/utils/Address.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (utils/Address.sol)

pragma solidity ^0.8.20;

/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev The ETH balance of the account is not enough to perform the operation.
     */
    error AddressInsufficientBalance(address account);

    /**
     * @dev There's no code at `target` (it is not a contract).
     */
    error AddressEmptyCode(address target);

    /**
     * @dev A call to an address target failed. The target may have reverted.
     */
    error FailedInnerCall();

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
     * https://solidity.readthedocs.io/en/v0.8.20/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        if (address(this).balance < amount) {
            revert AddressInsufficientBalance(address(this));
        }

        (bool success, ) = recipient.call{value: amount}("");
        if (!success) {
            revert FailedInnerCall();
        }
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason or custom error, it is bubbled
     * up by this function (like regular Solidity function calls). However, if
     * the call reverted with no returned reason, this function reverts with a
     * {FailedInnerCall} error.
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        if (address(this).balance < value) {
            revert AddressInsufficientBalance(address(this));
        }
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and reverts if the target
     * was not a contract or bubbling up the revert reason (falling back to {FailedInnerCall}) in case of an
     * unsuccessful call.
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata
    ) internal view returns (bytes memory) {
        if (!success) {
            _revert(returndata);
        } else {
            // only check if target is a contract if the call was successful and the return data is empty
            // otherwise we already know that it was a contract
            if (returndata.length == 0 && target.code.length == 0) {
                revert AddressEmptyCode(target);
            }
            return returndata;
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and reverts if it wasn't, either by bubbling the
     * revert reason or with a default {FailedInnerCall} error.
     */
    function verifyCallResult(bool success, bytes memory returndata) internal pure returns (bytes memory) {
        if (!success) {
            _revert(returndata);
        } else {
            return returndata;
        }
    }

    /**
     * @dev Reverts with returndata if present. Otherwise reverts with {FailedInnerCall}.
     */
    function _revert(bytes memory returndata) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert FailedInnerCall();
        }
    }
}

// File @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol@v5.0.1

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/utils/SafeERC20.sol)

pragma solidity ^0.8.20;

/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using Address for address;

    /**
     * @dev An operation with an ERC20 token failed.
     */
    error SafeERC20FailedOperation(address token);

    /**
     * @dev Indicates a failed `decreaseAllowance` request.
     */
    error SafeERC20FailedDecreaseAllowance(address spender, uint256 currentAllowance, uint256 requestedDecrease);

    /**
     * @dev Transfer `value` amount of `token` from the calling contract to `to`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeCall(token.transfer, (to, value)));
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeCall(token.transferFrom, (from, to, value)));
    }

    /**
     * @dev Increase the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        forceApprove(token, spender, oldAllowance + value);
    }

    /**
     * @dev Decrease the calling contract's allowance toward `spender` by `requestedDecrease`. If `token` returns no
     * value, non-reverting calls are assumed to be successful.
     */
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 requestedDecrease) internal {
        unchecked {
            uint256 currentAllowance = token.allowance(address(this), spender);
            if (currentAllowance < requestedDecrease) {
                revert SafeERC20FailedDecreaseAllowance(spender, currentAllowance, requestedDecrease);
            }
            forceApprove(token, spender, currentAllowance - requestedDecrease);
        }
    }

    /**
     * @dev Set the calling contract's allowance toward `spender` to `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful. Meant to be used with tokens that require the approval
     * to be set to zero before setting it to a non-zero value, such as USDT.
     */
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeCall(token.approve, (spender, value));

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeCall(token.approve, (spender, 0)));
            _callOptionalReturn(token, approvalCall);
        }
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

        bytes memory returndata = address(token).functionCall(data);
        if (returndata.length != 0 && !abi.decode(returndata, (bool))) {
            revert SafeERC20FailedOperation(address(token));
        }
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
        return success && (returndata.length == 0 || abi.decode(returndata, (bool))) && address(token).code.length > 0;
    }
}

// File @uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router01.sol@v1.1.0-beta.0

pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

// File @uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router02.sol@v1.1.0-beta.0

pragma solidity >=0.6.2;

interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

// File contracts/erc20/deploy/IMetadropERC20.sol

pragma solidity 0.8.24;

interface IMetadropERC20 is IERC20, IERC20Metadata {
  error LimitCannotBeChanged();
  error LimitMustBeHigherOrUnchanged();
  error LimitTooHigh();
  error InitialLiquidityNotYetAdded();
  error TransferFailed();
  error MaxTokensPerWalletExceeded();
  error IncorrectETHValueProvided();
  error DeductionsOnBuyExceedOrEqualOneHundredPercent();
  error AutoswapThresholdTooLow();
  error TotalSupplyNotMinted();
  error RecipientsAndAmountsMismatch();
  error CannotPerformDuringAutoswap();
  error NoETHForLiquidityPair();
  error InitialLiquidityAlreadyAdded();
  error InsufficientLockupPeriod();
  error InsufficientTokensForInitialLiquidity();
  error LPTokensBalanceMismatch();
  error LiquidityPoolCannotBeAddressZero();
  error CanOnlyReduce();
  error MaxBuysPerBlockExceeded();
  error MaxTokensPerTransactionExceeded();
  error CannotWithdrawThisToken();
  error AmountExceedsAvailable();
  error PlatformFeeAlreadyDistributed();

  event AutoswapThresholdUpdated(uint256 oldThreshold, uint256 newThreshold);
  event ExternalCallError(uint256 identifier);
  event InitialLiquidityAdded(uint256 tokenA, uint256 tokenB, uint256 lpToken);

  event MaxTokensPerWalletLimitUpdated(uint256 previousLimit, uint256 newLimit);
  event MaxTokensPerTransactionLimitUpdated(
    uint256 previousLimit,
    uint256 newLimit
  );
  event LiquidityLocked(
    uint256 lpTokens,
    uint256 lpLockupInDays,
    uint256 streamId
  );
  event LiquidityBurned(uint256 lpTokens);
  event LiquidityPoolCreated(address addedPool);
  event LiquidityPoolAddressUpdated(address account, bool isLiquidityPool);
  event TaxExemptAddressUpdated(address account, bool isTaxExempt);
  event MetadropTaxBasisPointsChanged(
    uint256 oldBuyBasisPoints,
    uint256 newBuyBasisPoints,
    uint256 oldSellBasisPoints,
    uint256 newSellBasisPoints
  );
  event ProjectTaxBasisPointsChanged(
    uint256 oldBuyBasisPoints,
    uint256 newBuyBasisPoints,
    uint256 oldSellBasisPoints,
    uint256 newSellBasisPoints
  );
  event ProjectTaxRecipientUpdated(address treasury);
  event UnlimitedAddressUpdated(address account, bool isUnlimited);
  event PlatformFeeDistributed();

  struct ERC20BaseParams {
    string name;
    string symbol;
    address metadropTreasury;
    address initialOwner;
    address uniswapV2Router;
    address tokenVault;
    uint256 totalSupply;
    uint256 deploymentFee;
    address[] supplyRecipients;
    uint256[] supplyAmounts;
  }

  struct ERC20LiquidityParams {
    address lpOwner;
  }

  struct ERC20AutoburnParams {
    uint256 autoBurnDurationInBlocks;
    uint256 autoBurnBasisPoints;
  }

  struct ERC20LaunchPoolParams {
    address launchPoolFactory;
    uint256 launchPoolSupply;
  }

  struct ERC20LimitParams {
    uint256 limitProtectionDurationInSeconds;
    uint256 maxTokensPerTransaction;
    uint256 maxTokensPerWallet;
  }

  struct ERC20TaxParams {
    uint256 projectBuyTaxBasisPoints;
    uint256 projectSellTaxBasisPoints;
    uint256 autoswapThresholdBasisPoints;
    uint256 metadropBuyTaxBasisPoints;
    uint256 metadropSellTaxBasisPoints;
    uint256 metadropTaxPeriodInDays;
    address projectTaxRecipient;
    address metadropTaxRecipient;
    uint256 metadropMinBuyTaxBasisPoints;
    uint256 metadropMinSellTaxBasisPoints;
    uint256 metadropBuyTaxProportionBasisPoints;
    uint256 metadropSellTaxProportionBasisPoints;
  }

  function addInitialLiquidity(
    uint256 lpLockupInDaysOverride_,
    bool burnLPTokensOverride_
  ) external payable;
  function isLiquidityPool(address address_) external view returns (bool);
  function isUnlimited(address address_) external view returns (bool);
  function isTaxExempt(address address_) external view returns (bool);
  function setIsLiquidityPool(address address_, bool isLiquidityPool_) external;
  function setIsUnlimited(address address_, bool isUnlimited_) external;
  function setIsTaxExempt(address address_, bool isTaxExempt_) external;
  function setProjectTaxRecipient(address projectTaxRecipient_) external;
  function setAutoswapThresholdBasisPoints(
    uint256 swapThresholdBasisPoints_
  ) external;
  function setProjectTaxRates(
    uint256 newProjectBuyTaxBasisPoints_,
    uint256 newProjectSellTaxBasisPoints_
  ) external;
  function setLimits(
    uint256 newMaxTokensPerTransaction_,
    uint256 newMaxTokensPerWallet_
  ) external;
  function limitsEnforced() external view returns (bool);
  function getMetadropBuyTaxBasisPoints() external view returns (uint256);
  function getMetadropSellTaxBasisPoints() external view returns (uint256);
  function totalBuyTaxBasisPoints() external view returns (uint256);
  function totalSellTaxBasisPoints() external view returns (uint256);
  function burn(uint256 value) external;
  function burnFrom(address account, uint256 value) external;

  function lpSupply() external returns (uint256);
  function limitProtectionDurationInSeconds() external returns (uint256);
  function metadropTaxPeriodInDays() external returns (uint256);
  function metadropBuyTaxProportionBasisPoints() external returns (uint256);
  function metadropSellTaxProportionBasisPoints() external returns (uint256);
  function metadropMinBuyTaxBasisPoints() external returns (uint256);
  function metadropMinSellTaxBasisPoints() external returns (uint256);
  function autoBurnDurationInBlocks() external returns (uint256);
  function autoBurnBasisPoints() external returns (uint256);
  function metadropTaxRecipient() external returns (address);
  function uniswapV2Pair() external returns (address);
  // function launchPool() external returns (address);
  function lpOwner() external returns (address);
  function distributePlatformFee() external;
}

// File @uniswap/v2-core/contracts/interfaces/IUniswapV2Factory.sol@v1.0.1

pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

// File @uniswap/v2-periphery/contracts/interfaces/IWETH.sol@v1.1.0-beta.0

pragma solidity >=0.5.0;

interface IWETH {
    function deposit() external payable;
    function transfer(address to, uint value) external returns (bool);
    function withdraw(uint) external;
}

// File contracts/vendor/sablier/ISablierV2LockupLinear.sol

pragma solidity >=0.8.19;

type UD60x18 is uint256;

struct Broker {
  address account;
  UD60x18 fee;
}

library LockupLinear {
  struct CreateWithDurations {
    address sender;
    address recipient;
    uint128 totalAmount;
    IERC20 asset;
    bool cancelable;
    bool transferable;
    Durations durations;
    Broker broker;
  }

  /// @notice Struct encapsulating the cliff duration and the total duration.
  /// @param cliff The cliff duration in seconds.
  /// @param total The total duration in seconds.
  struct Durations {
    uint40 cliff;
    uint40 total;
  }
}

interface ISablierV2LockupLinear {
  function createWithDurations(
    LockupLinear.CreateWithDurations calldata params
  ) external returns (uint256 streamId);
}

// File contracts/erc20/deploy/MetadropERC20.sol

pragma solidity 0.8.24;

// OpenZeppelin

// Uniswap V2

// Metadrop

contract MetadropERC20 is ERC20, ERC20Burnable, IMetadropERC20, Ownable {
  using SafeERC20 for IERC20;

  bytes32 public constant x_META_ID_HASH =
    0x4B48179FF6B22ADA391290DBE08627BBB5273D4968A19DA2E5C9797C34D52DC6;

  uint256 internal constant BP_DENOM = 10_000;
  uint256 internal constant ROUND_DEC = 100_000_000_000;
  uint256 internal constant CALL_GAS_LIMIT = 50_000;
  uint256 internal constant MAX_AUTOSWAP_THRESHOLD_MULTIPLE = 20;
  uint256 internal constant MIN_AUTOSWAP_THRESHOLD_BASIS_POINTS = 1;
  uint256 internal constant MAX_AUTOSWAPS_PER_BLOCK = 1;
  uint256 internal constant MAX_BUYS_PER_ORIGIN_PER_BLOCK = 1;

  // Metadrop
  uint256 internal immutable _platformFee;
  address public immutable metadropTreasury;
  // Liquidity
  address public immutable lpOwner;
  uint256 public immutable lpSupply;
  // Limits
  uint256 public immutable limitProtectionDurationInSeconds;
  // Taxes
  // We use the immutable var {_tokenHasTax} to avoid unneccesary storage writes and reads.
  bool internal immutable _tokenHasTax;
  uint256 public immutable metadropTaxPeriodInDays;
  uint256 public immutable metadropBuyTaxProportionBasisPoints;
  uint256 public immutable metadropSellTaxProportionBasisPoints;
  uint256 public immutable metadropMinBuyTaxBasisPoints;
  uint256 public immutable metadropMinSellTaxBasisPoints;
  address public immutable metadropTaxRecipient;
  // External addresses.
  address public uniswapV2Pair;
  // address immutable public launchPool;
  address public immutable tokenVault;
  address public immutable uniswapV2Router;
  // Auto-burn config.
  uint256 public immutable autoBurnDurationInBlocks;
  uint256 public immutable autoBurnBasisPoints;

  bool public platformFeeDistributed;
  bool public burnLPTokens;
  bool private _autoSwapInProgress;
  uint256 public lpLockupInDays;
  uint256 public fundedDate;
  uint256 public fundedBlock;
  uint256 public projectBuyTaxBasisPoints;
  uint256 public projectSellTaxBasisPoints;
  uint256 public metadropBuyTaxBasisPoints;
  uint256 public metadropSellTaxBasisPoints;
  uint256 public maxTokensPerWallet;
  uint256 public maxTokensPerTransaction;
  uint256 public autoswapThresholdBasisPoints;
  address public projectTaxRecipient;
  uint256 public projectTaxPendingSwap;
  uint256 public metadropTaxPendingSwap;

  /** @dev {_autoswapForBlock} Limit autoswaps per block */
  mapping(uint256 => uint256) private _autoswapForBlock;

  /** @dev {_originBuysPerBlock} Number of buys from this txn.origin by block */
  mapping(address => mapping(uint256 => uint256)) private _originBuysPerBlock;

  // Mapping for liquidity pool addresses
  mapping(address => bool) public isLiquidityPool;

  // Mapping for addresses where limits do not apply
  mapping(address => bool) public isUnlimited;

  // Mapping for addresses where taxes do not apply
  mapping(address => bool) public isTaxExempt;

  /**
   * @dev {constructor}
   */
  constructor(
    ERC20BaseParams memory baseParams_,
    ERC20LiquidityParams memory liquidityParams_,
    ERC20AutoburnParams memory autoburnParams_,
    ERC20LimitParams memory limitParams_,
    ERC20TaxParams memory taxParams_
  )
    payable
    ERC20(baseParams_.name, baseParams_.symbol)
    Ownable(baseParams_.initialOwner)
  {
    _platformFee = baseParams_.deploymentFee;

    _checkFee(_platformFee);
    // Process base params.
    uniswapV2Router = baseParams_.uniswapV2Router;
    tokenVault = baseParams_.tokenVault;
    metadropTreasury = baseParams_.metadropTreasury;

    // Mint initial supply
    _distributeInitialSupply(
      baseParams_.totalSupply,
      baseParams_.supplyRecipients,
      baseParams_.supplyAmounts
    );

    // Process autoburn params.
    autoBurnDurationInBlocks = autoburnParams_.autoBurnDurationInBlocks;
    autoBurnBasisPoints = autoburnParams_.autoBurnBasisPoints;

    // Process tax params.
    if (
      taxParams_.projectBuyTaxBasisPoints == 0 &&
      taxParams_.projectSellTaxBasisPoints == 0 &&
      taxParams_.metadropBuyTaxBasisPoints == 0 &&
      taxParams_.metadropSellTaxBasisPoints == 0
    ) {
      _tokenHasTax = false;
    } else {
      _tokenHasTax = true;
      // Validate that the sum of all buy deductions does not equal or exceed
      // 10_000 basis points (i.e. 100%).
      if (
        (taxParams_.projectBuyTaxBasisPoints +
          taxParams_.metadropBuyTaxBasisPoints +
          autoburnParams_.autoBurnBasisPoints) >= BP_DENOM
      ) {
        revert DeductionsOnBuyExceedOrEqualOneHundredPercent();
      }

      projectBuyTaxBasisPoints = taxParams_.projectBuyTaxBasisPoints;
      projectSellTaxBasisPoints = taxParams_.projectSellTaxBasisPoints;
      metadropBuyTaxBasisPoints = taxParams_.metadropBuyTaxBasisPoints;
      metadropSellTaxBasisPoints = taxParams_.metadropSellTaxBasisPoints;

      if (
        taxParams_.autoswapThresholdBasisPoints <
        MIN_AUTOSWAP_THRESHOLD_BASIS_POINTS
      ) {
        revert AutoswapThresholdTooLow();
      }

      autoswapThresholdBasisPoints = taxParams_.autoswapThresholdBasisPoints;
      metadropTaxPeriodInDays = taxParams_.metadropTaxPeriodInDays;
      metadropTaxRecipient = taxParams_.metadropTaxRecipient;
      projectTaxRecipient = taxParams_.projectTaxRecipient;
      metadropBuyTaxProportionBasisPoints = taxParams_
        .metadropBuyTaxProportionBasisPoints;
      metadropSellTaxProportionBasisPoints = taxParams_
        .metadropSellTaxProportionBasisPoints;
      metadropMinBuyTaxBasisPoints = taxParams_.metadropMinBuyTaxBasisPoints;
      metadropMinSellTaxBasisPoints = taxParams_.metadropMinSellTaxBasisPoints;
    }

    // Process liquidity params.
    lpOwner = liquidityParams_.lpOwner;

    // Process limit params.
    limitProtectionDurationInSeconds = limitParams_
      .limitProtectionDurationInSeconds;
    maxTokensPerWallet = limitParams_.maxTokensPerWallet;
    maxTokensPerTransaction = limitParams_.maxTokensPerTransaction;

    if (
      maxTokensPerTransaction == type(uint256).max ||
      maxTokensPerWallet == type(uint256).max
    ) {
      revert LimitTooHigh();
    }
    // Add some addresses to the unlimited set.
    isUnlimited[address(this)] = true;
    isUnlimited[address(0)] = true;
    isUnlimited[uniswapV2Router] = true;
    isUnlimited[tokenVault] = true;

    // At this point all tokens have been minted and distributed. The total supply should equal the expected total supply.
    if (totalSupply() != baseParams_.totalSupply) {
      revert TotalSupplyNotMinted();
    }
  }

  /**
   * @dev function {_checkFee}
   *
   * Verify that user paid exactly the expected fee
   *
   * @param platformFee_ The fee to be paid to the platform
   *
   */
  function _checkFee(uint256 platformFee_) internal view virtual {
    if (msg.value != platformFee_) {
      revert IncorrectETHValueProvided();
    }
  }
  /**
   * @dev function {_distributeInitialSupply}
   *
   * Mints and set unlimited to the initial mint recipients
   *
   * @param totalSupply_ The total supply
   * @param recipients_ The distribution addresses
   * @param amounts_ The distribution amounts
   *
   */
  function _distributeInitialSupply(
    uint256 totalSupply_,
    address[] memory recipients_,
    uint256[] memory amounts_
  ) internal returns (uint256 distributedSupply) {
    if (recipients_.length != amounts_.length) {
      revert RecipientsAndAmountsMismatch();
    }
    distributedSupply = 0;
    for (uint256 i = 0; i < recipients_.length; i++) {
      isUnlimited[recipients_[i]] = true;
      _mint(recipients_[i], amounts_[i]);
      distributedSupply += amounts_[i];
    }
    // mint the rest of the supply to this contract so that it can be used
    // to fund the lp in addInitialLiquidity
    // save gas by not checking that we're distributing less than the total supply
    // as this will revert on underflow anyways and we're in the constructor
    _mint(address(this), (totalSupply_ - distributedSupply));
    return distributedSupply;
  }

  /**
   * @dev {notDuringAutoswap}
   *
   * Throws if called during an autoswap
   */
  modifier notDuringAutoswap() {
    if (_autoSwapInProgress) {
      revert CannotPerformDuringAutoswap();
    }
    _;
  }

  /**
   * @dev function {addInitialLiquidity}
   *
   * Add initial liquidity to the uniswap pair
   *
   * @param lpLockupInDays_ The number of days to lock liquidity.
   * @param burnLPTokens_ If the LP tokens should be burned (otherwise they are locked).
   */
  function addInitialLiquidity(
    uint256 lpLockupInDays_,
    bool burnLPTokens_
  ) public payable virtual onlyOwner {
    if (msg.value == 0) {
      revert NoETHForLiquidityPair();
    }

    // Allow the user to override whether to burn LP tokens.
    burnLPTokens = burnLPTokens_;

    if (!burnLPTokens_ && lpLockupInDays_ < 30) {
      revert InsufficientLockupPeriod();
    }
    // Allow the user to set the lockup period.
    lpLockupInDays = lpLockupInDays_;

    _addInitialLiquidity(msg.value);
  }

  /**
   * @dev function {_addInitialLiquidity}
   *
   * Add initial liquidity to the uniswap pair (internal function that does processing)
   *
   * @param ethAmount_ The amount of ETH passed into the call
   *
   */
  function _addInitialLiquidity(uint256 ethAmount_) internal {
    // Funded date is the date of first funding. We can only add initial liquidity once. If this date is set, we cannot proceed.
    if (fundedDate != 0) {
      revert InitialLiquidityAlreadyAdded();
    }

    fundedDate = block.timestamp;
    fundedBlock = block.number;

    // Approve the Uniswap V2 router for an inifinite amount (max uint256).
    // This means that we don't need to worry about later incrememtal approvals on tax swaps, as the uniswap router allowance will never be decreased.
    _approve(address(this), uniswapV2Router, type(uint256).max);

    // Add the liquidity.
    (uint256 amountA, uint256 amountB, uint256 lpTokens) = IUniswapV2Router02(
      uniswapV2Router
    ).addLiquidityETH{value: ethAmount_}(
      address(this),
      balanceOf(address(this)),
      0,
      0,
      address(this),
      block.timestamp
    );

    uniswapV2Pair = IUniswapV2Factory(
      IUniswapV2Router02(uniswapV2Router).factory()
    ).getPair(address(this), IUniswapV2Router02(uniswapV2Router).WETH());

    // Add the Uniswap V2 pair to the unlimited set.
    isUnlimited[uniswapV2Pair] = true;

    // Add the Uniswap V2 pair to the liquidity pools set.
    isLiquidityPool[uniswapV2Pair] = true;

    _approve(address(this), uniswapV2Router, type(uint256).max);

    emit InitialLiquidityAdded(amountA, amountB, lpTokens);

    // The LP token amount to lock should be equal to the LP token balance in the contract. If it's not, something is wrong.
    if (lpTokens != IERC20(uniswapV2Pair).balanceOf(address(this))) {
      revert LPTokensBalanceMismatch();
    }

    // Are we locking, or burning?
    if (burnLPTokens) {
      _burnLiquidity(lpTokens);
    } else {
      _lockLiquidity(lpTokens);
    }
  }

  /**
   * @dev function {_lockLiquidity}
   *
   * Lock initial liquidity on vault contract
   *
   * @param lpTokens_ The amount of LP tokens to be locked
   */
  function _lockLiquidity(uint256 lpTokens_) internal {
    // Approve the vault contract to spend the LP tokens.
    IERC20(uniswapV2Pair).approve(tokenVault, lpTokens_);

    uint256 streamId = _createWithDurations(
      lpOwner,
      uniswapV2Pair,
      tokenVault,
      lpTokens_,
      uint40(lpLockupInDays * 1 days)
    );

    emit LiquidityLocked(lpTokens_, lpLockupInDays * 1 days, streamId);
  }

  /**
   * @dev function {_burnLiquidity}
   *
   * Burn LP tokens
   *
   * @param lpTokens_ The amount of LP tokens to be locked
   */
  function _burnLiquidity(uint256 lpTokens_) internal {
    IERC20(uniswapV2Pair).transfer(address(0), lpTokens_);
    emit LiquidityBurned(lpTokens_);
  }

  function setIsLiquidityPool(
    address address_,
    bool isLiquidityPool_
  ) public onlyOwner {
    // Don't allow calls that didn't pass an address.
    if (address_ == address(0)) {
      revert LiquidityPoolCannotBeAddressZero();
    }

    isLiquidityPool[address_] = isLiquidityPool_;
    emit LiquidityPoolAddressUpdated(address_, isLiquidityPool_);
  }

  function setIsUnlimited(
    address address_,
    bool isUnlimited_
  ) external onlyOwner {
    isUnlimited[address_] = isUnlimited_;
    emit UnlimitedAddressUpdated(address_, isUnlimited_);
  }

  function setIsTaxExempt(
    address address_,
    bool isTaxExempt_
  ) external onlyOwner {
    isTaxExempt[address_] = isTaxExempt_;
    emit TaxExemptAddressUpdated(address_, isTaxExempt_);
  }

  /**
   * @dev function {setProjectTaxRecipient} onlyOwner
   *
   * Allows the manager to set the project tax recipient address
   *
   * @param projectTaxRecipient_ New recipient address
   */
  function setProjectTaxRecipient(
    address projectTaxRecipient_
  ) external onlyOwner {
    projectTaxRecipient = projectTaxRecipient_;
    emit ProjectTaxRecipientUpdated(projectTaxRecipient_);
  }

  /**
   * @dev function {setAutoswapThresholdBasisPoints} onlyOwner
   *
   * Allows the manager to set the autoswap threshold
   *
   * @param autoswapThresholdBasisPoints_ New swap threshold in basis points
   */
  function setAutoswapThresholdBasisPoints(
    uint256 autoswapThresholdBasisPoints_
  ) external onlyOwner {
    if (autoswapThresholdBasisPoints < MIN_AUTOSWAP_THRESHOLD_BASIS_POINTS) {
      revert AutoswapThresholdTooLow();
    }
    uint256 oldAutoswapThresholdBasisPoints = autoswapThresholdBasisPoints;
    autoswapThresholdBasisPoints = autoswapThresholdBasisPoints_;
    emit AutoswapThresholdUpdated(
      oldAutoswapThresholdBasisPoints,
      autoswapThresholdBasisPoints_
    );
  }

  /**
   * @dev function {setProjectTaxRates} onlyOwner
   *
   * Change the tax rates, subject to only ever decreasing
   *
   * @param newProjectBuyTaxBasisPoints_ The new buy tax rate
   * @param newProjectSellTaxBasisPoints_ The new sell tax rate
   */
  function setProjectTaxRates(
    uint256 newProjectBuyTaxBasisPoints_,
    uint256 newProjectSellTaxBasisPoints_
  ) external onlyOwner {
    uint256 oldBuyTaxBasisPoints = projectBuyTaxBasisPoints;
    uint256 oldSellTaxBasisPoints = projectSellTaxBasisPoints;
    // Cannot increase, down only
    if (newProjectBuyTaxBasisPoints_ > oldBuyTaxBasisPoints) {
      revert CanOnlyReduce();
    }
    // Cannot increase, down only
    if (newProjectSellTaxBasisPoints_ > oldSellTaxBasisPoints) {
      revert CanOnlyReduce();
    }
    projectBuyTaxBasisPoints = newProjectBuyTaxBasisPoints_;
    projectSellTaxBasisPoints = newProjectSellTaxBasisPoints_;

    // We set the metadrop tax rates off of the project tax rates:
    //
    // 1) If the project tax rate is zero then the metadrop tax rate is zero
    // 2) If the project tax rate is not zero the metadrop tax rate is the
    //    greater of:
    //    a) The metadrop tax proportion basis points of the project rate
    //    b) the base metadrop tax rate.
    //
    // Examples:
    //
    // A) The project buy tax rate is zero and the sell tax rate is 3%. The metadrop
    // tax proportion basis points is 1000, meaning the metadrop proportion is 10% of the
    // project tax rate. The base metadrop tax rate is 50 basis points i.e. 0.5%.
    //
    // * Metadrop buy tax = 0% (as the project buy tax is zero)
    // * Metadrop sell tax = 0.5%. 10% of the project sell tax is 0.3%. As this is below
    // the base level of 0.5% we set the metadrop tax to 0.5%
    //
    // B) The project buy tax rate is 4% and the sell tax rate is 20%. The metadrop tax
    // proportion basis points is 1000, meaning the metadrop proportion is 10% of the
    // project tax rate. The base metadrop tax rate is 50 basis points i.e. 0.5%.
    //
    // * Metadrop buy tax = 0.5%. 10% of the project rate would be 0.4%, so we use the base rate)
    // * Metadrop sell tax = 2%. 10% of the project rate is 2%, which is higher than the
    //   base rate of 0.5%.

    uint256 oldMetadropBuyTaxBasisPoints = metadropBuyTaxBasisPoints;
    uint256 oldMetadropSellTaxBasisPoints = metadropSellTaxBasisPoints;

    // Process the buy tax rate first:
    if (newProjectBuyTaxBasisPoints_ == 0) {
      metadropBuyTaxBasisPoints = 0;
    } else {
      uint256 derivedMetadropBuyTaxRate = (newProjectBuyTaxBasisPoints_ *
        metadropBuyTaxProportionBasisPoints) / BP_DENOM;
      if (derivedMetadropBuyTaxRate < metadropMinBuyTaxBasisPoints) {
        metadropBuyTaxBasisPoints = metadropMinBuyTaxBasisPoints;
      } else {
        metadropBuyTaxBasisPoints = derivedMetadropBuyTaxRate;
      }
    }

    // And now the sell tax rate:
    if (newProjectSellTaxBasisPoints_ == 0) {
      metadropSellTaxBasisPoints = 0;
    } else {
      uint256 derivedMetadropSellTaxRate = (newProjectSellTaxBasisPoints_ *
        metadropSellTaxProportionBasisPoints) / BP_DENOM;
      if (derivedMetadropSellTaxRate < metadropMinSellTaxBasisPoints) {
        metadropSellTaxBasisPoints = metadropMinSellTaxBasisPoints;
      } else {
        metadropSellTaxBasisPoints = derivedMetadropSellTaxRate;
      }
    }

    // Emit a message if there has been a change:
    if (
      oldMetadropBuyTaxBasisPoints != metadropBuyTaxBasisPoints ||
      oldMetadropSellTaxBasisPoints != metadropSellTaxBasisPoints
    ) {
      emit MetadropTaxBasisPointsChanged(
        oldMetadropBuyTaxBasisPoints,
        metadropBuyTaxBasisPoints,
        oldMetadropSellTaxBasisPoints,
        metadropSellTaxBasisPoints
      );
    }

    emit ProjectTaxBasisPointsChanged(
      oldBuyTaxBasisPoints,
      newProjectBuyTaxBasisPoints_,
      oldSellTaxBasisPoints,
      newProjectSellTaxBasisPoints_
    );
  }

  /**
   * @dev function {setLimits} onlyOwner
   *
   * Change the limits on transactions and holdings
   *
   * @param newMaxTokensPerTransaction_ The new per txn limit
   * @param newMaxTokensPerWallet_ The new tokens per wallet limit
   */
  function setLimits(
    uint256 newMaxTokensPerTransaction_,
    uint256 newMaxTokensPerWallet_
  ) external onlyOwner {
    if (maxTokensPerTransaction == 0 && newMaxTokensPerTransaction_ != 0) {
      revert LimitCannotBeChanged();
    }

    if (maxTokensPerWallet == 0 && newMaxTokensPerWallet_ != 0) {
      revert LimitCannotBeChanged();
    }

    if (
      newMaxTokensPerTransaction_ != 0 &&
      newMaxTokensPerTransaction_ < maxTokensPerTransaction
    ) {
      revert LimitMustBeHigherOrUnchanged();
    }

    if (
      newMaxTokensPerWallet_ != 0 && newMaxTokensPerWallet_ < maxTokensPerWallet
    ) {
      revert LimitMustBeHigherOrUnchanged();
    }

    if (
      newMaxTokensPerTransaction_ == type(uint256).max ||
      newMaxTokensPerWallet_ == type(uint256).max
    ) {
      revert LimitTooHigh();
    }

    if (newMaxTokensPerTransaction_ != maxTokensPerTransaction) {
      emit MaxTokensPerTransactionLimitUpdated(
        maxTokensPerTransaction,
        newMaxTokensPerTransaction_
      );
      maxTokensPerTransaction = newMaxTokensPerTransaction_;
    }

    if (newMaxTokensPerWallet_ != maxTokensPerWallet) {
      emit MaxTokensPerWalletLimitUpdated(
        maxTokensPerWallet,
        newMaxTokensPerWallet_
      );
      maxTokensPerWallet = newMaxTokensPerWallet_;
    }
  }

  /**
   * @dev function {limitsEnforced}
   *
   * Return if limits are enforced on this contract
   *
   * @return bool : they are / aren't
   */
  function limitsEnforced() public view returns (bool) {
    // Limits are not enforced if:
    // the contract is renounced AND after the protection end date
    // OR prior to LP funding:
    // The second clause of !initialLiquidityAdded() isn't strictly needed, since with a funded
    // date of 0 we would always expect the block.timestamp to be less than 0 plus
    // the limitProtectionDurationInSeconds. But, to cover the miniscule chance of a user
    // selecting a truly enormous limit protection period, such that when added to 0 it
    // is more than the current block.timestamp, we have included this second clause. There
    // is no permanent gas overhead (the logic will be returning from the first clause after
    // the limit protection period has expired). During the limit protection period there is a minor
    // gas overhead from evaluating the initialLiquidityAdded() (which will be true), but this is minimal.
    if (
      (owner() == address(0) &&
        block.timestamp > fundedDate + limitProtectionDurationInSeconds) ||
      !initialLiquidityAdded()
    ) {
      return false;
    } else {
      // LP has been funded AND we are within the protection period:
      return true;
    }
  }

  /**
   * @dev getMetadropBuyTaxBasisPoints
   *
   * Return the metadrop buy tax basis points given the timed expiry.
   */
  function getMetadropBuyTaxBasisPoints() public view returns (uint256) {
    // If we are outside the metadrop tax period this is ZERO
    if (_isOutsideMetadropTaxPeriod()) {
      return 0;
    } else {
      return metadropBuyTaxBasisPoints;
    }
  }

  /**
   * @dev getMetadropSellTaxBasisPoints
   *
   * Return the metadrop sell tax basis points given the timed expiry
   */
  function getMetadropSellTaxBasisPoints() public view returns (uint256) {
    // If we are outside the metadrop tax period this is ZERO
    if (_isOutsideMetadropTaxPeriod()) {
      return 0;
    } else {
      return metadropSellTaxBasisPoints;
    }
  }

  function _isOutsideMetadropTaxPeriod() internal view returns (bool) {
    return block.timestamp > (fundedDate + (metadropTaxPeriodInDays * 1 days));
  }

  /**
   * @dev totalBuyTaxBasisPoints
   *
   * Provide easy to view tax total:
   */
  function totalBuyTaxBasisPoints() public view returns (uint256) {
    return projectBuyTaxBasisPoints + getMetadropBuyTaxBasisPoints();
  }

  /**
   * @dev totalSellTaxBasisPoints
   *
   * Provide easy to view tax total:
   */
  function totalSellTaxBasisPoints() public view returns (uint256) {
    return projectSellTaxBasisPoints + getMetadropSellTaxBasisPoints();
  }

  function _update(
    address from_,
    address to_,
    uint256 amount_
  ) internal virtual override {
    // This amount will be updated as we go through the process. It starts as the amount passed in, and is reduced as we apply tax, autoburn, etc.
    uint256 amountMinusDeductions = amount_;

    // Limits, autoburn, taxes, and autoswap are only applied after the initial liquidity has been added.
    if (!initialLiquidityAdded()) {
      // Prior to the initial liquidity being added, we need to prevent all transfers to liquidity pools, UNLESS the `from` address is this contract.
      // This ensures that the initial LP funding transaction is from this contract using the supply of tokens designated for the LP pool, and therefore the initial price in the pool is being set as expected.
      // This protects from, for example, tokens from a team minted supply being paired with ETH and added to the pool, setting the initial price, BEFORE the initial liquidity is added through this contract.
      if (to_ == _getPairAddress() && from_ != address(this)) {
        revert InitialLiquidityNotYetAdded();
      }
    } else {
      // At this point, initial liquidity has been added.
      // The initial buy must be unlimited, tax-free, etc.
      // Limits, autoburn, taxes, autoswap, etc can't apply until the launch pool has completed.

      // Before calculating tax and autoburn, we need to check the max tokens per transaction limit.
      _checkMaxTokensPerTxLimit(from_, to_, amount_);

      // Perform autoswap if eligible.
      _autoSwap(from_, to_);

      // Limit the number of buys per tx.origin per block as an anti-limit measure.
      _limitBuysPerBlock(from_);

      // Determine the amount to charge as a tax.
      uint256 taxAmount = _calculateTax(to_, from_, amount_);

      // If the tax amount is greater than zero, we need to perform the tax transfer.
      if (taxAmount > 0) {
        // Deduct the tax from_ the amount from_ the running total.
        amountMinusDeductions -= taxAmount;
        // Perform the tax transfer.
        super._update(from_, address(this), taxAmount);
      }

      // Process autoburn.
      uint256 autoburnAmount = _calculateAutoburn(from_, to_, amount_);

      // If the autoburn amount is greater than zero, we need to perform the autoburn.
      if (autoburnAmount > 0) {
        // Deduct the autoburn from_ the amount from_ the running total.
        amountMinusDeductions -= autoburnAmount;
        // Perform the autoburn.
        super._update(from_, address(0), autoburnAmount);
      }

      // After all deductions, we need to check the max tokens per wallet limit of the recipient.
      _checkMaxTokensPerWalletLimit(from_, to_, amountMinusDeductions);
    }

    super._update(from_, to_, amountMinusDeductions);
  }

  function _getPairAddress() private view returns (address pair) {
    address weth = IUniswapV2Router02(uniswapV2Router).WETH();
    (address token0, address token1) = address(this) < weth
      ? (address(this), weth)
      : (weth, address(this));
    pair = address(
      uint160(
        uint(
          keccak256(
            abi.encodePacked(
              hex"ff",
              IUniswapV2Router02(uniswapV2Router).factory(),
              keccak256(abi.encodePacked(token0, token1)),
              hex"96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f" // init code hash
            )
          )
        )
      )
    );
  }

  /**
   * @dev function {_blockMaxBuysPerOriginExceeded}
   *
   * Check if the max buy per origin per block been exceeded. If it has, revert. Otherwise, increment the count.
   */
  function _limitBuysPerBlock(address from) internal virtual {
    if (isLiquidityPool[from]) {
      if (
        _originBuysPerBlock[tx.origin][block.number] >=
        MAX_BUYS_PER_ORIGIN_PER_BLOCK
      ) {
        revert MaxBuysPerBlockExceeded();
      }
      _originBuysPerBlock[tx.origin][block.number]++;
    }
  }

  function initialLiquidityAdded() public view returns (bool) {
    return fundedDate != 0;
  }

  /**
   * @dev function {_checkMaxTokensPerTxLimit}
   *
   * Check the max tokens per transaction limit on buys from liquidity pools.
   *
   * @param from_ From address for the transaction
   * @param to_ To address for the transaction
   * @param amount_ Amount of the transaction
   */
  function _checkMaxTokensPerTxLimit(
    address from_,
    address to_,
    uint256 amount_
  ) internal view {
    if (
      limitsEnforced() &&
      maxTokensPerTransaction != 0 &&
      ((isLiquidityPool[from_] && !isUnlimited[to_]) ||
        (isLiquidityPool[to_] && !isUnlimited[from_]))
    ) {
      if (amount_ > _getBufferedLimit(maxTokensPerTransaction)) {
        revert MaxTokensPerTransactionExceeded();
      }
    }
  }

  /**
   * @dev function {_checkMaxTokensPerWalletLimit}
   *
   * Check the max tokens per wallet limit on buys from liquidity pools.
   *
   * @param from_ From address for the transaction
   * @param to_ To address for the transaction
   * @param amount_ Amount of the transaction
   */
  function _checkMaxTokensPerWalletLimit(
    address from_,
    address to_,
    uint256 amount_
  ) internal view {
    if (
      limitsEnforced() &&
      maxTokensPerWallet != 0 &&
      !isUnlimited[to_] &&
      isLiquidityPool[from_]
    ) {
      if ((amount_ + balanceOf(to_) > _getBufferedLimit(maxTokensPerWallet))) {
        revert MaxTokensPerWalletExceeded();
      }
    }
  }

  /**
  * Liquidity pools aren't always going to round cleanly. This can (and does)
  mean that a limit of 5,000 tokens (for example) will trigger on a max holding
  of 5,000 tokens, as the transfer to achieve that is actually for
  5,000.00000000000000213.
  * While 4,999 will work fine, it isn't hugely user friendly.
  * So we buffer the limit with rounding decimals, which in all cases are considerably
  less than one whole token:
  */
  function _getBufferedLimit(uint256 limit_) internal pure returns (uint256) {
    return limit_ + ROUND_DEC;
  }

  /**
   * @dev function {_calculateTax}
   *
   * Perform tax processing
   *
   * @param to_ The reciever of the token
   * @param from_ The sender of the token
   * @param sentAmount_ The amount being transferred from which to take the tax.
   * @return totalTax The amount that should be transferred as a tax.
   */
  function _calculateTax(
    address to_,
    address from_,
    uint256 sentAmount_
  ) internal returns (uint256 totalTax) {
    if (_tokenHasTax && !_autoSwapInProgress) {
      // On sells, if the seller is not tax exempt and there is a sell tax, we apply the tax.
      if (
        isLiquidityPool[to_] &&
        totalSellTaxBasisPoints() > 0 &&
        !isTaxExempt[from_]
      ) {
        if (projectSellTaxBasisPoints > 0) {
          uint256 projectTax = ((sentAmount_ * projectSellTaxBasisPoints) /
            BP_DENOM);
          projectTaxPendingSwap += projectTax;
          totalTax += projectTax;
        }
        uint256 metadropSellTax = getMetadropSellTaxBasisPoints();
        if (metadropSellTax > 0) {
          uint256 metadropTax = ((sentAmount_ * metadropSellTax) / BP_DENOM);
          metadropTaxPendingSwap += metadropTax;
          totalTax += metadropTax;
        }
      }
      // On buys, if the buyer is not tax exempt and there is a buy tax, we apply the tax.
      else if (
        isLiquidityPool[from_] &&
        totalBuyTaxBasisPoints() > 0 &&
        !isTaxExempt[to_]
      ) {
        if (projectBuyTaxBasisPoints > 0) {
          uint256 projectTax = ((sentAmount_ * projectBuyTaxBasisPoints) /
            BP_DENOM);
          projectTaxPendingSwap += projectTax;
          totalTax += projectTax;
        }
        uint256 metadropBuyTax = getMetadropBuyTaxBasisPoints();
        if (metadropBuyTax > 0) {
          uint256 metadropTax = ((sentAmount_ * metadropBuyTax) / BP_DENOM);
          metadropTaxPendingSwap += metadropTax;
          totalTax += metadropTax;
        }
      }
    }
  }

  /**
   * @dev function {_calculateAutoburn}
   *
   * Perform autoburn processing
   *
   * @param from_ The sender of the token
   * @param to_ The recipient of the token
   * @param originalSentAmount_ The original amount being sent, before any deductions (if appropriate)
   * @return amountToBurn The amount that should be burned.
   */
  function _calculateAutoburn(
    address from_,
    address to_,
    uint256 originalSentAmount_
  ) internal view virtual returns (uint256 amountToBurn) {
    // Perform autoburn processing, if appropriate:
    if (
      autoBurnDurationInBlocks != 0 &&
      autoBurnBasisPoints != 0 &&
      !_autoSwapInProgress &&
      isLiquidityPool[from_] &&
      // we leave out tax exempt addresses cause if they're bots
      // they're probably part of the launch mechanism
      !isTaxExempt[to_]
    ) {
      uint256 blocksElapsed = block.number - fundedBlock;
      if (blocksElapsed < autoBurnDurationInBlocks) {
        // Get the blocks remaining in the autoburn period. The more blocks
        // remaining, the higher the proportion of the burn we apply:
        uint256 burnBlocksRemaining = autoBurnDurationInBlocks - blocksElapsed;
        // Calculate the linear burn basis point per remaining block. For example, if our
        // burn basis points = 1500 (15%) and we are burning for three blocks then this
        // will be 1500 / 3 = 500 (5%):
        uint256 linearBurnPerRemainingBlock = autoBurnBasisPoints /
          autoBurnDurationInBlocks;
        // Finally, determine the burn basis points for this block by multiplying the per remaining
        // block burn % by the number of blocks remaining. To follow our example, in the 0th
        // block since funding there are three blocks remaining in the burn period, therefore
        // 500 * 3 = 1500 (15%). Two blocks after funding we have one block remaining in the burn
        // period, and therefore are burning 500 * 1 = 500 (5%). Three blocks after funding we do not
        // reach this point in the logic, as the blocksElapsed is 3 and needs to be UNDER 3 to enter
        // this code.
        uint256 burnBasisPointsForThisBlock = burnBlocksRemaining *
          linearBurnPerRemainingBlock;

        // This is eligible for burn. Send the basis points amount of
        // the originalSentAmount_ to the zero address:
        amountToBurn = ((originalSentAmount_ * burnBasisPointsForThisBlock) /
          BP_DENOM);
      }
    }
    return amountToBurn;
  }

  /**
   * @dev totalTaxPendingSwap
   *
   * Return the total tax awaiting swap:
   */
  function totalTaxPendingSwap() public view returns (uint256) {
    return projectTaxPendingSwap + metadropTaxPendingSwap;
  }

  /**
   * @dev function {_autoSwap}
   *
   * Automate the swap of accumulated tax fees to native token
   *
   * @param from_ The sender of the token
   * @param to_ The recipient of the token
   */
  function _autoSwap(address from_, address to_) internal {
    if (_tokenHasTax) {
      uint256 totalTaxBalance = totalTaxPendingSwap();
      uint256 swapBalance = totalTaxBalance;

      uint256 swapThresholdInTokens = (totalSupply() *
        autoswapThresholdBasisPoints) / BP_DENOM;

      if (
        _eligibleForAutoswap(from_, to_, swapBalance, swapThresholdInTokens)
      ) {
        // Store that a swap back is in progress:
        _autoSwapInProgress = true;
        // Increment the swaps per block counter:
        _autoswapForBlock[block.number] += 1;
        // Check if we need to reduce the amount of tokens for this swap:
        if (
          swapBalance > swapThresholdInTokens * MAX_AUTOSWAP_THRESHOLD_MULTIPLE
        ) {
          swapBalance = swapThresholdInTokens * MAX_AUTOSWAP_THRESHOLD_MULTIPLE;
        }
        // Perform the auto swap to native token.
        _swapTaxForNative(swapBalance, totalTaxBalance);

        // Flag that the autoswap is complete.
        _autoSwapInProgress = false;
      }
    }
  }

  /**
   * @dev function {_swapTaxForNative}
   *
   * Swap tokens taken as tax for native token
   *
   * @param swapBalance_ The current accumulated tax balance to swap
   * @param totalTaxBalance_ The current accumulated total tax balance
   */
  function _swapTaxForNative(
    uint256 swapBalance_,
    uint256 totalTaxBalance_
  ) internal {
    uint256 preSwapBalance = address(this).balance;

    address[] memory path = new address[](2);
    path[0] = address(this);
    path[1] = IUniswapV2Router02(uniswapV2Router).WETH();

    // Wrap external calls in try / catch to handle errors
    try
      IUniswapV2Router02(uniswapV2Router)
        .swapExactTokensForETHSupportingFeeOnTransferTokens(
          swapBalance_,
          0,
          path,
          address(this),
          block.timestamp + 600
        )
    {
      uint256 postSwapBalance = address(this).balance;

      uint256 balanceToDistribute = postSwapBalance - preSwapBalance;

      uint256 projectBalanceToDistribute = (balanceToDistribute *
        projectTaxPendingSwap) / totalTaxBalance_;

      uint256 metadropBalanceToDistribute = (balanceToDistribute *
        metadropTaxPendingSwap) / totalTaxBalance_;

      // We will not have swapped all tax tokens IF the amount was greater than the max auto swap.
      // We therefore cannot just set the pending swap counters to 0. Instead, in this scenario,
      // we must reduce them in proportion to the swap amount vs the remaining balance + swap
      // amount.
      //
      // For example:
      //  * swap Balance is 250
      //  * contract balance is 385.
      //  * projectTaxPendingSwap is 300
      //  * metadropTaxPendingSwap is 85.
      //
      // The new total for the projectTaxPendingSwap is:
      //   = 300 - ((300 * 250) / 385)
      //   = 300 - 194
      //   = 106
      // The new total for the metadropTaxPendingSwap is:
      //   = 85 - ((85 * 250) / 385)
      //   = 85 - 55
      //   = 30
      //

      if (swapBalance_ < totalTaxBalance_) {
        // Calculate the project tax spending swap reduction amount:
        uint256 projectTaxPendingSwapReduction = (projectTaxPendingSwap *
          swapBalance_) / totalTaxBalance_;
        projectTaxPendingSwap -= projectTaxPendingSwapReduction;

        // The metadrop tax pending swap reduction is therefore the total swap amount minus the
        // project tax spending swap reduction:
        metadropTaxPendingSwap -= swapBalance_ - projectTaxPendingSwapReduction;
      } else {
        (projectTaxPendingSwap, metadropTaxPendingSwap) = (0, 0);
      }

      // Distribute tax proceeds to the project recipient.
      if (projectBalanceToDistribute > 0) {
        _processTaxProceedsPayment(
          projectTaxRecipient,
          projectBalanceToDistribute,
          false
        );
      }

      // Distribute tax proceeds to Metadrop.
      if (metadropBalanceToDistribute > 0) {
        _processTaxProceedsPayment(
          metadropTaxRecipient,
          metadropBalanceToDistribute,
          true
        );
      }
    } catch {
      // Dont allow a failed external call (in this case to uniswap) to stop a transfer.
      // Emit that this has occured and continue.
      emit ExternalCallError(5);
    }
  }

  function _processTaxProceedsPayment(
    address recipient_,
    uint256 amount_,
    bool isMetadrop_
  ) internal {
    IWETH weth = IWETH(IUniswapV2Router02(uniswapV2Router).WETH());
    // If no gas limit was provided or provided gas limit greater than gas left, just use the remaining gas.
    uint256 gas = (CALL_GAS_LIMIT == 0 || CALL_GAS_LIMIT > gasleft())
      ? gasleft()
      : CALL_GAS_LIMIT;

    (bool success, ) = recipient_.call{value: amount_, gas: gas}("");

    // If the ETH transfer fails, wrap the ETH and send it as WETH. We do this so that a called
    // address cannot cause this transfer to fail, either intentionally or by mistake:
    if (!success) {
      try IWETH(weth).deposit{value: amount_}() {
        try IERC20(address(weth)).transfer(recipient_, amount_) {} catch {
          // Dont allow a failed external call (in this case to WETH) to stop a transfer.
          // Emit that this has occured and continue.
          emit ExternalCallError(isMetadrop_ ? 3 : 1);
        }
      } catch {
        // Dont allow a failed external call (in this case to WETH) to stop a transfer.
        // Emit that this has occured and continue.
        emit ExternalCallError(isMetadrop_ ? 4 : 2);
      }
    }
  }

  /**
   * @dev function {_eligibleForAutoswap}
   *
   * Is the current transfer eligible for autoswap
   *
   * @param from_ The sender of the token
   * @param to_ The recipient of the token
   * @param taxBalance_ The current accumulated tax balance
   * @param autoswapThresholdInTokens_ The swap threshold as a token amount
   */
  function _eligibleForAutoswap(
    address from_,
    address to_,
    uint256 taxBalance_,
    uint256 autoswapThresholdInTokens_
  ) internal view returns (bool) {
    return (// Ensure the tax balance is above the threshold.
    taxBalance_ >= autoswapThresholdInTokens_ &&
      // Ensure we are not in the middle of an autoswap.
      !_autoSwapInProgress &&
      // Ensure this is not a buy from a liquidity pool. It however can be a sell, or just a simple transfer.
      !isLiquidityPool[from_] &&
      // Ensure the from_ and to_ addresses are not the uniswap router.
      from_ != uniswapV2Router &&
      to_ != uniswapV2Router &&
      // Ensure we haven't already performed the maximum number of autoswaps for this block.
      _autoswapForBlock[block.number] < MAX_AUTOSWAPS_PER_BLOCK);
  }

  function burn(
    uint256 amount_
  ) public virtual override(ERC20Burnable, IMetadropERC20) {
    super.burn(amount_);
  }

  function burnFrom(
    address from_,
    uint256 amount_
  ) public virtual override(ERC20Burnable, IMetadropERC20) {
    super.burnFrom(from_, amount_);
  }

  function _createWithDurations(
    address lpOwner_,
    address uniswapV2PairAddress_,
    address tokenVault_,
    uint256 lpTokenAmount_,
    uint40 lpTokenLockupDuration_
  ) private returns (uint256 streamId) {
    // Setup lock Params.
    LockupLinear.CreateWithDurations memory params;
    // The sender will be able to cancel the stream
    params.sender = msg.sender;
    // The recipient of the streamed assets
    params.recipient = lpOwner_;
    // Total amount is the amount inclusive of all fees.
    params.totalAmount = uint128(lpTokenAmount_);
    // The streaming asset. Sablier requires casting as IERC20 from OZ v4.
    params.asset = IERC20(uniswapV2PairAddress_);
    // Whether the stream will be cancelable or not.
    params.cancelable = false;
    params.durations = LockupLinear.Durations({
      // Cliff duration (must be less than total duration or will revert).
      cliff: lpTokenLockupDuration_ - 1,
      // Total duration.
      total: lpTokenLockupDuration_
    });
    // Optional parameter for charging a variable fee.
    // params.broker = Broker(address(0), ud(0));

    // Create the LockupLinear stream using a function that sets the start time to `block.timestamp`
    streamId = ISablierV2LockupLinear(tokenVault_).createWithDurations(params);
  }
  /**
   * Anyone can call this.
   * For tokens that aren't using a launch pool, it can be called by any address at any time.
   * For tokens with a launch pool, it will be called automatically during contract creation.
   */
  function distributePlatformFee() public {
    if (!platformFeeDistributed) {
      platformFeeDistributed = true;
      (bool success, ) = metadropTreasury.call{value: _platformFee}("");
      if (!success) {
        revert TransferFailed();
      }
      emit PlatformFeeDistributed();
    } else {
      revert PlatformFeeAlreadyDistributed();
    }
  }

  receive() external payable {}
}

// @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
// @@                                                                                                @@
// @@   Metadrop has no affiliation with and does not endorse this token or its creators in any      @@
// @@   way, unless otherwise stated. For all terms and conditions associated with tokens launched   @@
// @@   using Metadrop software, refer to the terms published at metadrop[dot]com/legal.             @@
// @@                                                                                                @@
// @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@