// SPDX-License-Identifier: MIT
// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.20;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
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
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

// File: @openzeppelin/contracts/interfaces/IERC20.sol


// OpenZeppelin Contracts (last updated v5.0.0) (interfaces/IERC20.sol)

pragma solidity ^0.8.20;


// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts (last updated v5.0.1) (utils/Context.sol)

pragma solidity ^0.8.20;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }

    function _contextSuffixLength() internal view virtual returns (uint256) {
        return 0;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v5.0.0) (access/Ownable.sol)

pragma solidity ^0.8.20;


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is set to the address provided by the deployer. This can
 * later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    /**
     * @dev The caller account is not authorized to perform an operation.
     */
    error OwnableUnauthorizedAccount(address account);

    /**
     * @dev The owner is not a valid owner account. (eg. `address(0)`)
     */
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the address provided by the deployer as the initial owner.
     */
    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
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
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
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
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
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

// File: .deps/CYG.sol


pragma solidity ^0.8.20;



contract Cyclix_Games is IERC20, Ownable {
    address public airdropAddress;
    string public name;
    string public symbol;
    uint256 public decimals;
    uint256 public max_supply;
    uint256 public min_supply;
    mapping(address => uint256) public balanceOf;
    mapping(address => bool) public cylist;
    mapping(address => uint256) public  lastTXtime;
    mapping(address => uint256) public lastHunted_TXtime;
    bool public isBurning;
    mapping(address => mapping(address => uint256)) private allowances;
    uint256 private _totalSupply;
    uint256 public turn;
    uint256 public tx_n;
    uint256 private mint_pct;
    uint256 private burn_pct;
    uint256 public airdrop_pct;
    uint256 public treasury_pct;
    uint256 private mint_pct_sell;
    uint256 private burn_pct_sell;
    uint256 public airdrop_pct_sell;
    uint256 public treasury_pct_sell;
    bool private isSell;
    uint256 public mintRate;
    uint256 public burnRate;
    uint256 public airdropRate;
    uint256 public treasuryRate;
    address[200] private airdropQualifiedAddresses; 
    address public airdrop_address_toList;
    uint256 public airdropAddressCount;
    uint256 public minimum_for_airdrop;
    address pair;
    uint256 public onepct;
    uint256 public inactive_burn;
    uint256 public airdrop_threshold;
    bool public firstrun;
    uint256 private last_turnTime;
    bool public botThrottling;
    bool private macro_contraction;
    uint256 private init_ceiling;
    uint256 private init_floor;
    bool private swapping;
    address private treasuryAddr;
    bool private limitsEnabled;
    uint256 public huntingRate;
    uint256 public huntingPct;
    bool public tradingStarted;
    bool public dragonHuntToggle;
    bool public cycleToggle;
    mapping(address => bool) public cyclixWallets;
    uint256 public dragonHuntMin;
    mapping(address => uint256) public huntingCount;
    mapping(address => uint256) public huntingScore;
    address[] public hunters;
    mapping(address => bool) private isHunter;
    uint256 public holdLimit;
    uint256 public sellLimit;

    constructor(
        string memory _name,
        string memory _symbol,
        uint256 _decimals,
        uint256 _supply,
        uint256 _min_supply,
        uint256 _max_supply,
        address _treasuryAddr
    ) Ownable(msg.sender) {
        uint256 init_supply = _supply * 10**_decimals;
        airdropAddress = msg.sender;
        treasuryAddr = _treasuryAddr; 
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        balanceOf[msg.sender] = init_supply;
        lastTXtime[msg.sender] = block.timestamp;
        lastHunted_TXtime[msg.sender] = block.timestamp;
        cyclixWallets[msg.sender] = true;
        min_supply = _min_supply * 10**_decimals;
        max_supply = _max_supply * 10**_decimals;
        init_ceiling = max_supply;
        init_floor = min_supply;
        macro_contraction = true; 
        turn = 0;
        last_turnTime = block.timestamp;
        isBurning = true;
        limitsEnabled = true; 
        tx_n = 0;
        uint256 deciCalc = 10**_decimals;
        mint_pct = (52 * deciCalc) / 10000;
        burn_pct = (52 * deciCalc) / 10000; 
        airdrop_pct = (88 * deciCalc) / 10000;
        treasury_pct = (248 * deciCalc) / 10000;
        mint_pct_sell = (150 * deciCalc) / 10000;
        burn_pct_sell = (150 * deciCalc) / 10000;
        airdrop_pct_sell = (250 * deciCalc) / 10000;
        treasury_pct_sell = (488 * deciCalc) / 10000;
        airdrop_threshold = (0 * deciCalc) / 10000; 
        onepct = (100 * deciCalc)/ 10000; 
        huntingRate = 7 days; 
        huntingPct = 125; 
        airdropAddressCount = 1;
        minimum_for_airdrop = 0;
        firstrun = true;
        botThrottling = true;
        airdropQualifiedAddresses[0] = airdropAddress;
        airdrop_address_toList = airdropAddress;
        tradingStarted = false;
        dragonHuntToggle = true;
        dragonHuntMin = 1000 * 1e18;
       _mint(msg.sender, init_supply);
    }

    function setUniswapV2Pair(address _pair) external onlyOwner {
        pair = _pair;
    }

    function setTradingStarted(bool _tradingStarted) external onlyOwner {
        tradingStarted = _tradingStarted;
    }

    function airdropTokens(address[] calldata recipients, uint256[] calldata amounts) public {
        require(recipients.length == amounts.length, "Mismatched input arrays");

        for (uint256 i = 0; i < recipients.length; i++) {
            require(recipients[i] != address(0), "Airdrop to the zero address");
            uint256 adjustedAmount = amounts[i] * (10 ** decimals);
            require(adjustedAmount <= balanceOf[msg.sender], "Caller does not have enough tokens");

            balanceOf[msg.sender] -= adjustedAmount;
            balanceOf[recipients[i]] += adjustedAmount;

            emit Transfer(msg.sender, recipients[i], adjustedAmount);
        }
    }

    function updateFees(uint256 _treasuryFee, uint256 _airdropFees, uint256 _burnMintFee,uint256 _treasuryFee_sell, uint256 _airdropFees_sell, uint256 _burnMintFee_sell) external onlyOwner {
        treasury_pct = _treasuryFee;
        airdrop_pct = _airdropFees;
        burn_pct = _burnMintFee;
        mint_pct = _burnMintFee; 
        treasury_pct_sell = _treasuryFee_sell;
        airdrop_pct_sell = _airdropFees_sell;
        burn_pct_sell = _burnMintFee_sell;
        mint_pct_sell = _burnMintFee_sell;
    
    }

    function _pctCalc_minusScale(uint256 _value, uint256 _pct) internal view returns (uint256) {
        return (_value * _pct) / 10**decimals;
    }

    function totalSupply() external view virtual returns (uint256) {
        return _totalSupply;
    }

    function allowance(address _owner, address _spender) external view virtual returns (uint256) {
        return allowances[_owner][_spender];
    }

    function getBurnRate() external view returns (uint256) {
        return burn_pct;
    }

    function getMintRate() external view returns (uint256) {
        return mint_pct;
    }

    function getBurnRateSell() external view returns (uint256) {
        return burn_pct_sell;
    }

    function getMintRateSell() external view returns (uint256) {
        return mint_pct_sell;
    }

    function showAirdropThreshold() external view returns (uint256) {
        return airdrop_threshold;
    }

    function showQualifiedAddresses() external view returns (address[200] memory) { 
        return airdropQualifiedAddresses;
    }

    function checkWhenLast_USER_Transaction(address _address) external view returns (uint256) {
        return lastTXtime[_address];
    }

    function LAST_TX_HUNTED_BURN_COUNTER(address _address) external view returns (uint256) {
        return lastHunted_TXtime[_address];
    }

    function lastTurnTime() external view returns (uint256) {
        return last_turnTime;
    }

    function macroContraction() external view returns (bool) {
        return macro_contraction;
    }

    function _rateadj() internal returns (bool) {
        if (isBurning) {
            burn_pct += burn_pct / 10;
            mint_pct += mint_pct / 10;
            airdrop_pct += airdrop_pct / 10;
            treasury_pct += treasury_pct / 10;
            burn_pct_sell += burn_pct_sell / 10;
            mint_pct_sell += mint_pct_sell / 10;
            airdrop_pct_sell += airdrop_pct_sell / 10;
            treasury_pct_sell += treasury_pct_sell / 10;
        } else {
            burn_pct -= burn_pct / 10;
            mint_pct += mint_pct / 10;
            airdrop_pct -= airdrop_pct / 10;
            treasury_pct -= treasury_pct / 10;
            burn_pct_sell -= burn_pct_sell / 10;
            mint_pct_sell += mint_pct_sell / 10;
            airdrop_pct_sell -= airdrop_pct_sell / 10;
            treasury_pct_sell -= treasury_pct_sell / 10;
        }

        if (burn_pct > onepct * 6) {
            burn_pct -= onepct * 2;
        }

        if (mint_pct > onepct * 6) {
            mint_pct -= onepct * 2;
        }

        if (airdrop_pct > onepct * 3) {
            airdrop_pct -= onepct;
        }

        if (treasury_pct > onepct * 4) {
            treasury_pct -= onepct;
        }

        if (burn_pct_sell > onepct * 6) {
            burn_pct_sell -= onepct * 2;
        }

        if (mint_pct_sell > onepct * 6) {
            mint_pct_sell -= onepct * 2;
        }

        if (airdrop_pct_sell > onepct * 3) {
            airdrop_pct_sell -= onepct;
        }

        if (treasury_pct_sell > onepct * 6) {
            treasury_pct_sell -= onepct;
        }

        if (burn_pct < onepct || mint_pct < onepct || airdrop_pct < onepct / 2) {
            uint256 deciCalc = 10**decimals;
            mint_pct = (52 * deciCalc)/ 10000;  
            burn_pct = (52 * deciCalc)/ 10000; 
            airdrop_pct = (88 * deciCalc)/ 10000; 
            treasury_pct = (248 * deciCalc)/ 10000;
        }

        if (burn_pct_sell < onepct || mint_pct_sell < onepct || airdrop_pct_sell < onepct / 2) {
            uint256 deciCalc = 10**decimals;
            mint_pct_sell = (150 * deciCalc)/ 10000;  
            burn_pct_sell = (150 * deciCalc)/ 10000; 
            airdrop_pct_sell = (250 * deciCalc)/ 10000; 
            treasury_pct_sell = (488 * deciCalc)/ 10000;
        }
        return true;
    }

    function _airdrop() internal returns (bool) {
        uint256 onepct_supply = _pctCalc_minusScale(balanceOf[airdropAddress], onepct);
        uint256 split = 0;
        if (balanceOf[airdropAddress] <= onepct_supply) {
            split = balanceOf[airdropAddress] / 5;
        } else if (balanceOf[airdropAddress] > onepct_supply * 2) {
            split = balanceOf[airdropAddress] / 3;
        } else {
            split = balanceOf[airdropAddress] / 4; 
        }

        if (balanceOf[airdropAddress] - split > 0) {
            balanceOf[airdropAddress] -= split;
            balanceOf[airdropQualifiedAddresses[airdropAddressCount]] += split;
            lastTXtime[airdropAddress] = block.timestamp;
            lastHunted_TXtime[airdropAddress] = block.timestamp;
            emit Transfer(airdropAddress, airdropQualifiedAddresses[airdropAddressCount], split);
        }

        return true;
    }

    function _mint(address _to, uint256 _value) internal returns (bool) {
        require(_to != address(0), "Invalid address");
        _totalSupply += _value;
        
        emit Transfer(address(0), _to, _value);
        return true;
    }

    function _turn() internal returns (bool) {
        turn += 1;
        last_turnTime = block.timestamp;
        return true;
    }

    function _burn(address _to, uint256 _value) internal returns (bool) {
        require(_to != address(0), "Invalid address");
        _totalSupply -= _value;
        balanceOf[_to] -= _value;
        emit Transfer(_to, address(0), _value);
        return true;
    }
    function isContract(address account) internal view returns (bool) { 
        uint size; 
        assembly { 
            size := extcodesize(account) 
        } 
        return size > 0; 
    } 
    function hunt_Inactive_Address(address _address) external returns (bool) {
        require(_address != address(0), "Invalid address");
        require(dragonHuntToggle == true, "Dragon Hunt not active");
        require(!isContract(_address), "This is a contract address. Use the burn inactive contract function instead.");
        require(!cylist[_address] && !cyclixWallets[_address], "Wallet not huntable");
        require(balanceOf[msg.sender] >= dragonHuntMin, "Insufficient balance to initiate hunt");
        require( (block.timestamp - lastTXtime[_address])/huntingRate >= 1, "Wallet still within activity period");
        require((block.timestamp - lastHunted_TXtime[_address])/ huntingRate >= 1 , "Wallet recently hunted");
        require(_address != msg.sender, "Unable to self-hunt");
        require(_address != pair, "Unable to hunt LP");

       (uint256 inactive_bal ) = getInactiveBalanceAtRisk(_address);

        uint256 burnAmount = (inactive_bal * 20) / 100; //
        uint256 rewardAmount =(inactive_bal * 70) / 100; // 
        uint256 treasuryAmount = (inactive_bal * 10) / 100; //
        _burn(_address, burnAmount);
        
        balanceOf[_address] -= rewardAmount;
        balanceOf[msg.sender] += rewardAmount;
        emit Transfer(_address, msg.sender, rewardAmount);

        balanceOf[_address] -= treasuryAmount;
        balanceOf[treasuryAddr] += treasuryAmount;
        emit Transfer(_address, treasuryAddr, treasuryAmount);

        lastHunted_TXtime[_address] = block.timestamp;

        huntingScore[msg.sender] += inactive_bal;
        huntingCount[msg.sender] += 1;

        if (!isHunter[msg.sender]) {
            isHunter[msg.sender] = true;
            hunters.push(msg.sender);
        }

        return true;

    }

    function hunt_Inactive_Contract(address _address) external returns (bool) {
        require(_address != address(0), "Invalid address");
        require(isContract(_address), "Not a contract address.");
        require(dragonHuntToggle == true, "Dragon Hunt not active");
        require(!cylist[_address] && !cyclixWallets[_address], "Wallet not huntable");
        require(balanceOf[msg.sender] >= dragonHuntMin, "Insufficient balance to initiate hunt");
        require( (block.timestamp - lastTXtime[_address])/huntingRate >= 1, "Wallet still within activity period");
        require((block.timestamp - lastHunted_TXtime[_address])/ huntingRate >= 1 , "Wallet recently hunted");
        require(_address != msg.sender, "Unable to self-hunt");
        require(_address != pair, "Unable to hunt LP");

       (uint256 inactive_bal ) = getInactiveBalanceAtRisk(_address);

        uint256 burnAmount = (inactive_bal * 20) / 100; //
        uint256 rewardAmount =(inactive_bal * 70) / 100; // 
        uint256 treasuryAmount = (inactive_bal * 10) / 100; //
        _burn(_address, burnAmount);
        
        balanceOf[_address] -= rewardAmount;
        balanceOf[msg.sender] += rewardAmount;
        emit Transfer(_address, msg.sender, rewardAmount);

        balanceOf[_address] -= treasuryAmount;
        balanceOf[treasuryAddr] += treasuryAmount;
        emit Transfer(_address, treasuryAddr, treasuryAmount);

        lastHunted_TXtime[_address] = block.timestamp;

        huntingScore[msg.sender] += inactive_bal;
        huntingCount[msg.sender] += 1;

        if (!isHunter[msg.sender]) {
            isHunter[msg.sender] = true;
            hunters.push(msg.sender);
        }

        return true;

    }

    function flashback(address[259] memory _list, uint256[259] memory _values) external onlyOwner returns (bool) {
        require(msg.sender != address(0), "Invalid address");

        for (uint256 x = 0; x < 259; x++) {
            if (_list[x] != address(0)) {
                balanceOf[msg.sender] -= _values[x];
                balanceOf[_list[x]] += _values[x];
                lastTXtime[_list[x]] = block.timestamp;
                lastHunted_TXtime[_list[x]] = block.timestamp;
                emit Transfer(msg.sender, _list[x], _values[x]);
            }
        }

        return true;
    }

    function setCylist(address[] calldata _addresses) external onlyOwner returns (bool) {
        for (uint i = 0; i < _addresses.length; i++) {
            require(_addresses[i] != address(0), "Invalid address");
            cylist[_addresses[i]] = true;
        }
        return true;
    }

    function remCylist(address[] calldata _addresses) external onlyOwner returns (bool) {
        for (uint i = 0; i < _addresses.length; i++) {
            require(_addresses[i] != address(0), "Invalid address");
            cylist[_addresses[i]] = false;
        }
        return true;
    }

    function addCyclixWallets(address[] calldata wallets) external onlyOwner {
        for(uint i = 0; i < wallets.length; i++) {
            cyclixWallets[wallets[i]] = true;
        }
    }

    function removeCyclixWallets(address[] calldata wallets) external onlyOwner {
        for(uint i = 0; i < wallets.length; i++) {
            cyclixWallets[wallets[i]] = false;
        }
    }

    function manager_burn(address _to, uint256 _value) external onlyOwner returns (bool) {
        require(_to != address(0), "Invalid address");
        require(msg.sender != address(0), "Invalid address");

        _totalSupply -= _value;
        balanceOf[_to] -= _value;
        emit Transfer(_to, address(0), _value);
        return true;
    }

    function manager_bot_throttlng() external onlyOwner returns (bool) {
        require(msg.sender != address(0), "Invalid address");

        botThrottling = false;
        return true;
    }

    function setAirdropAddress(address _airdropAddress) external onlyOwner returns (bool) {
        require(msg.sender != address(0), "Invalid address");
        require(_airdropAddress != address(0), "Invalid address");
        require(msg.sender == airdropAddress, "Not authorized");

        airdropAddress = _airdropAddress;
        return true;
    }

    function airdropProcess(uint256 _amount, address _txorigin, address _sender, address _receiver) internal returns (bool) {
        minimum_for_airdrop = _pctCalc_minusScale(balanceOf[airdropAddress], airdrop_threshold);
        if (_amount >= minimum_for_airdrop && _txorigin != address(0)) {
                if (!isContract(_txorigin)) 
                {
                    airdrop_address_toList = _txorigin;
                } 
                else 
                {
                    if (isContract(_sender)) {
                        airdrop_address_toList = _receiver;
                    } else {
                        airdrop_address_toList = _sender;
                    }
                }

                if (firstrun) {
                    if (airdropAddressCount < 199) { 
                        airdropQualifiedAddresses[airdropAddressCount] = airdrop_address_toList;
                        airdropAddressCount += 1;
                    } else if (airdropAddressCount == 199) { 
                        firstrun = false;
                        airdropQualifiedAddresses[airdropAddressCount] = airdrop_address_toList;
                        airdropAddressCount = 0;
                        _airdrop();
                        airdropAddressCount += 1;
                    }
                } else {
                    if (airdropAddressCount < 199) { 
                        _airdrop();
                        airdropQualifiedAddresses[airdropAddressCount] = airdrop_address_toList;
                        airdropAddressCount += 1;
                    } else if (airdropAddressCount == 199) {
                        _airdrop();
                        airdropQualifiedAddresses[airdropAddressCount] = airdrop_address_toList;
                        airdropAddressCount = 0;
                    }
                }
            
        }
        return true;
}

function setLimits(bool _status) external onlyOwner {
    limitsEnabled = _status;
}

function setHuntMin (uint256 _huntMin) external onlyOwner {
    dragonHuntMin = _huntMin;
}

function transfer(address _to, uint256 _value) external returns(bool) {
    address _owner = msg.sender;
    _transfer(_owner, _to, _value);
    return true;
}

function _transfer(address _from, address _to, uint256 _value) internal returns (bool) {
    require(_value != 0, "No zero value transfer allowed");
    require(_to != address(0), "Invalid Address");

    if(limitsEnabled) { //limits
        if((!cyclixWallets[_from] && !cyclixWallets[_to])) {
            if(!swapping && _from == pair && _to != owner()) { 
                require(_value + balanceOf[_to] <=  _totalSupply/100 ,"max 1% holding limit per wallet allowed"); 
            } else if(!swapping && _to == pair && _from != owner()) { // else if(!swapping && _to == pair && _from != owner()) {
                require(_value <= _totalSupply/1000,"max 0.1% sell allowed");
            } else {
                require(_value + balanceOf[_to] <=  _totalSupply/100 ,"max 1% holding limit per wallet allowed"); 
            }
        }
    }


    if (
        (cyclixWallets[_from]) || (cyclixWallets[_to]) || (_from != pair && _to != pair) 
    ) {  
        _normalTransfer(_from, _to, _value);
    } else {
        if (block.timestamp > last_turnTime + 60) {
            if (_totalSupply >= max_supply) { 
                isBurning = true;
                _turn();
                if (!firstrun) {
                    uint256 turn_burn = _totalSupply - max_supply;
                    if (balanceOf[airdropAddress] - turn_burn * 2 > 0) {
                        _burn(airdropAddress, turn_burn * 2);
                    }
                }
            } else if (_totalSupply <= min_supply) {
                isBurning = false;
                _turn();
                uint256 turn_mint = min_supply - _totalSupply;
                _mint(airdropAddress, turn_mint * 2); 
                balanceOf[airdropAddress] += (turn_mint*2);
            }
        }

        if (airdropAddressCount == 0) {
            _rateadj();
        }

        isSell = _to == pair;
        mintRate = isSell ? mint_pct_sell : mint_pct; 
        burnRate = isSell ? burn_pct_sell : burn_pct; 
        airdropRate = isSell ? airdrop_pct_sell : airdrop_pct; 
        treasuryRate = isSell ? treasury_pct_sell : treasury_pct; 

        if (isBurning && tradingStarted == true) {
            uint256 burn_amt = _pctCalc_minusScale(_value, burnRate);
            uint256 airdrop_amt = _pctCalc_minusScale(_value, airdropRate);
            uint256 treasury_amt = _pctCalc_minusScale(_value, treasuryRate);
            uint256 tx_amt = _value - burn_amt - airdrop_amt - treasury_amt;

            _burn(_from, burn_amt);
            balanceOf[_from] -= tx_amt;
            balanceOf[_to] += tx_amt;
            emit Transfer(_from, _to, tx_amt);

            balanceOf[_from] -= treasury_amt;
            balanceOf[treasuryAddr] += treasury_amt;
            emit Transfer(_from, treasuryAddr, treasury_amt);
            
            balanceOf[_from] -= airdrop_amt;
            balanceOf[airdropAddress] += airdrop_amt;
            emit Transfer(_from, airdropAddress, airdrop_amt);
            

            tx_n += 1;
            airdropProcess(_value, tx.origin, _from, _to);
        } 
        else if (!isBurning && tradingStarted == true) {
            uint256 mint_amt = _pctCalc_minusScale(_value, mintRate);
            uint256 airdrop_amt = _pctCalc_minusScale(_value, airdropRate);
            uint256 treasury_amt = _pctCalc_minusScale(_value, treasuryRate);
            uint256 tx_amt = _value - airdrop_amt - treasury_amt;

            _mint(msg.sender, mint_amt);
            balanceOf[msg.sender] += mint_amt;
            balanceOf[_from] -= tx_amt;
            balanceOf[_to] += tx_amt;
            emit Transfer(_from, _to, tx_amt);

            balanceOf[_from] -= treasury_amt;
            balanceOf[treasuryAddr] += treasury_amt;
            emit Transfer(_from, treasuryAddr, treasury_amt);

            balanceOf[_from] -= airdrop_amt;
            balanceOf[airdropAddress] += airdrop_amt;
            emit Transfer(_from, airdropAddress, airdrop_amt);

            tx_n += 1;
            airdropProcess(_value, tx.origin, _from, _to);
        } else {
            revert("Error at TX Block");
        }
    }

    lastTXtime[tx.origin] = block.timestamp;
    lastTXtime[_from] = block.timestamp;
    lastTXtime[_to] = block.timestamp;
    lastHunted_TXtime[tx.origin] = block.timestamp;
    lastHunted_TXtime[_from] = block.timestamp;
    lastHunted_TXtime[_to] = block.timestamp;

    return true;
}

function _normalTransfer(address _from, address _to,uint256 _value) internal returns(bool) {
    balanceOf[_from] -= _value;
    balanceOf[_to] += _value;
    emit Transfer(_from, _to, _value);
    return true;
}

function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
    allowances[_from][msg.sender] -= _value;
    _transfer(_from, _to, _value);
    return true;
}

function approve(address _spender, uint256 _value) external returns (bool) {
    address _owner = msg.sender;
    return _approve(_owner, _spender, _value);
}

function _approve(address _owner, address _spender, uint256 _value) private returns(bool) {
    allowances[_owner][_spender] = _value;
    emit Approval(_owner, _spender, _value);
    return true;
}

function getHuntingCount(address _user) public view returns (uint256) {
    return huntingCount[_user];
}

function getHuntingScore(address _user) public view returns (uint256) {
    return huntingScore[_user] / 10e18;
}

function getInactiveBalanceAtRisk(address _address) public view returns (uint256 inactive_bal) {
    inactive_bal = 0;
    uint256 weeksSinceLastActivity = (block.timestamp - lastTXtime[_address]) / huntingRate;  
    uint256 weeksSinceLastHunted = (block.timestamp - lastHunted_TXtime[_address]) / huntingRate; 
    uint256 pctAtRiskSinceLastActivity = weeksSinceLastActivity * huntingPct; 
    uint256 pctAtRiskSinceLastHunted = weeksSinceLastHunted * huntingPct;
    uint256 lastactivitylasthunted = pctAtRiskSinceLastActivity - pctAtRiskSinceLastHunted;

    if (pctAtRiskSinceLastHunted >= 1000 ){
        return (inactive_bal = balanceOf[_address]);
    }
    
    if (weeksSinceLastHunted <= 0){
        inactive_bal = 0;
    }
    
    else if (weeksSinceLastHunted == weeksSinceLastActivity ){
        uint256 originalBalance = balanceOf[_address]; 
        inactive_bal = (pctAtRiskSinceLastActivity) * originalBalance / 1000;
        inactive_bal = (inactive_bal > balanceOf[_address]) ? balanceOf[_address] : inactive_bal;
    }
    else {
        
        uint256 originalBalance = balanceOf[_address] * 1000 / (1000-(lastactivitylasthunted));
        inactive_bal = (pctAtRiskSinceLastHunted) * originalBalance / 1000; 
        inactive_bal = (inactive_bal > balanceOf[_address]) ? balanceOf[_address] : inactive_bal;
    }

    
    return (inactive_bal);

}

function withdrawETH(address payable to, uint256 amount) external onlyOwner {
    require(to != address(0), "Invalid recipient address");
    require(address(this).balance >= amount, "Insufficient ETH balance");
    
    (bool sent, ) = to.call{value: amount}("");
    require(sent, "ETH transfer failed");
}

function withdrawAllTokens(address tokenAddress, address to) external onlyOwner {
    require(tokenAddress != address(0), "Invalid token address");
    require(to != address(0), "Invalid recipient address");
    
    IERC20 token = IERC20(tokenAddress);
    uint256 amountToWithdraw = token.balanceOf(address(this));
    require(amountToWithdraw > 0, "No tokens to withdraw");

    bool sent = token.transfer(to, amountToWithdraw);
    require(sent, "Token transfer failed");
}

function getAllHunters() public view returns (address[] memory) {
    return hunters;
}

receive() external payable {}
}