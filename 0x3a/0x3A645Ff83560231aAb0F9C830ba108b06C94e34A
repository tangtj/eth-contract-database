/**
https://t.me/LunarBetPortal

https://twitter.com/Lunar_bet

https://lunar.bet/
*/ 

// SPDX-License-Identifier: MIT

pragma solidity 0.8.20;


library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {return a + b;}
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {return a - b;}
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {return a * b;}
    function div(uint256 a, uint256 b) internal pure returns (uint256) {return a / b;}
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {return a % b;}
    
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {uint256 c = a + b; if(c < a) return(false, 0); return(true, c);}}

    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b > a) return(false, 0); return(true, a - b);}}

    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if (a == 0) return(true, 0); uint256 c = a * b;
        if(c / a != b) return(false, 0); return(true, c);}}

    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b == 0) return(false, 0); return(true, a / b);}}

    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {if(b == 0) return(false, 0); return(true, a % b);}}

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b <= a, errorMessage); return a - b;}}

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b > 0, errorMessage); return a / b;}}

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked{require(b > 0, errorMessage); return a % b;}}}

interface IERC20 {
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
    event Approval(address indexed owner, address indexed spender, uint256 value);}

abstract contract Ownable {
    address internal owner;
    constructor(address _owner) {owner = _owner;}
    modifier onlyOwner() {require(isOwner(msg.sender), "!OWNER"); _;}
    function isOwner(address account) public view returns (bool) {return account == owner;}
    function transferOwnership(address payable adr) public onlyOwner {owner = adr; emit OwnershipTransferred(adr);}
    event OwnershipTransferred(address owner);
}

interface IFactory{
        function createPair(address tokenA, address tokenB) external returns (address pair);
        function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IRouter {
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

    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);

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
        uint deadline) external;
}

contract Lunar is IERC20, Ownable {
    using SafeMath for uint256;
    string private constant _name = 'Lunar';
    string private constant _symbol = 'LUNAR';
    uint8 private constant _decimals = 9;
    uint256 private _totalSupply = 100000000 * (10 ** _decimals);
    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) public isFeeExempt;
    IRouter router;
    address public pair;
    bool private tradingAllowed = false;
    uint256 private liquidityFee = 50;
    uint256 private jackpotFee = 250;
    uint256 private developmentFee = 175;
    uint256 private holdersjackpotFee = 25;
    uint256 private tokencollectionFee = 0;
    uint256 private miscFee = 0;
    uint256 private totalFee = 500;
    uint256 private sellFee = 500;
    uint256 private transferFee = 0;
    uint256 private denominator = 10000;
    bool private swapEnabled = true;
    uint256 private swapTimes;
    bool private swapping; 
    uint256 public swapThreshold = ( _totalSupply * 2 ) / 4000;
    uint256 public _minTokenAmount = ( _totalSupply * 1 ) / 100000;
    modifier lockTheSwap {swapping = true; _; swapping = false;}

    address internal constant DEAD = 0x000000000000000000000000000000000000dEaD;
    address internal liquidity_receiver = 0xEe2b5021adBb5A3CA7C8e3f0088711D097391088;
    address internal development_receiver = 0x12E2D89901A7B3C41232bF9158Af8CE8D6E12251;
    address internal jackpot_receiver = 0x930458f87afC79d227fD6FBF37bd46D0d1b54b47;
    address internal holdersjackpot_receiver = 0x03CF4588A344A525595b562fbBEEa351E8e28830;
    address internal tokencollection_receiver = 0x7AF8d5a795eD013953Bb7ddC7956f0d218C64BBb;
    address internal misc_receiver = 0xaa7df58fc5B297c9996e7f4ebD13E8205207B0fc;
    

    constructor() Ownable(msg.sender) {
        IRouter _router = IRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        address _pair = IFactory(_router.factory()).createPair(address(this), _router.WETH());
        router = _router;
        pair = _pair;
        isFeeExempt[address(this)] = true;
        isFeeExempt[liquidity_receiver] = true;
        isFeeExempt[jackpot_receiver] = true;
        isFeeExempt[development_receiver] = true;
        isFeeExempt[holdersjackpot_receiver] = true;
        isFeeExempt[tokencollection_receiver] = true;
        isFeeExempt[misc_receiver] = true;
        isFeeExempt[msg.sender] = true;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}
    function name() public pure returns (string memory) {return _name;}
    function symbol() public pure returns (string memory) {return _symbol;}
    function decimals() public pure returns (uint8) {return _decimals;}
    function launch() external onlyOwner {tradingAllowed = true;}
    function getOwner() external view override returns (address) { return owner; }
    function balanceOf(address account) public view override returns (uint256) {return _balances[account];}
    function transfer(address recipient, uint256 amount) public override returns (bool) {_transfer(msg.sender, recipient, amount);return true;}
    function allowance(address owner, address spender) public view override returns (uint256) {return _allowances[owner][spender];}
    function isCont(address addr) internal view returns (bool) {uint size; assembly { size := extcodesize(addr) } return size > 0; }
    function setisfeeExempt(address _address, bool _enabled) external onlyOwner {isFeeExempt[_address] = _enabled;}
    function approve(address spender, uint256 amount) public override returns (bool) {_approve(msg.sender, spender, amount);return true;}
    function totalSupply() public view override returns (uint256) {return _totalSupply.sub(balanceOf(address(0)));}

    function preTxCheck(address sender, address recipient, uint256 amount) internal view {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount > uint256(0), "Transfer amount must be greater than zero");
        require(amount <= balanceOf(sender),"You are trying to transfer more than your balance");
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        preTxCheck(sender, recipient, amount);
        checkTradingAllowed(sender, recipient);
        swapbackCounters(sender, recipient);
        swapBack(sender, recipient, amount);
        _balances[sender] = _balances[sender].sub(amount);
        uint256 amountReceived = shouldTakeFee(sender, recipient) ? takeFee(sender, recipient, amount) : amount;
        _balances[recipient] = _balances[recipient].add(amountReceived);
        emit Transfer(sender, recipient, amountReceived);
    }

    function setnewFees(uint256 _liquidity, uint256 _jackpot, uint256 _development, uint256 _holdersjackpot, uint256 _tokencollection, uint256 _misc, uint256 _total, uint256 _sell, uint256 _trans) external onlyOwner {
        liquidityFee = _liquidity;
        jackpotFee = _jackpot;
        developmentFee = _development;
        holdersjackpotFee = _holdersjackpot;
        tokencollectionFee = _tokencollection;
        miscFee = _misc;
        totalFee = _total;
        sellFee = _sell;
        transferFee = _trans;
        require(totalFee <= denominator.div(10) && sellFee <= denominator.div(10), "totalFee and sellFee cannot be more than 10%");

    }

    function changeReceiverAddresses(address _liquidity_receiver, address _jackpot_receiver, address _development_receiver, address _holdersjackpot_receiver, address _tokencollection_receiver, address _misc_receiver) external onlyOwner {
        liquidity_receiver = _liquidity_receiver;
        jackpot_receiver = _jackpot_receiver;
        development_receiver = _development_receiver;
        holdersjackpot_receiver = _holdersjackpot_receiver;
        tokencollection_receiver = _tokencollection_receiver;
        misc_receiver = _misc_receiver;
    }

    function checkTradingAllowed(address sender, address recipient) internal view {
        if(!isFeeExempt[sender] && !isFeeExempt[recipient]){require(tradingAllowed, "tradingAllowed");}

    }

    function swapbackCounters(address sender, address recipient) internal {
        if(recipient == pair && !isFeeExempt[sender]){swapTimes += uint256(1);}

    }

    function swapAndLiquify(uint256 tokens) private lockTheSwap {
    uint256 totalFeeWithouttokencollection = liquidityFee.add(jackpotFee).add(developmentFee).add(holdersjackpotFee).add(miscFee);
    uint256 _denominator = totalFeeWithouttokencollection.mul(2).add(tokencollectionFee);
    uint256 tokensToAddLiquidityWith = tokens.mul(liquidityFee).div(_denominator);
    uint256 toSwap = tokens.sub(tokensToAddLiquidityWith);

    uint256 initialBalance = address(this).balance;
    swapTokensForETH(toSwap);
    uint256 deltaBalance = address(this).balance.sub(initialBalance);
    uint256 unitBalance = deltaBalance.div(_denominator.sub(liquidityFee).sub(tokencollectionFee));
    uint256 ETHToAddLiquidityWith = unitBalance.mul(liquidityFee);

    if (ETHToAddLiquidityWith > 0) {
        addLiquidity(tokensToAddLiquidityWith, ETHToAddLiquidityWith);
    }

    uint256 jackpotAmt = unitBalance.mul(jackpotFee).mul(2);
    if (jackpotAmt > 0) {
        payable(jackpot_receiver).transfer(jackpotAmt);
    }

    uint256 developmentAmt = unitBalance.mul(developmentFee).mul(2);
    if (developmentAmt > 0) {
        payable(development_receiver).transfer(developmentAmt);
    }

    uint256 holdersjackpotAmt = unitBalance.mul(holdersjackpotFee).mul(2);
    if (holdersjackpotAmt > 0) {
        payable(holdersjackpot_receiver).transfer(holdersjackpotAmt);
    }

    uint256 tokencollectionFeeTokens = tokens.mul(tokencollectionFee);
    uint256 tokencollectionAmt = tokencollectionFeeTokens.div(_denominator);
    if (tokencollectionAmt > 0) {
        _balances[address(this)] = _balances[address(this)].sub(tokencollectionAmt);
        _balances[tokencollection_receiver] = _balances[tokencollection_receiver].add(tokencollectionAmt);
        emit Transfer(address(this), tokencollection_receiver, tokencollectionAmt);
    }

    uint256 miscAmt = address(this).balance;
    if (miscAmt > 0) {
        payable(misc_receiver).transfer(miscAmt);
    }
}

    function addLiquidity(uint256 tokenAmount, uint256 ETHAmount) private {
        _approve(address(this), address(router), tokenAmount);
        router.addLiquidityETH{value: ETHAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            liquidity_receiver,
            block.timestamp);
    }

    function swapTokensForETH(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = router.WETH();
        _approve(address(this), address(router), tokenAmount);
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp);
    }

    function shouldSwapBack(address sender, address recipient, uint256 amount) internal view returns (bool) {
        bool aboveMin = amount >= _minTokenAmount;
        bool aboveThreshold = balanceOf(address(this)) >= swapThreshold;
        return !swapping && swapEnabled && tradingAllowed && aboveMin && !isFeeExempt[sender] && recipient == pair && swapTimes >= uint256(1) && aboveThreshold;
    }

    function swapBack(address sender, address recipient, uint256 amount) internal {
        if(shouldSwapBack(sender, recipient, amount)){swapAndLiquify(swapThreshold); swapTimes = uint256(0);}
    }

    function shouldTakeFee(address sender, address recipient) internal view returns (bool) {
        return !isFeeExempt[sender] && !isFeeExempt[recipient];
    }

    function getTotalFee(address sender, address recipient) internal view returns (uint256) {
        if(recipient == pair){return sellFee;}
        if(sender == pair){return totalFee;}
        return transferFee;
    }

    function setMinTokenAmountForSwap(uint256 newMinTokenAmount) external onlyOwner {
        require(newMinTokenAmount > 0 && newMinTokenAmount <= 100000, "Minimum token amount must be greater than 0 and less than 0.1% of total supply");
        _minTokenAmount = newMinTokenAmount * (10 ** _decimals);
    }

    function changeSwapthreshold(uint256 _swapThreshold) public onlyOwner {
        require(_swapThreshold > 0 && _swapThreshold <= 1000000, "Swap threshold must be greater than 0 and less than 1% of total supply");
        swapThreshold = _swapThreshold * (10 ** _decimals);
    }

    function takeFee(address sender, address recipient, uint256 amount) internal returns (uint256) {
        if(getTotalFee(sender, recipient) > 0){
        uint256 feeAmount = amount.div(denominator).mul(getTotalFee(sender, recipient));
        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);
        return amount.sub(feeAmount);} return amount;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    // Function to withdraw tokens from the contract
    function withdrawTokens(address tokenAddress, address to, uint256 amount) external onlyOwner {
    require(tokenAddress != address(0), "Token address cannot be the zero address");
    require(to != address(0), "Withdrawal address cannot be the zero address");
    require(tokenAddress != address(this), "Cannot withdraw the native token"); // Added check
    require(IERC20(tokenAddress).balanceOf(address(this)) >= amount, "Insufficient token balance in contract");

    bool sent = IERC20(tokenAddress).transfer(to, amount);
    require(sent, "Token transfer failed");
    }

    // Function to withdraw Ether from the contract
    function withdrawETH(uint256 amount) external onlyOwner {
        require(amount <= address(this).balance, "Insufficient balance in contract");
        payable(owner).transfer(amount);
    }

}