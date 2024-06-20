/*
    __________  ______________  ____  ___________
   / ____/ __ \/  _/ ____/ __ )/ __ \/_  __/ ___/
  / __/ / /_/ // // /   / __  / / / / / /  \__ \ 
 / /___/ ____// // /___/ /_/ / /_/ / / /  ___/ / 
/_____/_/   /___/\____/_____/\____/ /_/  /____/                                               

Welcome to EPICBOTS – Revolutionise your crypto journey with our cutting-edge AI-powered Telegram bots.

TG: https://t.me/EpicBotsPortal
Website: https://epicbots.io
Twitter: https://twitter.com/epicbots_io
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

contract EpicBots is Context, IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;

    address payable public marketingWallet;
    address payable public payoutWallet;

    uint256 public marketingShare = 60;
    uint256 public payoutShare = 40;

    uint256 public buyTax = 5;
    uint256 public sellTax = 5;
    uint256 private taxDelimiter = 100;

    uint8 private constant _decimals = 18;
    uint256 private constant _tTotal = 100_000_000 * 10 ** _decimals;
    string private constant _name = unicode"EPICBOTS";
    string private constant _symbol = unicode"EPIC";

    uint256 public taxSwapThreshold = (_tTotal * 20) / 10000; //0.2%
    uint256 public maxTaxSwap = (_tTotal * 100) / 10000; // 1%

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool public tradingOpen;
    bool private inSwap = false;

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    event TradingOpen();

    constructor(address _marketingWallet, address _payoutWallet) {
        marketingWallet = payable(_marketingWallet);
        payoutWallet = payable(_payoutWallet);
        _balances[owner()] = _tTotal;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[marketingWallet] = true;
        _isExcludedFromFee[payoutWallet] = true;
        emit Transfer(address(0), owner(), _tTotal);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
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
        if (!_isExcludedFromFee[from] && !_isExcludedFromFee[to]) {
            if (from == uniswapV2Pair && to != address(uniswapV2Router)) {
                taxAmount = amount * buyTax / taxDelimiter;
            } else if (to == uniswapV2Pair) {
                taxAmount = amount * sellTax / taxDelimiter;
                uint256 contractTokenBalance = balanceOf(address(this));
                if (
                    !inSwap && contractTokenBalance > taxSwapThreshold
                ) {
                    uint256 amountToSwap = (amount < contractTokenBalance && amount < maxTaxSwap) ? amount : (contractTokenBalance < maxTaxSwap) ? contractTokenBalance : maxTaxSwap;
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

    function sendETHToFee(uint256 amount) private {
        bool success;
        uint256 _marketingPart = amount * marketingShare / taxDelimiter;
        if (_marketingPart > 0) {
            (success, ) = marketingWallet.call{value: _marketingPart}("");
            require(success, "Failed to send ETH");
        }
        uint256 _payoutPart = amount - _marketingPart;
        if (_payoutPart > 0){
            (success, ) = payoutWallet.call{value: _payoutPart}("");
            require(success, "Failed to send ETH");
        }
    }

    function setPayoutWallet(address _payoutWallet) external onlyOwner {
        payoutWallet = payable(_payoutWallet);
    }

    function setMarketingWallet(address _marketingWallet) external onlyOwner {
        marketingWallet = payable(_marketingWallet);
    }

    function setShareRatios(uint256 _marketingShare, uint256 _payoutShare) external onlyOwner {
        require(_marketingShare + _payoutShare == 100, "Shares must add up to 100");
        marketingShare = _marketingShare;
        payoutShare = _payoutShare;
    }

    function setTax(uint256 _buyTax, uint256 _sellTax) external onlyOwner {
        require(_buyTax <= 5 && _sellTax <= 5, "Tax cannot be more than 5%");
        buyTax = _buyTax;
        sellTax = _sellTax;
    }

    function excludeFromFee(address account, bool value) external onlyOwner {
        _isExcludedFromFee[account] = value;
    }

    function setTaxSwapThreshold(uint256 threshold) external onlyOwner {
        require(threshold > (_tTotal * 1) / 10000, "Threshold cannot be less than 0.01%");
        require(threshold < maxTaxSwap, "Threshold cannot be more than 1%");
        taxSwapThreshold = threshold;
    }

    function rescueETH() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function rescueToken(address tokenAddress) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        uint256 tokenBalance = token.balanceOf(address(this));
        require(tokenBalance > 0, "No tokens to rescue");
        bool success = token.transfer(owner(), tokenBalance);
        require(success, "ERC20 rescue failed");
    }

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

    receive() external payable {}
}