//SPDX-License-Identifier: MIT Licensed
pragma solidity ^0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
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
    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external;

    function transfer(address to, uint256 value) external;

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external;

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);
}

interface AggregatorV3Interface {
    function decimals() external view returns (uint8);

    function description() external view returns (string memory);

    function version() external view returns (uint256);

    function getRoundData(uint80 _roundId)
        external
        view
        returns (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        );

    function latestRoundData()
        external
        view
        returns (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        );
}

contract PresaleCHANXEth is Ownable {
    IERC20 public mainToken;
    IERC20 public USDT = IERC20(0xdAC17F958D2ee523a2206206994597C13D831ec7);

    AggregatorV3Interface public priceFeed;

    struct Phase {
        uint256 tokensToSell;
        uint256 totalSoldTokens;
        uint256 tokenPerUsdPrice;
        uint256 minBuyLimit;
        uint256 maxBuyLimit;
    }
    mapping(uint256 => Phase) public phases;

    // Stats
    uint256 public totalStages;
    uint256 public currentStage;
    uint256 public soldToken;
    uint256 public amountRaised;
    uint256 public amountRaisedUSDT;
    uint256 public amountRaisedOverall;
    uint256 public uniqueBuyers;

    uint256[] public minimunToBuy = [2222 * 10**9, 3333 * 10**9, 4444 * 10**9];

    uint256[] public maximumToBuy = [
        22_222_222 * 10**9,
        33_333_333 * 10**9,
        44_433_322 * 10**9
    ];

    uint256[] public tokenPerUsdPrice = [
        128700128700,
        112612612612,
        100100100100
    ];
    uint256[] public tokensToSell = [
        3999555108 * 10**7,
        3999555108 * 10**7,
        1999777554 * 10**7
    ];

    address payable public fundReceiver;

    bool public presaleStatus;
    bool public isPresaleEnded;
    uint256 public claimStartTime;

    address[] public UsersAddresses;
    struct User {
        uint256 native_balance;
        uint256 usdt_balance;
        uint256 token_balance;
        uint256 claimed_tokens;
    }

    mapping(address => User) public users;
    mapping(address => bool) public isExist;

    event BuyToken(address indexed _user, uint256 indexed _amount);
    event ClaimToken(address indexed _user, uint256 indexed _amount);
    event UpdatePrice(uint256 _oldPrice, uint256 _newPrice);

    constructor(IERC20 _token, address _fundReceiver) {
        mainToken = _token;
        fundReceiver = payable(_fundReceiver);
        priceFeed = AggregatorV3Interface(
            0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419
        );
        for (uint256 i = 0; i < tokensToSell.length; i++) {
            phases[i].tokensToSell = tokensToSell[i];
            phases[i].tokenPerUsdPrice = tokenPerUsdPrice[i];
            phases[i].minBuyLimit = minimunToBuy[i];
            phases[i].maxBuyLimit = maximumToBuy[i];
        }
        totalStages = tokensToSell.length;
    }

    // update a presale
    function updatePresale(
        uint256 _phaseId,
        uint256 _tokensToSell,
        uint256 _tokenPerUsdPrice
    ) public onlyOwner {
        require(phases[_phaseId].tokensToSell > 0, "presale don't exist");
        phases[_phaseId].tokensToSell = _tokensToSell;
        phases[_phaseId].tokenPerUsdPrice = _tokenPerUsdPrice;
    }

    // to get real time price of Eth
    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return uint256(price);
    }

    // to buy token during preSale time with Eth => for web3 use
    function buyToken() public payable {
        require(!isPresaleEnded, "Presale ended!");
        require(presaleStatus, " Presale is Paused, check back later");
        if (!isExist[msg.sender]) {
            isExist[msg.sender] = true;
            uniqueBuyers++;
            UsersAddresses.push(msg.sender);
        }
        fundReceiver.transfer(msg.value);

        uint256 numberOfTokens;
        numberOfTokens = nativeToToken(msg.value, currentStage);
        require(
            phases[currentStage].totalSoldTokens + numberOfTokens <=
                phases[currentStage].tokensToSell,
            "Phase Limit Reached"
        );
        require(
            numberOfTokens >= phases[currentStage].minBuyLimit,
            "Atleast Buy above minimum limit"
        );
        require(
            numberOfTokens + users[msg.sender].token_balance <=
                phases[currentStage].maxBuyLimit,
            "exceeding Max limit"
        );
        soldToken = soldToken + (numberOfTokens);
        amountRaised = amountRaised + (msg.value);
        amountRaisedOverall = amountRaisedOverall + nativeToUsd(msg.value);

        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        phases[currentStage].totalSoldTokens += numberOfTokens;
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount) public {
        require(!isPresaleEnded, "Presale ended!");
        require(presaleStatus, " Presale is Paused, check back later");
        if (!isExist[msg.sender]) {
            isExist[msg.sender] = true;
            uniqueBuyers++;
            UsersAddresses.push(msg.sender);
        }
        USDT.transferFrom(msg.sender, fundReceiver, amount);

        uint256 numberOfTokens;
        numberOfTokens = usdtToToken(amount, currentStage);
        require(
            phases[currentStage].totalSoldTokens + numberOfTokens <=
                phases[currentStage].tokensToSell,
            "Phase Limit Reached"
        );
        require(
            numberOfTokens >= phases[currentStage].minBuyLimit,
            "Atleast Buy above minimum limit"
        );
        require(
            numberOfTokens + users[msg.sender].token_balance <=
                phases[currentStage].maxBuyLimit,
            "exceeding Max limit"
        );
        soldToken = soldToken + numberOfTokens;
        amountRaisedUSDT = amountRaisedUSDT + amount;
        amountRaisedOverall = amountRaisedOverall + amount;

        users[msg.sender].usdt_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            numberOfTokens;
        phases[currentStage].totalSoldTokens += numberOfTokens;
    }

    function changeClaimAddress(address _oldAddress, address _newWallet)
        public
        onlyOwner
    {
        require(isExist[_oldAddress], " Old User not a participant");
        User storage user = users[_oldAddress];
        User storage newUser = users[_newWallet];
        newUser.token_balance = user.token_balance;
        newUser.claimed_tokens = user.claimed_tokens;
        user.token_balance = 0;
        user.claimed_tokens = 0;
        isExist[_oldAddress] = false;
        isExist[_newWallet] = true;
    }

    function claimTokens() external {
        require(isPresaleEnded, "Presale has not ended yet");
        User storage user = users[msg.sender];
        require(user.token_balance > 0, "No tokens purchased");
        uint256 claimableTokens = user.token_balance - user.claimed_tokens;
        require(claimableTokens > 0, "No tokens to claim");
        user.claimed_tokens += claimableTokens;
        mainToken.transfer(msg.sender, claimableTokens);
        emit ClaimToken(msg.sender, claimableTokens);
    }

        function whitelistEthAddresses(
        address[] memory _addresses,
        uint256[] memory _tokenAmount
    ) external onlyOwner {
        require(
            _addresses.length == _tokenAmount.length,
            "Addresses and amounts must be equal"
        );

        for (uint256 i = 0; i < _addresses.length; i++) {
            users[_addresses[i]].token_balance += _tokenAmount[i];
        }
    }

    function getPhaseDetail(uint256 phaseInd)
        external
        view
        returns (
            uint256 tokenToSell,
            uint256 soldTokens,
            uint256 priceUsd
        )
    {
        Phase memory phase = phases[phaseInd];
        return (
            phase.tokensToSell,
            phase.totalSoldTokens,
            phase.tokenPerUsdPrice
        );
    }

    function setPresaleStatus(bool _status) external onlyOwner {
        presaleStatus = _status;
    }

    function endPresale() external onlyOwner {
        isPresaleEnded = true;
        claimStartTime = block.timestamp;
    }

    // to check number of token for given Eth
    function nativeToToken(uint256 _amount, uint256 phaseId)
        public
        view
        returns (uint256)
    {
        uint256 ethToUsd = (_amount * (getLatestPrice())) / (1 ether);
        uint256 numberOfTokens = (ethToUsd * phases[phaseId].tokenPerUsdPrice) /
            (1e8);
        return numberOfTokens;
    }

    // to check number of token for given usdt
    function usdtToToken(uint256 _amount, uint256 phaseId)
        public
        view
        returns (uint256)
    {
        uint256 numberOfTokens = (_amount * phases[phaseId].tokenPerUsdPrice) /
            (1e6);
        return numberOfTokens;
    }

    // Eth to USD
    function nativeToUsd(uint256 _amount) public view returns (uint256) {
        uint256 nativeTousd = (_amount * (getLatestPrice())) / (1e20);
        return nativeTousd;
    }

    function updateInfos(
        uint256 _sold,
        uint256 _raised,
        uint256 _raisedInUsdt,
        uint256 _amountRaisedOverall
    ) external onlyOwner {
        soldToken = _sold;
        amountRaised = _raised;
        amountRaisedUSDT = _raisedInUsdt;
        amountRaisedOverall = _amountRaisedOverall;
    }

    function addBscAmount(uint256 _raised, uint256 _soldAmount)
        public
        onlyOwner
    {
        amountRaisedOverall += _raised;
        soldToken += _soldAmount;
    }

    // change tokens
    function updateToken(address _token) external onlyOwner {
        mainToken = IERC20(_token);
    }

    //change tokens for buy
    function updateStableTokens(IERC20 _USDT) external onlyOwner {
        USDT = IERC20(_USDT);
    }

    // to withdraw funds for liquidity
    function initiateTransfer(uint256 _value) external onlyOwner {
        fundReceiver.transfer(_value);
    }

    // to withdraw funds for liquidity
    function changeFundReciever(address _addr) external onlyOwner {
        fundReceiver = payable(_addr);
    }

    // funtion is used to change the stage of presale
    function setCurrentStage(uint256 _stageNum) public onlyOwner {
        currentStage = _stageNum;
    }

    // to withdraw funds for liquidity
    function updatePriceFeed(AggregatorV3Interface _priceFeed)
        external
        onlyOwner
    {
        priceFeed = _priceFeed;
    }

    // to withdraw out tokens
    function transferTokens(IERC20 token, uint256 _value) external onlyOwner {
        token.transfer(msg.sender, _value);
    }

    function totalUsersCount() external view returns (uint256) {
        return UsersAddresses.length;
    }
}