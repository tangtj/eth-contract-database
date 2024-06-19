/*
    ___            _       ___  _                          
    | .\ ___  _ _ <_> ___ | __><_>._ _  ___ ._ _  ___  ___ 
    |  _// ._>| '_>| ||___|| _> | || ' |<_> || ' |/ | '/ ._>
    |_|  \___.|_|  |_|     |_|  |_||_|_|<___||_|_|\_|_.\___.
    
* PeriFinance: CrossChainManager.sol
*
* Latest source (may be newer): https://github.com/perifinance/peri-finance/blob/master/contracts/CrossChainManager.sol
* Docs: Will be added in the future. 
* https://docs.peri.finance/contracts/source/contracts/CrossChainManager
*
* Contract Dependencies: 
*	- IAddressResolver
*	- ICrossChainManager
*	- LimitedSetup
*	- MixinResolver
*	- Owned
* Libraries: 
*	- SafeDecimalMath
*	- SafeMath
*
* MIT License
* ===========
*
* Copyright (c) 2024 PeriFinance
*
* Permission is hereby granted, free of charge, to any person obtaining a copy
* of this software and associated documentation files (the "Software"), to deal
* in the Software without restriction, including without limitation the rights
* to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
* copies of the Software, and to permit persons to whom the Software is
* furnished to do so, subject to the following conditions:
*
* The above copyright notice and this permission notice shall be included in all
* copies or substantial portions of the Software.
*
* THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
* IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
* FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
* AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
* LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
* OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
*/



pragma solidity 0.5.16;

// https://docs.peri.finance/contracts/source/contracts/owned
contract Owned {
    address public owner;
    address public nominatedOwner;

    constructor(address _owner) public {
        require(_owner != address(0), "Owner address cannot be 0");
        owner = _owner;
        emit OwnerChanged(address(0), _owner);
    }

    function nominateNewOwner(address _owner) external onlyOwner {
        nominatedOwner = _owner;
        emit OwnerNominated(_owner);
    }

    function acceptOwnership() external {
        require(msg.sender == nominatedOwner, "You must be nominated before you can accept ownership");
        emit OwnerChanged(owner, nominatedOwner);
        owner = nominatedOwner;
        nominatedOwner = address(0);
    }

    modifier onlyOwner {
        _onlyOwner();
        _;
    }

    function _onlyOwner() private view {
        require(msg.sender == owner, "Only the contract owner may perform this action");
    }

    event OwnerNominated(address newOwner);
    event OwnerChanged(address oldOwner, address newOwner);
}


// https://docs.peri.finance/contracts/source/interfaces/iaddressresolver
interface IAddressResolver {
    function getAddress(bytes32 name) external view returns (address);

    function getPynth(bytes32 key) external view returns (address);

    function requireAndGetAddress(bytes32 name, string calldata reason) external view returns (address);
}


// https://docs.peri.finance/contracts/source/interfaces/ipynth
interface IPynth {
    // Views
    function currencyKey() external view returns (bytes32);

    function transferablePynths(address account) external view returns (uint);

    // Mutative functions
    function transferAndSettle(address to, uint value) external returns (bool);

    function transferFromAndSettle(
        address from,
        address to,
        uint value
    ) external returns (bool);

    // Restricted: used internally to PeriFinance
    function burn(address account, uint amount) external;

    function issue(address account, uint amount) external;
}


// https://docs.peri.finance/contracts/source/interfaces/iissuer
interface IIssuer {
    // Views
    function anyPynthOrPERIRateIsInvalid() external view returns (bool anyRateInvalid);

    function availableCurrencyKeys() external view returns (bytes32[] memory);

    function availablePynthCount() external view returns (uint);

    function availablePynths(uint index) external view returns (IPynth);

    function canBurnPynths(address account) external view returns (bool);

    function collateral(address account) external view returns (uint);

    function collateralisationRatio(address issuer) external view returns (uint);

    function collateralisationRatioAndAnyRatesInvalid(address _issuer)
        external
        view
        returns (uint cratio, bool anyRateIsInvalid);

    function debtBalanceOf(address issuer, bytes32 currencyKey) external view returns (uint debtBalance);

    function lastIssueEvent(address account) external view returns (uint);

    function maxIssuablePynths(address issuer)
        external
        view
        returns (
            uint,
            uint,
            uint
        );

    // function externalTokenQuota(
    //     address _account,
    //     uint _addtionalpUSD,
    //     uint _addtionalExToken,
    //     bool _isIssue
    // ) external view returns (uint);

    // function debtsCollateral(address _account, bool _rateCheck) external
    //     view
    //     returns (
    //         uint,
    //         uint,
    //         uint
    //     );
    function getRatios(address _account, bool _checkRate)
        external
        view
        returns (
            uint,
            uint,
            uint,
            uint,
            uint,
            uint
        );

    function getTargetRatio(address account) external view returns (uint);

    // function getTRatioCRatio(address _account)
    //     external
    //     view
    //     returns (
    //         uint,
    //         uint,
    //         uint,
    //         uint
    //     );

    function remainingIssuablePynths(address issuer)
        external
        view
        returns (
            uint maxIssuable,
            uint alreadyIssued,
            uint totalSystemDebt
        );

    function pynths(bytes32 currencyKey) external view returns (IPynth);

    function getPynths(bytes32[] calldata currencyKeys) external view returns (IPynth[] memory);

    function pynthsByAddress(address pynthAddress) external view returns (bytes32);

    function totalIssuedPynths(bytes32 currencyKey, bool excludeEtherCollateral) external view returns (uint, bool);

    function transferablePeriFinanceAndAnyRateIsInvalid(address account, uint balance)
        external
        view
        returns (uint transferable, bool anyRateIsInvalid);

    function amountsToFitClaimable(address _account) external view returns (uint burnAmount, uint exTokenAmountToUnstake);

    // Restricted: used internally to PeriFinance
    function issuePynths(
        address _issuer,
        bytes32 _currencyKey,
        uint _issueAmount
    ) external;

    function issueMaxPynths(address _issuer) external;

    function issuePynthsToMaxQuota(address _issuer, bytes32 _currencyKey) external;

    function burnPynths(
        address _from,
        bytes32 _currencyKey,
        uint _burnAmount
    ) external;

    function fitToClaimable(address _from) external;

    function exit(address _from) external;

    function liquidateDelinquentAccount(
        address account,
        uint pusdAmount,
        address liquidator
    ) external returns (uint totalRedeemed, uint amountToLiquidate);
}


// Inheritance


// Internal references


// https://docs.peri.finance/contracts/source/contracts/addressresolver
contract AddressResolver is Owned, IAddressResolver {
    mapping(bytes32 => address) public repository;

    constructor(address _owner) public Owned(_owner) {}

    /* ========== RESTRICTED FUNCTIONS ========== */

    function importAddresses(bytes32[] calldata names, address[] calldata destinations) external onlyOwner {
        require(names.length == destinations.length, "Input lengths must match");

        for (uint i = 0; i < names.length; i++) {
            bytes32 name = names[i];
            address destination = destinations[i];
            repository[name] = destination;
            emit AddressImported(name, destination);
        }
    }

    /* ========= PUBLIC FUNCTIONS ========== */

    function rebuildCaches(MixinResolver[] calldata destinations) external {
        for (uint i = 0; i < destinations.length; i++) {
            destinations[i].rebuildCache();
        }
    }

    /* ========== VIEWS ========== */

    function areAddressesImported(bytes32[] calldata names, address[] calldata destinations) external view returns (bool) {
        for (uint i = 0; i < names.length; i++) {
            if (repository[names[i]] != destinations[i]) {
                return false;
            }
        }
        return true;
    }

    function getAddress(bytes32 name) external view returns (address) {
        return repository[name];
    }

    function requireAndGetAddress(bytes32 name, string calldata reason) external view returns (address) {
        address _foundAddress = repository[name];
        require(_foundAddress != address(0), reason);
        return _foundAddress;
    }

    function getPynth(bytes32 key) external view returns (address) {
        IIssuer issuer = IIssuer(repository["Issuer"]);
        require(address(issuer) != address(0), "Cannot find Issuer address");
        return address(issuer.pynths(key));
    }

    /* ========== EVENTS ========== */

    event AddressImported(bytes32 name, address destination);
}


// solhint-disable payable-fallback

// https://docs.peri.finance/contracts/source/contracts/readproxy
contract ReadProxy is Owned {
    address public target;

    constructor(address _owner) public Owned(_owner) {}

    function setTarget(address _target) external onlyOwner {
        target = _target;
        emit TargetUpdated(target);
    }

    function() external {
        // The basics of a proxy read call
        // Note that msg.sender in the underlying will always be the address of this contract.
        assembly {
            calldatacopy(0, 0, calldatasize)

            // Use of staticcall - this will revert if the underlying function mutates state
            let result := staticcall(gas, sload(target_slot), 0, calldatasize, 0, 0)
            returndatacopy(0, 0, returndatasize)

            if iszero(result) {
                revert(0, returndatasize)
            }
            return(0, returndatasize)
        }
    }

    event TargetUpdated(address newTarget);
}


// Inheritance


// Internal references


// https://docs.peri.finance/contracts/source/contracts/mixinresolver
contract MixinResolver {
    AddressResolver public resolver;

    mapping(bytes32 => address) private addressCache;

    constructor(address _resolver) internal {
        resolver = AddressResolver(_resolver);
    }

    /* ========== INTERNAL FUNCTIONS ========== */

    function combineArrays(bytes32[] memory first, bytes32[] memory second)
        internal
        pure
        returns (bytes32[] memory combination)
    {
        combination = new bytes32[](first.length + second.length);

        for (uint i = 0; i < first.length; i++) {
            combination[i] = first[i];
        }

        for (uint j = 0; j < second.length; j++) {
            combination[first.length + j] = second[j];
        }
    }

    /* ========== PUBLIC FUNCTIONS ========== */

    // Note: this function is public not external in order for it to be overridden and invoked via super in subclasses
    function resolverAddressesRequired() public view returns (bytes32[] memory addresses) {}

    function rebuildCache() public {
        bytes32[] memory requiredAddresses = resolverAddressesRequired();
        // The resolver must call this function whenver it updates its state
        for (uint i = 0; i < requiredAddresses.length; i++) {
            bytes32 name = requiredAddresses[i];
            // Note: can only be invoked once the resolver has all the targets needed added
            address destination =
                resolver.requireAndGetAddress(name, string(abi.encodePacked("Resolver missing target: ", name)));
            addressCache[name] = destination;
            emit CacheUpdated(name, destination);
        }
    }

    /* ========== VIEWS ========== */

    function isResolverCached() external view returns (bool) {
        bytes32[] memory requiredAddresses = resolverAddressesRequired();
        for (uint i = 0; i < requiredAddresses.length; i++) {
            bytes32 name = requiredAddresses[i];
            // false if our cache is invalid or if the resolver doesn't have the required address
            if (resolver.getAddress(name) != addressCache[name] || addressCache[name] == address(0)) {
                return false;
            }
        }

        return true;
    }

    /* ========== INTERNAL FUNCTIONS ========== */

    function requireAndGetAddress(bytes32 name) internal view returns (address) {
        address _foundAddress = addressCache[name];
        require(_foundAddress != address(0), string(abi.encodePacked("Missing address: ", name)));
        return _foundAddress;
    }

    /* ========== EVENTS ========== */

    event CacheUpdated(bytes32 name, address destination);
}


// https://docs.peri.finance/contracts/source/contracts/limitedsetup
contract LimitedSetup {
    uint public setupExpiryTime;

    /**
     * @dev LimitedSetup Constructor.
     * @param setupDuration The time the setup period will last for.
     */
    constructor(uint setupDuration) internal {
        setupExpiryTime = now + setupDuration;
    }

    modifier onlyDuringSetup {
        require(now < setupExpiryTime, "Can only perform this action during setup");
        _;
    }
}


interface ICrossChainManager {
    // View Functions
    function crossChainState() external view returns (address);

    function debtManager() external view returns (address);

    // supply schedule wrapper functions
    // function mintableSupply() external view returns (uint);
    // // supply schedule wrapper functions
    // function isMintable() external view returns (bool);
    // // supply schedule wrapper functions
    // function minterReward() external view returns (uint);

    // function getCrossChainIds() external view returns (bytes32[] memory);

    // function getNetworkId(bytes32 _chainID) external view returns (uint);

    function currentNetworkIssuedDebtOf(bytes32 currencyKey) external view returns (uint, bool);

    function currentNetworkActiveDebtOf(bytes32 currencyKey) external view returns (uint, bool);

    function currentNetworkIssuedDebt() external view returns (uint);

    function currentNetworkActiveDebt() external view returns (uint);

    // function getCrossNetworkIssuedDebt(bytes32 _chainID) external view returns (uint);

    // function getCrossNetworkActiveDebt(bytes32 _chainID) external view returns (uint);

    function crossNetworkIssuedDebtAll() external view returns (uint);

    function crossNetworkActiveDebtAll() external view returns (uint);

    function currentNetworkDebtPercentage() external view returns (uint);

    function movedAmount(uint _inboundOutbound, uint targetNetworkId) external view returns (uint);

    function outboundSumToCurrentNetwork() external view returns (uint);

    function syncTimestamp() external view returns (uint);

    function syncStale() external view returns (bool);

    // Mutative functions
    function setCrossChainState(address) external;

    function setDebtManager(address) external;

    // supply schedule wrapper functions
    // function recordMintEvent(uint supplyMinted) external returns (bool);

    // function setCrosschain(bytes32 _chainID) external;

    // function addCrosschain(bytes32 _chainID) external;

    // function addNetworkId(bytes32 _chainID, uint _networkId) external;

    // function addNetworkIds(uint[] calldata _networkIds) external;

    function addCurrentNetworkIssuedDebt(uint _amount) external;

    function subtractCurrentNetworkIssuedDebt(uint _amount) external;

    function setCrossNetworkIssuedDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external;

    function setCrossNetworkActiveDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external;

    function setOutboundSumToCurrentNetwork(uint _amount) external;

    // deprecated functions --> to be removed. they do nothing inside the contract
    // for backwards compatibility with Exchanger
    function addTotalNetworkDebt(uint amount) external;

    // for backwards compatibility with Exchanger
    function subtractTotalNetworkDebt(uint amount) external;

    // for backwards compatibility with Issuer
    function setCrossNetworkUserDebt(address account, uint userStateDebtLedgerIndex) external;

    // for backwards compatibility with Issuer
    function clearCrossNetworkUserDebt(address account) external;
}


interface ICrossChainState {
    struct CrossNetworkUserData {
        // total network debtLedgerIndex
        uint totalNetworkDebtLedgerIndex;
        // user state debtledgerIndex
        uint userStateDebtLedgerIndex;
    }

    // Views
    function getChainID() external view returns (uint);

    // function getCrossChainCount() external view returns (uint);

    function getCurrentNetworkIssuedDebt() external view returns (uint);

    function getTotalNetworkIssuedDebt() external view returns (uint);

    function getCrossNetworkIssuedDebtAll() external view returns (uint);

    function getCrossNetworkActiveDebtAll() external view returns (uint);

    function getOutboundSumToCurrentNetwork() external view returns (uint);

    // Mutative functions
    // function addNetworkId(uint _networkId) external;

    function setCrossNetworkIssuedDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external;

    function setCrossNetworkActiveDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external;

    function setCrossNetworkDebtsAll(
        uint[] calldata _chainIDs,
        uint[] calldata _debts,
        uint[] calldata _activeDebts,
        uint _inbound
    ) external;

    // function addCrossNetworkNDebts(
    //     uint _chainID,
    //     uint _issuedDebt,
    //     uint _activeDebt
    // ) external;

    function addIssuedDebt(uint _chainID, uint _amount) external;

    function subtractIssuedDebt(uint _chainID, uint _amount) external;

    function setOutboundSumToCurrentNetwork(uint _amount) external;

    function setInitialCurrentIssuedDebt(uint _amount) external;

    // deprecated functions --> to be removed. they do nothing inside the contract
    // function totalNetworkDebtLedgerLength() external view returns (uint);

    // function lastTotalNetworkDebtLedgerEntry() external view returns (uint);

    // function getTotalNetworkDebtEntryAtIndex(uint) external view returns (uint);

    // function getCrossNetworkUserData(address) external view returns (uint, uint);

    // function getCrossChainIds() external view returns (bytes32[] memory);

    // function getNetworkId(bytes32 _chainID) external view returns (uint);

    // function getCrossNetworkIssuedDebt(bytes32 _chainID) external view returns (uint);

    // function getCrossNetworkActiveDebt(bytes32 _chainID) external view returns (uint);

    // function setCrossNetworkUserData(address, uint) external;

    // function clearCrossNetworkUserData(address) external;

    // function appendTotalNetworkDebtLedger(uint) external;

    // function addTotalNetworkDebtLedger(uint amount) external;

    // function subtractTotalNetworkDebtLedger(uint amount) external;

    // function setCrosschain(bytes32 _chainID) external;

    // function addCrosschain(bytes32 chainID) external;
}


interface IDebtCache {
    // Views

    function cachedDebt() external view returns (uint);

    function cachedPynthDebt(bytes32 currencyKey) external view returns (uint);

    function cacheTimestamp() external view returns (uint);

    function cacheInvalid() external view returns (bool);

    function cacheStale() external view returns (bool);

    function currentPynthDebts(bytes32[] calldata currencyKeys)
        external
        view
        returns (uint[] memory debtValues, bool anyRateIsInvalid);

    function cachedPynthDebts(bytes32[] calldata currencyKeys) external view returns (uint[] memory debtValues);

    function currentDebt() external view returns (uint debt, bool anyRateIsInvalid);

    function cacheInfo()
        external
        view
        returns (
            uint debt,
            uint timestamp,
            bool isInvalid,
            bool isStale
        );

    // Mutative functions

    function updateCachedPynthDebts(bytes32[] calldata currencyKeys) external;

    function updateCachedPynthDebtWithRate(bytes32 currencyKey, uint currencyRate) external;

    function updateCachedPynthDebtsWithRates(bytes32[] calldata currencyKeys, uint[] calldata currencyRates) external;

    function updateDebtCacheValidity(bool currentlyInvalid) external;

    function purgeCachedPynthDebt(bytes32 currencyKey) external;

    function takeDebtSnapshot() external;
}


pragma experimental ABIEncoderV2;

interface IBridgeState {
    // ----VIEWS

    struct Signature {
        bytes32 r;
        bytes32 s;
        uint8 v;
    }

    function networkOpened(uint chainId) external view returns (bool);

    function accountOutboundings(
        address account,
        uint periodId,
        uint index
    ) external view returns (uint);

    function accountInboundings(address account, uint index) external view returns (uint);

    function inboundings(uint index)
        external
        view
        returns (
            address,
            uint,
            uint,
            uint,
            bool,
            Signature memory
        );

    function outboundings(uint index)
        external
        view
        returns (
            address,
            uint,
            uint,
            uint,
            Signature memory
        );

    function outboundPeriods(uint index)
        external
        view
        returns (
            uint,
            uint,
            uint[] memory,
            bool
        );

    function srcOutboundingIdRegistered(uint chainId, uint srcOutboundingId) external view returns (bool isRegistered);

    function numberOfOutboundPerPeriod() external view returns (uint);

    function periodDuration() external view returns (uint);

    function outboundingsLength() external view returns (uint);

    function getTotalOutboundAmount() external view returns (uint);

    function inboundingsLength() external view returns (uint);

    function getTotalInboundAmount() external view returns (uint);

    function outboundIdsInPeriod(uint outboundPeriodId) external view returns (uint[] memory);

    function isOnRole(bytes32 roleKey, address account) external view returns (bool);

    function accountOutboundingsInPeriod(address _account, uint _period) external view returns (uint[] memory);

    function applicableInboundIds(address account) external view returns (uint[] memory);

    function outboundRequestIdsInPeriod(address account, uint periodId) external view returns (uint[] memory);

    function periodIdsToProcess() external view returns (uint[] memory);

    function getMovedAmount(uint _inboundOutbound, uint targetNetworkId) external view returns (uint);

    // ----MUTATIVES

    function appendOutboundingRequest(
        address account,
        uint amount,
        uint destChainIds,
        Signature calldata sign
    ) external;

    function appendMultipleInboundingRequests(
        address[] calldata accounts,
        uint[] calldata amounts,
        uint[] calldata srcChainIds,
        uint[] calldata srcOutboundingIds,
        Signature[] calldata sign
    ) external;

    function appendInboundingRequest(
        address account,
        uint amount,
        uint srcChainId,
        uint srcOutboundingId,
        bytes32 r,
        bytes32 s,
        uint8 v
    ) external;

    function claimInbound(uint index, uint _amount) external;
}


// https://docs.peri.finance/contracts/source/interfaces/iexchangerates
interface IExchangeRates {
    // Structs
    struct RateAndUpdatedTime {
        uint216 rate;
        uint40 time;
    }

    struct InversePricing {
        uint entryPoint;
        uint upperLimit;
        uint lowerLimit;
        bool frozenAtUpperLimit;
        bool frozenAtLowerLimit;
    }

    // Views
    function aggregators(bytes32 currencyKey) external view returns (address);

    function aggregatorWarningFlags() external view returns (address);

    function anyRateIsInvalid(bytes32[] calldata currencyKeys) external view returns (bool);

    function canFreezeRate(bytes32 currencyKey) external view returns (bool);

    function currentRoundForRate(bytes32 currencyKey) external view returns (uint);

    function currenciesUsingAggregator(address aggregator) external view returns (bytes32[] memory);

    function effectiveValue(
        bytes32 sourceCurrencyKey,
        uint sourceAmount,
        bytes32 destinationCurrencyKey
    ) external view returns (uint value);

    function effectiveValueAndRates(
        bytes32 sourceCurrencyKey,
        uint sourceAmount,
        bytes32 destinationCurrencyKey
    )
        external
        view
        returns (
            uint value,
            uint sourceRate,
            uint destinationRate
        );

    function effectiveValueAtRound(
        bytes32 sourceCurrencyKey,
        uint sourceAmount,
        bytes32 destinationCurrencyKey,
        uint roundIdForSrc,
        uint roundIdForDest
    ) external view returns (uint value);

    function getCurrentRoundId(bytes32 currencyKey) external view returns (uint);

    function getLastRoundIdBeforeElapsedSecs(
        bytes32 currencyKey,
        uint startingRoundId,
        uint startingTimestamp,
        uint timediff
    ) external view returns (uint);

    function inversePricing(bytes32 currencyKey)
        external
        view
        returns (
            uint entryPoint,
            uint upperLimit,
            uint lowerLimit,
            bool frozenAtUpperLimit,
            bool frozenAtLowerLimit
        );

    function lastRateUpdateTimes(bytes32 currencyKey) external view returns (uint256);

    function oracle() external view returns (address);

    function rateAndTimestampAtRound(bytes32 currencyKey, uint roundId) external view returns (uint rate, uint time);

    function rateAndUpdatedTime(bytes32 currencyKey) external view returns (uint rate, uint time);

    function rateAndInvalid(bytes32 currencyKey) external view returns (uint rate, bool isInvalid);

    function rateForCurrency(bytes32 currencyKey) external view returns (uint);

    function rateIsFlagged(bytes32 currencyKey) external view returns (bool);

    function rateIsFrozen(bytes32 currencyKey) external view returns (bool);

    function rateIsInvalid(bytes32 currencyKey) external view returns (bool);

    function rateIsStale(bytes32 currencyKey) external view returns (bool);

    function rateStalePeriod() external view returns (uint);

    function ratesAndUpdatedTimeForCurrencyLastNRounds(bytes32 currencyKey, uint numRounds)
        external
        view
        returns (uint[] memory rates, uint[] memory times);

    function ratesAndInvalidForCurrencies(bytes32[] calldata currencyKeys)
        external
        view
        returns (uint[] memory rates, bool anyRateInvalid);

    function ratesForCurrencies(bytes32[] calldata currencyKeys) external view returns (uint[] memory);

    // Mutative functions
    function freezeRate(bytes32 currencyKey) external;
}


// https://docs.peri.finance/contracts/source/interfaces/isupplyschedule
interface ISupplySchedule {
    // Views
    function mintableSupply() external view returns (uint);

    function isMintable() external view returns (bool);

    function minterReward() external view returns (uint);

    // Mutative functions
    function recordMintEvent(uint supplyMinted) external returns (bool);
}


// https://docs.peri.finance/contracts/source/interfaces/isystemsettings
interface ISystemSettings {
    // Views
    function priceDeviationThresholdFactor() external view returns (uint);

    function waitingPeriodSecs() external view returns (uint);

    function issuanceRatio() external view returns (uint);

    function feePeriodDuration() external view returns (uint);

    function targetThreshold() external view returns (uint);

    function liquidationDelay() external view returns (uint);

    function liquidationRatio() external view returns (uint);

    function liquidationPenalty() external view returns (uint);

    function rateStalePeriod() external view returns (uint);

    function exchangeFeeRate(bytes32 currencyKey) external view returns (uint);

    function minimumStakeTime() external view returns (uint);

    function externalTokenQuota() external view returns (uint);

    function bridgeTransferGasCost() external view returns (uint);

    function bridgeClaimGasCost() external view returns (uint);

    function syncStaleThreshold() external view returns (uint);

    function debtSnapshotStaleTime() external view returns (uint);
}


/**
 * @dev Wrappers over Solidity's arithmetic operations with added overflow
 * checks.
 *
 * Arithmetic operations in Solidity wrap on overflow. This can easily result
 * in bugs, because programmers usually assume that an overflow raises an
 * error, which is the standard behavior in high level programming languages.
 * `SafeMath` restores this intuition by reverting the transaction when an
 * operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "SafeMath: subtraction overflow");
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-solidity/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, "SafeMath: division by zero");
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b != 0, "SafeMath: modulo by zero");
        return a % b;
    }
}


// Libraries


// https://docs.peri.finance/contracts/source/libraries/safedecimalmath
library SafeDecimalMath {
    using SafeMath for uint;

    /* Number of decimal places in the representations. */
    uint8 public constant decimals = 18;
    uint8 public constant highPrecisionDecimals = 27;

    /* The number representing 1.0. */
    uint public constant UNIT = 10**uint(decimals);

    /* The number representing 1.0 for higher fidelity numbers. */
    uint public constant PRECISE_UNIT = 10**uint(highPrecisionDecimals);
    uint private constant UNIT_TO_HIGH_PRECISION_CONVERSION_FACTOR = 10**uint(highPrecisionDecimals - decimals);

    /**
     * @return Provides an interface to UNIT.
     */
    function unit() external pure returns (uint) {
        return UNIT;
    }

    /**
     * @return Provides an interface to PRECISE_UNIT.
     */
    function preciseUnit() external pure returns (uint) {
        return PRECISE_UNIT;
    }

    /**
     * @return The result of multiplying x and y, interpreting the operands as fixed-point
     * decimals.
     *
     * @dev A unit factor is divided out after the product of x and y is evaluated,
     * so that product must be less than 2**256. As this is an integer division,
     * the internal division always rounds down. This helps save on gas. Rounding
     * is more expensive on gas.
     */
    function multiplyDecimal(uint x, uint y) internal pure returns (uint) {
        /* Divide by UNIT to remove the extra factor introduced by the product. */
        return x.mul(y) / UNIT;
    }

    /**
     * @return The result of safely multiplying x and y, interpreting the operands
     * as fixed-point decimals of the specified precision unit.
     *
     * @dev The operands should be in the form of a the specified unit factor which will be
     * divided out after the product of x and y is evaluated, so that product must be
     * less than 2**256.
     *
     * Unlike multiplyDecimal, this function rounds the result to the nearest increment.
     * Rounding is useful when you need to retain fidelity for small decimal numbers
     * (eg. small fractions or percentages).
     */
    function _multiplyDecimalRound(
        uint x,
        uint y,
        uint precisionUnit
    ) private pure returns (uint) {
        /* Divide by UNIT to remove the extra factor introduced by the product. */
        uint quotientTimesTen = x.mul(y) / (precisionUnit / 10);

        if (quotientTimesTen % 10 >= 5) {
            quotientTimesTen += 10;
        }

        return quotientTimesTen / 10;
    }

    /**
     * @return The result of safely multiplying x and y, interpreting the operands
     * as fixed-point decimals of a precise unit.
     *
     * @dev The operands should be in the precise unit factor which will be
     * divided out after the product of x and y is evaluated, so that product must be
     * less than 2**256.
     *
     * Unlike multiplyDecimal, this function rounds the result to the nearest increment.
     * Rounding is useful when you need to retain fidelity for small decimal numbers
     * (eg. small fractions or percentages).
     */
    function multiplyDecimalRoundPrecise(uint x, uint y) internal pure returns (uint) {
        return _multiplyDecimalRound(x, y, PRECISE_UNIT);
    }

    /**
     * @return The result of safely multiplying x and y, interpreting the operands
     * as fixed-point decimals of a standard unit.
     *
     * @dev The operands should be in the standard unit factor which will be
     * divided out after the product of x and y is evaluated, so that product must be
     * less than 2**256.
     *
     * Unlike multiplyDecimal, this function rounds the result to the nearest increment.
     * Rounding is useful when you need to retain fidelity for small decimal numbers
     * (eg. small fractions or percentages).
     */
    function multiplyDecimalRound(uint x, uint y) internal pure returns (uint) {
        return _multiplyDecimalRound(x, y, UNIT);
    }

    /**
     * @return The result of safely dividing x and y. The return value is a high
     * precision decimal.
     *
     * @dev y is divided after the product of x and the standard precision unit
     * is evaluated, so the product of x and UNIT must be less than 2**256. As
     * this is an integer division, the result is always rounded down.
     * This helps save on gas. Rounding is more expensive on gas.
     */
    function divideDecimal(uint x, uint y) internal pure returns (uint) {
        /* Reintroduce the UNIT factor that will be divided out by y. */
        return x.mul(UNIT).div(y);
    }

    /**
     * @return The result of safely dividing x and y. The return value is as a rounded
     * decimal in the precision unit specified in the parameter.
     *
     * @dev y is divided after the product of x and the specified precision unit
     * is evaluated, so the product of x and the specified precision unit must
     * be less than 2**256. The result is rounded to the nearest increment.
     */
    function _divideDecimalRound(
        uint x,
        uint y,
        uint precisionUnit
    ) private pure returns (uint) {
        uint resultTimesTen = x.mul(precisionUnit * 10).div(y);

        if (resultTimesTen % 10 >= 5) {
            resultTimesTen += 10;
        }

        return resultTimesTen / 10;
    }

    /**
     * @return The result of safely dividing x and y. The return value is as a rounded
     * standard precision decimal.
     *
     * @dev y is divided after the product of x and the standard precision unit
     * is evaluated, so the product of x and the standard precision unit must
     * be less than 2**256. The result is rounded to the nearest increment.
     */
    function divideDecimalRound(uint x, uint y) internal pure returns (uint) {
        return _divideDecimalRound(x, y, UNIT);
    }

    /**
     * @return The result of safely dividing x and y. The return value is as a rounded
     * high precision decimal.
     *
     * @dev y is divided after the product of x and the high precision unit
     * is evaluated, so the product of x and the high precision unit must
     * be less than 2**256. The result is rounded to the nearest increment.
     */
    function divideDecimalRoundPrecise(uint x, uint y) internal pure returns (uint) {
        return _divideDecimalRound(x, y, PRECISE_UNIT);
    }

    /**
     * @dev Convert a standard decimal representation to a high precision one.
     */
    function decimalToPreciseDecimal(uint i) internal pure returns (uint) {
        return i.mul(UNIT_TO_HIGH_PRECISION_CONVERSION_FACTOR);
    }

    /**
     * @dev Convert a high precision decimal to a standard decimal representation.
     */
    function preciseDecimalToDecimal(uint i) internal pure returns (uint) {
        uint quotientTimesTen = i / (UNIT_TO_HIGH_PRECISION_CONVERSION_FACTOR / 10);

        if (quotientTimesTen % 10 >= 5) {
            quotientTimesTen += 10;
        }

        return quotientTimesTen / 10;
    }

    /**
     * @dev Round down the value with given number
     */
    function roundDownDecimal(uint x, uint d) internal pure returns (uint) {
        return x.div(10**d).mul(10**d);
    }

    /**
     * @dev Round up the value with given number
     */
    function roundUpDecimal(uint x, uint d) internal pure returns (uint) {
        uint _decimal = 10**d;

        if (x % _decimal > 0) {
            x = x.add(10**d);
        }

        return x.div(_decimal).mul(_decimal);
    }
}


// Inheritance


// Libraries


contract CrossChainManager is Owned, MixinResolver, LimitedSetup, ICrossChainManager {
    using SafeMath for uint;
    using SafeDecimalMath for uint;

    bytes32 internal constant pUSD = "pUSD";

    address internal _crossChainState;
    address internal _debtManager;

    uint internal _syncTimestamp;
    bool internal _isStale;

    bytes32 private constant CONTRACT_DEBTCACHE = "DebtCache";
    bytes32 private constant CONTRACT_BRIDGESTATEPUSD = "BridgeStatepUSD";
    bytes32 private constant CONTRACT_EXCHANGERATES = "ExchangeRates";
    bytes32 private constant CONTRACT_SUPPLYSCHEDULE = "SupplySchedule";
    bytes32 private constant CONTRACT_SYSTEMSETTINGS = "SystemSettings";

    constructor(
        address _owner,
        address _resolver,
        address _crossChainStateAddress,
        address _debtManagerAddress
    ) public Owned(_owner) MixinResolver(_resolver) LimitedSetup(2 weeks) {
        _crossChainState = _crossChainStateAddress;
        _debtManager = _debtManagerAddress;
        _syncTimestamp = block.timestamp;
        _isStale = false;
    }

    //*********************** View functions ***************************
    /**
     * @notice return addresses of required resolver instance
     * @return address of required resolver addresses
     */
    function resolverAddressesRequired() public view returns (bytes32[] memory addresses) {
        addresses = new bytes32[](5);
        addresses[0] = CONTRACT_DEBTCACHE;
        addresses[1] = CONTRACT_BRIDGESTATEPUSD;
        addresses[2] = CONTRACT_EXCHANGERATES;
        addresses[3] = CONTRACT_SUPPLYSCHEDULE;
        addresses[4] = CONTRACT_SYSTEMSETTINGS;
    }

    /**
     * @notice return debtCache instance
     * @return debtCache instance
     */
    function debtCache() internal view returns (IDebtCache) {
        return IDebtCache(requireAndGetAddress(CONTRACT_DEBTCACHE));
    }

    /**
     * @notice return cross chain state instance
     * @return cross chain state instance
     */
    function state() internal view returns (ICrossChainState) {
        return ICrossChainState(_crossChainState);
    }

    /**
     * @notice return bridge state instance
     * @return bridge state instance
     */
    function bridgeStatepUSD() internal view returns (IBridgeState) {
        return IBridgeState(requireAndGetAddress(CONTRACT_BRIDGESTATEPUSD));
    }

    /**
     * @notice return exchange rates instance
     * @return exchange rates instance
     */
    function exchangeRates() internal view returns (IExchangeRates) {
        return IExchangeRates(requireAndGetAddress(CONTRACT_EXCHANGERATES));
    }

    /**
     * @notice return system settings instance
     * @return system settings instance
     */
    function systemSettings() internal view returns (ISystemSettings) {
        return ISystemSettings(requireAndGetAddress(CONTRACT_SYSTEMSETTINGS));
    }

    /**
     * @notice return supply schedule instance
     * @dev called by inFlationalMint
     * @return supply schedule instance
     */
    function supplySchedule() internal view returns (ISupplySchedule) {
        return ISupplySchedule(requireAndGetAddress(CONTRACT_SUPPLYSCHEDULE));
    }

    /**
     * @notice return current network's mintable supply
     * @dev called by inFlationalMint
     * @return mintable supply
     */
    function mintableSupply() external view returns (uint supplyToMint) {
        uint _currRate = _currentNetworkDebtPercentage();
        require(SafeDecimalMath.preciseUnit() >= _currRate, "Network rate invalid");
        require(!_syncStale(_syncTimestamp) || SafeDecimalMath.preciseUnit() == _currRate, "Cross chain debt is stale");

        supplyToMint = supplySchedule().mintableSupply();
        require(supplyToMint > 0, "No mintable supply");

        supplyToMint = supplyToMint
            .decimalToPreciseDecimal()
            .multiplyDecimalRoundPrecise(_currRate)
            .preciseDecimalToDecimal();

        return supplyToMint;
    }

    function isMintable() external view returns (bool) {
        return supplySchedule().isMintable();
    }

    function minterReward() external view returns (uint) {
        return supplySchedule().minterReward();
    }

    /**
     * @notice return current chain id
     * @return current chain id
     */
    function getChainID() external view returns (uint) {
        return state().getChainID();
    }

    /**
     * @notice return cross chain state instance address
     * @return cross chain state instance address
     */
    function crossChainState() external view returns (address) {
        return _crossChainState;
    }

    /**
     * @notice return debt manager instance address
     * @return debt manager instance address
     */
    function debtManager() external view returns (address) {
        return _debtManager;
    }

    /**
     * @notice return cross chain synchronization timestamp
     * @return cross chain synchronization timestamp
     */
    function syncTimestamp() external view returns (uint) {
        return _syncTimestamp;
    }

    /**
     * @notice return cross chain synchronization stale flag
     * @return cross chain synchronization stale flag
     */
    function isStale() external view returns (bool) {
        return _isStale;
    }

    /**
     * @notice return current cross chain count
     * @return current cross chain count
     */
    // function crossChainCount() external view returns (uint) {
    //     return state().getCrossChainCount();
    // }

    /**
     * @notice return current sum of total network system debt
     * @dev current network's active debt by currency key
     * @param currencyKey currency key
     * @return totalSystemValue debt,
     * @return anyRateIsInvalid any rate is invalid
     */
    function currentNetworkIssuedDebtOf(bytes32 currencyKey)
        external
        view
        returns (uint totalSystemValue, bool anyRateIsInvalid)
    {
        totalSystemValue = _currentNetworkIssuedDebt();

        if (currencyKey == pUSD) {
            return (totalSystemValue, false);
        }

        (uint currencyRate, bool currencyRateInvalid) = exchangeRates().rateAndInvalid(currencyKey);

        return (totalSystemValue.divideDecimalRound(currencyRate), currencyRateInvalid);
    }

    /**
     * @notice Get current network's active debt by currency key
     * @dev needs to consider issued debt change by staking and burning between the cross chain synchronization
     * @return totalSystemValue debt,
     * @return anyRateIsInvalid any rate is invalid
     */
    function currentNetworkActiveDebtOf(bytes32 currencyKey)
        external
        view
        returns (uint totalSystemValue, bool anyRateIsInvalid)
    {
        (totalSystemValue, anyRateIsInvalid) = _currentNetworkActiveDebt();

        if (currencyKey == pUSD) {
            return (totalSystemValue, anyRateIsInvalid);
        }

        (uint currencyRate, bool currencyRateInvalid) = exchangeRates().rateAndInvalid(currencyKey);

        totalSystemValue = totalSystemValue
            .decimalToPreciseDecimal()
            .divideDecimalRoundPrecise(currencyRate.decimalToPreciseDecimal())
            .preciseDecimalToDecimal();

        anyRateIsInvalid = anyRateIsInvalid || currencyRateInvalid;

        // return (totalSystemValue.divideDecimalRound(currencyRate), anyRateIsInvalid || currencyRateInvalid);
    }

    /**
     * @notice Get current network's issued debt
     * @dev deprecated
     * @return outbound amount
     */
    function currentNetworkIssuedDebt() external view returns (uint) {
        uint issuedDebt = _currentNetworkIssuedDebt();
        return issuedDebt;
    }

    /**
     * @notice Get current network's active debt
     * @dev needs to consider issued debt change by staking and burning between the cross chain synchronization
     * @return outbound amount
     */
    function currentNetworkActiveDebt() external view returns (uint) {
        (uint activeDebt, ) = _currentNetworkActiveDebt();
        return activeDebt;
    }

    /**
     * @notice Get connected chain's total issued debt
     * @return issued debt
     */
    function crossNetworkIssuedDebtAll() external view returns (uint) {
        return state().getCrossNetworkIssuedDebtAll();
    }

    /**
     * @notice Get connected chain's total active debt
     * @dev may need more robust way of secure the crosschain debts
     * @return active debt
     */
    function crossNetworkActiveDebtAll() external view returns (uint) {
        return state().getCrossNetworkActiveDebtAll();
    }

    /**
     * @notice Get CURRENT debt percentage of network by total networks
     * @dev external function
     * @return current debt ratio of network by total network debt
     */
    function currentNetworkDebtPercentage() external view returns (uint) {
        return _currentNetworkDebtPercentage();
    }

    /**
     * @notice Get current network's in&outbound net amount
     * @dev used for cross chain debt synchronization
     * @return outbound amount
     */
    function movedAmount(uint _inboundOutbound, uint targetNetworkId) external view returns (uint) {
        return bridgeStatepUSD().getMovedAmount(_inboundOutbound, targetNetworkId);
    }

    /**
     * @notice Get current network's inbound amount compiled by other networks
     * @dev used for cross chain debt synchronization
     * @return inbound amount
     */
    function outboundSumToCurrentNetwork() external view returns (uint) {
        return state().getOutboundSumToCurrentNetwork();
    }

    /**
     * @notice Get current cross chain synchronization stale flag
     * @dev used for cross chain debt synchronization
     * @return inbound amount
     */
    function syncStale() external view returns (bool) {
        return _syncStale(_syncTimestamp) && SafeDecimalMath.preciseUnit() != _currentNetworkDebtPercentage();
    }

    // ********************* Internal View functions ***************************

    /**
     * @notice Get CURRENT debt percentage of network by total networks
     * @dev internal function
     * @return current debt ratio of network by total network debt
     */
    function _currentNetworkDebtPercentage() internal view returns (uint networkPercentage) {
        uint totalIssuedDebt = state().getTotalNetworkIssuedDebt();

        networkPercentage = totalIssuedDebt == 0
            ? SafeDecimalMath.preciseUnit()
            : _currentNetworkIssuedDebt().decimalToPreciseDecimal().divideDecimalRoundPrecise(
                totalIssuedDebt.decimalToPreciseDecimal()
            );
    }

    /**
     * @notice Get CURRENT network's issued debt
     * @dev internal function
     * @return currentNetworkIssuedDebt current network's issued debt
     */
    function _currentNetworkIssuedDebt() internal view returns (uint networkIssuedDebt) {
        networkIssuedDebt = state().getCurrentNetworkIssuedDebt();
    }

    /**
     * @notice Get CURRENT network's in&out included debt percentage of network by total networks
     * @dev possibly deprecated
     * @return currentNetworActivekDebt current network's active debt
     */
    function _currentNetworkActiveDebt() internal view returns (uint currentNetworActivekDebt, bool anyRateIsInvalid) {
        bool cachStale;
        (currentNetworActivekDebt, , anyRateIsInvalid, cachStale) = debtCache().cacheInfo();
        anyRateIsInvalid = anyRateIsInvalid || cachStale;

        // get current network's active debt by applying In&Out amount
        (uint inboundAmount, uint outboundAmount) = _getInOutAmount();
        currentNetworActivekDebt = currentNetworActivekDebt.add(outboundAmount).sub(inboundAmount);

        // get current network's active debt after multiplying the debt percentage to the total active debt
        currentNetworActivekDebt = currentNetworActivekDebt
            .add(state().getCrossNetworkActiveDebtAll())
            .decimalToPreciseDecimal()
            .multiplyDecimalRoundPrecise(_currentNetworkDebtPercentage())
            .preciseDecimalToDecimal();
    }

    /**
     * @notice Get CURRENT network's in&out
     * @dev internal function
     * @return inboundAmount inbound amount
     * @return outboundAmount  outbound amount
     */
    function _getInOutAmount() internal view returns (uint inboundAmount, uint outboundAmount) {
        outboundAmount = bridgeStatepUSD().getTotalOutboundAmount();
        inboundAmount = bridgeStatepUSD().getTotalInboundAmount();

        // if the amount is less than the amount comiled by the other networks, which means bridge is not synchronized yet
        uint outboundFromOtherNetwork = state().getOutboundSumToCurrentNetwork();
        // so, we need to use the-others's sum of outbound targeted to current network
        inboundAmount = inboundAmount < outboundFromOtherNetwork ? outboundFromOtherNetwork : inboundAmount;
    }

    /**
     * @notice Get synced timestamp
     * @dev Note a 0 timestamp means that the sync is uninitialised.
     * @param timestamp cross chain synchronization timestamp
     */
    function _syncStale(uint timestamp) internal view returns (bool) {
        return systemSettings().debtSnapshotStaleTime() < block.timestamp - timestamp || timestamp == 0 || _isStale;
    }

    //************************* Mutative functions ***************************
    /**
     * @notice set state instance address for cross chain state management
     * @param crossChainStateAddress address of cross chain state instance
     */
    function setCrossChainState(address crossChainStateAddress) external onlyOwner {
        _crossChainState = crossChainStateAddress;
    }

    /**
     * @notice set debt manager address for cross chain debt operation
     * @param debtManagerAddress address of cross chain instance
     */
    function setDebtManager(address debtManagerAddress) external onlyOwner {
        _debtManager = debtManagerAddress;
    }

    /**
     * @notice add multiple connected chain name ids and network ids for cross chain state management
     * @param _networkIds network ids
     */
    // function addNetworkIds(uint[] calldata _networkIds) external onlyOwner {
    //     for (uint i; i < _networkIds.length; i++) {
    //         state().addNetworkId(_networkIds[i]);
    //     }
    // }

    /**
     * @notice add current network's issued debt
     * @dev keep track of current network's issued debt
     * @param _amount debt amount
     */
    function addCurrentNetworkIssuedDebt(uint _amount) external {
        state().addIssuedDebt(state().getChainID(), _amount);

        _setStaleByDebtChangeRate(_amount);

        emit IssuedDebtAdded(_amount, state().getCurrentNetworkIssuedDebt(), block.timestamp, _isStale);
    }

    /**
     * @notice subtract current network's issued debt
     * @dev keep track of current network's issued debt
     * @param _amount debt amount
     */
    function subtractCurrentNetworkIssuedDebt(uint _amount) external {
        state().subtractIssuedDebt(state().getChainID(), _amount);

        _setStaleByDebtChangeRate(_amount);

        emit IssuedDebtSubtracted(_amount, state().getCurrentNetworkIssuedDebt(), block.timestamp, _isStale);
    }

    /**
     * @notice set all connected cross-chain's issued debt
     * @dev issued debt is the debt that is issued by the connected chain
     * @param _chainIDs chain ids
     * @param _amounts debt array
     */
    function setCrossNetworkIssuedDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external onlyDebtManager {
        state().setCrossNetworkIssuedDebtAll(_chainIDs, _amounts);
        _syncTimestamp = block.timestamp;
        _isStale = false;

        for (uint i; i < _chainIDs.length; i++) {
            emit CrossChainIssuedDebtSynced(_chainIDs[i], _amounts[i], block.timestamp);
        }
    }

    /**
     * @notice set all connected cross-chain's active debt
     * @dev active debt is the debt that is all issued pynths assets' floating amount calculated by the exchange rate
     * @param _chainIDs chain ids
     * @param _amounts debt array
     */
    function setCrossNetworkActiveDebtAll(uint[] calldata _chainIDs, uint[] calldata _amounts) external onlyDebtManager {
        state().setCrossNetworkActiveDebtAll(_chainIDs, _amounts);
        _syncTimestamp = block.timestamp;
        _isStale = false;

        for (uint i; i < _chainIDs.length; i++) {
            emit CrossChainActiveDebtSynced(_chainIDs[i], _amounts[i], block.timestamp);
        }
    }

    /**
     * @notice set all connected cross-chain's issued debt and inbound amount
     * @dev called by scheduler
     * @param _chainIDs chain ids
     * @param _activeDebts debt array
     * @param _issuedDebts debt array
     * @param _inbound current network's inbound amount compiled by other networks
     */
    function setCrossNetworkDebtsAll(
        uint[] calldata _chainIDs,
        uint[] calldata _issuedDebts,
        uint[] calldata _activeDebts,
        uint _inbound
    ) external onlyDebtManager {
        state().setCrossNetworkDebtsAll(_chainIDs, _issuedDebts, _activeDebts, _inbound);
        _syncTimestamp = block.timestamp;
        _isStale = false;

        for (uint i; i < _chainIDs.length; i++) {
            emit CrossChainSynced(_chainIDs[i], _issuedDebts[i], _activeDebts[i], block.timestamp);
        }
    }

    /**
     * @notice set current network's issued debt
     * @dev calling the function is strictly limited to the setup period
    //**** When upgrading the contract, the function SHOULD BE NOT BE CALLED AGAIN.
     *      Instead, you need move to previose network's issued debt to the new contract 
     */
    function setInitialCurrentIssuedDebt(address _prevState) external onlyOwner onlyDuringSetup {
        state().setInitialCurrentIssuedDebt(ICrossChainState(_prevState).getCurrentNetworkIssuedDebt());
    }

    /**
     * @notice set inbound to current network from other networks
     * @param _amount debt amount
     */
    function setOutboundSumToCurrentNetwork(uint _amount) external onlyDebtManager {
        state().setOutboundSumToCurrentNetwork(_amount);
    }

    // ************************* Internal Mutative functions ***************************

    /**
     * @notice set cross chain synchronization stale flag
     * @dev internal function
     * @param _amount changed debt amount
     */
    function _setStaleByDebtChangeRate(uint _amount) internal {
        if (
            state().getCurrentNetworkIssuedDebt() > 0 &&
            _amount.divideDecimalRound(state().getCurrentNetworkIssuedDebt()) >= systemSettings().syncStaleThreshold()
        ) {
            _isStale = true;
        }
    }

    //*********************** */ Modifiers *******************************
    /**
     * @notice check if the caller is debt manager
     * @dev modifier
     */
    modifier onlyDebtManager() {
        require(msg.sender == _debtManager, "Only the debt manager may perform this action");
        _;
    }

    // /**
    //  * @notice check if the caller is debtCache or exchanger
    //  */
    // modifier onlyDebtCache() {
    //     _onlyDebtCache(); // Use an internal function to save code size.
    //     _;
    // }

    // /**
    //  * @notice check if the caller is debtCache or exchanger
    //  */
    // function _onlyDebtCache() internal view {
    //     bool isDebtCache = msg.sender == address(debtCache());
    //     require(isDebtCache, "CrossChainManager: Only the debtCache contract can perform this action");
    // }

    //****************** deprecated *******************/
    //****************** deprecated *******************/
    // View functions

    // /**
    //  * @notice Get cross-chain ids
    //  * @return current debt ratio of network by total network debt
    //  */
    // function getCrossChainIds() external view returns (bytes32[] memory) {
    //     return state().getCrossChainIds();
    // }

    // /**
    //  * @notice Get connected chain's issued debt
    //  * @dev possibly deprecated
    //  * @param _chainID chain id
    //  * @return issued debt
    //  */
    // function getCrossNetworkIssuedDebt(bytes32 _chainID) external view returns (uint) {
    //     return state().getCrossNetworkIssuedDebt(_chainID);
    // }

    // /**
    //  * @notice Get connected chain's active debt
    //  * @param _chainID chain id
    //  * @return active debt
    //  */
    // function getCrossNetworkActiveDebt(bytes32 _chainID) external view returns (uint) {
    //     return state().getCrossNetworkActiveDebt(_chainID);
    // }

    // /**
    //  * @notice return current sum of total network system debt
    //  * @dev deprecated
    //  * @return totalNetworkDebt uint
    //  */
    // function currentTotalNetworkDebt() external view returns (uint) {
    //     return state().lastTotalNetworkDebtLedgerEntry();
    // }

    // /**
    //  * @notice return user's cross chain debt entry index
    //  * @dev possibly deprecated
    //  */
    // function userIssuanceDataForTotalNetwork(address account)
    //     external
    //     view
    //     returns (uint crossChainDebtEntryIndex, uint userStateDebtLedgerIndex)
    // {
    //     (crossChainDebtEntryIndex, userStateDebtLedgerIndex) = state().getCrossNetworkUserData(account);
    // }

    // Mutative functions

    // /**
    //  * @notice add connected chain's issued debt
    //  * @dev deprecated
    //  * @param _chainID chain id
    //  * @param _amount debt amount
    //  */
    // function addIssuedDebt(bytes32 _chainID, uint _amount) external {
    //     state().addIssuedDebt(_chainID, _amount);
    //     return;
    // }

    // /**
    //  * @notice subtract connected chain's issued debt
    //  * @dev deprecated
    //  * @param _chainID chain id
    //  * @param _amount debt amount
    //  */
    // function subtractIssuedDebt(bytes32 _chainID, uint _amount) external {
    //     state().subtractIssuedDebt(_chainID, _amount);
    //     return;
    // }

    // /**
    //  * @notice save current sum of total network system debt to the state
    //  * @dev deprecated
    //  * @param totalNetworkDebt uint
    //  */
    // function appendTotalNetworkDebt(uint totalNetworkDebt) external onlyDebtManager {
    //     state().appendTotalNetworkDebtLedger(totalNetworkDebt);
    // }

    /**
     * @notice add amount to current sum of total network system debt
     * @dev deprecated
     * @param amount  debt amount
     */
    function addTotalNetworkDebt(uint amount) external {}

    /**
     * @notice subtract amount from current sum of total network system debt
     * @dev deprecated
     * @param amount debt amount
     */
    function subtractTotalNetworkDebt(uint amount) external {}

    /**
     * @notice set user's cross chain debt entry index
     * @dev deprecated
     * @param account user's address
     */
    function setCrossNetworkUserDebt(address account, uint userStateDebtLedgerIndex) external {}

    /**
     * @notice clear user's cross chain debt entry index
     * @dev deprecated
     * @param account user's address
     */
    function clearCrossNetworkUserDebt(address account) external {}

    //*********************** Events ***************************
    /**
     * @notice Emitted when current network issued debt has added
     * @param amount uint
     * @param latestNetworkDebt uint
     * @param timestamp uint
     * @param syncInvalid bool
     */
    event IssuedDebtAdded(uint amount, uint latestNetworkDebt, uint timestamp, bool syncInvalid);

    /**
     * @notice Emitted when current network issued debt has subtracted
     * @param amount uint
     * @param latestNetworkDebt uint
     * @param timestamp uint
     * @param syncInvalid bool
     */
    event IssuedDebtSubtracted(uint amount, uint latestNetworkDebt, uint timestamp, bool syncInvalid);

    /**
     * @notice Emitted when current network debt has synchronized
     * @param chainID uint
     * @param issuedDebt uint
     * @param activeDebt uint
     * @param timestamp uint
     */
    event CrossChainSynced(uint chainID, uint issuedDebt, uint activeDebt, uint timestamp);

    /**
     * @notice Emitted when current network issued debt has subtracted
     * @param chainID uint
     * @param issuedDebt uint
     * @param timestamp uint
     */
    event CrossChainIssuedDebtSynced(uint chainID, uint issuedDebt, uint timestamp);

    /**
     * @notice Emitted when current network active debt has subtracted
     * @param chainID uint
     * @param activeDebt uint
     * @param timestamp uint
     */
    event CrossChainActiveDebtSynced(uint chainID, uint activeDebt, uint timestamp);
}