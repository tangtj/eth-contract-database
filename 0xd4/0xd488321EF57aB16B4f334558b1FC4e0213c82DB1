/**
 *Submitted for verification at Etherscan.io on 2024-03-13
 */

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.15;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

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

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
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

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] + addedValue
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
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
    }

    function _createInitialSupply(address account, uint256 amount)
        internal
        virtual
    {
        require(account != address(0), "ERC20: mint to the zero address");

        _totalSupply += amount;
        _balances[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);
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
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() external virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IDexRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
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
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 airdrop
        );
}

interface IDexFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

contract WCTrades is ERC20, Ownable {
    uint256 public maxBuyLimit;
    uint256 public maxSellLimit;
    uint256 public maxWalletSize;

    IDexRouter public dexRouter;
    address public dexPair;

    bool private swapping;
    uint256 public swapTokensAtAmount;

    address marketingAddress;
    address devAddress;
    address airdropAddress;

    uint256 public tradingActiveBlock = 0; // 0 means trading is not active
    uint256 public blockForPenaltyEnd;
    mapping(address => bool) public boughtEarly;
    uint256 public botsCaught;

    bool public limitsInEffect = true;
    bool public tradingActive = false;
    bool public swapEnabled = false;

    // Anti-bot and anti-whale mappings and variables
    mapping(address => uint256) private _holderLastTransferTimestamp; // to hold last Transfers temporarily during launch
    bool public transferDelayEnabled = true;

    uint256 public buyTotalTaxes;
    uint256 public buyMarketingTax;
    uint256 public buyAirdropTax;
    uint256 public buyDevTax;

    uint256 public sellTotalTaxes;
    uint256 public sellMarketingsTax;
    uint256 public sellAirdropTax;
    uint256 public sellDevTax;

    /******************/

    // exlcude from fees and max transaction amount
    mapping(address => bool) private _isExcludedFromTaxes;
    mapping(address => bool) public _isExcludedMaxTransactionAmount;

    // store addresses that a automatic market maker pairs. Any transfer *to* these addresses
    // could be subject to a maximum transfer amount
    mapping(address => bool) public automatedMarketMakerPairs;

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    event Launched();

    event RemovedLimits();

    event ExcludeFromTaxes(address indexed account, bool isExcluded);

    event UpdatedMaxBuyAmount(uint256 newAmount);

    event UpdatedMaxSellAmount(uint256 newAmount);

    event UpdatedMaxWalletAmount(uint256 newAmount);

    event UpdatedMarketingsAddress(address indexed newWallet);

    event MaxTransactionExclusion(address _address, bool exempted);

    event BuyBackTriggered(uint256 amount);

    event OwnerForcedSwapBack(uint256 timestamp);

    event CaughtEarlyBuyer(address sniper);

    event SwapAndLiquify(
        uint256 tokensSwapped,
        uint256 ethReceived,
        uint256 tokensIntoAirdrop
    );

    event TransferForeignToken(address token, uint256 amount);

    constructor() ERC20("WCTrades", "WCT") {
        address newOwner = msg.sender;
        IDexRouter _dexRouter = IDexRouter(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        dexRouter = _dexRouter;

        // create pair
        dexPair = IDexFactory(_dexRouter.factory()).createPair(
            address(this),
            _dexRouter.WETH()
        );
        _exemptFromMaxTransaction(address(dexPair), true);
        _setAutomatedMarketMakerPair(address(dexPair), true);

        uint256 totalSupply = 1 * 1e9 * 1e18;

        maxBuyLimit = (totalSupply * 1) / 100;
        maxSellLimit = (totalSupply * 1) / 100;
        maxWalletSize = (totalSupply * 1) / 100;
        swapTokensAtAmount = 2500000 * 1e18;

        buyMarketingTax = 10;
        buyAirdropTax = 5;
        buyDevTax = 10;
        buyTotalTaxes = buyMarketingTax + buyAirdropTax + buyDevTax;

        sellMarketingsTax = 10;
        sellAirdropTax = 5;
        sellDevTax = 10;
        sellTotalTaxes = sellMarketingsTax + sellAirdropTax + sellDevTax;

        _exemptFromMaxTransaction(newOwner, true);
        _exemptFromMaxTransaction(address(this), true);
        _exemptFromMaxTransaction(address(0xdead), true);

        exemptFromTaxes(newOwner, true);
        exemptFromTaxes(address(this), true);
        exemptFromTaxes(address(0xdead), true);
        // Change Here

        marketingAddress = 0xd42be4397593986dc62151E2B681b8B28Ac9a65f;
        devAddress = 0x56Fd91Cb10CC38ED59a2FaC96cCc79484BA63157;
        airdropAddress = 0xB8813F93ba7673724c8eb22FA8451DFfcfB34F3F;

        _createInitialSupply(newOwner, totalSupply);
        transferOwnership(newOwner);
    }

    receive() external payable {}

    // only enable if no plan to airdrop

    function enableTrading(uint256 _deadblocks) external onlyOwner {
        require(!tradingActive, "Cannot reenable trading");
        tradingActive = true;
        swapEnabled = true;
        tradingActiveBlock = block.number;
        blockForPenaltyEnd = tradingActiveBlock + _deadblocks;
        emit Launched();
    }

    function disableTrading() external onlyOwner {
        require(tradingActive, "Cannot redisable trading");
        tradingActive = false;
        swapEnabled = false;
    }

    // remove limits after token is stable
    function removeLimits() external onlyOwner {
        limitsInEffect = false;
        transferDelayEnabled = false;
        emit RemovedLimits();
    }

    function manageEarly(address wallet, bool flag) external onlyOwner {
        boughtEarly[wallet] = flag;
    }

    function disableTransferDelay() external onlyOwner {
        transferDelayEnabled = false;
    }

    function updateMaxBuy(uint256 newValue) external onlyOwner {
        require(
            newValue >= ((totalSupply() * 2) / 1000) / 1e18,
            "Cannot set max buy amount lower than 0.2%"
        );
        maxBuyLimit = newValue * (10**18);
        emit UpdatedMaxBuyAmount(maxBuyLimit);
    }

    function updateMaxSell(uint256 newValue) external onlyOwner {
        require(
            newValue >= ((totalSupply() * 2) / 1000) / 1e18,
            "Cannot set max sell amount lower than 0.2%"
        );
        maxSellLimit = newValue * (10**18);
        emit UpdatedMaxSellAmount(maxSellLimit);
    }

    function updateMaxWallet(uint256 newValue) external onlyOwner {
        require(
            newValue >= ((totalSupply() * 3) / 1000) / 1e18,
            "Cannot set max wallet amount lower than 0.3%"
        );
        maxWalletSize = newValue * (10**18);
        emit UpdatedMaxWalletAmount(maxWalletSize);
    }

    function updateSwapTokensAtAmount(uint256 newAmount) external onlyOwner {
        swapTokensAtAmount = newAmount;
    }

    function _exemptFromMaxTransaction(address updAds, bool isExcluded)
        private
    {
        _isExcludedMaxTransactionAmount[updAds] = isExcluded;
        emit MaxTransactionExclusion(updAds, isExcluded);
    }

    function exemptFromMaxTX(address updAds, bool isEx) external onlyOwner {
        if (!isEx) {
            require(
                updAds != dexPair,
                "Cannot remove uniswap pair from max txn"
            );
        }
        _isExcludedMaxTransactionAmount[updAds] = isEx;
    }

    function setAMM(address pair, bool value) external onlyOwner {
        require(pair != dexPair, "The pair cannot be removed");

        _setAutomatedMarketMakerPair(pair, value);
        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;
        _exemptFromMaxTransaction(pair, value);
        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function updateTaxes(
        uint256 _marketingsTax,
        uint256 _airdropTax,
        uint256 _DevTax
    ) external onlyOwner {
        sellMarketingsTax = _marketingsTax;
        sellAirdropTax = _airdropTax;
        sellDevTax = _DevTax;

        buyMarketingTax = _marketingsTax;
        buyAirdropTax = _airdropTax;
        buyDevTax = _DevTax;

        buyTotalTaxes = buyMarketingTax + buyAirdropTax + buyDevTax;
        sellTotalTaxes = sellMarketingsTax + sellAirdropTax + sellDevTax;
        require(sellTotalTaxes <= 25, "Must keep fees at 25% or less");
    }

    function exemptFromTaxes(address account, bool exempted) public onlyOwner {
        _isExcludedFromTaxes[account] = exempted;
        emit ExcludeFromTaxes(account, exempted);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal override {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "amount must be greater than 0");

        if (!tradingActive) {
            require(
                _isExcludedFromTaxes[from] || _isExcludedFromTaxes[to],
                "Trading is not active."
            );
        }

        if (blockForPenaltyEnd > 0) {
            require(
                !boughtEarly[from] || to == owner() || to == address(0xdead),
                "Bots cannot transfer tokens in or out except to owner or dead address."
            );
        }

        if (limitsInEffect) {
            if (
                from != owner() &&
                to != owner() &&
                to != address(0) &&
                to != address(0xdead) &&
                !_isExcludedFromTaxes[from] &&
                !_isExcludedFromTaxes[to]
            ) {
                // at launch if the transfer delay is enabled, ensure the block timestamps for purchasers is set -- during launch.
                if (transferDelayEnabled) {
                    if (to != address(dexRouter) && to != address(dexPair)) {
                        require(
                            _holderLastTransferTimestamp[tx.origin] <
                                block.number - 2 &&
                                _holderLastTransferTimestamp[to] <
                                block.number - 2,
                            "_transfer:: Transfer Delay enabled.  Try again later."
                        );
                        _holderLastTransferTimestamp[tx.origin] = block.number;
                        _holderLastTransferTimestamp[to] = block.number;
                    }
                }

                //when buy
                if (
                    automatedMarketMakerPairs[from] &&
                    !_isExcludedMaxTransactionAmount[to]
                ) {
                    require(
                        amount <= maxBuyLimit,
                        "Buy transfer amount exceeds the max buy."
                    );
                    require(
                        amount + balanceOf(to) <= maxWalletSize,
                        "Cannot Exceed max wallet"
                    );
                }
                //when sell
                else if (
                    automatedMarketMakerPairs[to] &&
                    !_isExcludedMaxTransactionAmount[from]
                ) {
                    require(
                        amount <= maxSellLimit,
                        "Sell transfer amount exceeds the max sell."
                    );
                } else if (!_isExcludedMaxTransactionAmount[to]) {
                    require(
                        amount + balanceOf(to) <= maxWalletSize,
                        "Cannot Exceed max wallet"
                    );
                }
            }
        }

        uint256 contractTokenBalance = balanceOf(address(this));

        bool canSwap = contractTokenBalance >= swapTokensAtAmount;

        if (
            canSwap &&
            swapEnabled &&
            !swapping &&
            !automatedMarketMakerPairs[from] &&
            !_isExcludedFromTaxes[from] &&
            !_isExcludedFromTaxes[to]
        ) {
            swapping = true;

            swapBack();

            swapping = false;
        }

        bool takeTax = true;
        // if any account belongs to _isExcludedFromTax account then remove the fee
        if (_isExcludedFromTaxes[from] || _isExcludedFromTaxes[to]) {
            takeTax = false;
        }

        uint256 fees = 0;
        // only take fees on buys/sells, do not take on wallet transfers
        if (takeTax) {
            // bot/sniper penalty.
            if (
                earlyBuyPenaltyInEffect() &&
                automatedMarketMakerPairs[from] &&
                !automatedMarketMakerPairs[to] &&
                buyTotalTaxes > 0
            ) {
                if (!boughtEarly[to]) {
                    boughtEarly[to] = true;
                    botsCaught += 1;
                    emit CaughtEarlyBuyer(to);
                }

                fees = (amount * 99) / 100;
            }
            // on sell
            else if (automatedMarketMakerPairs[to] && sellTotalTaxes > 0) {
                fees = (amount * sellTotalTaxes) / 100;
            }
            // on buy
            else if (automatedMarketMakerPairs[from] && buyTotalTaxes > 0) {
                fees = (amount * buyTotalTaxes) / 100;
            }

            if (fees > 0) {
                super._transfer(from, address(this), fees);
            }

            amount -= fees;
        }

        super._transfer(from, to, amount);
    }

    function earlyBuyPenaltyInEffect() public view returns (bool) {
        return block.number < blockForPenaltyEnd;
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
        bool success;
        swapTokensForEth(swapTokensAtAmount);

        uint256 ethBalance = address(this).balance;
        uint256 ethForAirdrop = (ethBalance * buyAirdropTax) / buyTotalTaxes;
        uint256 ethForMarketings = (ethBalance * buyMarketingTax) /
            buyTotalTaxes;
        uint256 ethForDev = (ethBalance * buyDevTax) / buyTotalTaxes;

        (success, ) = address(devAddress).call{value: ethForDev}("");
        (success, ) = address(airdropAddress).call{value: ethForAirdrop}("");
        (success, ) = address(marketingAddress).call{value: ethForMarketings}(
            ""
        );
    }

    function withdrawStuckETH() external onlyOwner {
        bool success;
        (success, ) = address(msg.sender).call{value: address(this).balance}(
            ""
        );
    }

    function setTaxAddresses(
        address _marketingAddress,
        address _devAddress,
        address _airdropAddress
    ) external onlyOwner {
        require(
            _marketingAddress != address(0) &&
                _devAddress != address(0) &&
                _airdropAddress != address(0),
            "Addresses cannot be 0"
        );
        marketingAddress = payable(_marketingAddress);
        devAddress = payable(_devAddress);
        airdropAddress = payable(_airdropAddress);
    }

    // force Swap back if slippage issues.
    function forceSwapBack() external onlyOwner {
        swapping = true;
        swapBack();
        swapping = false;
        emit OwnerForcedSwapBack(block.timestamp);
    }

    // useful for buybacks or to reclaim any ETH on the contract in a way that helps holders.
    function buyBack(uint256 amountInWei) external onlyOwner {
        require(
            amountInWei <= 10 ether,
            "May not buy more than 10 ETH in a single buy to reduce sandwich attacks"
        );

        address[] memory path = new address[](2);
        path[0] = dexRouter.WETH();
        path[1] = address(this);

        // make the swap
        dexRouter.swapExactETHForTokensSupportingFeeOnTransferTokens{
            value: amountInWei
        }(
            0, // accept any amount of Ethereum
            path,
            address(0xdead),
            block.timestamp
        );
        emit BuyBackTriggered(amountInWei);
    }
}