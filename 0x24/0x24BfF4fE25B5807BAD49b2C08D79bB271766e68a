//SPDX-License-Identifier: UNLICENSED

//Website: https:https://alea.gg
//Telegram: https://t.me/aleaeth

pragma solidity ^0.8.8;

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );

        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }
}

abstract contract Ownable {
    address private _owner;
    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _setOwner(msg.sender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == msg.sender, "Ownable: caller is not the owner");
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

interface IERC20 {
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

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

contract ERC20 is IERC20 {
    mapping(address => uint256) internal _balances;
    mapping(address => mapping(address => uint256)) internal _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;

    constructor(
        string memory name_,
        string memory symbol_,
        uint256 totalSupply_
    ) {
        _name = name_;
        _symbol = symbol_;
        _totalSupply = totalSupply_;
    }

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
        return _totalSupply;
    }

    function balanceOf(
        address account
    ) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(
        address owner,
        address spender
    ) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(
        address spender,
        uint256 amount
    ) public virtual override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        _approve(sender, msg.sender, currentAllowance - amount);

        return true;
    }

    function increaseAllowance(
        address spender,
        uint256 addedValue
    ) public virtual returns (bool) {
        _approve(
            msg.sender,
            spender,
            _allowances[msg.sender][spender] + addedValue
        );
        return true;
    }

    function decreaseAllowance(
        address spender,
        uint256 subtractedValue
    ) public virtual returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        _approve(msg.sender, spender, currentAllowance - subtractedValue);

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");

        uint256 senderBalance = _balances[sender];
        require(
            senderBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}

interface IFactory {
    function createPair(
        address tokenA,
        address tokenB
    ) external returns (address pair);
}

interface IRouter {
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

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Alea is ERC20, Ownable {
    using Address for address payable;

    uint256 private launchFeeBlocks = 1;
    uint256 private launchFee = 99;

    uint256 public buyLimit = 2e4 * 10 ** 18;
    uint256 public sellLimit = 2e4 * 10 ** 18;
    uint256 public walletLimit = 2e4 * 10 ** 18;

    address public marketingWallet = 0x6Ba5Ea74374Ca22194Fb8882B741C1af90cA83a3;
    address public devWallet = 0x42582B97651d2Cb9941626e9431a66c12cC5563A;
    address public dev2Wallet = 0xD5f7288e24D1a6b9129D3210f592E2471d2f4b60;
    address public ecosystemWallet = 0xf1a5BfF4fdd5a17bE56A0dE932952f9455768620;

    struct Fees {
        uint256 marketing;
        uint256 liquidity;
        uint256 ecosystem;
        uint256 dev;
        uint256 dev2;
    }

    Fees public fees = Fees(0, 0, 0, 0, 0);
    Fees public sellFees = Fees(0, 0, 0, 0, 0);

    mapping(address => bool) public feeExempts;

    IRouter public router;
    address public pair;

    constructor() ERC20("Alea", "ALEA", 1e6 * 10 ** decimals()) {
        router = IRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        pair = IFactory(router.factory()).createPair(
            address(this),
            router.WETH()
        );

        _balances[msg.sender] = totalSupply();

        feeExempts[msg.sender] = true;
        feeExempts[address(this)] = true;

        feeExempts[deadWallet] = true;

        feeExempts[marketingWallet] = true;
        feeExempts[ecosystemWallet] = true;
        feeExempts[devWallet] = true;
        feeExempts[dev2Wallet] = true;
    }

    function approve(
        address spender,
        uint256 amount
    ) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        _approve(sender, msg.sender, currentAllowance - amount);

        return true;
    }

    function transfer(
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    address public constant deadWallet =
        0x000000000000000000000000000000000000dEaD;

    bool public tradingEnabled = false;
    uint256 public tradingStartBlock;

    bool public liquificationEnabled = false;
    uint256 public liquificationThreshold = 5e3 * 10 ** 18;

    bool public transferRateLimitEnabled = true;
    uint256 public transferRateLimit = 5 seconds;
    mapping(address => uint256) private _sellTimestamps;

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal override {
        require(amount > 0, "Transfer amount must be greater than zero");
        if (!feeExempts[sender] && !feeExempts[recipient]) {
            require(tradingEnabled, "Trading is not started yet");
        }
        if (sender == pair && !feeExempts[recipient] && !_reentrantLock) {
            require(amount <= buyLimit, "Buy limit exceeded");
            require(
                balanceOf(recipient) + amount <= walletLimit,
                "Per wallet limit exceeded"
            );
        }
        if (
            sender != pair &&
            !feeExempts[recipient] &&
            !feeExempts[sender] &&
            !_reentrantLock
        ) {
            require(amount <= sellLimit, "Sell limit exceeded");
            if (recipient != pair) {
                require(
                    balanceOf(recipient) + amount <= walletLimit,
                    "Per wallet limit exceeded"
                );
            }
            if (transferRateLimitEnabled) {
                uint256 delta = block.timestamp - _sellTimestamps[sender];
                require(
                    delta >= transferRateLimit,
                    "Transfer rate limit exceeded"
                );
                _sellTimestamps[sender] = block.timestamp;
            }
        }
        uint256 liquificationFee;
        uint256 totalFee;
        uint256 fee;
        Fees memory currentFees;
        bool useLaunchFee = !feeExempts[sender] &&
            !feeExempts[recipient] &&
            block.number < tradingStartBlock + launchFeeBlocks;
        if (_reentrantLock || feeExempts[sender] || feeExempts[recipient])
            fee = 0;
        else if (recipient == pair && !useLaunchFee) {
            liquificationFee =
                sellFees.liquidity +
                sellFees.marketing +
                sellFees.ecosystem +
                sellFees.dev +
                sellFees.dev2;
            totalFee = liquificationFee;
            currentFees = sellFees;
        } else if (!useLaunchFee) {
            liquificationFee =
                fees.liquidity +
                fees.marketing +
                fees.ecosystem +
                fees.dev +
                fees.dev2;
            totalFee = liquificationFee;
            currentFees = fees;
        } else {
            liquificationFee = launchFee;
            totalFee = launchFee;
        }
        fee = (amount * totalFee) / 100;
        if (liquificationEnabled && sender != pair)
            liquify(liquificationFee, currentFees);
        super._transfer(sender, recipient, amount - fee);
        if (fee > 0) {
            if (liquificationFee > 0) {
                uint256 feeAmount = (amount * liquificationFee) / 100;
                super._transfer(sender, address(this), feeAmount);
            }
        }
    }

    bool private _reentrantLock = false;

    modifier reentrantLock() {
        if (!_reentrantLock) {
            _reentrantLock = true;
            _;
            _reentrantLock = false;
        }
    }

    function liquify(
        uint256 liquificationFee,
        Fees memory swapFees
    ) private reentrantLock {
        if (liquificationFee == 0) {
            return;
        }
        uint256 balance = balanceOf(address(this));
        if (balance >= liquificationThreshold) {
            if (liquificationThreshold > 1) {
                balance = liquificationThreshold;
            }

            uint256 d = liquificationFee * 2;
            uint256 liquifyAmount = (balance * swapFees.liquidity) / d;
            uint256 swapAmount = balance - liquifyAmount;
            uint256 initialBalance = address(this).balance;

            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = router.WETH();
            _approve(address(this), address(router), swapAmount);
            router.swapExactTokensForETHSupportingFeeOnTransferTokens(
                swapAmount,
                0,
                path,
                address(this),
                block.timestamp
            );

            uint256 delta = address(this).balance - initialBalance;
            uint256 unitBalance = delta / (d - swapFees.liquidity);
            uint256 liquidityETH = unitBalance * swapFees.liquidity;
            if (liquidityETH > 0) {
                addLiquidity(liquifyAmount, liquidityETH);
            }
            uint256 marketing = unitBalance * 2 * swapFees.marketing;
            if (marketing > 0) {
                payable(marketingWallet).sendValue(marketing);
            }
            uint256 ecosystem = unitBalance * 2 * swapFees.ecosystem;
            if (ecosystem > 0) {
                payable(ecosystemWallet).sendValue(ecosystem);
            }
            uint256 dev = unitBalance * 2 * swapFees.dev;
            if (dev > 0) {
                payable(devWallet).sendValue(dev);
            }
            uint256 dev2 = unitBalance * 2 * swapFees.dev2;
            if (dev2 > 0) {
                payable(dev2Wallet).sendValue(dev2);
            }
        }
    }

    function addLiquidity(uint256 tokenAmount, uint256 ethAmount) private {
        _approve(address(this), address(router), tokenAmount);
        router.addLiquidityETH{value: ethAmount}(
            address(this),
            tokenAmount,
            0,
            0,
            deadWallet,
            block.timestamp
        );
    }

    function setLaunchFeeBlocksNumber(uint256 _blocks) external onlyOwner {
        require(!tradingEnabled, "Trading is already started");
        require(_blocks < 5, "Launch fee blocks number should not exceed 4");
        launchFeeBlocks = _blocks;
    }

    function enableTrading() external onlyOwner {
        require(!tradingEnabled, "Trading is already started");
        tradingEnabled = true;
        liquificationEnabled = true;
        tradingStartBlock = block.number;
    }

    function setLimits(
        uint256 buy,
        uint256 sell,
        uint256 wallet
    ) external onlyOwner {
        require(buy >= 1e4, "Buy limit should be at least 1%");
        require(sell >= 1e4, "Sell limit should be at least 1%");
        require(wallet >= 1e4, "Per wallet limit should be at least 1%");
        buyLimit = buy * 10 ** decimals();
        sellLimit = sell * 10 ** decimals();
        walletLimit = wallet * 10 ** decimals();
    }

    function setTransferFees(
        uint256 _marketing,
        uint256 _liquidity,
        uint256 _ecosystem,
        uint256 _dev,
        uint256 _dev2
    ) external onlyOwner {
        fees = Fees(_marketing, _liquidity, _ecosystem, _dev, _dev2);
        require(
            (_marketing + _liquidity + _ecosystem + _dev + _dev2) <= 25,
            "Total fee should not exceed 25%"
        );
    }

    function setSellFees(
        uint256 _marketing,
        uint256 _liquidity,
        uint256 _ecosystem,
        uint256 _dev,
        uint256 _dev2
    ) external onlyOwner {
        sellFees = Fees(_marketing, _liquidity, _ecosystem, _dev, _dev2);
        require(
            (_marketing + _liquidity + _ecosystem + _dev + _dev2) <= 25,
            "Total fee should not exceed 25%"
        );
    }

    function setFeeExempt(address _address, bool state) external onlyOwner {
        feeExempts[_address] = state;
    }

    function setFeeExempts(
        address[] memory _addresses,
        bool state
    ) external onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            feeExempts[_addresses[i]] = state;
        }
    }

    function setMarketingWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Address cannot be zero");
        marketingWallet = newWallet;
    }

    function setEcosystemWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Address cannot be zero");
        ecosystemWallet = newWallet;
    }

    function setDevWallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Address cannot be zero");
        devWallet = newWallet;
    }

    function setDev2Wallet(address newWallet) external onlyOwner {
        require(newWallet != address(0), "Address cannot be zero");
        dev2Wallet = newWallet;
    }

    function enableLiquification(bool state) external onlyOwner {
        liquificationEnabled = state;
    }

    function setLiquificationThreshold(uint256 threshold) external onlyOwner {
        require(
            threshold <= 1e4,
            "Liquification threshold should not exceed 1%"
        );
        liquificationThreshold = threshold * 10 ** decimals();
    }

    function setTransferRateLimit(bool state, uint256 time) external onlyOwner {
        transferRateLimit = time * 1 seconds;
        transferRateLimitEnabled = state;
        require(time <= 300, "Transfer rate limit should not exceed 5 minutes");
    }

    function retreiveETH() external onlyOwner {
        payable(owner()).transfer(address(this).balance);
    }

    function retreiveERC20(
        address erc20address,
        uint256 amount
    ) external onlyOwner {
        require(
            erc20address != address(this),
            "Should provide other ERC20 contract address"
        );
        IERC20(erc20address).transfer(owner(), amount);
    }

    receive() external payable {}
}