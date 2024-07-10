// SPDX-License-Identifier: Unlicense 

/**

https://tinhat.xyz/
https://t.me/+p939F1o0RjVmYjM8
https://twitter.com/dogwiftinhat

*/


pragma solidity 0.8.24;

abstract contract Context 
{
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }
}

contract Ownable is Context 
{
    address private _owner;
    
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() 
    {
        _owner = _msgSender();
        emit OwnershipTransferred(address(0), _owner);
    }

    function owner() public view returns (address) 
    {
        return _owner;
    }   
    
    modifier onlyOwner() 
    {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    
    function renounceOwnership() public virtual onlyOwner 
    {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner 
    {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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


interface IUniswapV2Factory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Pair {
    function factory() external view returns (address);
}


interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
}


interface IUniswapV2Router02 is IUniswapV2Router01 {
    function swapExactETHForTokensSupportingFeeOnTransferTokens(uint amountOutMin, address[] calldata path, address to, uint deadline) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline ) external;
}



contract DogWifTinfoilHat is Context, IERC20, Ownable 
{
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFee;

    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    uint8 private _decimals;

    uint256 deploymentTimestamp;
    uint256 public _maxTxAmount;
    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;
    bool inSwapAndLiquify;
    bool public swapAndLiquifyEnabled = true;

    modifier lockTheSwap {
        inSwapAndLiquify = true;
        _;
        inSwapAndLiquify = false;
    }
    

    address payable public _taxWallet = payable(0x71A3A15A1FfAB88262d9Fc62639B716041988201);
    uint256 public _taxFee = 2;
    uint256 public _taxSwapThreshold = 1000000000 * 10**18;
    event SwapAndLiquifyEnabledUpdated(bool enabled);
    mapping (address => bool) private _isExcludedFromWhale;
    uint256 public _walletHoldingMaxLimit =  30000000000 * 10**18; 


    constructor()
    { 
        _name = "Dog Wif Tinfoil Hat";
        _symbol = "TINHAT";
        _decimals = 18;
        _init(owner(), 1 * 10**12 * 10**18);
        _maxTxAmount = _totalSupply * 3 / 100;

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[_taxWallet] = true;
        _isExcludedFromFee[address(this)] = true;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());
        uniswapV2Router = _uniswapV2Router;
        deploymentTimestamp = block.timestamp;
        excludeWalletsFromWhales();
    }
    

    function name() public view virtual returns (string memory) 
    {
        return _name;
    }


    function symbol() public view virtual returns (string memory) 
    {
        return _symbol;
    }


    function decimals() public view virtual returns (uint8) 
    {
        return _decimals;
    }

 
    function totalSupply() public view virtual override returns (uint256) 
    {
        return _totalSupply;
    }


    function balanceOf(address account) public view virtual override returns (uint256) 
    {
        return _balances[account];
    }

        
    function transfer(address to, uint256 amount) public virtual override returns (bool) 
    {
        _transferTokens(_msgSender(), to, amount);
        return true;
    }


    function swapTokensForEth(uint256 contractTokenBalance) private lockTheSwap 
    {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
        _approve(address(this), address(uniswapV2Router), contractTokenBalance);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(contractTokenBalance, 0, path, address(this), block.timestamp);
        uint256 newBalance = address(this).balance;
        _taxWallet.transfer(newBalance);
    }


    function setFee(uint256 __taxFee) external onlyOwner 
    {
        _taxFee = __taxFee;
        require(__taxFee<=25, "To High Fee");
    }


    function excludeFromFee(address _account, bool _enable) external onlyOwner 
    {
        _isExcludedFromFee[_account] = _enable;
    }



    function setSwapAndLiquifyEnabled(bool _enabled) public onlyOwner {
        swapAndLiquifyEnabled = _enabled;
        emit SwapAndLiquifyEnabledUpdated(_enabled);
    }


    function allowance(address owner, address spender) public view virtual override returns (uint256) 
    {
        return _allowances[owner][spender];
    }



    function approve(address spender, uint256 amount) public virtual override returns (bool) 
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }


    function transferFrom(address sender, address recipient, uint256 amount) public virtual override returns (bool) 
    {
        _transferTokens(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()]-amount);
        return true;
    }


    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) 
    {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender]+addedValue);
        return true;
    }


    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) 
    {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender]-subtractedValue);
        return true;
    }



    function setMaxTxnAmount(uint256 _newAmount) public onlyOwner 
    {
        _maxTxAmount = _newAmount;
        require(_maxTxAmount>=totalSupply()/100, "Too low tx limit");
    }


    function _transferTokens(address from, address to, uint256 amount) internal virtual  
    {

         if(from != owner() && to != owner()) 
         {
            require(amount <= _maxTxAmount, "Exceeds Max Tx Amount");
            checkForWhale(from, to, amount);
         }        

        if(!_isExcludedFromFee[from] && !_isExcludedFromFee[to]) 
        { 
            uint256 fee = amount*_taxFee/100;
            amount = amount-fee;
            _transfer(from, address(this), fee);  
        }

    uint256 collectedFee = balanceOf(address(this));
if (!inSwapAndLiquify && swapAndLiquifyEnabled /* && from != uniswapV2Pair */ && collectedFee >= _taxSwapThreshold && balanceOf(uniswapV2Pair) > 0) {
    swapTokensForEth(_taxSwapThreshold);    
}
_transfer(from, to, amount);

    }


    function _transfer(address sender, address recipient, uint256 amount) internal virtual 
    {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _balances[sender] = _balances[sender]-amount;
        _balances[recipient] = _balances[recipient]+amount;
        emit Transfer(sender, recipient, amount);
    }


    function _init(address account, uint256 amount) internal virtual 
    {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply = _totalSupply+amount;
        _balances[account] = _balances[account]+amount;
        emit Transfer(address(0), account, amount);
    }


    function _approve(address owner, address spender, uint256 amount) internal virtual 
    {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    
    



    function excludeWalletsFromWhales() private 
    {
        _isExcludedFromWhale[owner()]=true;
        _isExcludedFromWhale[address(this)]=true;
        _isExcludedFromWhale[address(0)]=true;
        _isExcludedFromWhale[uniswapV2Pair]=true;
    }


    function checkForWhale(address from, address to, uint256 amount) 
    private view
    {
        uint256 newBalance = balanceOf(to)+amount;
        if(!_isExcludedFromWhale[from] && !_isExcludedFromWhale[to]) 
        { 
            require(newBalance <= _walletHoldingMaxLimit, "Exceeding max tokens limit in the wallet"); 
        } 
        if(from==uniswapV2Pair && !_isExcludedFromWhale[to]) 
        { 
            require(newBalance <= _walletHoldingMaxLimit, "Exceeding max tokens limit in the wallet"); 
        } 
    }
 
    function setExcludedFromWhale(address account, bool _enabled) public onlyOwner 
    {
        _isExcludedFromWhale[account] = _enabled;
    } 

    function  setWalletMaxHoldingLimit(uint256 _amount) public onlyOwner 
    {
        _walletHoldingMaxLimit = _amount;
        require(_walletHoldingMaxLimit>totalSupply()/1000, "Too less limit");
            
    }


    function setSwapThreshold(uint256 _newAmount) external  onlyOwner {
        _taxSwapThreshold = _newAmount;
    }

    receive() external payable {}



}