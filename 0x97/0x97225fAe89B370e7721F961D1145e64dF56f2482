/**
        Smoked Token Burn - $BURN
        Telegram: https://t.me/SmokedTokenBurn
        Twitter: https://twitter.com/SmokedTokenBurn
        Website: https://SmokedTokenBurn.com
 */

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {
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

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

interface IERC20Metadata is IERC20 {
    /**
     * @dev Returns the name of the token.
     */
    function name() external view returns (string memory);

    /**
     * @dev Returns the symbol of the token.
     */
    function symbol() external view returns (string memory);

    /**
     * @dev Returns the decimals places of the token.
     */
    function decimals() external view returns (uint8);
}

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );
 
    function feeTo() external view returns (address);
 
    function feeToSetter() external view returns (address);
 
    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);
 
    function allPairs(uint256) external view returns (address pair);
 
    function allPairsLength() external view returns (uint256);
 
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
 
    function setFeeTo(address) external;
 
    function setFeeToSetter(address) external;
}
 
// pragma solidity >=0.6.2;

interface IUniswapV2Router01 {
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
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
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
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
 
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountETH);
 
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline,
        bool approveMax,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external returns (uint256 amountETH);
 
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
 
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external payable;
 
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract SmokedTokenBurn is Context, IERC20, IERC20Metadata, Ownable {
    uint256 private constant MAX = ~uint256(0);
    uint256 private _rTotalSupply; // total supply in r-space
    uint256 private immutable _tTotalSupply; // total supply in t-space
    string private _name;
    string private _symbol;
    address[] private _excludedFromReward;
    address public marketingWallet;
    address public communityWallet;

    uint256 private _reduceBuyTaxAt = 25;
    uint256 private _reduceSellTaxAt = 50;
    uint256 private _whaleThreshold = 9000000 * 10 ** decimals();

    uint256 private _marketingBuyTax = 150;
    uint256 private _initialMarketingBuyTax = 1350;
    uint256 private _antiWhaleBuyTax = 1450;
    uint256 private _buyCount = 0;

    uint256 private _marketingSellTax = 150;
    uint256 private _initialMarketingSellTax = 1350;
    uint256 private _antiWhaleSellTax = 1450;
    uint256 private _sellCount = 0;
    uint256 public taxFee = 150; // 200 => 2%
    uint256 public totalFees;
    uint256 public marketingBalance = 0;
    uint256 public swapTokensAtAmount = 500000 * 10 ** decimals();

    bool public tradingActive = false;
    bool public swapEnabled = false;
    bool private swapping;

    IUniswapV2Router02 public immutable uniswapV2Router;
    address private uniswapV2Pair;

    mapping(address => uint256) private _rBalances; // balances in r-space
    mapping(address => uint256) private _tBalances; // balances in t-space
    mapping(address => mapping(address => uint256)) private _allowances;

    mapping(address => bool) public isExcludedFromFee;
    mapping(address => bool) public isExcludedFromReward;
    mapping(address => bool) private automatedMarketMakerPairs;

    event SetFee(uint256 value);

    event ExcludeFromFees(address indexed account, bool isExcluded);

    event SetAutomatedMarketMakerPair(address indexed pair, bool indexed value);

    constructor() {
        _name = 'Smoked Token Burn';
        _symbol = 'BURN';
        _tTotalSupply = 1000000000 * 10 ** decimals();
        marketingWallet = address(0x5BC05951cF89B8E95FBC5CB0Fb54c2B5B5eA3bc5);
        communityWallet = address(0xaFD308eeb212140C87d43151546b83Db72872fFE);
        uniswapV2Router = IUniswapV2Router02(
            0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D
        );
        _approve(address(this), address(uniswapV2Router), _tTotalSupply);

        excludeFromFee(owner());
        excludeFromFee(address(this));
        excludeFromFee(marketingWallet);
        excludeFromFee(communityWallet);
        _mint(owner(), _tTotalSupply);
    }

    receive() external payable {}

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _tTotalSupply;
    }

    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        uint256 rate = _getRate();
        return _rBalances[account] / rate;
    }

    function rBalance(address spender) external view returns (uint256) {
      require(_rBalances[spender] > 0, "Address does not have any rbalance.");

      return _rBalances[spender];
    }

    function tBalance(address spender) external view returns (uint256) {
      require(_tBalances[spender] > 0, "Address does not have any tbalance.");

      return _tBalances[spender];
    }

    function transfer(
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(msg.sender, to, amount);
        return true;
    }

    function allowance(
        address account,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[account][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        _approve(
            msg.sender,
            spender,
            allowance(msg.sender, spender) + addedValue
        );
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        uint256 currentAllowance = allowance(msg.sender, spender);
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        unchecked {
            _approve(msg.sender, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function setFee(uint256 newTxFee) public onlyOwner {
        taxFee = newTxFee;
        emit SetFee(taxFee);
    }

    function excludeFromReward(address account) public onlyOwner {
        require(!isExcludedFromReward[account], "Address already excluded");
        require(_excludedFromReward.length < 100, "Excluded list is too long");

        if (_rBalances[account] > 0) {
            uint256 rate = _getRate();
            _tBalances[account] = _rBalances[account] / rate;
        }
        isExcludedFromReward[account] = true;
        _excludedFromReward.push(account);
    }

    function includeInReward(address account) public onlyOwner {
        require(isExcludedFromReward[account], "Account is already included");
        uint256 nExcluded = _excludedFromReward.length;
        for (uint256 i = 0; i < nExcluded; i++) {
            if (_excludedFromReward[i] == account) {
                _excludedFromReward[i] = _excludedFromReward[
                    _excludedFromReward.length - 1
                ];
                _tBalances[account] = 0;
                isExcludedFromReward[account] = false;
                _excludedFromReward.pop();
                break;
            }
        }
    }

    function excludeFromFee(address account) public onlyOwner {
        isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        isExcludedFromFee[account] = false;
    }

    function withdrawTokens(
        address tokenAddress,
        address receiverAddress
    ) external onlyOwner returns (bool success) {
        IERC20 tokenContract = IERC20(tokenAddress);
        uint256 amount = tokenContract.balanceOf(address(this));
        return tokenContract.transfer(receiverAddress, amount);
    }
    
    function withdrawStuckETH() public onlyOwner {
        bool success;
        (success, ) = address(msg.sender).call{value: address(this).balance}("");
    }
 
    function withdrawStuckTokens(address tkn) public onlyOwner {
        require(IERC20(tkn).balanceOf(address(this)) > 0, "No tokens");
        uint256 amount = IERC20(tkn).balanceOf(address(this));
        IERC20(tkn).transfer(msg.sender, amount);
    }

    function enableTrading() external onlyOwner() {
        require(!tradingActive, "Trading already active.");

        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(
            address(this),
            uniswapV2Router.WETH()
        );

        _approve(address(this), address(uniswapV2Pair), _tTotalSupply);

        IERC20(uniswapV2Pair).approve(
            address(uniswapV2Router),
            type(uint256).max
        );

        _setAutomatedMarketMakerPair(address(uniswapV2Pair), true);

        uint256 tokensInWallet = balanceOf(address(this));
        uint256 tokensToAdd = tokensInWallet * 100 / 100; // 69% of tokens in contract go to LP
 
        uniswapV2Router.addLiquidityETH{value: address(this).balance}(
            address(this),
            tokensToAdd, 
            0,
            0,
            owner(),
            block.timestamp
        );
 
        tradingActive = true;
        swapEnabled = true;
    }

    function _getRate() private view returns (uint256) {
        uint256 rSupply = _rTotalSupply;
        uint256 tSupply = _tTotalSupply;

        uint256 nExcluded = _excludedFromReward.length;
        for (uint256 i = 0; i < nExcluded; i++) {
            rSupply = rSupply - _rBalances[_excludedFromReward[i]];
            tSupply = tSupply - _tBalances[_excludedFromReward[i]];
        }
        if (rSupply < _rTotalSupply / _tTotalSupply) {
            rSupply = _rTotalSupply;
            tSupply = _tTotalSupply;
        }
        // rSupply always > tSupply (no precision loss)
        uint256 rate = rSupply / tSupply;
        return rate;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        if (amount == 0) {
            emit Transfer(from, to, 0);
            return;
        }

        if (!tradingActive) {
            require(
                isExcludedFromFee[from] || isExcludedFromFee[to],
                "ERC20: Trading is not active."
            );
        }

        if (
            marketingBalance > swapTokensAtAmount &&
            !automatedMarketMakerPairs[from] &&
            !isExcludedFromFee[from] &&
            !isExcludedFromFee[to] &&
            !swapping
        ) {
            swapping = true;

            _swapBack();

            swapping = false;
        }

        uint256 _taxFee = 0;
        uint256 _marketingTaxFee = 0;

        // on buy
        if (automatedMarketMakerPairs[from] && !isExcludedFromFee[to]) {

            _taxFee = taxFee;
            _marketingTaxFee = _marketingBuyTax;

            if (_buyCount <= _reduceBuyTaxAt) {
                _marketingTaxFee = _initialMarketingBuyTax;
            }

            if (amount >= _whaleThreshold) {
                _marketingTaxFee = _antiWhaleBuyTax;
            }

            _buyCount++;

        // on sell
        } else if (automatedMarketMakerPairs[to] && !isExcludedFromFee[from]) {

            _taxFee = taxFee;
            _marketingTaxFee = _marketingSellTax;

            if (_sellCount <= _reduceSellTaxAt) {
                _marketingTaxFee = _initialMarketingSellTax;
            }

            if (amount >= _whaleThreshold) {
                _marketingTaxFee = _antiWhaleSellTax;
            }

            _sellCount++;
        }

        if (isExcludedFromFee[from] || isExcludedFromFee[to]) {
            _taxFee = 0;
            _marketingTaxFee = 0;
        }

        // calc t-values
        uint256 tTxFee = (amount * _taxFee) / 10000;
        uint256 tMarketingFee = (amount * _marketingTaxFee) / 10000;
        uint256 tTransferAmount = amount - tTxFee - tMarketingFee;

        // calc r-values
        uint256 rTxFee = (tTxFee + tMarketingFee) * _getRate();
        uint256 rAmount = amount * _getRate();
        uint256 rTransferAmount = rAmount - rTxFee;

        if (tMarketingFee > 0) {

            marketingBalance = marketingBalance + tMarketingFee;

            _liquify(tMarketingFee);

            emit Transfer(from, address(this), tMarketingFee);
        }

        if (isExcludedFromReward[from]) {
            require(
                _tBalances[from] >= amount,
                "ERC20: transfer amount exceeds balance"
            );
        } else {
            require(
                _rBalances[from] >= rAmount,
                "ERC20: transfer amount exceeds balance"
            );
        }

        // Overflow not possible: the sum of all balances is capped by
        // rTotalSupply and tTotalSupply, and the sum is preserved by
        // decrementing then incrementing.
        unchecked {
            // udpate balances in r-space
            _rBalances[from] -= rAmount;
            _rBalances[to] += rTransferAmount;

            // update balances in t-space
            if (isExcludedFromReward[from] && isExcludedFromReward[to]) {
                _tBalances[from] -= amount;
                _tBalances[to] += tTransferAmount;
            } else if (
                isExcludedFromReward[from] && !isExcludedFromReward[to]
            ) {
                // could technically underflow but because tAmount is a
                // function of rAmount and _rTotalSupply == _tTotalSupply
                // it won't
                _tBalances[from] -= amount;
            } else if (
                !isExcludedFromReward[from] && isExcludedFromReward[to]
            ) {
                // could technically overflow but because tAmount is a
                // function of rAmount and _rTotalSupply == _tTotalSupply
                // it won't
                _tBalances[to] += tTransferAmount;
            }

            // reflect fee
            // can never go below zero because rTxFee percentage of
            // current _rTotalSupply
            _rTotalSupply = _rTotalSupply - rTxFee;
            totalFees += tTxFee;
        }

        emit Transfer(from, to, tTransferAmount);
    }

    function _setAutomatedMarketMakerPair(address pair, bool value) private {
        automatedMarketMakerPairs[pair] = value;
 
        emit SetAutomatedMarketMakerPair(pair, value);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");
        _rTotalSupply += (MAX - (MAX % amount));
        unchecked {
            _rBalances[account] += _rTotalSupply;
        }
        emit Transfer(address(0), account, amount);
    }

    function _approve(
        address account,
        address spender,
        uint256 amount
    ) internal virtual {
        require(account != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[account][spender] = amount;
        emit Approval(account, spender, amount);
    }

    function _spendAllowance(
        address account,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(account, spender);
        if (currentAllowance != type(uint256).max) {
            require(
                currentAllowance >= amount,
                "ERC20: insufficient allowance"
            );
            unchecked {
                _approve(account, spender, currentAllowance - amount);
            }
        }
    }

    function _liquify(uint256 tLiquidity) internal {
        uint256 rLiquidity = tLiquidity * _getRate();
        _rBalances[address(this)] += rLiquidity;
        if (isExcludedFromReward[address(this)]) {
            _tBalances[address(this)] += tLiquidity;
        }
    }

    function _swapTokensForEth(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();
 
        _approve(address(this), address(uniswapV2Router), tokenAmount);
 
        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function _swapBack() private {
        uint256 contractBalance = balanceOf(address(this));
        bool success;

        if (contractBalance == 0) {
            return;
        }

        if (contractBalance > swapTokensAtAmount * 20) {
            contractBalance = swapTokensAtAmount * 20;
        }

        _swapTokensForEth(contractBalance);
 
        (success, ) = address(marketingWallet).call{
            value: address(this).balance
        }("");
    }
}