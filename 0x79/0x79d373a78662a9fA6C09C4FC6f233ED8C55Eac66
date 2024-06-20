// SPDX-License-Identifier: Unlicensed
pragma solidity 0.8.20;

/*

https://x.com/elonmusk/status/1756750454237659616?s=20

Telegram: https://t.me/BoobsTokenPortal

*/

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
}

contract Ownable is Context {
    address private _owner;

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

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
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

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn, 
        uint amountOutMin, 
        address[] calldata path, 
        address to, 
        uint deadline
    ) external;
}

contract BOOBS is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string private constant _name = unicode"Boobs Just Rock";
    string private constant _symbol = unicode"BOOBS";

    uint8 private constant _decimals = 9;
    uint256 private constant _totalSupply = 1_000_000_000 * (10 ** _decimals);

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    IUniswapV2Router02 private _uniswapV2Router;
    address private _uniswapV2Pair;

    address private _taxWallet;

    mapping(address => bool) private _isExcludedFromFee;

    uint256 private _initialBuyTax = 22;
    uint256 private _finalBuyTax = 0;
    uint256 private _reduceBuyTaxAt = 24;

    uint256 private _initialSellTax = 24;
    uint256 private _finalSellTax = 0;
    uint256 private _reduceSellTaxAt = 26;

    uint256 private _buyCount;
    uint256 private _sellCount;
    uint256 private _lastSellBlock;
    uint256 private _preventSwapBefore = 26;
    uint256 private _maxSellsPerBlock = 3;

    uint256 private _maxTxAmount = _totalSupply.mul(2).div(100);
    uint256 private _maxWalletAmount = _totalSupply.mul(2).div(100);

    uint256 private _swapThresholdAmount = _totalSupply.div(100);
    uint256 private _maxSwapAmount = _totalSupply.div(100);

    bool private _inSwap;
    bool private _swapEnabled;
    bool private _tradingEnabled;

    bool private _limitsEnabled = true;
    uint256 private _launchBlock;
    uint256 private _disableLimitsAfterBlock = 10;

    modifier onlyTaxWallet() {
        require(_msgSender() == _taxWallet, "Caller not authorized");
        _;
    }

    modifier lockTheSwap() {
        _inSwap = true;
        _;
        _inSwap = false;
    }

    constructor() payable {
        require(msg.value > 0, "Not enough ETH to launch");

        _taxWallet = _msgSender();
        address devWallet = 0x5C1AB69Bd46A5548BF2512Ffa1c02A7Bc347dA2C;

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallet] = true;
        _isExcludedFromFee[devWallet] = true;

        uint256 taxWalletAmount = _totalSupply.mul(2).div(100);
        uint256 devWalletAmount = _totalSupply.mul(2).div(100);
        uint256 liquidityAmount = _totalSupply.sub(taxWalletAmount.add(devWalletAmount));

        _balances[_taxWallet] = taxWalletAmount;
        _balances[devWallet] = devWalletAmount;
        _balances[address(this)] = liquidityAmount;

        emit Transfer(address(0), _taxWallet, taxWalletAmount);
        emit Transfer(address(0), devWallet, devWalletAmount);
        emit Transfer(address(0), address(this), liquidityAmount);
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
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 taxAmount = 0;

        if (_tradingEnabled && _limitsEnabled && block.number > _launchBlock.add(_disableLimitsAfterBlock)) {
            _disableLimits();
        }

        if (from != owner() && to != owner()) {
            taxAmount = amount.mul((_buyCount > _reduceBuyTaxAt) ? _finalBuyTax : _initialBuyTax).div(100);

            if (from == _uniswapV2Pair && to != address(_uniswapV2Router) && !_isExcludedFromFee[to]) {
                require(amount <= _maxTxAmount, "Exceeds the max TX amount");
                require(balanceOf(to) + amount <= _maxWalletAmount, "Exceeds the max wallet amount");

                _buyCount++;
            }

            if (to != _uniswapV2Pair && !_isExcludedFromFee[to]) {
                require(balanceOf(to) + amount <= _maxWalletAmount, "Exceeds the max wallet amount");
            }

            if (to == _uniswapV2Pair && from != address(this)) {
                taxAmount = amount.mul((_buyCount > _reduceSellTaxAt) ? _finalSellTax : _initialSellTax).div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));

            if (
                !_inSwap && 
                to == _uniswapV2Pair && 
                _swapEnabled && 
                _buyCount > _preventSwapBefore && 
                contractTokenBalance > _swapThresholdAmount
            ) {
                if (block.number > _lastSellBlock) {
                    _sellCount = 0;
                }

                require(_sellCount < _maxSellsPerBlock, "Max sells per block exceeded");

                _sellCount++;
                _lastSellBlock = block.number;

                _swapTokensForEth(_min(amount, _min(contractTokenBalance, _maxSwapAmount)));

                _sendETHToFee();
            }
        }

        if (taxAmount > 0) {
            _balances[address(this)] = _balances[address(this)].add(taxAmount);

            emit Transfer(from, address(this), taxAmount);
        }

        _balances[from] = _balances[from].sub(amount);
        _balances[to] = _balances[to].add(amount.sub(taxAmount));

        emit Transfer(from, to, amount.sub(taxAmount));
    }

    function _min(uint256 a, uint256 b) private pure returns (uint256) {
        return (a > b) ? b : a;
    }

    function _swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _uniswapV2Router.WETH();

        _approve(address(this), address(_uniswapV2Router), tokenAmount);

        try _uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        ) {} catch {}
    }

    function _sendETHToFee() private {
        uint256 contractETHBalance = address(this).balance;

        if (contractETHBalance == 0) {
            return;
        }

        bool success;
        (success,) = address(_taxWallet).call{value: contractETHBalance}("");
    }

    function _disableLimits() private {
        _maxTxAmount = totalSupply();
        _maxWalletAmount = totalSupply();

        _limitsEnabled = false;
    }

    function disableLimits() external onlyTaxWallet {
        _disableLimits();
    }

    function reduceTaxes(uint256 buyTax_, uint256 sellTax_) external onlyTaxWallet {
        require(buyTax_ <= _finalBuyTax, "New buy tax cannot exceed current buy tax");
        require(sellTax_ <= _finalSellTax, "New sell tax cannot exceed current sell tax");

        _finalBuyTax = buyTax_;
        _finalSellTax = sellTax_;
    }

    function manualSwap() external onlyTaxWallet {
        uint256 contractTokenBalance = balanceOf(address(this));

        if (contractTokenBalance > 0) {
            _swapTokensForEth(contractTokenBalance);

            _sendETHToFee();
        }
    }

    function manualSendETH() external onlyTaxWallet {
        _sendETHToFee();
    }

    function manualSendTokens(uint256 tokenAmount) external onlyTaxWallet {
        require(tokenAmount <= balanceOf(address(this)), "Transfer amount exceeds balance");

        IERC20(address(this)).transfer(_msgSender(), tokenAmount);
    }

    function openTrading() external onlyOwner {
        require(!_tradingEnabled, "Trading already open");

        _uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);

        _approve(address(this), address(_uniswapV2Router), totalSupply());

        _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());

        _uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            balanceOf(address(this)),
            0,
            0,
            owner(),
            block.timestamp
        );

        IERC20(_uniswapV2Pair).approve(address(_uniswapV2Router), type(uint256).max);

        _swapEnabled = true;
        _tradingEnabled = true;
        _launchBlock = block.number;
    }

    receive() external payable {}
}