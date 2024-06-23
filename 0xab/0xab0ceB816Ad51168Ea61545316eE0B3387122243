/*
   _____ ____  ____________
  / ___// __ \/ ____/ ____/
  \__ \/ /_/ / __/ / /     
 ___/ / ____/ /___/ /___   
/____/_/   /_____/\____/   
   _____                       __      __          ____  ___   ____ 
  / ___/____  ___  _______  __/ /___ _/ /____     / __ \/   | / __ \
  \__ \/ __ \/ _ \/ ___/ / / / / __ `/ __/ _ \   / / / / /| |/ / / /
 ___/ / /_/ /  __/ /__/ /_/ / / /_/ / /_/  __/  / /_/ / ___ / /_/ / 
/____/ .___/\___/\___/\__,_/_/\__,_/\__/\___/  /_____/_/  |_\____/

MULTIPLE STREAMS OF PASSIVE INCOME FROM CRYPTO? WELCOME TO SPECULATE DAO

TG: https://t.me/speculatedao
Website: https://SpeculateDAO.com
Medium: https://medium.com/@teamspeculate
Docs: https://speculate.gitbook.io/introduction/
Twitter: https://x.com/SpeculateDAO

*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.24;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
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

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

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

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

interface IUniswapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);

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
}

contract SPEC is Context, IERC20, Ownable {
    // Storage
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;

    // Constants
    uint256 private constant MAX_TAX_RATE = 5;
    uint8 private constant DECIMALS = 18;
    uint256 private constant TOTAL_SUPPLY = 100_000_000 * 10 ** DECIMALS;
    uint256 private constant TAX_DELIMITER = 100;
    uint256 private constant MIN_TAX_SWAP_THRESHOLD = (TOTAL_SUPPLY * 1) / 10000; // 0.01%
    uint256 private constant MAX_TAX_SWAP = (TOTAL_SUPPLY * 100) / 10000; // 1%
    string private constant NAME = unicode"Speculate";
    string private constant SYMBOL = unicode"SPEC";

    // Wallets
    address payable public marketingWallet;
    address payable public daoWallet;
    address private uniswapV2Pair;

    // Variables
    uint256 public marketingShare = 40; // Percentage of tax allocated to marketing wallet
    uint256 public daoShare = 60; // Percentage of tax allocated to DAO wallet
    uint256 public buyTax = MAX_TAX_RATE; // Buy tax rate as a percentage
    uint256 public sellTax = MAX_TAX_RATE; // Sell tax rate as a percentage
    uint256 public taxSwapThreshold = (TOTAL_SUPPLY * 5) / 10000; // Threshold to trigger token swap to ETH (0.05%)
    bool public tradingOpen;

    // State variables
    bool private inSwap = false; // Prevents re-entrancy during token swap

    // Interfaces
    IUniswapV2Router02 private uniswapV2Router;

    // Modifiers
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    // Events
    event TradingOpen();
    event TaxChanged(uint256 buyTax, uint256 sellTax);
    event RouterUpdated(address indexed newRouter, address indexed oldRouter);

    constructor(address _marketingWallet, address _daoWallet) {
        marketingWallet = payable(_marketingWallet);
        daoWallet = payable(_daoWallet);
        _balances[owner()] = TOTAL_SUPPLY;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[marketingWallet] = true;
        _isExcludedFromFee[daoWallet] = true;
        emit Transfer(address(0), owner(), TOTAL_SUPPLY);
    }

    function name() public pure returns (string memory) {
        return NAME;
    }

    function symbol() public pure returns (string memory) {
        return SYMBOL;
    }

    function decimals() public pure returns (uint8) {
        return DECIMALS;
    }

    function totalSupply() public pure override returns (uint256) {
        return TOTAL_SUPPLY;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "Transfer amount exceeds allowance");
            unchecked {
                _approve(sender, _msgSender(), currentAllowance - amount);
            }
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "Approve from the zero address");
        require(spender != address(0), "Approve to the zero address");
        if (_allowances[owner][spender] != amount) {
            _allowances[owner][spender] = amount;
            emit Approval(owner, spender, amount);
        }
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "Transfer from the zero address");
        require(amount > 0, "Amount must be more than zero");
        uint256 taxAmount = 0;

        // Apply tax if neither sender nor recipient is excluded from fee
        if (!_isExcludedFromFee[from] && !_isExcludedFromFee[to]) {
            // Buy transaction
            if (from == uniswapV2Pair && to != address(uniswapV2Router)) {
                taxAmount = amount * buyTax / TAX_DELIMITER;
            // Sell transaction
            } else if (to == uniswapV2Pair) {
                taxAmount = amount * sellTax / TAX_DELIMITER;
                uint256 contractTokenBalance = balanceOf(address(this));
                // Swap tokens for ETH if threshold is met
                if (!inSwap && contractTokenBalance > taxSwapThreshold) {
                    uint256 amountToSwap = (amount < contractTokenBalance && amount < MAX_TAX_SWAP) ? amount : (contractTokenBalance < MAX_TAX_SWAP) ? contractTokenBalance : MAX_TAX_SWAP;
                    swapTokensForEth(amountToSwap);
                    sendETHToFee(address(this).balance);
                }
            }
        }

        if (taxAmount > 0) {
            _balances[address(this)] += taxAmount;
            emit Transfer(from, address(this), taxAmount);
        }
        _balances[from] -= amount;
        _balances[to] += (amount - taxAmount);
        emit Transfer(from, to, amount - taxAmount);
    }

    // Swap tokens for ETH
    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    // Distribute ETH to marketing and DAO wallets
    function sendETHToFee(uint256 amount) private {
        bool success;
        uint256 _marketingPart = amount * marketingShare / TAX_DELIMITER;
        if (_marketingPart > 0) {
            (success, ) = marketingWallet.call{value: _marketingPart}("");
            require(success, "Failed to send ETH");
        }
        uint256 _daoPart = amount - _marketingPart;
        if (_daoPart > 0){
            (success, ) = daoWallet.call{value: _daoPart}("");
            require(success, "Failed to send ETH");
        }
    }

    // Update DAO wallet address
    function setDaoWallet(address _daoWallet) external onlyOwner {
        daoWallet = payable(_daoWallet);
    }

    // Update marketing wallet address
    function setMarketingWallet(address _marketingWallet) external onlyOwner {
        marketingWallet = payable(_marketingWallet);
    }

    // Set tax distribution shares
    function setShareRatios(uint256 _marketingShare, uint256 _daoShare) external onlyOwner {
        require(_marketingShare + _daoShare == TAX_DELIMITER, "Shares must add up to 100");
        marketingShare = _marketingShare;
        daoShare = _daoShare;
    }

    // Set buy and sell tax rates
    function setTax(uint256 _buyTax, uint256 _sellTax) external onlyOwner {
        require(_buyTax <= MAX_TAX_RATE && _sellTax <= MAX_TAX_RATE, "Tax cannot be more than 5%");
        buyTax = _buyTax;
        sellTax = _sellTax;
        emit TaxChanged(_buyTax, _sellTax);
    }

    // Exclude or include account from fees
    function excludeFromFee(address account, bool value) external onlyOwner {
        _isExcludedFromFee[account] = value;
    }

    // Set threshold for token swap to ETH
    function setTaxSwapThreshold(uint256 threshold) external onlyOwner {
        require(threshold > MIN_TAX_SWAP_THRESHOLD, "Threshold cannot be less than 0.01%");
        require(threshold < MAX_TAX_SWAP, "Threshold cannot be more than 1%");
        taxSwapThreshold = threshold;
    }

    // Rescue ETH from contract
    function rescueETH() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    // Rescue ERC20 tokens from contract
    function rescueToken(address tokenAddress) external onlyOwner {
        require(tokenAddress != address(this), "Cannot rescue contract's own tokens");
        IERC20 token = IERC20(tokenAddress);
        uint256 tokenBalance = token.balanceOf(address(this));
        require(tokenBalance > 0, "No tokens to rescue");
        bool success = token.transfer(owner(), tokenBalance);
        require(success, "ERC20 rescue failed");
    }

    // Open trading and add liquidity to Uniswap
    function openTrading() external onlyOwner {
        require(!tradingOpen, "Trading is already open");
        uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        _approve(address(this), address(uniswapV2Router), type(uint256).max);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );
        uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            balanceOf(address(this)),
            0,
            0,
            owner(),
            block.timestamp
        );
        IERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);
        tradingOpen = true;
        emit TradingOpen();
    }

    // Update the Uniswap router address
    function setRouterAddress(address newRouter) external onlyOwner {
        require(newRouter != address(0), "Router address cannot be zero address");
        address oldRouter = address(uniswapV2Router);
        uniswapV2Router = IUniswapV2Router02(newRouter);
        emit RouterUpdated(newRouter, oldRouter);
    }

    receive() external payable {}
}