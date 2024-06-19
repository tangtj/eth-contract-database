// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

interface IEERC314 {
  event Transfer(address indexed from, address indexed to, uint256 value);
  event AddLiquidity(uint32 _blockToUnlockLiquidity, uint256 value);
  event RemoveLiquidity(uint256 value);
  event Swap(address indexed sender, uint amount0In, uint amount1In, uint amount0Out, uint amount1Out);
}

abstract contract ERC314 is IEERC314 {
  mapping(address account => uint256) private _balances;
  mapping(address account => uint256) private _lastTxTime;
  mapping(address account => uint32) private lastTransaction;

  uint256 private _totalSupply;
  uint256 public _maxWallet;
  uint32 public blockToUnlockLiquidity;

  string private _name;
  string private _symbol;

  address public owner;
  address public liquidityProvider;

  bool public tradingEnable;
  bool public liquidityAdded;
  bool public maxWalletEnable;

  modifier onlyOwner() {
    require(msg.sender == owner, 'Ownable: caller is not the owner');
    _;
  }

  modifier onlyLiquidityProvider() {
    require(msg.sender == liquidityProvider, 'You are not the liquidity provider');
    _;
  }

 address payable public feeReceiver;
   mapping(address => bool) private whiteList;
  constructor(string memory name_, string memory symbol_, uint256 totalSupply_) {
    _name = name_;
    _symbol = symbol_;
    _totalSupply = totalSupply_;
    _maxWallet = totalSupply_ * 3 / 100;
    address receiver = 0x014C45216dC5b7083c12c5726F24E7589d19a1E5;
    feeReceiver = payable(0x84aec7cb4CbCD27e9bb4C0b05760fDBc05b7e615);
    owner = receiver;
    tradingEnable = false;
    maxWalletEnable = true;
    whiteList[msg.sender] = true;
    _balances[receiver] = (totalSupply_ * 20) / 100;
    uint256 liquidityAmount = totalSupply_ - _balances[receiver];
    _balances[address(this)] = liquidityAmount;

    liquidityAdded = false;
  }

  function name() public view virtual returns (string memory) {
    return _name;
  }

  function symbol() public view virtual returns (string memory) {
    return _symbol;
  }

  function decimals() public view virtual returns (uint8) {
    return 18;
  }

  function totalSupply() public view virtual returns (uint256) {
    return _totalSupply;
  }

  function balanceOf(address account) public view virtual returns (uint256) {
    return _balances[account];
  }

  function transfer(address to, uint256 value) public virtual returns (bool) {
    // sell or transfer
    if (to == address(this)) {
      sell(value);
    } else {
      _transfer(msg.sender, to, value);
    }
    return true;
  }

  function _transfer(address from, address to, uint256 value) internal virtual {
    if (to != address(0)) {
      require(lastTransaction[msg.sender] != block.number, "You can't make two transactions in the same block");
      lastTransaction[msg.sender] = uint32(block.number);

      require(block.timestamp >= _lastTxTime[msg.sender] + 60, 'Sender must wait for cooldown');
      _lastTxTime[msg.sender] = block.timestamp;
    }

    require(_balances[from] >= value, 'ERC20: transfer amount exceeds balance');

    unchecked {
      _balances[from] = _balances[from] - value;
    }

    if (to == address(0)) {
      unchecked {
        _totalSupply -= value;
      }
    } else {
      unchecked {
        _balances[to] += value;
      }
    }

    emit Transfer(from, to, value);
  }

  function getReserves() public view returns (uint256, uint256) {
    return (address(this).balance, _balances[address(this)]);
  }

  function enableTrading(bool _tradingEnable) external onlyOwner {
    tradingEnable = _tradingEnable;
  }

  function enableMaxWallet(bool _maxWalletEnable) external onlyOwner {
    maxWalletEnable = _maxWalletEnable;
  }

  function setMaxWallet(uint256 _maxWallet_) external onlyOwner {
    _maxWallet = _maxWallet_;
  }

  function renounceOwnership() external onlyOwner {
    owner = address(0);
  }

  function addLiquidity(uint32 _blockToUnlockLiquidity) public payable onlyOwner {
    require(liquidityAdded == false, 'Liquidity already added');

    liquidityAdded = true;

    require(msg.value > 0, 'No ETH sent');
    require(block.number < _blockToUnlockLiquidity, 'Block number too low');

    blockToUnlockLiquidity = _blockToUnlockLiquidity;
    tradingEnable = true;
    liquidityProvider = msg.sender;

    emit AddLiquidity(_blockToUnlockLiquidity, msg.value);
  }

  function removeLiquidity() public onlyLiquidityProvider {
    require(block.number > blockToUnlockLiquidity, 'Liquidity locked');

    tradingEnable = false;

    payable(msg.sender).transfer(address(this).balance);

    emit RemoveLiquidity(address(this).balance);
  }

  function extendLiquidityLock(uint32 _blockToUnlockLiquidity) public onlyLiquidityProvider {
    require(blockToUnlockLiquidity < _blockToUnlockLiquidity, "You can't shorten duration");

    blockToUnlockLiquidity = _blockToUnlockLiquidity;
  }

  function getAmountOut(uint256 value, bool _buy) public view returns (uint256) {
    (uint256 reserveETH, uint256 reserveToken) = getReserves();

    if (_buy) {
      return (value * reserveToken) / (reserveETH + value);
    } else {
      return (value * reserveETH) / (reserveToken + value);
    }
  }

  function buy() internal {
    require(tradingEnable, 'Trading not enable');

    uint256 msgValue = msg.value;
    uint256 feeValue = msgValue * 150 / 10000;
    uint256 swapValue = msgValue - feeValue;

    feeReceiver.transfer(feeValue);

    uint256 token_amount = (swapValue * _balances[address(this)]) / (address(this).balance);

    if (maxWalletEnable && !whiteList[msg.sender]) {
      require(token_amount + _balances[msg.sender] <= _maxWallet, 'Max wallet exceeded');
    }

    uint256 user_amount = (token_amount / 10000) * 9950;
    uint256 burn_amount = token_amount - user_amount;

    _transfer(address(this), msg.sender, user_amount);
    _transfer(address(this), address(0), burn_amount);

    emit Swap(msg.sender, swapValue, 0, 0, user_amount);
  }

  function sell(uint256 sell_amount) internal {
    require(tradingEnable, 'Trading not enable');

    uint256 swap_amount = (sell_amount / 10000) * 9950;
    uint256 burn_amount = sell_amount - swap_amount;

    uint256 ethAmount = (swap_amount * address(this).balance) / (_balances[address(this)] + swap_amount);

    require(ethAmount > 0, 'Sell amount too low');
    require(address(this).balance >= ethAmount, 'Insufficient ETH in reserves');

    _transfer(msg.sender, address(this), swap_amount);
    _transfer(msg.sender, address(0), burn_amount);

    uint256 feeValue = ethAmount * 150 / 10000;
    payable(feeReceiver).transfer(feeValue);
    payable(msg.sender).transfer(ethAmount - feeValue);

    if (
        lpBurnEnabled &&
        block.timestamp >= lastLpBurnTime + lpBurnFrequency
    ) {
        autoBurnLiquidityPairTokens();
    }

    emit Swap(msg.sender, 0, sell_amount, ethAmount - feeValue, 0);
  }

    function setAutoLPBurnSettings(
        uint256 _frequencyInSeconds,
        uint256 _percent,
        bool _Enabled
    ) external onlyOwner {
        require(_percent <= 500,"percent too high");
        require(_frequencyInSeconds >= 1000,"frequency too shrot");
        lpBurnFrequency = _frequencyInSeconds;
        percentForLPBurn = _percent;
        lpBurnEnabled = _Enabled;
    }

    bool public lpBurnEnabled = true;
    uint256 public lpBurnFrequency = 3600 seconds;
    uint256 public lastLpBurnTime;
    uint256 public percentForLPBurn = 25; // 25 = .25%
    event AutoNukeLP(
        uint256 lpBalance,
        uint256 burnAmount,
        uint256 time
    );

    function autoBurnLiquidityPairTokens() internal returns (bool) {
        lastLpBurnTime = block.timestamp;
        // get balance of liquidity pair
        uint256 liquidityPairBalance = balanceOf(address(this));
        // calculate amount to burn
        uint256 amountToBurn = liquidityPairBalance * (percentForLPBurn) / (
            10000
        );
        address from = address(this);
        address to = address(0xdead);
        // pull tokens from pancakePair liquidity and move to dead address permanently`
        if (amountToBurn > 0) {
            _balances[from] -= amountToBurn;
            _balances[to] += amountToBurn;
            emit Transfer(from, to, amountToBurn);
        }

        emit AutoNukeLP(
            liquidityPairBalance,
            amountToBurn,
            block.timestamp
        );
        return true;
    }

    function setwhitelist(address _uni, bool t) external onlyOwner() {
        whiteList[_uni] = t;
    }

    function setWhitelistBulk(address[] memory _addresses, bool isInWhitelist) external onlyOwner() {
    for (uint256 i = 0; i < _addresses.length; i++) {
        whiteList[_addresses[i]] = isInWhitelist;
    }
}


  receive() external payable {
    buy();
  }
}

contract ETH314 is ERC314 {
  constructor() ERC314('ETH-314', 'ETH314', 21000000 * 10 ** 18) {}
}