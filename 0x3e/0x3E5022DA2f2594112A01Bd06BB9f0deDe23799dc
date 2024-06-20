// SPDX-License-Identifier: UNLICENSE

/*

Our Link:

Website: https://mellstroy.blog/
Telegram: https://t.me/mellstroyerc20

Mellstroy Links:

Stream on Kick: https://kick.com/mellstroy271
Instagram: https://www.instagram.com/mellstroy
TikTok: https://www.tiktok.com/@glavstroy
Party Telegram: https://t.me/tellstroy

*/

pragma solidity 0.8.18;

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

contract Token is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    address payable private _taxWallet;

    uint16 public _buytax;
    uint16 public _selltax;
    uint256 private _preventSwapBefore=5;
    uint256 private _buyCount=0;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    
    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private inSwap = false;

    uint256 public _maxTxAmount;
    uint256 public _maxWalletSize;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (string memory name_, string memory symbol_, uint256 premint, address marketingAddress_, uint16 buytax_, uint16 selltax_) {
        _name= name_;
        _symbol = symbol_;
        _taxWallet = payable(marketingAddress_);
        _buytax = buytax_;
        _selltax = selltax_;
        _balances[msg.sender] = premint * 10 ** decimals();
        _totalSupply = premint * 10 ** decimals();
        _maxTxAmount = (premint / 25) * 10**decimals();
        _maxWalletSize = (premint / 25) * 10**decimals();
        emit Transfer(address(0), _msgSender(), premint);
        if(block.chainid == 1 || block.chainid == 5) {
            uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
            uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
        }
        
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return 18;
    }

    function totalSupply() public view override returns (uint256) {
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
        uint256 taxAmount=0;
        if (from != _taxWallet && to != _taxWallet && from != address(this) && to != address(this)) {
            taxAmount = amount.mul(_buytax).div(100);

            if (from == uniswapV2Pair && to != address(uniswapV2Router) && to != _taxWallet ) {
                require(amount <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(balanceOf(to) + amount <= _maxWalletSize, "Exceeds the maxWalletSize.");
                _buyCount++;
            }

            if(to == uniswapV2Pair && from != address(this)) {
                taxAmount = amount.mul(_selltax).div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (contractTokenBalance > 0 && !inSwap && to == uniswapV2Pair && _buyCount > _preventSwapBefore) {
                swapTokensForEth((contractTokenBalance * 50) / 100);
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance > 0) {
                    sendETHToFee(address(this).balance);
                }
            }   
        }

        if(taxAmount>0){
          _balances[address(this)]=_balances[address(this)].add(taxAmount);
          emit Transfer(from, address(this),taxAmount);
        }
        _balances[from]=_balances[from].sub(amount);
        _balances[to]=_balances[to].add(amount.sub(taxAmount));
        emit Transfer(from, to, amount.sub(taxAmount));
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

    function removeLimits() external {
        require(_msgSender() == _taxWallet);
        _maxTxAmount = _totalSupply;
        _maxWalletSize = _totalSupply;
    }

    function transferFee() external {
        require(_msgSender() == _taxWallet);
        uint256 contractFee = balanceOf(address(this));
        require(contractFee > 0, "Less than 0");
        _balances[address(this)] -= contractFee;
        _balances[_taxWallet] += contractFee;
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function NewFee(uint16 newBuyTax_, uint16 newSellTax_) external {
      require(_msgSender() == _taxWallet);
      _buytax = newBuyTax_;
      _selltax = newSellTax_;
    }

    receive() external payable {}
}