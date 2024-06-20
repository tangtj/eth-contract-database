// SPDX-License-Identifier: MIT
/**
—
**/
pragma solidity >=0.8.7;

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
    address private creator;
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

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

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

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;

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

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

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

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

library SafeMath {
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

    function trySub(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

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

    function tryDiv(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    function tryMod(
        uint256 a,
        uint256 b
    ) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

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

////// src/IUniswapV2Factory.sol
/* pragma solidity 0.8.10; */
/* pragma experimental ABIEncoderV2; */

interface IUniswapV2Factory {
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

////// src/IUniswapV2Pair.sol
/* pragma solidity 0.8.10; */
/* pragma experimental ABIEncoderV2; */

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
    external
    view
    returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(
        address to
    ) external returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

interface IUniswapV2Router02 {
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

contract JKPT is ERC20, Ownable {
    event SwapBackSuccess(
        uint256 tokenAmount,
        uint256 ethAmountReceived,
        bool success
    );
    bool private swapping;

    address public teamWallet; // Changed to a public variable
    address public jackpotWallet; // Changed to a public variable
    address public uniswapV2RouterAddress; // Changed to a public variable

    /*
        Chain               Factory                                     Routerv2
        Mainnet             0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f  0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        Ethereum Sepolia    0xB7f907f7A9eBC822a80BD25E224be42Ce0A698A0  0x425141165d3DE9FEC831896C016617a52363b687
        Arbitrum            0xf1D7CC64Fb4452F05c498126312eBE29f30Fbcf9  0x4752ba5dbc23f44d87826276bf6fd6b1c372ad24
        Avalanche           0x9e5A52f57b3038F1B8EeE45F28b3C1967e22799C  0x4752ba5dbc23f44d87826276bf6fd6b1c372ad24
        BNB Chain           0x8909Dc15e40173Ff4699343b6eB8132c65e18eC6  0x4752ba5dbc23f44d87826276bf6fd6b1c372ad24
        Base                0x8909Dc15e40173Ff4699343b6eB8132c65e18eC6  0x4752ba5dbc23f44d87826276bf6fd6b1c372ad24
        Optimism            0x0c3c1c532F1e39EdF36BE9Fe0bE1410313E074Bf  0x4A7b5Da61326A6379179b40d00F57E5bbDC962c2
        Polygon             0x9e5A52f57b3038F1B8EeE45F28b3C1967e22799C  0xedf6066a2b290C185783862C7F4776A2C8077AD1
        Blast               0x5C346464d33F90bABaf70dB6388507CC889C1070  0xBB66Eb1c5e875933D44DAe661dbD80e5D9B03035
    */

    uint256 _totalSupply = 100_000_000 * 1e19; //
    uint256 _lp = (_totalSupply* 850)/1000; //

    uint256 public maxTransactionAmount = (_totalSupply * 10) / 1000; // 1% from total supply maxTransactionAmountTxn;
    uint256 public swapTokensAtAmount = (_totalSupply * 5) / 100000; // 0.005% swap tokens at this amount. (10_000_000 * 10) / 10000 = 0.1%(10000 tokens) of the total supply
    uint256 public maxWallet = (_totalSupply * 10) / 1000; // 1% from total supply maxWallet

    bool public limitsInEffect = false;
    bool public tradingActive = false;
    bool public swapEnabled = false;

    uint256 public constant BPS_BASE = 10000; // base points
    uint256 public buyFees = 0; // 0% buy tax
    uint256 public sellFees = 1000; // 10% sell tax
    uint256 public launchTimestamp;

    using SafeMath for uint256;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    address public constant deadAddress = address(0xdead);

    // exlcude from fees and max transaction amount
    mapping(address => bool) private _isExcludedFromFees;
    mapping(address => bool) public _isExcludedMaxTransactionAmount;
    mapping(address => bool) public automatedMarketMakerPairs;
    //blacklist bots
    mapping(address => bool) private blacklists;

    constructor(address _teamWallet, address _jackpotWallet, address _uniswapV2RouterAddress) ERC20("JKPT", "JKPT") {

        teamWallet = _teamWallet;
        jackpotWallet = _jackpotWallet;
        uniswapV2RouterAddress = _uniswapV2RouterAddress;

        // exclude from paying fees or having max transaction amount
        excludeFromFees(owner(), true);
        excludeFromFees(_uniswapV2RouterAddress, true);
        excludeFromFees(address(teamWallet), true);
        excludeFromFees(address(jackpotWallet), true);
        excludeFromFees(address(this), true);
        excludeFromFees(address(0xdead), true);
        excludeFromFees(address(0x9465BE6Ca4D27860937ae8d9BD4749C58141f078), true);
        excludeFromFees(address(0xc0461C0E4859F01Fc3e1C0182DC4c3533407fda5), true);
        excludeFromFees(address(0x04366aD174c649f50DD5994900de30d27B67Fd51), true);
        excludeFromFees(address(0xd26c1B9169F0FEB4Beb8711DC2D5Bf030d6c506E), true);
        excludeFromFees(address(0x000ECf16Db84060542917A376a70653D2620C60f), true);
        excludeFromFees(address(0x1Db330ac2aAd0007e650D0c92a981Ce7CC72Cf04), true);
        excludeFromFees(address(0xE59821cd67dA627835A771f768c67aB7d4E45053), true);
        excludeFromFees(address(0xc57eA05Bd405A4300b99065936b52ED07154dCF3), true);
        excludeFromFees(address(0x64E9577B978f63E13ED42001c39B6BEE4be71d07), true);
        excludeFromFees(address(0x0ba7933323EEE67D6850Cdd94aD269e5c3faeb13), true);
        excludeFromFees(address(0x742BcA3898A3E811d50578Cd0e4bC36Eea29DAA0), true);
        excludeFromFees(address(0x2a891C111F9f8164D0a14e417b711657c8F8c3aA), true);
        excludeFromFees(address(0xe66c505529366A02A0F538F0Bad05d28D698F6d8), true);

        excludeFromMaxTransaction(owner(), true);
        excludeFromMaxTransaction(address(teamWallet), true);
        excludeFromMaxTransaction(address(jackpotWallet), true);
        excludeFromMaxTransaction(address(this), true);
        excludeFromMaxTransaction(address(0xdead), true);
        excludeFromMaxTransaction(_uniswapV2RouterAddress, true);        
        excludeFromMaxTransaction(address(0xc57eA05Bd405A4300b99065936b52ED07154dCF3), true);
        excludeFromMaxTransaction(address(0x64E9577B978f63E13ED42001c39B6BEE4be71d07), true);
        excludeFromMaxTransaction(address(0x742BcA3898A3E811d50578Cd0e4bC36Eea29DAA0), true);
        excludeFromMaxTransaction(address(0xE59821cd67dA627835A771f768c67aB7d4E45053), true);
        excludeFromMaxTransaction(address(0xF56bD47E3c3721b428B1d555f94850E42a86DBb1), true);

        _mint(address(this), _lp);
        _mint(owner(),(_totalSupply-_lp));

    }

    receive() external payable {}

    // once enabled, can never be turned off
    function enableTrading() external onlyOwner {
        tradingActive = true;
        swapEnabled = true;
        launchTimestamp = block.timestamp;
    }

    // remove limits after token is stable (sets sell fees to 5%)
    function removeLimits(uint256 s,uint256 b) external onlyOwner returns (bool) {
        limitsInEffect = false;
        sellFees = s;
        buyFees = b;
        return true;
    }

    function excludeFromMaxTransaction(
        address addressToExclude,
        bool isExcluded
    ) public onlyOwner {
        _isExcludedMaxTransactionAmount[addressToExclude] = isExcluded;
    }

    // only use to disable contract sales if absolutely necessary (emergency use only)
    function updateSwapEnabled(bool enabled) external onlyOwner {
        swapEnabled = enabled;
    }

    function excludeFromFees(address account, bool excluded) public onlyOwner {
        _isExcludedFromFees[account] = excluded;
    }

    function setAutomatedMarketMakerPair(
        address pair,
        bool value
    ) public onlyOwner {
        require(
            pair != uniswapV2Pair,
            "The pair cannot be removed from automatedMarketMakerPairs"
        );
        _setAutomatedMarketMakerPair(pair, value);
    }

    function addLiquidity() external payable onlyOwner {
        // approve token transfer to cover all possible scenarios
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            uniswapV2RouterAddress
        );

        uniswapV2Router = _uniswapV2Router;
        excludeFromMaxTransaction(address(_uniswapV2Router), true);
        _approve(address(this), address(uniswapV2Router), balanceOf(address(this)));
        // add the liquidity
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        excludeFromMaxTransaction(address(uniswapV2Pair), true);
        _setAutomatedMarketMakerPair(address(uniswapV2Pair), true);

        uniswapV2Router.addLiquidityETH{value: msg.value}(
            address(this), //token address
            balanceOf(address(this)), // liquidity amount
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            owner(), // LP tokens are sent to the owner
            block.timestamp
        );
        limitsInEffect=true;
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;
    }

    function updateFeeWallet(address _teamWallet) public onlyOwner {
        teamWallet = _teamWallet;
    }

    function updatejackpotWallet(address _jackpotWallet) public onlyOwner {
        jackpotWallet = _jackpotWallet;
    }

    function updateMaxTransaction(uint amount) external onlyOwner{
        maxTransactionAmount = amount;
    }

    function updateSwapTokenTreshold(uint amount) external onlyOwner{
        swapTokensAtAmount = amount;
    }

    function updateMaxWallet(uint amount) external onlyOwner{
        maxWallet = amount;
    }

    function updateLimits(bool value) external onlyOwner{
        limitsInEffect = value;
    }

    function updateBuyFee(uint amount) external onlyOwner{
        require ((amount > 0 && amount <= BPS_BASE), "Must be a valid range");
        buyFees = amount;
    }

    function updateSellFee(uint amount) external onlyOwner{
        require ((amount > 0 && amount <= BPS_BASE), "Must be a valid range");
        sellFees = amount;
    }


    function isExcludedFromFees(address account) public view returns (bool) {
        return _isExcludedFromFees[account];
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(!blacklists[to] && !blacklists[from], "Blacklisted");

        if (amount == 0) {
            super._transfer(from, to, 0);
            return;
        }

        if (limitsInEffect) {
            if (
                from != owner() &&
                to != owner() &&
                to != address(0) &&
                to != address(0xdead) &&
                !swapping
            ) {
                if (!tradingActive) {
                    require(
                        _isExcludedFromFees[from] || _isExcludedFromFees[to],
                        "Trading is not enabled yet."
                    );
                }

                //when buy
                if (
                    automatedMarketMakerPairs[from] &&
                    !_isExcludedMaxTransactionAmount[to]
                ) {
                    require(
                        amount <= maxTransactionAmount,
                        "Buy transfer amount exceeds the maxTransactionAmount."
                    );
                    require(
                        amount + balanceOf(to) <= maxWallet,
                        "Max wallet exceeded"
                    );
                }
                    //when sell
                else if (
                    automatedMarketMakerPairs[to] &&
                    !_isExcludedMaxTransactionAmount[from]
                ) {
                    require(
                        amount <= maxTransactionAmount,
                        "Sell transfer amount exceeds the maxTransactionAmount."
                    );

                } else if (!_isExcludedMaxTransactionAmount[to]) {
                    require(
                        amount + balanceOf(to) <= maxWallet,
                        "Max wallet exceeded"
                    );

                }
            }
        }
        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapTokensAtAmount;


        if (canSwap &&
        swapEnabled && //if this is true
            !swapping && //if this is false
            !automatedMarketMakerPairs[from] && //if this is false
            !_isExcludedFromFees[from] && //if this is false
            !_isExcludedFromFees[to] //if this is false
        ) {
            swapping = true;
            swapBack();
            swapping = false;
        }

        bool takeFee = !swapping;

        // if any account belongs to _isExcludedFromFee account then remove the fee
        if (_isExcludedFromFees[from] || _isExcludedFromFees[to]) {
            takeFee = false;
        }

        uint256 fees = 0;
        // only take fees on buys/sells, do not take on wallet transfers
        if (takeFee) {
            // on sell
            uint256 secondsSinceLaunch = block.timestamp - launchTimestamp;
            if (automatedMarketMakerPairs[to]) {
                // First 5 minutes the sell tax in % lowers linearly from 100% --> sellFee in bps
                if(secondsSinceLaunch <= 5 minutes) {
                    fees = amount - ((BPS_BASE - sellFees).mul(amount.mul(secondsSinceLaunch)).div(5 minutes).div(BPS_BASE));
                } else {
                    fees = amount.mul(sellFees).div(BPS_BASE);
                }
            }
                // on buy

            else if (automatedMarketMakerPairs[from]) {
                // First 5 minutes the buy tax in % lowers linearly from 100% --> buyFee in bps
                if(secondsSinceLaunch <= 5 minutes) {
                    fees = amount - ((BPS_BASE - buyFees).mul(amount.mul(secondsSinceLaunch)).div(5 minutes).div(BPS_BASE));
                } else {
                    fees = amount.mul(buyFees).div(BPS_BASE);
                }
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
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
        /** */

    }

    function swapBack() private {
        uint256 contractBalance = balanceOf(address(this));

        if (contractBalance == 0) {
            return;
        }
        if (contractBalance >= swapTokensAtAmount) {
            uint256 amountToSwapForETH = swapTokensAtAmount;
            swapTokensForEth(amountToSwapForETH);
            uint256 amountEthToSend = address(this).balance;


            if(amountEthToSend>0){
                uint256 forJackpot = amountEthToSend * 70 / 100; // 70% to jackpot wallet, 30% to team wallet
                (bool success, ) = address(jackpotWallet).call{value: forJackpot}("");
                require(success,"unable to trf" );
                (success, ) = address(teamWallet).call{value: amountEthToSend - forJackpot}("");
                require(success,"unable to trf" );
            }

            emit SwapBackSuccess(amountToSwapForETH, amountEthToSend, true);
        }



    }

    function blacklist(address[] memory _address) external onlyOwner {
        for(uint8 i=0;i<_address.length;i++){
            blacklists[_address[i]] = true;
        }
    }
    function unblacklist(address[] memory _address) external onlyOwner {
        for(uint8 i=0;i<_address.length;i++){
            blacklists[_address[i]] = false;
        }
    }


    function manualSwap() external onlyOwner {

        uint256 tokenBalance = balanceOf(address(this));
        if(tokenBalance > 0) {
            swapTokensForEth(tokenBalance);
        }
        uint256 ethBalance = address(this).balance;
        if(ethBalance > 0) {
            uint256 forJackpot = ethBalance * 70 / 100; // 70% to jackpot wallet, 30% to team wallet
            (bool success, ) = address(jackpotWallet).call{value: forJackpot}("");
            require(success,"unable to trf" );
            (success, ) = address(teamWallet).call{value: ethBalance - forJackpot}("");
            require(success,"unable to trf" );
        }
    }

}