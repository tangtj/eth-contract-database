// SPDX-License-Identifier: MIT

pragma solidity 0.8.20;

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
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
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

/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
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
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

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
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
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
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
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
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
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
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
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
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
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
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
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
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
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
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

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
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
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

/// Uniswap factory interface
interface IFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

/// Uniswap Router interface
interface IUniswapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function getAmountsOut(
        uint256 amountIn,
        address[] calldata path
    ) external view returns (uint256[] memory amounts);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external payable;
}

/// @title MoonShot Index token
contract MoonShot is ERC20, Ownable {
    /// @notice MAX SUPPLY 100 million tokens
    uint256 private constant MAX_SUPPLY = 100_000_000 * 1e18;
    /// @notice max wallet limit
    uint256 public maxWallet = (MAX_SUPPLY * 6) / 100;
    /// @notice max Tx amount
    uint256 public maxTxAmount = (MAX_SUPPLY * 2) / 100;
    /// @notice swap tokens at amount
    uint256 public swapTokensAtAmount = 100000e18;
    /// @notice fee divisor
    uint256 public constant FEE_DIVISOR = 1000;
    /// @notice marketing wallet
    address public marketingWallet = address(0xe8f80939FD3d0beD793E452a5A635ab102D95d48);
    /// @notice dev wallet
    address public devWallet = address(0x448613f6d6F311f17ed630f32aEacF7aB12de15e);
    /// @notice liquidity receiver wallet
    address public lpReceiverWallet = address(0xeCceC24a442689169E057A37dd1b95F98c60684E);

    /// @notice uniswapV2Router
    IUniswapRouter public immutable uniswapV2Router;
    /// @notice uniswapPair
    address public immutable uniswapPair;

    struct BuyFees {
        uint256 marketing;
        uint256 dev;
        uint256 autoLP;
    }

    struct SellFees {
        uint256 marketing;
        uint256 dev;
        uint256 autoLP;
    }
    /// @notice buyFee
    BuyFees public buyFee;
    /// @notice sellFee
    SellFees public sellFee;

    uint256 totalBuyFee;
    uint256 totalSellFee;

    /// @notice swapping status
    bool swapping = false;
    /// @notice enable normal tax mode
    bool normalTaxMode = false;
    /// @notice manage amm pairs
    mapping(address => bool) public isMarketPair;
    /// @notice manage exclude from fees
    mapping(address => bool) public isExcludedFromFees;
    /// @notice manage blacklist
    mapping(address => bool) public isBlacklisted;
    /// buy count
    uint256 private buyCount;

    ///  errors
    error OnlyMarketingWallet();
    error EthClaimFailed();
    error ZeroAddress();
    error CannotModifyMainPair();
    error CannotClaimNativeToken();
    error UpdateBoolValue();
    error FromOrToIsBlacklisted();
    error MaxBuyPerTxExceeds();
    error MaxSellPerTxExceeds();
    error MaxWalletLimitExceeds();

    event WalletsUpdated(
        address indexed marketing,
        address indexed dev,
        address indexed lp
    );
    event BuyFeesUpdated(
        uint256 indexed marketing,
        uint256 indexed dev,
        uint256 indexed lp
    );
    event SellFeesUpdated(
        uint256 indexed marketing,
        uint256 indexed dev,
        uint256 indexed lp
    );

    constructor() ERC20("MoonShot Index", "MSHOT") {
        uniswapV2Router = IUniswapRouter(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D // uniswap v2 router eth
        );
        uniswapPair = IFactory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );

        isMarketPair[uniswapPair] = true;

        buyFee.marketing = 20; // 2%
        buyFee.dev = 20; // 2%
        buyFee.autoLP = 10; // 1%

        totalBuyFee = 50; // 5%

        sellFee.marketing = 20; // 2%
        sellFee.dev = 20; // 2%
        sellFee.autoLP = 10; // 1%
        totalSellFee = 50; // 5%

        isExcludedFromFees[owner()] = true;
        isExcludedFromFees[address(this)] = true;
        _mint(msg.sender, MAX_SUPPLY);
    }

    receive() external payable {}

    /// @dev claim any erc20 token apart from native token, accidently sent to token contract
    /// @param token: token to rescue
    /// @param amount: amount to rescue
    /// Requirements -
    /// only marketing wallet can rescue stucked tokens
    function claimStuckedERC20(address token, uint256 amount) external {
        if (msg.sender != marketingWallet) {
            revert OnlyMarketingWallet();
        }
        if (token == address(this)) {
            revert CannotClaimNativeToken();
        }
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0xa9059cbb, marketingWallet, amount)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "ERC20: TOKEN_CLAIM_FAILED"
        );
    }
     

    /// @notice owner update the swap tokens at amount globally
    /// limited to 1000 to 1,000,000 tokens
    /// @param _newAmount: new swap tokens at amount
    function setSwapTokensAtAmount(uint256  _newAmount) external onlyOwner {
        if(_newAmount >= 1000 &&  _newAmount <= 1000000){
            uint256 amount = _newAmount * 1e18;
            swapTokensAtAmount = amount;
        }
    }

    /// @dev enable normal tax mode
    /// remove high tax globally before 25 buys
    function switchToNormalTaxMode() external onlyOwner {
        normalTaxMode = true;
    }

    /// @dev claim stucked eth
    function claimStuckedEth(address wallet) external {
        if (msg.sender != marketingWallet) {
            revert OnlyMarketingWallet();
        }
        (bool sent, ) = wallet.call{value: address(this).balance}("");
        if (!sent) {
            revert EthClaimFailed();
        }
    }

    /// @notice manage exclude/include from fees/limits
    /// @param user: address of user to exclude or include
    /// @param value: true or false (true to exclude and false to include)
    function excludeFromFees(address user, bool value) external onlyOwner {
        if (isExcludedFromFees[user] == value) {
            revert UpdateBoolValue();
        }
        isExcludedFromFees[user] = value;
    }

    /// @dev update max wallet percent globally
    /// @notice percent: new percent for max wallet
    function setMaxWalletPercent(uint256 percent) external onlyOwner {
        if (percent >= 1) {
            maxWallet = (MAX_SUPPLY * percent) / 100;
        }
    }

    /// @dev update max tx amount globally
    /// @notice percent: new percent for max tx
    function setMaxTxAmount(uint256 percent) external onlyOwner {
        if (percent >= 1) {
            maxTxAmount = (MAX_SUPPLY * percent) / 100;
        }
    }

    /// @dev update buy fee globally
    /// @param _mkt: new marketing fees on buy
    /// @param _dev: new dev fees on buy
    /// @param _lp: new autoLp fees on buy
    /// max buy fees is limited to 10 percent
    /// fees divisor is 1000, so 10 input means 1%
    function setBuyFees(
        uint256 _mkt,
        uint256 _dev,
        uint256 _lp
    ) external onlyOwner {
        if (_mkt + _dev + _lp <= 100) {
            buyFee.marketing = _mkt;
            buyFee.dev = _dev;
            buyFee.autoLP = _lp;
            totalBuyFee = _mkt + _dev + _lp;
            emit BuyFeesUpdated(_mkt, _dev, _lp);
        }
    }

    /// @dev update sell fees globally
    /// @param _mkt: new marketing fees on sell
    /// @param _dev: new dev fees on sell
    /// @param _lp: new autoLp fees on sell
    /// max sell fees is limited to 10 percent
    /// fees divisor is 1000, so 10 input means 1%
    function setSellFees(
        uint256 _mkt,
        uint256 _dev,
        uint256 _lp
    ) external onlyOwner {
        if (_mkt + _dev + _lp <= 100) {
            sellFee.marketing = _mkt;
            sellFee.dev = _dev;
            sellFee.autoLP = _lp;
            totalSellFee = _mkt + _dev + _lp;
            emit SellFeesUpdated(_mkt, _dev, _lp);
        }
    }

    /// @notice manage blacklist of illicit accounts
    /// @param account: account to exclude or include
    /// @param value: true or false (true to exclude and false to include)
    function blacklist(address account, bool value) external onlyOwner {
        if (isBlacklisted[account] == value) {
            revert UpdateBoolValue();
        }
        isBlacklisted[account] = value;
    }

    /// @dev blacklist multiple
    /// @dev see{blacklist}
    function blacklistMultiple(
        address[] calldata accounts,
        bool value
    ) external onlyOwner {
        for (uint256 i = 0; i < accounts.length; ++i) {
            isBlacklisted[accounts[i]] = value;
        }
    }

    /// @dev add or remove new pairs
    /// @param pair: new pair to be added or removed
    /// @param value: bool value
    function setMarketPair(address pair, bool value) external onlyOwner {
        if (pair == uniswapPair) {
            revert CannotModifyMainPair();
        }
        isMarketPair[pair] = value;
    }

    /// @dev update dev and marketing wallet
    /// @param _market new marketing wallet
    /// @param _dev new dev wallet
    /// @param _lp new lp receiver wallet
    function setWallets(
        address _market,
        address _dev,
        address _lp
    ) external onlyOwner {
        if (_market != address(0) && _dev != address(0)) {
            devWallet = _dev;
            marketingWallet = _market;
            lpReceiverWallet = _lp;
            emit WalletsUpdated(_market, _dev, _lp);
        }
    }

    /// @notice manage token transfer and fees
    /// starting from 30% to normal tax as follows
    /// first 5 buys - 30%
    /// next 5 buys - 25%
    /// next 5 buys - 20%
    /// next 5 buys - 15%
    /// next 5 buys - 10%
    /// after global normal tax
    /// on sell side, if sold within first 10 buys, it's 10 percent
    /// else global sell tax
    /// owner can switch to normal before 30 buys if he want
    /// @dev See {ERC20-_transfer}
    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        bool takeFee = true;
        if (isBlacklisted[to] || isBlacklisted[from]) {
            revert FromOrToIsBlacklisted();
        }
        if (isExcludedFromFees[from] || isExcludedFromFees[to]) {
            takeFee = false;
        }

        if (takeFee) {
            uint256 fee;
            if (!isMarketPair[to]) {
                if (balanceOf(to) + amount > maxWallet) {
                    revert MaxWalletLimitExceeds();
                }
            }
            if (isMarketPair[from]) {
                if (amount > maxTxAmount) {
                    revert MaxBuyPerTxExceeds();
                }
                if (totalBuyFee > 0) {
                    if (buyCount > 25 || normalTaxMode) {
                        fee = (amount * totalBuyFee) / FEE_DIVISOR;
                    } else {
                        uint buyf;
                        if (buyCount <= 5) {
                            buyf = 300;
                        } else if (buyCount > 5 && buyCount <= 10) {
                            buyf = 250;
                        } else if (buyCount > 10 && buyCount <= 15) {
                            buyf = 200;
                        } else if (buyCount > 15 && buyCount <= 20) {
                            buyf = 150;
                        } else {
                            buyf = 100;
                        }
                        fee = (amount * buyf) / FEE_DIVISOR;
                        buyCount++;
                    }
                }
            } else if (isMarketPair[to]) {
                if (amount > maxTxAmount) {
                    revert MaxSellPerTxExceeds();
                }
                if (totalSellFee > 0) {
                    if (buyCount > 10 || normalTaxMode) {
                        fee = (amount * totalSellFee) / FEE_DIVISOR;
                    } else {
                        fee = (amount * 100) / FEE_DIVISOR;
                    }
                }
            }

            amount = amount - fee;

            if (fee > 0) {
                super._transfer(from, address(this), fee);
            }
        }
        uint256 contractBalance = balanceOf(address(this));

        bool canSwap = contractBalance >= swapTokensAtAmount &&
            !isMarketPair[from] &&
            from != owner() &&
            !swapping;
        if (canSwap) {
            swapping = true;
            swapAndLiquify(contractBalance);
            swapping = false;
        }

        super._transfer(from, to, amount);
    }

    /// @notice swap and liquify
    /// transfer collected tax to designated addresses as per there share
    function swapAndLiquify(uint256 amount) private {
        uint256 totalFees = totalBuyFee + totalSellFee;
        if (totalFees > 0) {
            uint256 tokensForLP = (amount * (buyFee.autoLP + sellFee.autoLP)) /
                (totalFees);
            uint256 lpHalf = tokensForLP / 2;
            uint256 devTokens = (amount * (buyFee.dev + sellFee.dev)) /
                totalFees;
            uint256 tokensForSwap = amount - lpHalf;
            uint256 ethBeforeSwap = address(this).balance;
            if (tokensForSwap > 0) {
                swapTokensForETH(tokensForSwap);
            }
            uint256 newEth = address(this).balance - ethBeforeSwap;
            uint256 ethForLp = (newEth * lpHalf) / tokensForSwap;
            uint256 ethForDev = (newEth * devTokens) / tokensForSwap;
            bool sent;

            /// add liquidity
            if (ethForLp > 0 && lpHalf > 0) {
                addLiquidity(ethForLp, lpHalf);
            }
            /// send dev eth share to dev wallet
            if (ethForDev > 0) {
                (sent, ) = devWallet.call{value: ethForDev}("");
            }

            /// sent remaining eth to marketing wallet
            if (address(this).balance > 0) {
                (sent, ) = marketingWallet.call{value: address(this).balance}(
                    ""
                );
            }
        } else {
            swapTokensForETH(amount);
            bool y;
            (y, ) = marketingWallet.call{value: address(this).balance}("");
        }
    }

    /// @notice add lp with given tokens and eth
    function addLiquidity(uint256 eth, uint256 tokens) private {
        if (allowance(address(this), address(uniswapV2Router)) < tokens) {
            _approve(
                address(this),
                address(uniswapV2Router),
                type(uint256).max
            );
        }

        uniswapV2Router.addLiquidityETH{value: eth}(
            address(this),
            tokens,
            0,
            0,
            lpReceiverWallet, // burn lp
            block.timestamp + 360
        );
    }

    ///@notice swap the tax tokens for eth and send to marketing wallet
    function swapTokensForETH(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        if (allowance(address(this), address(uniswapV2Router)) < tokenAmount) {
            _approve(
                address(this),
                address(uniswapV2Router),
                type(uint256).max
            );
        }

        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp + 360
        );
    }
}