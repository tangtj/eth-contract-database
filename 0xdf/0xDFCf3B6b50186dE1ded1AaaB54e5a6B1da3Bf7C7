// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.19;

library Address {

    function isContract(address account) internal view returns (bool) {
        // According to EIP-1052, 0x0 is the value returned for not-yet created accounts
        // and 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 is returned
        // for accounts without code, i.e. `keccak256('')`
        bytes32 codehash;
        bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
        // solhint-disable-next-line no-inline-assembly
        assembly { codehash := extcodehash(account) }
        return (codehash != accountHash && codehash != 0x0);
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
      return functionCall(target, data, "Address: low-level call failed");
    }

    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }

    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}

abstract contract Context {
    function _msgSender() internal view returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
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
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    // K8u#El(o)nG3a#t!e c&oP0Y
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
     /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
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





// CHANGE
contract Nexan is IERC20, Ownable {
    using SafeMath for uint256;

    // ERC20
    string constant _name = "Nexan";
    string constant _symbol = "NXAN";
    uint8 constant _decimals = 18;
    uint256 _totalSupply = 1000000000 * (10 ** _decimals); // 1,000,000,000

    // CONSTANT ADDRESSES
    address WETH = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2;
    address DEAD = 0x000000000000000000000000000000000000dEaD;
    address ZERO = 0x0000000000000000000000000000000000000000;
    address ROUTER = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D;
    IDEXRouter public router = IDEXRouter(ROUTER);

    // LIMITTER
    uint256 public _maxBuyTxAmount = (_totalSupply * 10) / 1000; // 1%
    uint256 public _maxSellTxAmount = (_totalSupply * 10) / 1000; // 1%
    uint256 public _maxWalletSize = (_totalSupply * 20) / 1000; // 2%

    mapping (address => uint256) _balances;
    mapping (address => mapping (address => uint256)) _allowances;

    mapping (address => bool) isFeeExempt;
    mapping (address => bool) isTxLimitExempt;
    
    
    // FEES
    uint256 treasuryFee = 50;
    uint256 devFee = 200; 
    uint256 mktFee = 150; 
    uint256 totalFee = 400; 
    uint256 feeDenominator = 10000;
    

    // declare variables
    address public devFeeReceiver;
    address public mktFeeReceiver;
    address public treasuryFeeReceiver;
    bool public swapBackEnabled = true;   
    address public pair;

    

    uint256 public swapThreshold = _totalSupply / 2000; // 0.05%
    bool inSwap;
    
    modifier swapping() { inSwap = true; _; inSwap = false; }

    constructor () {
        
        pair = IDEXFactory(router.factory()).createPair(WETH, address(this));
        // addresses
        devFeeReceiver = address(0xB98B0322f0443adF8F2Fb2bB015bB001CE631BA6);
        mktFeeReceiver = address(0x33277B5BdDd86786b3828256743Cd2043038E1C4);
        treasuryFeeReceiver = address(0xaD0fCE7813EB24AfEd0810A3Fbc8efEFC8CADdFD);
       

        _allowances[address(this)][address(router)] = type(uint256).max;
        

        isFeeExempt[msg.sender] = true;
        isFeeExempt[devFeeReceiver] = true;
        isFeeExempt[mktFeeReceiver] = true;
        isFeeExempt[treasuryFeeReceiver] = true;
        isFeeExempt[address(this)] = true;
        
        isTxLimitExempt[msg.sender] = true;
        isTxLimitExempt[devFeeReceiver] = true;
        isTxLimitExempt[mktFeeReceiver] = true;
        isTxLimitExempt[treasuryFeeReceiver] = true;
        isTxLimitExempt[ZERO] = true;
        isTxLimitExempt[address(this)] = true;
       
        
        _balances[devFeeReceiver] = _totalSupply.mul(5).div(100);
        _balances[treasuryFeeReceiver] = _totalSupply.mul(15).div(100);
        _balances[msg.sender] = _totalSupply.mul(80).div(100);
    }

    receive() external payable { }

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

    function _transferFrom(address sender, address recipient, uint256 amount) internal returns (bool) {
        if(inSwap){ return _basicTransfer(sender, recipient, amount); }
        
        
        // coniditional Boolean
        bool isTxExempted = (isTxLimitExempt[sender] || isTxLimitExempt[recipient]);
        bool isContractTransfer = (sender==address(this) || recipient==address(this));
        bool isLiquidityTransfer = ((sender == pair && recipient == address(router)) || (recipient == pair && sender == address(router) ));
        
        if(!isTxExempted && !isContractTransfer && !isLiquidityTransfer ){
            txLimitter(sender,recipient, amount);
        }
        
        if (recipient != pair && recipient != DEAD) {
            require(isTxLimitExempt[recipient] || _balances[recipient] + amount <= _maxWalletSize, "Transfer amount exceeds the wallet size.");
        }

        if(shouldSwapBack()){ swapBack(); }
    
        uint256 amountReceived = shouldTakeFee(sender,recipient) ? takeFee(sender, amount) : amount;
        _balances[sender] = _balances[sender].sub(amount, "Insufficient Balance");
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

    function txLimitter(address sender, address recipient, uint256 amount) internal view {
        
        bool isBuy = sender == pair || sender == address(router);
        bool isSell = recipient== pair || recipient == address(router);
        
        if(isBuy){
            require(amount <= _maxBuyTxAmount, "TX Limit Exceeded");
        }
        
        if(isSell){
            require(amount <= _maxSellTxAmount, "TX Limit Exceeded");
        }
        
    }
    
    function shouldTakeFee(address sender, address recipient) internal view returns (bool) {
        return  (sender == pair || recipient == pair) && !isFeeExempt[sender] && !isFeeExempt[recipient] ;
    } 

    function getTotalFee() public view returns (uint256) {
        return totalFee;
    }

    function takeFee(address sender, uint256 amount) internal returns (uint256) {
        uint256 feeAmount = amount.mul(getTotalFee()).div(feeDenominator);

        _balances[address(this)] = _balances[address(this)].add(feeAmount);
        emit Transfer(sender, address(this), feeAmount);

        return amount.sub(feeAmount);
    }
    
    function shouldSwapBack() internal view returns (bool) {
        return msg.sender != pair
        && !inSwap
        && swapBackEnabled
        && _balances[address(this)] >= swapThreshold;
    }
    
    function swapBack() internal swapping {
        
        uint256 amountToSwap = swapThreshold;

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

        uint256 amountWETH = address(this).balance.sub(balanceBefore);
        uint256 totalWETHFee = totalFee;
        uint256 amountWETHDev = amountWETH.mul(devFee).div(totalWETHFee);
        uint256 amountWETHMkt = amountWETH.mul(mktFee).div(totalWETHFee);
        uint256 amountWETHTreasury = amountWETH.mul(treasuryFee).div(totalWETHFee);

        sendPayable(amountWETHDev, amountWETHMkt, amountWETHTreasury);

        
    }

    function sendPayable(uint256 amtDev, uint256 amtMkt, uint256 amtTreasury) internal {
        (bool successone,) = payable(devFeeReceiver).call{value: amtDev, gas: 30000}("");
        (bool successtwo,) = payable(mktFeeReceiver).call{value: amtMkt, gas: 30000}("");
        (bool successthree,) = payable(treasuryFeeReceiver).call{value: amtTreasury, gas: 30000}("");
        require(successone && successtwo && successthree, "receiver rejected ETH transfer");
    }
    // used for collecting collected Tax to DevFee Address
    function withdrawCollectedTax() external onlyOwner {
        uint256 bal = address(this).balance; // return the native token ( WETH )
        (bool success,) = payable(devFeeReceiver).call{value: bal, gas: 30000}("");
        require(success, "receiver rejected ETH transfer");
    }
    
    
    /**
     * 
     * SETTER SECTIONS
     * 
     */

    function setMaxWallet(uint256 numerator, uint256 divisor) external onlyOwner{
        require(numerator > 0 && divisor > 0 && divisor <= 10000);
        _maxWalletSize = _totalSupply.mul(numerator).div(divisor);
    }
    
    function setDevFee(uint256 fee) external onlyOwner {
        // total fee should not be more than 4%;
        uint256 simulatedFee = fee.add(treasuryFee).add(mktFee);
        require(simulatedFee <= 400, "Fees too high !!");
        devFee = fee;
        totalFee = simulatedFee;
    }
   
    function setTreasuryFee(uint256 fee) external onlyOwner {
        // total fee should not be more than 4%;
        uint256 simulatedFee = fee.add(devFee).add(mktFee);
        require(simulatedFee <= 400, "Fees too high !!");
        treasuryFee = fee;
        totalFee = simulatedFee;
    }
    
    function setMarketingFee(uint256 fee) external onlyOwner {
        // total fee should not be more than 4%;
        uint256 simulatedFee = fee.add(devFee).add(treasuryFee);
        require(simulatedFee <= 400, "Fees too high !!");
        mktFee = fee;
        totalFee = simulatedFee;
    }
    
    function setBuyTxMaximum(uint256 max) external onlyOwner{
        uint256 minimumTreshold = (_totalSupply * 5) / 1000; // 0.5% is the minimum tx limit, we cant set below this
        uint256 simulatedMaxTx = (_totalSupply * max) / 1000;
        require(simulatedMaxTx >= minimumTreshold, "Tx Limit is too low");
        _maxBuyTxAmount = simulatedMaxTx;
    }
    
    function setSellTxMaximum(uint256 max) external onlyOwner {
        uint256 minimumTreshold = (_totalSupply * 5) / 1000; // 0.5% is the minimum tx limit, we cant set below this
        uint256 simulatedMaxTx = (_totalSupply * max) / 1000;
        require(simulatedMaxTx >= minimumTreshold, "Tx Limit is too low");
        _maxSellTxAmount = simulatedMaxTx;
    }
    
    
    
    function setIsFeeExempt(address holder, bool exempt) external onlyOwner {
        isFeeExempt[holder] = exempt;
    }

    function setIsTxLimitExempt(address holder, bool exempt) external onlyOwner {
        isTxLimitExempt[holder] = exempt;
    }
    
    function setFeeReceivers(address _devFeeReceiver, address _mktFeeReceiver, address _treasuryFeeReceiver) external onlyOwner {
        
        devFeeReceiver = _devFeeReceiver;
        mktFeeReceiver = _mktFeeReceiver;
        treasuryFeeReceiver = _treasuryFeeReceiver;
    }

    function setSwapBackSettings(bool _enabled, uint256 _amount) external onlyOwner {
        swapBackEnabled = _enabled;
        swapThreshold = _amount;
    }

    
    function getCirculatingSupply() public view returns (uint256) {
        return _totalSupply.sub(balanceOf(DEAD)).sub(balanceOf(ZERO));
    }

    function totalSupply() external view override returns (uint256) { return _totalSupply; }
    function decimals() external pure returns (uint8) { return _decimals; }
    function symbol() external pure returns (string memory) { return _symbol; }
    function name() external pure returns (string memory) { return _name; }
    function getOwner() external view returns (address) { return owner(); }
    function balanceOf(address account) public view override returns (uint256) { return _balances[account]; }
    function allowance(address holder, address spender) external view override returns (uint256) { return _allowances[holder][spender]; }
    
}