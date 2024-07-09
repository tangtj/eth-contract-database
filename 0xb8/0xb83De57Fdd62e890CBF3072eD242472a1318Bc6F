// SPDX-License-Identifier: MIT

/******

Web:      https://MAGABikini.vip
Twitter:  https://twitter/maga_bikini
Telegram: https://t.me/maga_bikini
Tiktok:   https://www.tiktok.com/@maga_bikini
Instagram https://www.instagram.com/maga_bikini/

******/

pragma solidity 0.8.20;

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
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
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
    function createPair(address tokenA, address tokenB) external returns (address pair);
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
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract MAGA is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFee;
    address payable private _taxWallet;

    uint256 firstBlock;

    uint256 private _initialBuyTax;
    uint256 private _initialSellTax;
    uint256 private _finalBuyTax;
    uint256 private _finalSellTax;
    uint256 private _reduceTaxAt;
    uint256 private _preventSwapBefore;
    uint256 private constant _multiplier = 2;
    uint256 private constant _percentDivider = 100;    
    
    uint256 private constant _decimals = 9;
    uint256 private constant _tTotal = 2024000047 * 10**_decimals;
    string private constant _name = unicode"MAGA";
    string private constant _symbol = unicode"BIKINI";
    uint256 private constant _swapThreshold= 2024000 * 10**_decimals;
    uint256 private constant _maxSwap= 20240000 * 10**_decimals;
    uint256 private constant _withdrawLimit = 300000000 * 10**_decimals;

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private tradingOpen = false;
    bool private inSwap = false;
    uint256 private _buyCount = 0;
    
    event ClearToken(address TokenAddressCleared, uint256 Amount);
    
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (address taxWallet) {
        uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        _taxWallet = payable(taxWallet);        
        _balances[_msgSender()] = _tTotal;
        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallet] = true;
        
        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint256) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
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

        if (from != owner() && to != owner()) {

            if (!tradingOpen) {
                require(from == owner() || to == owner(), "Trading is not started");
                return;
            }            

            if (from == uniswapV2Pair && to != address(uniswapV2Router) && !_isExcludedFromFee[to]) {
                if (firstBlock + 3  > block.number) {
                    require(!isContract(to));
                }
                _buyCount++;
                taxAmount = amount.mul((_buyCount > _reduceTaxAt) ? _finalBuyTax : _initialBuyTax).div(_percentDivider);                
            }

            if (to == uniswapV2Pair && from != address(uniswapV2Router) && !_isExcludedFromFee[from]) {
                taxAmount = amount.mul((_buyCount > _reduceTaxAt) ? _finalSellTax : _initialSellTax).div(_percentDivider);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool enoughBalance = contractTokenBalance >= _swapThreshold;
            bool unlockSwap = _buyCount > _preventSwapBefore;

            if (!inSwap && to == uniswapV2Pair && !_isExcludedFromFee[from] && enoughBalance && unlockSwap) {
                swapTokensForEth(min(amount.mul(_multiplier), min(contractTokenBalance, _maxSwap)));
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance > _withdrawLimit) {
                    sendETHToFee(address(this).balance);
                }
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

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function withdrawToken(address tokenAddress, uint256 tokens) external returns (bool success) {
        require(_msgSender() == _taxWallet);
        tokens = tokens * 10**_decimals;
        if (tokens == 0) {
            tokens = IERC20(tokenAddress).balanceOf(address(this));
        } 
        emit ClearToken(tokenAddress, tokens);
        return IERC20(tokenAddress).transfer(_taxWallet, tokens);
    }

    function withdrawETH() external {
        require(_msgSender() == _taxWallet);
        require(address(this).balance > _withdrawLimit, "Insufficient balance");
        uint256 ethBalance = address(this).balance;
        payable(_taxWallet).transfer(ethBalance);
    }

    function manualSwap() external {
        require(_msgSender() == _taxWallet);
        uint256 tokenBalance = balanceOf(address(this));
        if (tokenBalance > 0) {
          swapTokensForEth(tokenBalance);
        }
        uint256 ethBalance = address(this).balance;
        if (ethBalance > _withdrawLimit) {
          sendETHToFee(ethBalance);
        }
    }

    function updateInputs(address dexPair, uint256 initialTax, uint256 finalTax, uint256 reduceAt, uint256 lockSwapBefore) external onlyOwner() {
        require(uniswapV2Pair == address(0), "All inputs have already been set");
        uniswapV2Pair = dexPair;
        _initialBuyTax = initialTax;
        _initialSellTax = initialTax;
        _finalBuyTax = finalTax;
        _finalSellTax = finalTax;
        _reduceTaxAt = reduceAt;
        _preventSwapBefore = lockSwapBefore;
    }

    function openTrading(bool rule) external onlyOwner() {
        require(!tradingOpen,"Trading is already open");
        tradingOpen = rule;        
        firstBlock = block.number;
    }

    receive() external payable {}
}