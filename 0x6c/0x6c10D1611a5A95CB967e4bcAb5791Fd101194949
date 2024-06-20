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
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Ownable is Context {
    address public _owner;
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
contract XProject is Context , IERC20, Ownable {
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    address payable private _taxWallet;
    address payable private _taxWallet2;

    uint256 public _tax = 30; // 3%

    // Token Information
    uint8 public constant _decimals = 18;
    uint256 public constant _tTotal = 1_000_000_000 * 10**_decimals;
    string public constant _name = unicode"X Project";
    string public constant _symbol = unicode"XERS";

    // Contract Swap Rules
    uint256 private _taxSwapThreshold= _tTotal * 1 / 10000; //0.01%
    uint256 private _maxTaxSwap= _tTotal * 5 / 1000; // 0.5%

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;
    address immutable tokenBridge;

    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor (address _bridgeAddress) {
        _owner = _msgSender();
        _taxWallet = payable(0x120a4F52342dDd7291c391410C7c278d7d0373B2);
        _taxWallet2 = payable(0xE542543DdFEC6D9eAD1EDeC3f9F0eb1A49580Fe3);
        _balances[_msgSender()] = _tTotal;
        uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        _approve(address(this), address(uniswapV2Router), type(uint).max);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
        tokenBridge = _bridgeAddress;

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

    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function _spendAllowance(address owner, address spender, uint256 amount) internal {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
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
        if (from != owner() && to != owner() && from != tokenBridge) {
            require(tradingOpen, "Trading not open");

            if((from == uniswapV2Pair || to == uniswapV2Pair) && !inSwap){
                taxAmount = amount * _tax / 1000;
                amount -= taxAmount;
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap && to == uniswapV2Pair && swapEnabled && contractTokenBalance>_taxSwapThreshold) {
                swapTokensForEth((amount < contractTokenBalance) ? 
                amount : (contractTokenBalance < _maxTaxSwap) ? contractTokenBalance : _maxTaxSwap);
                
                uint256 contractETHBalance = address(this).balance;
                if(contractETHBalance > 0) {
                    sendETHToFee(_taxWallet, contractETHBalance * 2 / 3);
                    sendETHToFee(_taxWallet2, address(this).balance);
                }
            }
        }

        if(taxAmount>0){
            _balances[address(this)]=_balances[address(this)] + taxAmount;
            emit Transfer(from, address(this),taxAmount);
        }

        _balances[from]=_balances[from] - amount - taxAmount;
        _balances[to]=_balances[to] + amount;
        emit Transfer(from, to, amount);
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
    function sendETHToFee(address wallet, uint256 amount) private {
        bool success;
        (success,) = wallet.call{value:amount}("");
    }

    function openTrading() external onlyOwner() {  
        require(!tradingOpen,"trading is already open");
        swapEnabled = true;
        tradingOpen = true;
    }
  
    receive() external payable {}
}