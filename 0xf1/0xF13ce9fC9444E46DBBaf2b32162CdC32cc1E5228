/*
 * SPDX-License-Identifier: MIT
 * https://KermitTheFrog.lol 
 * https://twitter.com/KermitFrogOnETH
 * https://t.me/KermitOnETH
 */

pragma solidity 0.8.19;

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the substraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryMul(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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
    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
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

interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * The default value of {decimals} is 18. To select a different value for
     * {decimals} you should overload it.
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
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the value {ERC20} uses, unless this function is
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `recipient` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(
        address owner,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the upd allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * Requirements:
     *
     * - `sender` and `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     * - the caller must have allowance for ``sender``'s tokens of at least
     * `amount`.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        unchecked {
            _approve(sender, _msgSender(), currentAllowance - amount);
        }

        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the upd allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] + addedValue
        );
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the upd allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(_msgSender(), spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `sender` to `recipient`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `sender` cannot be the zero address.
     * - `recipient` cannot be the zero address.
     * - `sender` must have a balance of at least `amount`.
     */
    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);

        uint256 senderBalance = _balances[sender];
        require(
            senderBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[sender] = senderBalance - amount;
        }
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);

        _afterTokenTransfer(sender, recipient, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
        }
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
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
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `amount` tokens have been minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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

interface IDexFactory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);

    function setFeeTo(address) external;

    function setFeeToSetter(address) external;
}

interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Kermit is ERC20, Ownable {
    using SafeMath for uint256;

    IDexRouter private immutable dexRouter;
    address public immutable dexPair;

    // Swapback
    bool private swapping;

    bool private swapbackEnabled = false;
    uint256 private swapBackValueMin;
    uint256 private swapBackValueMax;

    //Anti-whale
    bool private limitsEnabled = true;
    bool private transferDelayEnabled = true;
    uint256 private walletLimit;
    uint256 private txLimit;
    mapping(address => uint256) private _holderLastTransferTimestamp; // to hold last Transfers temporarily during launch

    bool public tradingLive = false;

    // Fee receivers
    address private treasuryWallet;
    address private projectWallet;

    uint256 private totalBuyTax;
    uint256 private treasuryBuyTax;
    uint256 private projectBuyTax;

    uint256 private totalSellTax;
    uint256 private treasurySellTax;
    uint256 private projectSellTax;

    uint256 private tokensForTreasury;
    uint256 private tokensForProject;

    /******************/

    // exlcude from fees and max transaction amount
    mapping(address => bool) private whitelistFromFee;
    mapping(address => bool) private whitelistFromLimit;
    mapping(address => bool) private automatedMarketMakerPairs;

    // store addresses that a automatic market maker pairs. Any transfer *to* these addresses
    // could be subject to a maximum transfer amount

    event UpdateUniswapV2Router(
        address indexed newAddress,
        address indexed oldAddress
    );

    event ExcludeFromFees(address indexed account, bool isExcluded);
    event ExcludeFromLimits(address indexed account, bool isExcluded);
    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);
    event TradingEnabled(uint256 indexed timestamp);
    event LimitsRemoved(uint256 indexed timestamp);
    event DisabledTransferDelay(uint256 indexed timestamp);

    event SwapbackSettingsUpdated(
        bool enabled,
        uint256 swapBackValueMin,
        uint256 swapBackValueMax
    );
    event MaxTxUpdated(uint256 txLimit);
    event MaxWalletUpdated(uint256 walletLimit);

    event treasuryWalletUpdated(
        address indexed newWallet,
        address indexed oldWallet
    );

    event projectWalletUpdated(
        address indexed newWallet,
        address indexed oldWallet
    );

    event lpReceiverUpdated(
        address indexed newWallet,
        address indexed oldWallet
    );

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiquidity
    );

    event BuyFeeUpdated(
        uint256 totalBuyTax,
        uint256 treasuryBuyTax,
        uint256 projectBuyTax
    );

    event SellFeeUpdated(
        uint256 totalSellTax,
        uint256 treasurySellTax,
        uint256 projectSellTax
    );

    constructor() ERC20("Kermit", "KERMIT") {
        IDexRouter _dexRouter = IDexRouter(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );

        excludeFromMaxTransaction(address(_dexRouter), true);
        dexRouter = _dexRouter;

        dexPair = IDexFactory(_dexRouter.factory()).createPair(
            address(this),
            _dexRouter.WETH()
        );
        excludeFromMaxTransaction(address(dexPair), true);
        _setAutomatedMarketMakerPair(address(dexPair), true);

        uint256 _treasuryBuyTax = 30;
        uint256 _projectBuyTax = 0;

        uint256 _treasurySellTax = 30;
        uint256 _projectSellTax = 0;

        uint256 totalSupply = 1_000_000_000 * 10 ** decimals();

        txLimit = (totalSupply * 10) / 1000;
        walletLimit = (totalSupply * 10) / 1000;

        swapBackValueMin = (totalSupply * 1) / 1000;
        swapBackValueMax = (totalSupply * 2) / 100;

        treasuryBuyTax = _treasuryBuyTax;
        projectBuyTax = _projectBuyTax;
        totalBuyTax = treasuryBuyTax + projectBuyTax;

        treasurySellTax = _treasurySellTax;
        projectSellTax = _projectSellTax;
        totalSellTax = treasurySellTax + projectSellTax;

        treasuryWallet = address(0x513d5a881DD4AdF873DeA2Ea0b998598D7F99b04);
        projectWallet = address(msg.sender);

        // exclude from paying fees or having max transaction amount
        excludeFromFees(msg.sender, true);
        excludeFromFees(address(this), true);
        excludeFromFees(address(0xdead), true);
        excludeFromFees(treasuryWallet, true);

        excludeFromMaxTransaction(msg.sender, true);
        excludeFromMaxTransaction(address(this), true);
        excludeFromMaxTransaction(address(0xdead), true);
        excludeFromMaxTransaction(treasuryWallet, true);

        transferOwnership(msg.sender);

        /*
            _mint is an internal function in ERC20.sol that is only called here,
            and CANNOT be called ever again
        */
        _mint(msg.sender, totalSupply);
    }

    receive() external payable {}

    /**
     * @notice  Opens public trading for the token
     * @dev     onlyOwner.
     */
    function open_trading() external onlyOwner {
        tradingLive = true;
        swapbackEnabled = true;
        emit TradingEnabled(block.timestamp);
    }

    /**
     * @notice Removes the max wallet and max transaction limits
     * @dev onlyOwner.
     * Emits an {LimitsRemoved} event
     */
    function disableLimits() external onlyOwner {
        limitsEnabled = false;
        emit LimitsRemoved(block.timestamp);
    }

    /**
     * @notice Removes the transfer delay
     * @dev onlyOwner.
     * Emits an {DisabledTransferDelay} event
     */
    function disableTransferDelay() external onlyOwner {
        transferDelayEnabled = false;
        emit DisabledTransferDelay(block.timestamp);
    }

    /**
     * @notice sets if swapback is enabled and sets the minimum and maximum amounts
     * @dev onlyOwner.
     * Emits an {SwapbackSettingsUpdated} event
     * @param _enabled If swapback is enabled
     * @param _min The minimum amount of tokens the contract must have before swapping tokens for ETH. Base 10000, so 1% = 100.
     * @param _max The maximum amount of tokens the contract can swap for ETH. Base 10000, so 1% = 100.
     */
    function setSwapBackSettings(
        bool _enabled,
        uint256 _min,
        uint256 _max
    ) external onlyOwner {
        require(
            _min >= 1,
            "Swap amount cannot be lower than 0.01% total supply."
        );
        require(_max >= _min, "maximum amount cant be higher than minimum");

        swapbackEnabled = _enabled;
        swapBackValueMin = (totalSupply() * _min) / 10000;
        swapBackValueMax = (totalSupply() * _max) / 10000;
        emit SwapbackSettingsUpdated(_enabled, _min, _max);
    }

    /**
     * @notice Changes the maximum amount of tokens that can be bought or sold in a single transaction
     * @dev onlyOwner.
     * Emits an {MaxTxUpdated} event
     * @param newNum Base 1000, so 1% = 10
     */
    function setTxLimit(uint256 newNum) external onlyOwner {
        require(newNum >= 2, "Cannot set txLimit lower than 0.2%");
        txLimit = (newNum * totalSupply()) / 1000;
        emit MaxTxUpdated(txLimit);
    }

    /**
     * @notice Changes the maximum amount of tokens a wallet can hold
     * @dev onlyOwner.
     * Emits an {MaxWalletUpdated} event
     * @param newNum Base 1000, so 1% = 10
     */
    function setWalletLimit(uint256 newNum) external onlyOwner {
        require(newNum >= 5, "Cannot set walletLimit lower than 0.5%");
        walletLimit = (newNum * totalSupply()) / 1000;
        emit MaxWalletUpdated(walletLimit);
    }

    /**
     * @notice Sets if a wallet is excluded from the max wallet and tx limits
     * @dev onlyOwner.
     * Emits an {ExcludeFromLimits} event
     * @param updAds The wallet to update
     * @param isEx If the wallet is excluded or not
     */
    function excludeFromMaxTransaction(
        address updAds,
        bool isEx
    ) public onlyOwner {
        whitelistFromLimit[updAds] = isEx;
        emit ExcludeFromLimits(updAds, isEx);
    }

    /**
     * @notice Sets the fees for buys
     * @dev onlyOwner.
     * Emits a {BuyFeeUpdated} event
     * All fees added up must be less than 100
     * @param _marketingFee The fee for the marketing wallet
     * @param _devFee The fee for the dev wallet
     */
    function setBuyFees(
        uint256 _marketingFee,
        uint256 _devFee
    ) external onlyOwner {
        treasuryBuyTax = _marketingFee;
        projectBuyTax = _devFee;
        totalBuyTax = treasuryBuyTax + projectBuyTax;
        require(totalBuyTax <= 100, "Total buy fee cannot be higher than 100%");
        emit BuyFeeUpdated(totalBuyTax, treasuryBuyTax, projectBuyTax);
    }

    /**
     * @notice Sets the fees for sells
     * @dev onlyOwner.
     * Emits a {SellFeeUpdated} event
     * All fees added up must be less than 100
     * @param _marketingFee The fee for the marketing wallet
     * @param _devFee The fee for the dev wallet
     */
    function setSellFees(
        uint256 _marketingFee,
        uint256 _devFee
    ) external onlyOwner {
        treasurySellTax = _marketingFee;
        projectSellTax = _devFee;
        totalSellTax = treasurySellTax + projectSellTax;
        require(
            totalSellTax <= 100,
            "Total sell fee cannot be higher than 100%"
        );
        emit SellFeeUpdated(totalSellTax, treasurySellTax, projectSellTax);
    }

    /**
     * @notice Sets if an address is excluded from fees
     * @dev onlyOwner.
     * Emits an {ExcludeFromFees} event
     * @param account The wallet to update
     * @param excluded If the wallet is excluded or not
     */
    function excludeFromFees(address account, bool excluded) public onlyOwner {
        whitelistFromFee[account] = excluded;
        emit ExcludeFromFees(account, excluded);
    }

    /**
     * @notice Sets an address as a new liquidity pair. You probably dont want to do this.
     * @dev onlyOwner.
     * Emits a {SetAutomatedMarketMakerPair} event
     * @param pair the address of the pair
     * @param value If the pair is a automated market maker pair or not
     */
    function setAutomatedMarketMakerPair(
        address pair,
        bool value
    ) public onlyOwner {
        require(
            pair != dexPair,
            "The pair cannot be removed from automatedMarketMakerPairs"
        );

        _setAutomatedMarketMakerPair(pair, value);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;

        emit SetAutomatedMarketMakerPair(pair, value);
    }

    /**
     * @notice Sets the marketing wallet
     * @dev onlyOwner.
     * Emits an {treasuryWalletUpdated} event
     * @param newWallet The new marketing wallet
     */
    function setMarketingWallet(address newWallet) external onlyOwner {
        emit treasuryWalletUpdated(newWallet, treasuryWallet);
        treasuryWallet = newWallet;
    }

    /**
     * @notice Sets the dev wallet
     * @dev onlyOwner.
     * Emits an {projectWalletUpdated} event
     * @param newWallet The new dev wallet
     */
    function setDevWallet(address newWallet) external onlyOwner {
        emit projectWalletUpdated(newWallet, projectWallet);
        projectWallet = newWallet;
    }

    /**
     * @notice  Information about the swapback settings
     * @return  _swapbackEnabled  if swapback is enabled
     * @return  _swapBackValueMin  the minimum amount of tokens in the contract balance to trigger swapback
     * @return  _swapBackValueMax  the maximum amount of tokens in the contract balance to trigger swapback
     */
    function swapbackValues()
        external
        view
        returns (
            bool _swapbackEnabled,
            uint256 _swapBackValueMin,
            uint256 _swapBackValueMax
        )
    {
        _swapbackEnabled = swapbackEnabled;
        _swapBackValueMin = swapBackValueMin;
        _swapBackValueMax = swapBackValueMax;
    }

    /**
     * @notice  Information about the anti whale parameters
     * @return  _limitsEnabled  if the wallet limits are in effect
     * @return  _transferDelayEnabled  if the transfer delay is enabled
     * @return  _walletLimit  The maximum amount of tokens that can be held by a wallet
     * @return  _txLimit  The maximum amount of tokens that can be bought or sold in a single transaction
     */
    function txLimitValues()
        external
        view
        returns (
            bool _limitsEnabled,
            bool _transferDelayEnabled,
            uint256 _walletLimit,
            uint256 _txLimit
        )
    {
        _limitsEnabled = limitsEnabled;
        _transferDelayEnabled = transferDelayEnabled;
        _walletLimit = walletLimit;
        _txLimit = txLimit;
    }

    /**
     * @notice The wallets that receive the collected fees
     * @return _treasuryWallet The wallet that receives the marketing fees
     * @return _projectWallet The wallet that receives the dev fees
     */
    function taxReceivers()
        external
        view
        returns (address _treasuryWallet, address _projectWallet)
    {
        return (treasuryWallet, projectWallet);
    }

    /**
     * @notice Fees for buys, sells, and transfers
     * @return _totalBuyTax The total fee for buys
     * @return _treasuryBuyTax The fee for buys that gets sent to marketing
     * @return _projectBuyTax The fee for buys that gets sent to dev
     * @return _totalSellTax The total fee for sells
     * @return _treasurySellTax The fee for sells that gets sent to marketing
     * @return _projectSellTax The fee for sells that gets sent to dev
     */
    function taxValues()
        external
        view
        returns (
            uint256 _totalBuyTax,
            uint256 _treasuryBuyTax,
            uint256 _projectBuyTax,
            uint256 _totalSellTax,
            uint256 _treasurySellTax,
            uint256 _projectSellTax
        )
    {
        _totalBuyTax = totalBuyTax;
        _treasuryBuyTax = treasuryBuyTax;
        _projectBuyTax = projectBuyTax;
        _totalSellTax = totalSellTax;
        _treasurySellTax = treasurySellTax;
        _projectSellTax = projectSellTax;
    }

    /**
     * @notice  If the wallet is excluded from fees and max transaction amount and if the wallet is a automated market maker pair
     * @param   _target  The wallet to check
     * @return  _whitelistFromFee  If the wallet is excluded from fees
     * @return  _whitelistFromLimit  If the wallet is excluded from max transaction amount
     * @return  _automatedMarketMakerPairs If the wallet is a automated market maker pair
     */
    function checkMappings(
        address _target
    )
        external
        view
        returns (
            bool _whitelistFromFee,
            bool _whitelistFromLimit,
            bool _automatedMarketMakerPairs
        )
    {
        _whitelistFromFee = whitelistFromFee[_target];
        _whitelistFromLimit = whitelistFromLimit[_target];
        _automatedMarketMakerPairs = automatedMarketMakerPairs[_target];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if (amount == 0) {
            super._transfer(from, to, 0);
            return;
        }

        if (limitsEnabled) {
            if (
                from != owner() &&
                to != owner() &&
                to != address(0) &&
                to != address(0xdead) &&
                !swapping
            ) {
                if (!tradingLive) {
                    require(
                        whitelistFromFee[from] || whitelistFromFee[to],
                        "_transfer:: Trading is not active."
                    );
                }

                // at launch if the transfer delay is enabled, ensure the block timestamps for purchasers is set -- during launch.
                if (transferDelayEnabled) {
                    if (
                        to != owner() &&
                        to != address(dexRouter) &&
                        to != address(dexPair)
                    ) {
                        require(
                            _holderLastTransferTimestamp[tx.origin] <
                                block.number,
                            "_transfer:: Transfer Delay enabled.  Only one purchase per block allowed."
                        );
                        _holderLastTransferTimestamp[tx.origin] = block.number;
                    }
                }

                //when buy
                if (
                    automatedMarketMakerPairs[from] && !whitelistFromLimit[to]
                ) {
                    require(
                        amount <= txLimit,
                        "Buy transfer amount exceeds the txLimit."
                    );
                    require(
                        amount + balanceOf(to) <= walletLimit,
                        "Max wallet exceeded"
                    );
                }
                //when sell
                else if (
                    automatedMarketMakerPairs[to] && !whitelistFromLimit[from]
                ) {
                    require(
                        amount <= txLimit,
                        "Sell transfer amount exceeds the txLimit."
                    );
                } else if (!whitelistFromLimit[to]) {
                    require(
                        amount + balanceOf(to) <= walletLimit,
                        "Max wallet exceeded"
                    );
                }
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapBackValueMin;

        if (
            canSwap &&
            swapbackEnabled &&
            !swapping &&
            !automatedMarketMakerPairs[from] &&
            !whitelistFromFee[from] &&
            !whitelistFromFee[to]
        ) {
            swapping = true;

            swapBack();

            swapping = false;
        }

        bool takeFee = !swapping;

        // if any account belongs to _isExcludedFromFee account then remove the fee
        if (whitelistFromFee[from] || whitelistFromFee[to]) {
            takeFee = false;
        }

        uint256 fees = 0;
        // only take fees on buys/sells, do not take on wallet transfers
        if (takeFee) {
            // on sell
            if (automatedMarketMakerPairs[to] && totalSellTax > 0) {
                fees = amount.mul(totalSellTax).div(100);
                tokensForProject += (fees * projectSellTax) / totalSellTax;
                tokensForTreasury += (fees * treasurySellTax) / totalSellTax;
            }
            // on buy
            else if (automatedMarketMakerPairs[from] && totalBuyTax > 0) {
                fees = amount.mul(totalBuyTax).div(100);
                tokensForProject += (fees * projectBuyTax) / totalBuyTax;
                tokensForTreasury += (fees * treasuryBuyTax) / totalBuyTax;
            }

            if (fees > 0) {
                super._transfer(from, address(this), fees);
            }

            amount -= fees;
        }

        super._transfer(from, to, amount);
    }

    function swapTokensForEth(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = dexRouter.WETH();

        _approve(address(this), address(dexRouter), tokenAmount);

        // make the swap
        dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function swapBack() private {
        uint256 contractBalance = balanceOf(address(this));
        uint256 totalTokensToSwap = contractBalance;
        bool success;

        if (contractBalance == 0) {
            return;
        }

        if (contractBalance > swapBackValueMax) {
            contractBalance = swapBackValueMax;
        }

        uint256 amountToSwapForETH = contractBalance;

        uint256 initialETHBalance = address(this).balance;

        swapTokensForEth(amountToSwapForETH);

        uint256 ethBalance = address(this).balance.sub(initialETHBalance);

        uint256 ethForDev = ethBalance.mul(tokensForProject).div(
            totalTokensToSwap
        );

        tokensForTreasury = 0;
        tokensForProject = 0;

        (success, ) = address(projectWallet).call{value: ethForDev}("");

        (success, ) = address(treasuryWallet).call{
            value: address(this).balance
        }("");
    }
}