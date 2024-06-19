/*
Telegram: https://t.me/bluarctic
Twitter: https://twitter.com/0xBluArctic

*/

// SPDX-License-Identifier:MIT

pragma solidity ^0.8.10;

abstract contract Context {
    
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address _account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Ownable is Context {

    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _setOwner(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any _account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IDexSwapFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDexSwapRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

}

library Math {

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
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

}

//GAS OPTIMIZED ERROR MESSAGES
error ERC20InvalidSender(address sender);
error ERC20InvalidReceiver(address receiver);
error ERC20InvalidApprover(address approver);
error ERC20InvalidSpender(address spender);
error ERC20ZeroTransfer();

contract BARC is Context, IERC20, Ownable {
    
    using Math for uint256;

    mapping (address => bool) public ischargepair;
    mapping (address => bool) public isMarketPair;

    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;

    string private _name = "The Blu Arctic Water Company"; 
    string private _symbol = "BARC"; 
    uint8 private _decimals = 9; 

    uint256 private _totalSupply = 109_000_000 * 10 ** _decimals;
    uint256 public swapThreshold = _totalSupply.mul(5).div(10000);  // 0.05%

    bool public swapEnabled = true;

    bool sellProtection;  
    bool taxFree;
    uint256 currentIndex;
    uint256 startTime;
    uint256[3] timerSlot = [1800,86400,172800];    // Dynamic Tax Settings.
    uint256[4] timerCharge = [24,20,10,0];         // Dynamix percentage.

    IDexSwapRouter public DexRouter;
    address public DexPair;

    address private marketingWallet = address(0x9f9d9C48D1aE8f5536bc1f25cc2f797041CbE705);
    address private deployer;

    bool inSwap;
    
    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }
 
    event SwapTokensForETH(
        uint256 amountIn,
        address[] path
    );

    constructor() {

        deployer = msg.sender;

        IDexSwapRouter _dexRouter = IDexSwapRouter(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );

        DexPair = IDexSwapFactory(_dexRouter.factory()).createPair(
            address(this),
            _dexRouter.WETH()
        );

        DexRouter = _dexRouter;

        ischargepair[address(this)] = true;
        ischargepair[address(msg.sender)] = true;

        isMarketPair[address(DexPair)] = true;

        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function decimals() public view returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
       return _balances[account];     
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }

        _allowances[owner][spender] = amount;
        
        emit Approval(owner, spender, amount);
    }

    receive() external payable {}

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        _transfer(sender, recipient, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {

        if (sender == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (recipient == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        if(amount == 0) {
            revert ERC20ZeroTransfer();
        }
        
        if (inSwap) {
            return _basicTransfer(sender, recipient, amount);
        }
        else {

            if (startTime != 0 && !taxFree) {
                dynamicTaxSetter();
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool overMinimumTokenBalance = contractTokenBalance >= swapThreshold;

            if (
                overMinimumTokenBalance && 
                !inSwap && 
                !isMarketPair[sender] && 
                swapEnabled &&
                !ischargepair[sender] &&
                !ischargepair[recipient]
                ) {
                swapBack(contractTokenBalance);
            }
            
            _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");

            uint256 finalAmount = shouldNotTakeFee(sender,recipient) ? amount : takeFee(sender, recipient, amount);

            _balances[recipient] = _balances[recipient].add(finalAmount);

            emit Transfer(sender, recipient, finalAmount);
            return true;

        }

    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }
    
    function shouldNotTakeFee(address sender, address recipient) internal view returns (bool) {
        
        if(taxFree) {
            return true;
        }

        if(ischargepair[sender] || ischargepair[recipient]) {
            return true;
        }
        else if (isMarketPair[sender] || isMarketPair[recipient]) {
            return false;
        }
        else {
            return false;
        }
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {

        if(!sellProtection) {
            sellProtection = true;
            startTime = block.timestamp;
        }
        
        uint feeAmount;

        unchecked {
            
            // sell tax
            if(isMarketPair[recipient]) { 
                feeAmount = amount.mul(timerCharge[currentIndex]).div(100);
            }

            if(feeAmount > 0) {
                _balances[address(this)] = _balances[address(this)].add(feeAmount);
                emit Transfer(sender, address(this), feeAmount);
            }

            return amount.sub(feeAmount);
        }
        
    }

    function dynamicTaxSetter() internal {

        if (block.timestamp <= startTime + timerSlot[0]) {
            currentIndex = 0;
        }
        if (block.timestamp > startTime + timerSlot[0] && block.timestamp <= startTime + timerSlot[1]) {
            currentIndex = 1;
        }
        if (block.timestamp > startTime + timerSlot[1] && block.timestamp <= startTime + timerSlot[2]) {
            currentIndex = 2;
        }
        if (block.timestamp > startTime + timerSlot[2]) {
            currentIndex = 3;
            taxFree = true;
        }
            
    }

    function swapBack(uint contractBalance) internal swapping {

        uint256 initialBalance = address(this).balance;
        swapTokensForEth(contractBalance);
        uint256 amountReceived = address(this).balance.sub(initialBalance);

        if(amountReceived > 0) {
            payable(marketingWallet).transfer(amountReceived);
        }
     
    }

    function swapTokensForEth(uint256 tokenAmount) private {
        // generate the uniswap pair path of token -> weth
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = DexRouter.WETH();

        _approve(address(this), address(DexRouter), tokenAmount);

        // make the swap
        DexRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0, // accept any amount of ETH
            path,
            address(this), // The contract
            block.timestamp
        );
        
        emit SwapTokensForETH(tokenAmount, path);
    }
    
    function rescueFunds() external {
        require(msg.sender == deployer,"Unauthorized");
        (bool success,) = payable(deployer).call{value: address(this).balance}("");
        require(success, 'Token payment failed');
    }

    function clearStuckTokens(address _token, uint256 _amount) external {
        require(msg.sender == deployer,"Unauthorized");
        (bool success, ) = address(_token).call(abi.encodeWithSignature('transfer(address,uint256)',  deployer, _amount));
        require(success, 'Token payment failed');
    }

    function getDynamicInfo() external view returns (bool,bool,uint,uint) {
        return (sellProtection,taxFree,currentIndex,startTime);
    }

    function setMarketingWallet(address _newAddres) external onlyOwner {
        marketingWallet = _newAddres;
    }

    function setDeployerWallet(address _newAddres) external onlyOwner {
        deployer = _newAddres;
    }

    function enableChargePair(address _adr,bool _status) external onlyOwner {
        ischargepair[_adr] = _status;
    }

    function setSwapBackSettings(bool _enabled, uint256 _amount)
        external
    {
        require(msg.sender == deployer,"Unauthorized");
        swapEnabled = _enabled;
        swapThreshold = _amount;
    }

}