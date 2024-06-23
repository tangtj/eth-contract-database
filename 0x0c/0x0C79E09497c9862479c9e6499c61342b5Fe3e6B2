/*
Website: https://hashpower.bot
Docs: https://docs.hashpower.bot
Twitter: https://twitter.com/hashpowerai
Telegram : https://t.me/hashpowerai
Bot: https://t.me/hashpowerai_bot
*/

// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;

    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );
}

contract Ownable is Context {
    address private _owner;
    address private _previousOwner;
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

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
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

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

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

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

contract HashPowerAI is
    Context,
    IERC20,
    Ownable
{
    using SafeMath for uint256;

    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => uint256) private _balances;

    uint256 private constant _tTotal = 10000000 * 10**9;
    uint256 private _rTotal = (MAX - (MAX % _tTotal));

    uint8 private constant _decimals = 9;
    string private constant _name = "HashPower AI";
    string private constant _symbol = "HASH";

    uint256 private constant MAX = ~uint256(0);

    address payable private _mrktAddress = payable(0xa3c95a3305389C150c81543A345c48339E21cE5d);

    bool private inSwap = false;
    bool private isTradingLive;
    bool private allowSwap = true;

    uint256 private _oldTaxFee = _taxFee;
    uint256 private _taxFee = _taxFeeOnSell;
    uint256 private _taxFeeOnBuy = 5;
    uint256 private _taxFeeOnSell = 5;

    IUniswapV2Router02 public uniswapV2Router;
    address public uniswapV2Pair;

    uint256 public _minSwap = 1000 * 10**9;
    uint256 public _maxTxLimit = 100000 * 10**9;
    uint256 public _maxBalance = 100000 * 10**9;

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    constructor() {
        _balances[_msgSender()] = _rTotal;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        uniswapV2Router = _uniswapV2Router;

        uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), _uniswapV2Router.WETH());

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[_mrktAddress] = true;
        _isExcludedFromFee[address(this)] = true;

        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    function balanceOf(address account) public view override returns (uint256) {
        return tokenFromReflection(_balances[account]);
    }

    function name() public pure returns (string memory) {
        return _name;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function allowance(address owner, address spender)
        public
        view
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function deactivateFee() private {
        if (_taxFee == 0) return;

        _oldTaxFee = _taxFee;

        _taxFee = 0;
    }

    function activateFee() private {
        _taxFee = _oldTaxFee;
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function tokenFromReflection(uint256 rAmount)
        private
        view
        returns (uint256)
    {
        require(
            rAmount <= _rTotal,
            "Amount has to be less than total reflections"
        );
        uint256 currentRate = _getRate();
        return rAmount.div(currentRate);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        require(owner != address(0), "Can't approve from zero address");
        require(spender != address(0), "Can't approve to zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "Cant transfer from address zero");
        require(to != address(0), "Cant transfer to address zero");
        require(amount > 0, "Amount should be above zero");

        if (from != owner() && to != owner()) {
            //Trade start check
            if (!isTradingLive) {
                require(
                    from == owner(),
                    "Only owner can trade before trading activation"
                );
            }

            require(amount <= _maxTxLimit, "Exceeded max transaction limit");

            if (to != uniswapV2Pair) {
                require(
                    balanceOf(to) + amount < _maxBalance,
                    "Exceeds max wallet balance"
                );
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            bool canSwap = contractTokenBalance >= _minSwap;

            if (contractTokenBalance >= _maxTxLimit) {
                contractTokenBalance = _maxTxLimit;
            }

            if (
                canSwap &&
                !inSwap &&
                from != uniswapV2Pair &&
                allowSwap &&
                !_isExcludedFromFee[from] &&
                !_isExcludedFromFee[to]
            ) {
                swapTokensForEth(contractTokenBalance);
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance > 0) {
                    transferEthToMrkt(address(this).balance);
                }
            }
        }

        bool takeFee = true;

        //Transfer Tokens
        if (
            (_isExcludedFromFee[from] || _isExcludedFromFee[to]) ||
            (from != uniswapV2Pair && to != uniswapV2Pair)
        ) {
            takeFee = false;
        } else {
            //Set Fee for Buys
            if (from == uniswapV2Pair && to != address(uniswapV2Router)) {
                _taxFee = _taxFeeOnBuy;
            }

            //Set Fee for Sells
            if (to == uniswapV2Pair && from != address(uniswapV2Router)) {
                _taxFee = _taxFeeOnSell;
            }
        }

        _tokenTransfer(from, to, amount, takeFee);
    }

    function transferEthToMrkt(uint256 amount) private {
        _mrktAddress.transfer(amount);
    }

    function changeTradingStatus(bool _isTradingLive) public onlyOwner {
        isTradingLive = _isTradingLive;
    }

    function setMinTknsToSwap(uint256 swapTokensAtAmount) public onlyOwner {
        _minSwap = swapTokensAtAmount;
    }

    function setMaxBalance(uint256 maxWalletSize) public onlyOwner {
        _maxBalance = maxWalletSize;
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
                "the transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function _getCurrentSupply() private view returns (uint256, uint256) {
        return (_rTotal, _tTotal);
    }

    function setAllowSwap(bool _allowSwap) public onlyOwner {
        allowSwap = _allowSwap;
    }

    function manualsend() external {
        require(_msgSender() == _mrktAddress);
        uint256 contractETHBalance = address(this).balance;
        transferEthToMrkt(contractETHBalance);
    }

    function _tokenTransfer(
        address sender,
        address recipient,
        uint256 amount,
        bool takeFee
    ) private {
        if (!takeFee) deactivateFee();
        _transferTakingFees(sender, recipient, amount);
        if (!takeFee) activateFee();
    }

    function _transferTakingFees(
        address sender,
        address recipient,
        uint256 tAmount
    ) private {
        (
            uint256 rAmount,
            uint256 rTransferAmount,
            uint256 tTransferAmount,
            uint256 tTeam
        ) = _getFeeValues(tAmount);

        _balances[sender] = _balances[sender].sub(rAmount);
        _balances[recipient] = _balances[recipient].add(rTransferAmount);

        _sendFeeToMrkt(tTeam);
        emit Transfer(sender, recipient, tTransferAmount);
    }

    function _sendFeeToMrkt(uint256 tTeam) private {
        uint256 currentRate = _getRate();
        uint256 rTeam = tTeam.mul(currentRate);
        _balances[address(this)] = _balances[address(this)].add(rTeam);
    }

    function manualswap() external {
        require(_msgSender() == _mrktAddress);
        uint256 contractBalance = balanceOf(address(this));
        swapTokensForEth(contractBalance);
    }

    function _getFeeValues(uint256 tAmount)
        private
        view
        returns (
            uint256,
            uint256,
            uint256,
            uint256
        )
    {
        (uint256 tTransferAmount, uint256 tTeam) = _getTaxValues(
            tAmount,
            _taxFee
        );
        uint256 currentRate = _getRate();
        (uint256 rAmount, uint256 rTransferAmount) = _getReflectedValues(
            tAmount,
            tTeam,
            currentRate
        );
        return (rAmount, rTransferAmount, tTransferAmount, tTeam);
    }

    function excludeAccountsFromFees(address[] calldata accounts, bool excluded)
        public
        onlyOwner
    {
        for (uint256 i = 0; i < accounts.length; i++) {
            _isExcludedFromFee[accounts[i]] = excluded;
        }
    }

    function _getRate() private view returns (uint256) {
        (uint256 rSupply, uint256 tSupply) = _getCurrentSupply();
        return rSupply.div(tSupply);
    }

    function setFee(uint256 taxFeeOnBuy, uint256 taxFeeOnSell)
        public
        onlyOwner
    {
        require(taxFeeOnBuy >= 0 && taxFeeOnBuy <= 45);
        require(taxFeeOnSell >= 0 && taxFeeOnSell <= 45);

        _taxFeeOnBuy = taxFeeOnBuy;
        _taxFeeOnSell = taxFeeOnSell;
    }

    function setMaxTxLimit(uint256 maxTxAmount) public onlyOwner {
        _maxTxLimit = maxTxAmount;
    }

    function _getTaxValues(uint256 tAmount, uint256 taxFee)
        private
        pure
        returns (uint256, uint256)
    {
        uint256 tTeam = tAmount.mul(taxFee).div(100);
        uint256 tTransferAmount = tAmount.sub(tTeam);
        return (tTransferAmount, tTeam);
    }

    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
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

    receive() external payable {}

    function _getReflectedValues(
        uint256 tAmount,
        uint256 tTeam,
        uint256 currentRate
    ) private pure returns (uint256, uint256) {
        uint256 rAmount = tAmount.mul(currentRate);
        uint256 rTeam = tTeam.mul(currentRate);
        uint256 rTransferAmount = rAmount.sub(rTeam);
        return (rAmount, rTransferAmount);
    }
}