// SPDX-License-Identifier: MIT

/**

https://linktr.ee/predictcrypto

*/

pragma solidity =0.8.24;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function getOwner() external view returns (address);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address _owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Burn(address indexed from, address indexed to, uint256 value);
}

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

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    address internal ZERO = 0x0000000000000000000000000000000000000000;

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(ZERO);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != ZERO, "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IDEXFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IDEXRouter {
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
    )
        external
        payable
        returns (uint amountToken, uint amountETH, uint liquidity);

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

contract PREDICTCRYPTO is IERC20, Ownable {
    address private immutable WETH;
    address public immutable pair;
    IDEXRouter public constant router =
        IDEXRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
    address private constant DEAD = 0x000000000000000000000000000000000000dEaD;
    string private constant _name = "Predict Crypto";
    string private constant _symbol = "PREAI";
    uint8 private constant _decimals = 18;
    uint8 private constant ANTI_BOT_BLOCKS = 100;
    uint16 private constant ANTI_BOT_BUY_AI = 900;
    uint16 private constant ANTI_BOT_SELL_AI = 2400;
    uint16 private constant ANTI_BOT_BUY_LP = 100;
    uint16 private constant ANTI_BOT_SELL_LP = 100;
    uint16 private constant MAX_FEE = 500;
    uint16 private constant DENOMINATOR = 10000;
    uint256 private constant TOTAL_SUPPLY = 1 * 10 ** 10 * (10 ** _decimals);
    uint256 private constant MINIMUM_SWAP_LIMIT = 10_000 ether;

    uint256 public swapThreshold = TOTAL_SUPPLY / 1000; // Starting at 0.1%
    uint256[2] public taxesCollected = [0, 0];

    uint32 public launchedAt;
    address public liquidityPool = DEAD;
    // All fees are in basis points (100 = 1%)
    uint16 private _buyAi = 300;
    uint16 private _sellAi = 300;
    uint16 private _buyLP = 200;
    uint16 private _sellLP = 200;
    address public marketingWallet = 0x41267eE8F02312bd9AF8622806b092892974304a;
    bool private _inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;
    bool public tradingOpen = false;
    bool public isBlacklistFunctionAvailable = true;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _blacklist;
    mapping(address => bool) public isFeeExempt;

    modifier lockTheSwap() {
        _inSwapAndLiquify = true;
        _;
        _inSwapAndLiquify = false;
    }

    //Event Logs
    event LiquidityPoolUpdated(address indexed _newPool);
    event MarketingWalletUpdated(address indexed _newWallet);
    event BuyFeesUpdated(uint16 _newAi, uint16 _newLp);
    event SellFeesUpdated(uint16 _neAi, uint16 _newLp);
    event StuckETHCleared(uint256 _amount);
    event StuckTokensCleared(address _token, uint256 _amount);
    event FeeExemptionChanged(address indexed _exemptWallet, bool _exempt);
    event SwapbackSettingsChanged(bool _enabled, uint256 _newSwapbackAmount);
    event Blacklisted(address indexed _wallet, bool _status);
    event BlacklistDisabled();
    event LaunchSequenceStarted();
    event StuckETH(uint256 _amount);

    error InvalidAddress();
    error InvalidAmount();
    error InvalidFee();
    error Unavailable();
    error TransferFromZeroAddress();
    error TransferToZeroAddress();

    constructor() {
        WETH = router.WETH();

        pair = IDEXFactory(router.factory()).createPair(WETH, address(this));

        _allowances[address(this)][address(router)] = type(uint256).max;

        isFeeExempt[owner()] = true;
        isFeeExempt[marketingWallet] = true;
        isFeeExempt[address(this)] = true;

        _balances[owner()] = TOTAL_SUPPLY;

        emit Transfer(address(0), owner(), TOTAL_SUPPLY);
    }

    function blacklistAddress(
        address _wallet,
        bool _status
    ) external onlyOwner {
        if (!isBlacklistFunctionAvailable) revert Unavailable();
        if (_wallet == address(0) || _wallet == pair) revert InvalidAddress();
        _blacklist[_wallet] = _status;
        emit Blacklisted(_wallet, _status);
    }

    function disableBlacklist() external onlyOwner {
        /// @dev permanently disables any future changes to the blacklist
        if (!isBlacklistFunctionAvailable) revert Unavailable();
        isBlacklistFunctionAvailable = false;
        emit BlacklistDisabled();
    }

    function launchSequence() external onlyOwner {
        if (launchedAt != 0) revert Unavailable();
        launchedAt = uint32(block.number);
        tradingOpen = true;
        emit LaunchSequenceStarted();
    }

    function getCirculatingSupply() external view returns (uint256) {
        return TOTAL_SUPPLY - balanceOf(DEAD) - balanceOf(ZERO);
    }

    function totalSupply() external pure override returns (uint256) {
        return TOTAL_SUPPLY;
    }

    function decimals() external pure override returns (uint8) {
        return _decimals;
    }

    function symbol() external pure override returns (string memory) {
        return _symbol;
    }

    function name() external pure override returns (string memory) {
        return _name;
    }

    function getOwner() external view override returns (address) {
        return owner();
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function allowance(
        address holder,
        address spender
    ) external view override returns (uint256) {
        return _allowances[holder][spender];
    }

    function baseBuyFee() external view returns (uint16) {
        return _buyAi + _buyLP;
    }

    function baseSellFee() external view returns (uint16) {
        return _sellAi + _sellLP;
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, type(uint256).max);
    }

    //Transfer Functions

    function transfer(
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        return _transfer(msg.sender, recipient, amount);
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external override returns (bool) {
        if (_allowances[sender][msg.sender] != type(uint256).max) {
            _allowances[sender][msg.sender] =
                _allowances[sender][msg.sender] -
                amount;
        }
        return _transfer(sender, recipient, amount);
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) private returns (bool) {
        if (sender == address(0)) revert TransferFromZeroAddress();
        if (recipient == address(0)) revert TransferToZeroAddress();
        if (amount == 0) revert InvalidAmount();
        if (_inSwapAndLiquify) {
            return _basicTransfer(sender, recipient, amount);
        }
        if (_blacklist[sender] || _blacklist[recipient]) revert Unavailable();
        if (isFeeExempt[sender] || isFeeExempt[recipient]) {
            return _basicTransfer(sender, recipient, amount);
        }
        if (!tradingOpen) revert Unavailable();
        _balances[sender] -= amount;
        uint256 finalAmount = amount;
        if (sender == pair) {
            finalAmount = _handleBuyTax(sender, amount);
        } else if (recipient == pair) {
            if (
                swapAndLiquifyEnabled &&
                taxesCollected[0] + taxesCollected[1] >= swapThreshold
            ) {
                _swapBack();
            }
            finalAmount = _handleSellTax(sender, amount);
        }
        _balances[recipient] += finalAmount;
        emit Transfer(sender, recipient, finalAmount);
        return true;
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) private returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    //Tax Functions

    function _handleBuyTax(
        address sender,
        uint256 amount
    ) private returns (uint256) {
        bool chargeBaseFee = _chargeBaseFee();
        uint16 buyAi = chargeBaseFee ? _buyAi : ANTI_BOT_BUY_AI;
        uint16 buyLP = chargeBaseFee ? _buyLP : ANTI_BOT_BUY_LP;
        uint256 aiTaxB = (amount * buyAi) / DENOMINATOR;
        uint256 lpTaxB = (amount * buyLP) / DENOMINATOR;
        return amount - _handleTaxCollection(sender, aiTaxB, lpTaxB);
    }

    function _handleSellTax(
        address sender,
        uint256 amount
    ) private returns (uint256) {
        bool chargeBaseFee = _chargeBaseFee();
        uint16 sellAi = chargeBaseFee ? _sellAi : ANTI_BOT_SELL_AI;
        uint16 sellLP = chargeBaseFee ? _sellLP : ANTI_BOT_SELL_LP;
        uint256 aiTaxS = (amount * sellAi) / DENOMINATOR;
        uint256 lpTaxS = (amount * sellLP) / DENOMINATOR;
        return amount - _handleTaxCollection(sender, aiTaxS, lpTaxS);
    }

    function _chargeBaseFee() private view returns (bool) {
        /// @dev launchedAt will always be > 0 at this point
        return launchedAt + ANTI_BOT_BLOCKS < block.number;
    }

    function _handleTaxCollection(
        address sender,
        uint256 ai,
        uint256 lp
    ) private returns (uint256 tax) {
        taxesCollected[0] += ai;
        taxesCollected[1] += lp;
        tax = ai + lp;
        _balances[address(this)] += tax;
        emit Transfer(sender, address(this), tax);
        return tax;
    }

    //LP and Swapback Functions
    function _swapTokensForETH(
        uint256 tokenAmount
    ) private lockTheSwap returns (uint256) {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        approve(address(this), tokenAmount);
        uint256 ethBefore = address(this).balance;
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
        return address(this).balance - ethBefore;
    }

    function _addLiquidity(
        uint256 tokenAmount,
        uint256 ETHAmount
    ) private lockTheSwap {
        router.addLiquidityETH{value: ETHAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            liquidityPool,
            block.timestamp
        );
    }

    function _swapBack() private {
        uint256 aiShare = taxesCollected[0];
        uint256 lpShare = taxesCollected[1];
        uint256 totalTax = aiShare + lpShare;
        uint256 tokensForLiquidity = lpShare / 2;
        uint256 amountToSwap = totalTax - tokensForLiquidity;

        uint256 ethReceived = _swapTokensForETH(amountToSwap);

        uint256 ETHForLiquidity = (ethReceived * tokensForLiquidity) /
            amountToSwap;
        uint256 ETHForAi = ethReceived - ETHForLiquidity;

        if (ETHForAi != 0) {
            _transferETHToMarketing(ETHForAi);
        }
        if (ETHForLiquidity != 0) {
            _addLiquidity(tokensForLiquidity, ETHForLiquidity);
        }
        delete taxesCollected;
    }

    function manualSwapBack() external onlyOwner {
        _swapBack();
    }

    // Update/Change Functions

    function changeIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
        emit FeeExemptionChanged(holder, exempt);
    }

    function setMarketingWallet(address newMarketingWallet) external onlyOwner {
        if (newMarketingWallet == address(0)) revert InvalidAddress();
        isFeeExempt[marketingWallet] = false;
        marketingWallet = newMarketingWallet;
        isFeeExempt[newMarketingWallet] = true;
        emit MarketingWalletUpdated(newMarketingWallet);
    }

    function setLiquidityPool(address newLiquidityPool) external onlyOwner {
        if (newLiquidityPool == address(0)) revert InvalidAddress();
        liquidityPool = newLiquidityPool;
        emit LiquidityPoolUpdated(newLiquidityPool);
    }

    function changeSwapBackSettings(
        bool enableSwapback,
        uint256 newSwapbackLimit
    ) external onlyOwner {
        if (newSwapbackLimit < MINIMUM_SWAP_LIMIT) revert InvalidAmount();
        swapAndLiquifyEnabled = enableSwapback;
        swapThreshold = newSwapbackLimit;
        emit SwapbackSettingsChanged(enableSwapback, newSwapbackLimit);
    }

    function updateBuyFees(
        uint16 newBuyAiFee,
        uint16 newBuyLpFee,
        uint16 newSellAiFee,
        uint16 newSellLpFee
    ) external onlyOwner {
        uint16 totalNewBuyFee = newBuyAiFee + newBuyLpFee;
        uint16 totalNewSellFee = newSellAiFee + newSellLpFee;
        if (totalNewBuyFee > MAX_FEE || totalNewSellFee > MAX_FEE)
            revert InvalidFee();
        _buyAi = newBuyAiFee;
        _buyLP = newBuyLpFee;
        _sellAi = newSellAiFee;
        _sellLP = newSellLpFee;
        emit BuyFeesUpdated(newBuyAiFee, newBuyLpFee);
        emit SellFeesUpdated(newSellAiFee, newSellLpFee);
    }

    function clearStuckETH() external onlyOwner {
        uint256 contractETHBalance = address(this).balance;
        if (contractETHBalance == 0) revert InvalidAmount();
        _transferETHToMarketing(contractETHBalance);
        emit StuckETHCleared(contractETHBalance);
    }

    function clearStuckTokens(IERC20 token) external onlyOwner {
        if (address(token) == address(0)) revert InvalidAddress();
        uint256 balance = token.balanceOf(address(this));
        if (balance == 0) revert InvalidAmount();
        if (address(token) == address(this)) {
            delete taxesCollected;
        }
        token.transfer(marketingWallet, balance);
        emit StuckTokensCleared(address(token), balance);
    }

    function _transferETHToMarketing(uint256 amount) private {
        (bool success, ) = marketingWallet.call{value: amount}("");
        if (!success) {
            /// @dev owner can claim ETH via clearStuckETH()
            emit StuckETH(amount);
        }
    }

    receive() external payable {}
}