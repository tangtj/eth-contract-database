/**
 *Submitted for verification at Etherscan.io on 2024-02-24
*/

//SPDX-License-Identifier: MIT Licensed
pragma solidity ^0.8.17;

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
    constructor(address _newOwner) {
        _transferOwnership(_newOwner);
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

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 value) external;

    function transfer(address to, uint256 value) external;

    function transferFrom(address from, address to, uint256 value) external;

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

    function getRoundData(
        uint80 _roundId
    )
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

contract Presale is Ownable {
    IERC20 public mainToken;
    IERC20 public USDT = IERC20(0xdAC17F958D2ee523a2206206994597C13D831ec7);

    AggregatorV3Interface public priceFeed;

    struct Phase {
        uint256 tokensToSell;
        uint256 totalSoldTokens;
        uint256 tokenPerUsdPrice;
    }
    mapping(uint256 => Phase) public phases;

    uint256 public totalStages;
    uint256 public currentStage;
    uint256 public totalUsers;
    uint256 public soldToken;
    uint256 public amountRaised;
    uint256 public amountRaisedUSDT;
    address payable public fundReceiver;
    uint256 tokenDecimals;
    uint256[] tokensToSell;
    uint256[] tokenPerUsdPrice;

    bool public presaleStatus;
    bool public isPresaleEnded;

    address[] public UsersAddresses;

    mapping(address => bool) public oldBuyer;
    struct User {
        uint256 native_balance;
        uint256 usdt_balance;
        uint256 token_balance;
        uint256 claimed_tokens;
    }

    mapping(address => User) public users;

    struct Transaction {
        uint256 trxAt;
        uint256 presaleId;
        uint256 buyAmount;
        uint256 tokenAmount;
        bool isEther;
    }

    struct TopBuyer {
        address userAddress;
        uint256 tokenAmount;
    }
    TopBuyer[3] public topBuyersData;
    Transaction[] public transactionsHistory;

    event BuyToken(address indexed _user, uint256 indexed _amount);
    event ClaimToken(address indexed _user, uint256 indexed _amount);
    event UpdatePrice(uint256 _oldPrice, uint256 _newPrice);

    constructor(IERC20 _token, address _fundReceiver) Ownable(msg.sender) {
        mainToken = _token;
        fundReceiver = payable(_fundReceiver);
        priceFeed = AggregatorV3Interface(
            0x5f4eC3Df9cbd43714FE2740f5E3616155c5b8419
        );
        tokenDecimals = mainToken.decimals();
        tokensToSell = [
            250_000_000 * 10 ** tokenDecimals,
            250_000_000 * 10 ** tokenDecimals,
            250_000_000 * 10 ** tokenDecimals,
            250_000_000 * 10 ** tokenDecimals,
            250_000_000 * 10 ** tokenDecimals,
            250_000_000 * 10 ** tokenDecimals
        ];
        tokenPerUsdPrice = [
            2500 * (10 ** tokenDecimals),
            2000 * (10 ** tokenDecimals),
            1666 * (10 ** tokenDecimals),
            1428 * (10 ** tokenDecimals),
            1250 * (10 ** tokenDecimals),
            1111 * (10 ** tokenDecimals)
        ];
        for (uint256 i = 0; i < tokensToSell.length; i++) {
            phases[i].tokensToSell = tokensToSell[i];
            phases[i].tokenPerUsdPrice = tokenPerUsdPrice[i];
        }
        totalStages = tokensToSell.length;
    }

    // to get real time price of BNB
    function getLatestPrice() public view returns (uint256) {
        (, int256 price, , , ) = priceFeed.latestRoundData();
        return uint256(price);
    }

    // to buy token during preSale time with BNB => for web3 use

    function buyToken() public payable {
        require(!isPresaleEnded, "Presale ended!");
        require(presaleStatus, " Presale is Paused, check back later");
        if (!oldBuyer[msg.sender]) {
            totalUsers += 1;
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
        soldToken = soldToken + (numberOfTokens);
        amountRaised = amountRaised + BnbToUsd(msg.value);

        users[msg.sender].native_balance =
            users[msg.sender].native_balance +
            (msg.value);
        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            (numberOfTokens);
        phases[currentStage].totalSoldTokens += numberOfTokens;
        oldBuyer[msg.sender] = true;

        updateTopBuyerData(msg.sender, users[msg.sender].token_balance);
        updateTransactionHistory(currentStage, msg.value, numberOfTokens, true);
    }

    // to buy token during preSale time with USDT => for web3 use
    function buyTokenUSDT(uint256 amount) public {
        require(!isPresaleEnded, "Presale ended!");
        require(presaleStatus, " Presale is Paused, check back later");
        if (!oldBuyer[msg.sender]) {
            totalUsers += 1;
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
        soldToken = soldToken + numberOfTokens;
        amountRaisedUSDT = amountRaisedUSDT + amount;

        users[msg.sender].usdt_balance += amount;

        users[msg.sender].token_balance =
            users[msg.sender].token_balance +
            numberOfTokens;
        phases[currentStage].totalSoldTokens += numberOfTokens;
        oldBuyer[msg.sender] = true;

        updateTopBuyerData(msg.sender, users[msg.sender].token_balance);
        updateTransactionHistory(currentStage, amount, numberOfTokens, false);
    }

    function updateTopBuyerData(address _user, uint256 _tokenAmount) internal {
        // Check if the user is already in the top buyers
        for (uint256 i = 0; i < topBuyersData.length; i++) {
            if (_user == topBuyersData[i].userAddress) {
                topBuyersData[i].tokenAmount = _tokenAmount;
                return;
            }
        }

        for (uint256 i = 0; i < topBuyersData.length; i++) {
            if (_tokenAmount > topBuyersData[i].tokenAmount) {
                for (uint256 j = topBuyersData.length - 1; j > i; j--) {
                    topBuyersData[j] = topBuyersData[j - 1];
                }

                topBuyersData[i] = TopBuyer(_user, _tokenAmount);
                break;
            }
        }
    }

    function updateTransactionHistory(
        uint256 _presaleId,
        uint256 _buyAmount,
        uint256 _noOfTokens,
        bool _isEthTrx
    ) internal {
        Transaction memory newTransaction = Transaction(
            block.timestamp,
            _presaleId,
            _buyAmount,
            _noOfTokens,
            _isEthTrx
        );
        transactionsHistory.push(newTransaction);
        if (transactionsHistory.length > 20) {
            for (uint256 i = 0; i < transactionsHistory.length - 1; i++) {
                transactionsHistory[i] = transactionsHistory[i + 1];
            }
            transactionsHistory.pop();
        }
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

    function getPhaseDetail(
        uint256 phaseInd
    )
        external
        view
        returns (uint256 tokenToSell, uint256 soldTokens, uint256 priceUsd)
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
    }

    // to check number of token for given BNB
    function nativeToToken(
        uint256 _amount,
        uint256 phaseId
    ) public view returns (uint256) {
        uint256 bnbToUsd = (_amount * (getLatestPrice())) / (1 ether);
        uint256 numberOfTokens = (bnbToUsd * phases[phaseId].tokenPerUsdPrice) /
            (1e8);
        return numberOfTokens;
    }

    // BNB to USD
    function BnbToUsd(uint256 _amount) public view returns (uint256) {
        uint256 bnbToUsd = (_amount * (getLatestPrice())) / (1e8);
        return bnbToUsd;
    }

    // to check number of token for given usdt
    function usdtToToken(
        uint256 _amount,
        uint256 phaseId
    ) public view returns (uint256) {
        uint256 numberOfTokens = (_amount * phases[phaseId].tokenPerUsdPrice) /
            (10 ** USDT.decimals());
        return numberOfTokens;
    }

    function updateInfos(
        uint256 _sold,
        uint256 _raised,
        uint256 _raisedInUsdt
    ) external onlyOwner {
        soldToken = _sold;
        amountRaised = _raised;
        amountRaisedUSDT = _raisedInUsdt;
    }

    // change tokens
    function updateToken(address _token) external onlyOwner {
        mainToken = IERC20(_token);
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

    //change tokens for buy
    function updateStableTokens(IERC20 _USDT) external onlyOwner {
        USDT = IERC20(_USDT);
    }

    // to withdraw funds for liquidity
    function initiateTransfer(uint256 _value) external onlyOwner {
        fundReceiver.transfer(_value);
    }

    function totalUsersCount() external view returns (uint256) {
        return UsersAddresses.length;
    }

    // to withdraw funds for liquidity
    function changeFundReciever(address _addr) external onlyOwner {
        fundReceiver = payable(_addr);
    }

    // to withdraw funds for liquidity
    function updatePriceFeed(
        AggregatorV3Interface _priceFeed
    ) external onlyOwner {
        priceFeed = _priceFeed;
    }

    // funtion is used to change the stage of presale
    function setCurrentPhase(uint256 _stageNum) public onlyOwner {
        currentStage = _stageNum;
    }

    // to withdraw out tokens
    function transferStuckTokens(
        IERC20 token,
        uint256 _value
    ) external onlyOwner {
        token.transfer(msg.sender, _value);
    }
}