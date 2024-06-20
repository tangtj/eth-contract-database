/*
     _    _ _    ____ _____ ____  _
    / \  | | |_ / ___|_   _|  _ \| |
   / _ \ | | __| |     | | | |_) | |
  / ___ \| | |_| |___  | | |  _ <| |___
 /_/   \_\_|\__|\____| |_| |_| \_\_____|

 Website: https://altctrl.com/
 Telegram: https://t.me/OffcialAltCTRL/
 Twitter: https://twitter.com/OfficialAltCTRL/
*/

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.11;

abstract contract Context {
    function _msgSender() internal view returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {
  function totalSupply() external view returns (uint256);
  function decimals() external view returns (uint8);
  function symbol() external view returns (string memory);
  function name() external view returns (string memory);
  function getOwner() external view returns (address);
  function balanceOf(address account) external view returns (uint256);
  function transfer(address recipient, uint256 amount) external returns (bool);
  function allowance(address _owner, address spender) external view returns (uint256);
  function approve(address spender, uint256 amount) external returns (bool);
  function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
  event Transfer(address indexed from, address indexed to, uint256 value);
  event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address lpPair, uint);
    function getPair(address tokenA, address tokenB) external view returns (address lpPair);
    function createPair(address tokenA, address tokenB) external returns (address lpPair);
}

interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);
    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function factory() external view returns (address);
}

interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

interface IUniswapV2Router02 is IUniswapV2Router01 {
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

contract AltCTRL is Context, IERC20 {
    // Ownership moved to in-contract for customizability.
    address private _owner;

    mapping (address => uint256) private _tOwned;
    mapping (address => bool) lpPairs;
    uint256 private timeSinceLastPair = 0;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) private _liquidityHolders;
    mapping (address => bool) private _isExcludedFromFees;
    mapping (address => bool) public ExcludedFromWalletRestrictions;

    mapping (address => bool) private _isSniper;

    mapping (address => uint256) private lastTrade;

    uint256 private startingSupply = 42_000_000;

    string private _name = "AltCTRL";
    string private _symbol = "CTRL";
//==========================
    // FEES
    struct taxes {
    uint buyFee;
    uint sellFee;
    uint transferFee;
    }

    taxes public Fees = taxes(
    {buyFee: 3000, sellFee: 3000, transferFee: 3000}); // token starts at 30%
//==========================

    struct Maxima {
    uint maxBuy;
    uint maxSell;
    uint maxTransfer;
    }

    Maxima public maxFees = Maxima(
    {maxBuy: 500, maxSell: 500, maxTransfer: 500});

//==========================
    //Proportions of Taxes
    struct feeProportions {
    uint liquidity;
    uint stockpileFee;
    uint TreasuryFee;
    uint developmentFee;
    }

    feeProportions public Ratios = feeProportions(
    { liquidity:200 , stockpileFee: 0, TreasuryFee: 400, developmentFee: 400});

    uint256 private constant masterTaxDivisor = 10000;
    uint256 private constant MAX = ~uint256(0);
    uint8 constant private _decimals = 9;

    uint256 private _tTotal = startingSupply * 10**_decimals;
    uint256 private _tFeeTotal;

    IUniswapV2Router02 public dexRouter;
    address public lpPair;

    address constant private _routerAddress = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D;

    address constant public DEAD = 0x000000000000000000000000000000000000dEaD;

    address payable public _treasuryWallet = payable(0x9C5e41A241b822eFeb2B2bc11d6cE549b41E2cCb);  // Also acts as the wallet for the optional stockpiling of tokens.
    address payable public _developmentWallet = payable(0x246b6e01092f08ea5d4790195CEB2FDD0F626cCa);
    address payable public _liquidityWallet = payable(0x04DDB1aB5d2f702ca116a2b6a158B4398cEC2205);

    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = false;

    uint256 private maxTxPercent = 1; // 1%
    uint256 private maxTxDivisor = 100;

    uint256 private _maxTxAmount = (_tTotal * maxTxPercent) / maxTxDivisor;

    uint256 private maxWalletPercent = 2; // 2%
    uint256 private maxWalletDivisor = 100;
    uint256 private _maxWalletSize = (_tTotal * maxWalletPercent) / maxWalletDivisor;

    uint256 private swapThreshold = (_tTotal * 5) / 10_000;
    uint256 private swapAmount = (_tTotal * 5) / 1_000;

    bool private sniperProtection = true;
    bool public _hasLiqBeenAdded = false;
    uint256 private _liqAddStatus = 0;
    uint256 private _liqAddBlock = 0;
    uint256 private _liqAddStamp = 0;
    uint256 private _initialLiquidityAmount = 0; // make constant
    uint256 private snipeBlockAmt = 0;
    uint256 public snipersCaught = 0;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event MinTokensBeforeSwapUpdated(uint256 minTokensBeforeSwap);
    event SwapAndLiquifyEnabledUpdated(bool enabled);
    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoLiqudity
    );
    event SniperCaught(address sniperAddress);

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Caller != owner.");
        _;
    }

    constructor () payable {
        _tOwned[_msgSender()] = _tTotal;

        // Set the owner.
        _owner = msg.sender;

        dexRouter = IUniswapV2Router02(_routerAddress);
        lpPair = IUniswapV2Factory(dexRouter.factory()).createPair(dexRouter.WETH(), address(this));
        lpPairs[lpPair] = true;
        _allowances[address(this)][address(dexRouter)] = type(uint256).max;

        _isExcludedFromFees[owner()] = true;
        _isExcludedFromFees[address(this)] = true;
        _isExcludedFromFees[_treasuryWallet] = true;
        _isExcludedFromFees[DEAD] = true;

        _liquidityHolders[owner()] = true;
        _liquidityHolders[_treasuryWallet] = true;
        _liquidityHolders[_developmentWallet] = true;

        // Approve the owner for Uniswap, time saver.
        _approve(_msgSender(), _routerAddress, _tTotal);

        // Event regarding the tTotal transferred to the _msgSender.
        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    receive() external payable {}

//===============================================================================================================
//===============================================================================================================
//===============================================================================================================

    // Ownable removed as a lib and added here to allow for custom transfers and renouncement.
    // This allows for removal of ownership privileges from the owner once renounced or transferred.
    function owner() public view returns (address) {
        return _owner;
    }

    function transferOwner(address newOwner) external onlyOwner() {
        require(newOwner != address(0), "Call renounceOwnership to transfer owner to the zero address.");
        require(newOwner != DEAD, "Call renounceOwnership to transfer owner to the zero address.");
        setExcludedFromFees(_owner, false);
        setExcludedFromFees(newOwner, true);


        _allowances[_owner][newOwner] = balanceOf(_owner);
        if(balanceOf(_owner) > 0) {
            _transfer(_owner, newOwner, balanceOf(_owner));
        }

        _owner = newOwner;
        emit OwnershipTransferred(_owner, newOwner);
    }

    function renounceOwnership() public virtual onlyOwner() {
        setExcludedFromFees(_owner, false);
        _owner = address(0);
        emit OwnershipTransferred(_owner, address(0));
    }

//===============================================================================================================
//===============================================================================================================
//===============================================================================================================

    function totalSupply() external view override returns (uint256) { return _tTotal; }
    function decimals() external pure override returns (uint8) { return _decimals; }
    function symbol() external view override returns (string memory) { return _symbol; }
    function name() external view override returns (string memory) { return _name; }
    function getOwner() external view override returns (address) { return owner(); }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address sender, address spender, uint256 amount) private {
        require(sender != address(0), "ERC20: Zero Address");
        require(spender != address(0), "ERC20: Zero Address");

        _allowances[sender][spender] = amount;
        emit Approval(sender, spender, amount);
    }

    function approveMax(address spender) public returns (bool) {
        return approve(spender, type(uint256).max);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if (_allowances[sender][msg.sender] != type(uint256).max) {
            _allowances[sender][msg.sender] -= amount;
        }

        return _transfer(sender, recipient, amount);
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender] - subtractedValue);
        return true;
    }

    function setNewRouter(address newRouter) public onlyOwner() {
        IUniswapV2Router02 _newRouter = IUniswapV2Router02(newRouter);
        address get_pair = IUniswapV2Factory(_newRouter.factory()).getPair(address(this), _newRouter.WETH());
        if (get_pair == address(0)) {
            lpPair = IUniswapV2Factory(_newRouter.factory()).createPair(address(this), _newRouter.WETH());
        }
        else {
            lpPair = get_pair;
        }
        dexRouter = _newRouter;
    }

    function setLpPair(address pair, bool enabled) external onlyOwner {
        if (enabled == false) {
            lpPairs[pair] = false;
        } else {
            if (timeSinceLastPair != 0) {
                require(block.timestamp - timeSinceLastPair > 1 weeks, "One week cool down.");
            }
            lpPairs[pair] = true;
            timeSinceLastPair = block.timestamp;
        }
    }

    function isExcludedFromFees(address account) public view returns(bool) {
        return _isExcludedFromFees[account];
    }

    function setExcludedFromFees(address account, bool enabled) public onlyOwner {
        _isExcludedFromFees[account] = enabled;
    }

    // used for dApps e.g staking or some other utility or treasuries.
    function excludeWalletRestrictions(address excludeAddress) public onlyOwner{
        ExcludedFromWalletRestrictions[excludeAddress] = true;
    }

    function revokeWalletFreedom(address includeAddress) public onlyOwner{
        ExcludedFromWalletRestrictions[includeAddress] = false;
    }

    function isSniper(address account) public view returns (bool) {
        return _isSniper[account];
    }

    function init() external onlyOwner {
        require (_liqAddStatus == 0, "Error.");
        _liqAddStatus = 1;
        snipeBlockAmt = 0;
    }

    function setBlacklistEnabled(address account, bool enabled) external onlyOwner() {
        _isSniper[account] = enabled;
    }

    function setRatios(uint _liquidity, uint _TreasuryFee, uint _developmentFee , uint _stockpileFee) external onlyOwner {
        require ( (_liquidity + _TreasuryFee + _developmentFee + _stockpileFee) == 100, "!= 100"); // to change the ratio, it must require the sum equal 100
        Ratios.liquidity = _liquidity;
        Ratios.TreasuryFee = _TreasuryFee;
        Ratios.developmentFee = _developmentFee;
        Ratios.stockpileFee = _stockpileFee;
    }

    function setBuyFee(uint _buyFee) external onlyOwner {
        require(_buyFee <= maxFees.maxBuy, "Cannot exceed maximums.");
         Fees.buyFee = _buyFee;
    }

    function setSellFee(uint _sellFee) external onlyOwner {
        require(_sellFee <= maxFees.maxSell, "Cannot exceed maximums.");
         Fees.sellFee = _sellFee;
    }

    function setTaxes(uint _buyFee, uint _sellFee, uint _transferFee) external onlyOwner {
        require(_buyFee <= maxFees.maxBuy
                && _sellFee <= maxFees.maxSell
                && _transferFee <= maxFees.maxTransfer,
                "Cannot exceed maximums.");
         Fees.buyFee = _buyFee;
         Fees.sellFee = _sellFee;
         Fees.transferFee = _transferFee;
    }

    function setMaxTxPercent(uint percent, uint divisor) external onlyOwner {
        uint256 check = (_tTotal * percent) / divisor;
        require(check >= (_tTotal / 300), "Must be above 0.33~% of total supply.");
        _maxTxAmount = check;
    }

    function setMaxWalletSize(uint percent, uint divisor) external onlyOwner {
        uint256 check = (_tTotal * percent) / divisor;
        require(check >= (_tTotal / 300), "Must be above 0.33~% of total supply.");
        _maxWalletSize = check;

    }

    function setSwapSettings(uint256 thresholdPercent, uint256 thresholdDivisor, uint256 amountPercent, uint256 amountDivisor) external onlyOwner {
        swapThreshold = (_tTotal * thresholdPercent) / thresholdDivisor;
        swapAmount = (_tTotal * amountPercent) / amountDivisor;
    }

    function setWallets(address payable treasuryWallet, address payable developmentWallet) external onlyOwner {
        _treasuryWallet = payable(treasuryWallet);
        _developmentWallet = payable(developmentWallet);
    }

    function setLiquidityWallet(address payable liquidityReceiver) external onlyOwner {
        _liquidityWallet = payable(liquidityReceiver);

    }

    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }

    function _hasLimits(address from, address to) private view returns (bool) {
        return from != owner()
            && to != owner()
            && !_liquidityHolders[to]
            && !_liquidityHolders[from]
            && to != DEAD
            && to != address(0)
            && from != address(this);
    }

    function _transfer(address from, address to, uint256 amount) internal returns (bool) {
        require(from != address(0), "ERC20: Zero address.");
        require(to != address(0), "ERC20: Zero address.");
        require(amount > 0, "Must >0.");
        if(_hasLimits(from, to)) {

            if(!(ExcludedFromWalletRestrictions[from] || ExcludedFromWalletRestrictions[to])) {
                if(lpPairs[from] || lpPairs[to]){
                require(amount <= _maxTxAmount, "Exceeds the maxTxAmount.");
                }
                if(to != _routerAddress && !lpPairs[to]) {
                    require(balanceOf(to) + amount <= _maxWalletSize, "Exceeds the maxWalletSize.");
                }

            }

        }
        bool takeFee = true;
        if(_isExcludedFromFees[from] || _isExcludedFromFees[to]){
            takeFee = false;
        }

        if (lpPairs[to]) {
            if (!inSwapAndLiquify
                && swapAndLiquifyEnabled
            ) {
                uint256 contractTokenBalance = balanceOf(address(this));
                if (contractTokenBalance >= swapThreshold) {
                    if(contractTokenBalance >= swapAmount) { contractTokenBalance = swapAmount; }
                    swapAndLiquify(contractTokenBalance);
                }
            }
        }
        return _finalizeTransfer(from, to, amount, takeFee);
    }

    function swapAndLiquify(uint256 contractTokenBalance) private lockTheSwap {
        if (Ratios.liquidity + Ratios.TreasuryFee + Ratios.developmentFee == 0)
            return;
        uint256 toLiquify = ((contractTokenBalance * Ratios.liquidity) / (Ratios.liquidity + Ratios.TreasuryFee + Ratios.developmentFee) ) / 2;

        uint256 toSwapForEth = contractTokenBalance - toLiquify;
        swapTokensForEth(toSwapForEth);

        uint256 currentBalance = address(this).balance;
        uint256 liquidityBalance = ((currentBalance * Ratios.liquidity) / (Ratios.liquidity + Ratios.TreasuryFee + Ratios.developmentFee) ) / 2;

        bool success;

        if (toLiquify > 0) {
            addLiquidity(toLiquify, liquidityBalance);
            emit SwapAndLiquify(toLiquify, liquidityBalance, toLiquify);
        }
        if (contractTokenBalance - toLiquify > 0) {
            uint ethBal = address(this).balance;
            uint ethBalForOperations = ((ethBal * Ratios.TreasuryFee) / (Ratios.TreasuryFee + Ratios.developmentFee));

            (success,) = address(_treasuryWallet).call{value: ethBalForOperations}("");
            (success,) = address(_developmentWallet).call{value: address(this).balance}("");
        }
    }

    function swapTokensForEth(uint256 tokenAmount) internal {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = dexRouter.WETH();

        dexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this),
            block.timestamp
        );
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        dexRouter.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            _liquidityWallet,
            block.timestamp
        );
    }

    function _checkLiquidityAdd(address from, address to) private {
        require(!_hasLiqBeenAdded, "Liquidity already added and marked.");
        if (!_hasLimits(from, to) && to == lpPair) {
            if (snipeBlockAmt != 0) {
                _liqAddBlock = block.number; // removed + 5000
            } else {
                _liqAddBlock = block.number;
            }
            _liquidityHolders[from] = true;
            _hasLiqBeenAdded = true;
            _liqAddStamp = block.timestamp;

            swapAndLiquifyEnabled = true;
            emit SwapAndLiquifyEnabledUpdated(true);
        }
    }

    function _finalizeTransfer(address from, address to, uint256 amount, bool takeFee) private returns (bool) {
        if (sniperProtection){
            if (isSniper(from) || isSniper(to)) {
                revert("Sniper rejected.");
            }

            if (!_hasLiqBeenAdded) {
                _checkLiquidityAdd(from, to);
                if (!_hasLiqBeenAdded && _hasLimits(from, to)) {
                    revert("Only owner can transfer at this time.");
                }
            } else {
                if (_liqAddBlock > 0
                    && lpPairs[from]
                    && _hasLimits(from, to)
                ) {
                    if (block.number - _liqAddBlock < snipeBlockAmt) {
                        _isSniper[to] = true;
                        snipersCaught ++;
                        emit SniperCaught(to);
                    }
                }
            }
        }

        _tOwned[from] -= amount;
        uint256 amountReceived = (takeFee) ? takeTaxes(from, to, amount) : amount; //A
        _tOwned[to] += amountReceived;

        emit Transfer(from, to, amountReceived);
        return true;
    }

    function takeTaxes(address from, address to, uint256 amount) internal returns (uint256) {
        uint256 currentFee;

        if (to == lpPair) {currentFee = Fees.sellFee;}

        else if (from == lpPair) {currentFee = Fees.buyFee;}

        else {currentFee = Fees.transferFee;}

        if (_hasLimits(from, to)){
            if (_liqAddStatus == 0 || _liqAddStatus != (1)) {
                revert();
            }
        }
        uint256 stockpileFeeAmt = (amount * currentFee * Ratios.stockpileFee) / (Ratios.stockpileFee + Ratios.liquidity + Ratios.TreasuryFee + Ratios.developmentFee ) / masterTaxDivisor;
        uint256 feeAmount = (amount * currentFee / masterTaxDivisor) - stockpileFeeAmt;
        _tOwned[_treasuryWallet] += stockpileFeeAmt;
        _tOwned[address(this)] += (feeAmount);
        emit Transfer(from, _treasuryWallet, stockpileFeeAmt);
        emit Transfer(from, address(this), feeAmount);
        return amount - feeAmount - stockpileFeeAmt;
    }
}