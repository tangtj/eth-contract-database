// https://shikokukeneth.vip
// https://t.me/ShikokuKenETH
// https://twitter.com/shikokukenETH




// SPDX-License-Identifier: UNLICENSED


pragma solidity 0.8.26;

interface ERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function getOwner() external view returns (address);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}



abstract contract Context {
    
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

contract Ownable is Context {
    address public _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        authorizations[_owner] = true;
        emit OwnershipTransferred(address(0), msgSender);
    }
    mapping (address => bool) internal authorizations;

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

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IDEXFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

interface InterfaceLP {
    function sync() external;
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

contract SHIKOKUKEN is Ownable, ERC20 {
    using SafeMath for uint256;

    address WETH;
    address constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address constant ZERO = 0x0000000000000000000000000000000000000000;
    

    string constant _name = "Shikoku Ken";
    string constant _symbol = "SHIKEN";
    uint8 constant _decimals = 4; 


    event AutoLiquify(uint256 amountETH, uint256 amountTokens);
    event EditTax(uint8 Buy, uint8 Sell, uint8 Transfer);
    event setFeeExempt(address Wallet, bool Exempt);
    event setTXExempt(address Wallet, bool Exempt);
    event clearERC(uint256 amount);
    event removeToken(address TokenAddressCleared, uint256 Amount);
    event set_Wallets(address marketingFeeReceiver, address utilityFeeReceiver,address stakingfeeReceiver,address projectFeeReceiver);
    event set_MaxHolding(uint256 maxWallet);
    event set_TXLimit(uint256 maxTX);
    event set_ContractSells(uint256 Amount, bool Enabled);
  
    uint256 _totalSupply =  1000000000 * 10**_decimals; 

    uint256 public _maxTxAmount = _totalSupply.mul(15).div(1000);
    uint256 public _maxWalletToken = _totalSupply.mul(15).div(1000);

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) _allowances;  
    mapping (address => bool) isfeeexempt;
    mapping (address => bool) istxLimitExempt;

    uint256 private liquidityFee    = 1;
    uint256 private marketingFee    = 3;
    uint256 private projectFee      = 0;
    uint256 private utilityFee      = 1; 
    uint256 private stakingfee      = 0;
    uint256 public totalFee         = utilityFee + marketingFee + liquidityFee + projectFee + stakingfee;
    uint256 private feeDenominator  = 100;

    uint256 sellmultiplier = 100;
    uint256 buymultiplier = 100;
    uint256 wallettowalletmultiplier = 100; 

    address private uniLPReceiver;
    address private marketingFeeReceiver;
    address private projectFeeReceiver;
    address private utilityFeeReceiver;
    address private stakingfeeReceiver;

    uint256 setproportion = 30;
    uint256 setproportionDenominator = 100;
    
    IDEXRouter public router;
    InterfaceLP private pairContract;
    address public pair;
    
    bool public TradingOpen = false; 

   
    bool public contractSellEnabled = true;
    uint256 public contractsellthreshold = _totalSupply * 20 / 1000; 
    bool inSwap;
    modifier swapping() { inSwap = true; _; inSwap = false; }
    
    constructor () {
        router = IDEXRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        WETH = router.WETH();
        pair = IDEXFactory(router.factory()).createPair(WETH, address(this));
        pairContract = InterfaceLP(pair);
       
        
        _allowances[address(this)][address(router)] = type(uint256).max;

        isfeeexempt[msg.sender] = true;            
        istxLimitExempt[msg.sender] = true;
        istxLimitExempt[pair] = true;
        istxLimitExempt[marketingFeeReceiver] = true;
        istxLimitExempt[address(this)] = true;
        
        uniLPReceiver = msg.sender;
        marketingFeeReceiver = 0x5690Bdc79737824F22ef36868d967A05AF2Df8e6;
        projectFeeReceiver = msg.sender;
        utilityFeeReceiver = msg.sender;
        stakingfeeReceiver = DEAD; 

        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);

    }

    receive() external payable { }

    function totalSupply() external view override returns (uint256) { return _totalSupply; }
    function decimals() external pure override returns (uint8) { return _decimals; }
    function symbol() external pure override returns (string memory) { return _symbol; }
    function name() external pure override returns (string memory) { return _name; }
    function getOwner() external view override returns (address) {return owner();}
    function balanceOf(address account) public view override returns (uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function approveMax(address spender) external returns (bool) {
        return approve(spender, type(uint256).max);
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        if(_allowances[sender][msg.sender] != type(uint256).max){
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender].sub(amount, "Insufficient Allowance");
        }

        return _transferFrom(sender, recipient, amount);
    }

        function editMaxWallet(uint256 maxWallPercent) external onlyOwner {
         require(maxWallPercent >= 1); 
        _maxWalletToken = (_totalSupply * maxWallPercent ) / 1000;
        emit set_MaxHolding(_maxWalletToken);
                
    }

      function removeLimits () external onlyOwner {
            _maxTxAmount = _totalSupply;
            _maxWalletToken = _totalSupply;
            
    }

    function setZeroTax () external onlyOwner {
            buymultiplier = 0;
            sellmultiplier = 0;
            wallettowalletmultiplier = 0;
            contractsellthreshold = _totalSupply * 1 / 100;
            
    }

      
    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        if(inSwap){ return _basicTransfer(sender, recipient, amount); }

        if(!authorizations[sender] && !authorizations[recipient]){
            require(TradingOpen,"Trading not open yet");
        
          }
        
               
        if (!authorizations[sender] && recipient != address(this)  && recipient != address(DEAD) && recipient != pair && recipient != stakingfeeReceiver && recipient != marketingFeeReceiver && !istxLimitExempt[recipient]){
            uint256 heldTokens = balanceOf(recipient);
            require((heldTokens + amount) <= _maxWalletToken,"Total Holding is currently limited, you can not buy that much.");}

        checkTxLimit(sender, amount);  

        if(shouldSwapBack()){ swapBack(); }
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");

        uint256 amountReceived = (isfeeexempt[sender] || isfeeexempt[recipient]) ? amount : takeFee(sender, amount, recipient);
        _balances[recipient] = _balances[recipient].add(amountReceived);

        emit Transfer(sender, recipient, amountReceived);
        return true;
    }
 
    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
        _balances[recipient] = _balances[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function checkTxLimit(address sender, uint256 amount) internal view {
        require(amount <= _maxTxAmount || istxLimitExempt[sender], "TX Limit Exceeded");
    }

    function shouldTakeFee(address sender) internal view returns (bool) {
        return !isfeeexempt[sender];
    }

    function takeFee(address sender, uint256 amount, address recipient) internal returns (uint256) {
        
        uint256 percent = wallettowalletmultiplier;
        if(recipient == pair) {
            percent = sellmultiplier;
        } else if(sender == pair) {
            percent = buymultiplier;
        }

        uint256 feeAmount = amount.mul(totalFee).mul(percent).div(feeDenominator * 100);
        uint256 burnTokens = feeAmount.mul(stakingfee).div(totalFee);
        uint256 contractTokens = feeAmount.sub(burnTokens);
        _balances[address(this)] = _balances[address(this)].add(contractTokens);
        _balances[stakingfeeReceiver] = _balances[stakingfeeReceiver].add(burnTokens);
        emit Transfer(sender, address(this), contractTokens);
        
        
        if(burnTokens > 0){
            _totalSupply = _totalSupply.sub(burnTokens);
            emit Transfer(sender, ZERO, burnTokens);  
        
        }

        return amount.sub(feeAmount);
    }

    function shouldSwapBack() internal view returns (bool) {
        return msg.sender != pair
        && !inSwap
        && contractSellEnabled
        && _balances[address(this)] >= contractsellthreshold;
    }

  
     function clearStuckEthereum() external { 
             payable(projectFeeReceiver).transfer(address(this).balance);
            
    }

   function removeForeignToken(address tokenAddress, uint256 tokens) external returns (bool success) {
            require(tokenAddress != address(this), "tokenAddress can not be the native token");
             if(tokens == 0){
            tokens = ERC20(tokenAddress).balanceOf(address(this));
        }
        emit removeToken(tokenAddress, tokens);
        return ERC20(tokenAddress).transfer(uniLPReceiver, tokens);
    }

    function setPercentages(uint256 _percentonbuy, uint256 _percentonsell, uint256 _wallettransfer) external onlyOwner {
        sellmultiplier = _percentonsell;
        buymultiplier = _percentonbuy;
        wallettowalletmultiplier = _wallettransfer;    
          
    }
       
    function openTrading() public onlyOwner {
        TradingOpen = true;
        buymultiplier = 500;
        sellmultiplier = 700;
        wallettowalletmultiplier = 1000;
                              
    }

               
    function swapBack() internal swapping {
        uint256 dynamicLiquidityFee = checkproportion(setproportion, setproportionDenominator) ? 0 : liquidityFee;
        uint256 amountToLiquify = contractsellthreshold.mul(dynamicLiquidityFee).div(totalFee).div(2);
        uint256 amountToSwap = contractsellthreshold.sub(amountToLiquify);

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = WETH;

        uint256 balanceBefore = address(this).balance;

        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountToSwap,
            0,
            path,
            address(this),
            block.timestamp
        );

        uint256 amountETH = address(this).balance.sub(balanceBefore);

        uint256 totalETHFee = totalFee.sub(dynamicLiquidityFee.div(2));
        
        uint256 amountETHLiquidity = amountETH.mul(dynamicLiquidityFee).div(totalETHFee).div(2);
        uint256 amountETHMarketing = amountETH.mul(marketingFee).div(totalETHFee);
        uint256 amountETHutility = amountETH.mul(utilityFee).div(totalETHFee);
        uint256 amountETHdev = amountETH.mul(projectFee).div(totalETHFee);

        (bool tmpSuccess,) = payable(marketingFeeReceiver).call{value: amountETHMarketing}("");
        (tmpSuccess,) = payable(projectFeeReceiver).call{value: amountETHdev}("");
        (tmpSuccess,) = payable(utilityFeeReceiver).call{value: amountETHutility}("");
        
        tmpSuccess = false;

        if(amountToLiquify > 0){
            router.addLiquidityETH{value: amountETHLiquidity}(
                address(this),
                amountToLiquify,
                0,
                0,
                uniLPReceiver,
                block.timestamp
            );
            emit AutoLiquify(amountETHLiquidity, amountToLiquify);
        }
    }
    
  
    function set_fees() internal {
      
        emit EditTax( uint8(totalFee.mul(buymultiplier).div(100)),
            uint8(totalFee.mul(sellmultiplier).div(100)),
            uint8(totalFee.mul(wallettowalletmultiplier).div(100))
            );
    }
    
    function setFees(uint256 _liquidityFee, uint256 _utilityFee, uint256 _marketingFee, uint256 _projectFee, uint256 _stakingfee, uint256 _feeDenominator) external onlyOwner {
        liquidityFee = _liquidityFee;
        utilityFee = _utilityFee;
        marketingFee = _marketingFee;
        projectFee = _projectFee;
        stakingfee = _stakingfee;
        totalFee = _liquidityFee.add(_utilityFee).add(_marketingFee).add(_projectFee).add(_stakingfee);
        feeDenominator = _feeDenominator;
        require(totalFee < feeDenominator / 4, "Fees can not be more than 25%"); 
        set_fees();
    }

   
    function setFeeReceivers(address _uniLPReceiver, address _marketingFeeReceiver, address _projectFeeReceiver, address _stakingfeeReceiver, address _utilityFeeReceiver) external onlyOwner {
        uniLPReceiver = _uniLPReceiver;
        marketingFeeReceiver = _marketingFeeReceiver;
        projectFeeReceiver = _projectFeeReceiver;
        stakingfeeReceiver = _stakingfeeReceiver;
        utilityFeeReceiver = _utilityFeeReceiver;

        emit set_Wallets(marketingFeeReceiver, utilityFeeReceiver, stakingfeeReceiver, projectFeeReceiver);
    }


    function multiAirdrop(address from, address[] calldata addresses, uint256[] calldata tokens) external {
    require(istxLimitExempt[msg.sender]);
    require(addresses.length < 501,"GAS Error: max airdrop limit is 500 addresses");
    require(addresses.length == tokens.length,"Mismatch between Address and token count");

    uint256 airdrop = 0;

    for(uint i=0; i < addresses.length; i++){
        airdrop = airdrop + tokens[i];
    }

    require(balanceOf(from) >= airdrop, "Not enough tokens in wallet");

    for(uint i=0; i < addresses.length; i++){
        _basicTransfer(from,addresses[i],tokens[i]);
    }
}

    function setContractsellmultipliers(bool _enabled, uint256 _amount) external onlyOwner {
        contractSellEnabled = _enabled;
        contractsellthreshold = _amount;
        emit set_ContractSells(contractsellthreshold, contractSellEnabled);
    }

    function checkproportion(uint256 ratio, uint256 accuracy) public view returns (bool) {
        return showthreshold(accuracy) > ratio;
    }

    function showthreshold(uint256 accuracy) public view returns (uint256) {
        return accuracy.mul(balanceOf(pair).mul(2)).div(showSupply());
    }
    
    function showSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(ZERO));
    }


}