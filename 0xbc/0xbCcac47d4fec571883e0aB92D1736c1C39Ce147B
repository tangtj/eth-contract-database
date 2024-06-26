// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;


/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address recipient, uint256 amount) external returns (bool);

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
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

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
}

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
}


/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        require(!paused(), "Pausable: paused");
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        require(paused(), "Pausable: not paused");
        _;
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
    }
}


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

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

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
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

    /**
    * @title AggregatorV3Interface
    * @dev This interface represents the set of functions provided by the Chainlink V3 price feeds.
    */
    interface AggregatorV3Interface {
        function decimals() external view returns (uint8); // Returns the number of decimals the price is reported with
        function description() external view returns (string memory); // Returns a human-readable description of the price feed
        function version() external view returns (uint256); // Returns the version of the price feed contract
        function getRoundData(uint80 _roundId) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound); // Returns the data for a specific round ID
        function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound); // Returns the data for the latest round
    }


    /**
    * @title IUniswapV3Pool
    * @dev Interface for interacting with a Uniswap V3 Pool.
    *
    * Uniswap V3 Pools are core components in the Uniswap V3 protocol, which facilitate
    * token swaps and provide pricing information. This interface abstracts the method
    * to get essential data from a Uniswap V3 pool.
    */
    interface IUniswapV3Pool {
        function slot0() external view returns (
            uint160 sqrtPriceX96,
            int24 tick,
            uint16 observationIndex,
            uint16 observationCardinality,
            uint16 observationCardinalityNext,
            uint8 feeProtocol,
            bool unlocked
        );
    }

/**
 * @title Subscription Contract for NEFSTER.COM
 * @dev This contract allows users to subscribe to a service using the NEF token.
 */
contract Subscription is Ownable, Pausable {

    enum SubscriptionPeriod { ThirtyDays, NinetyDays, OneEightyTwoDays, ThreeSixtyFiveDays }

    struct Subscriber {
        uint256 start;
        uint256 end;
        SubscriptionPeriod period;
    }

    mapping(address => Subscriber) public subscribers;
    mapping(SubscriptionPeriod => uint256) public subscriptionPricesUSD;

    AggregatorV3Interface private priceFeedETH;

    // Declare NEF Token Address as constant
    address private constant NEF_ADDRESS = 0xDa6593dBF7604744972B1B6C6124cB6981b3c833; 
    IERC20 private constant NEF = IERC20(NEF_ADDRESS);

    address private constant UNISWAP_NEF_USDC_PAIR = 0xcB3214329F83EF1265c5db47FD368408B470844A; // Address of NEF/USDC pair on Uniswap v3 

    IUniswapV3Pool private uniswapV3Pool;

    uint256 public subscriptionPriceUSD;

    event SubscribeEvent(address indexed user, uint256 start, uint256 end, address token);
    event UnsubscribeEvent(address indexed user);
    event CollectPaymentEvent(address indexed user, uint256 amount);
    event UnfundedErrorEvent(address indexed user);

    /**
     * @dev Contract constructor. Sets initial subscription price and token addresses.
     */
    constructor() {

        // Initialize default prices
        subscriptionPricesUSD[SubscriptionPeriod.ThirtyDays] = 20; 
        subscriptionPricesUSD[SubscriptionPeriod.NinetyDays] = 60; 
        subscriptionPricesUSD[SubscriptionPeriod.OneEightyTwoDays] = 110; 
        subscriptionPricesUSD[SubscriptionPeriod.ThreeSixtyFiveDays] = 200; 

        // Initialize Uniswap V3 Pool for NEF/USDC
        uniswapV3Pool = IUniswapV3Pool(UNISWAP_NEF_USDC_PAIR);
    }

    /**
    * @dev Sets the subscription price for a specified period.
    * This function can only be called by the contract owner.
    * It updates the USD price for a given subscription period in the `subscriptionPricesUSD` mapping.
    *
    * @param _period The subscription period (enum SubscriptionPeriod) for which the price is being set.
    *                This can be one of the predefined periods like NinetyDays, OneEightyTwoDays, or ThreeSixtyFiveDays.
    * @param _priceUSD The price in USD that is being set for the given subscription period.
    *                  This is the cost in USD for the entire duration of the subscription period.
    *
    * Requirements:
    * - The caller must be the owner of the contract.
    * - The `_priceUSD` must be a positive number.
    *
    * Emits no events.
    */
    function setSubscriptionPriceForPeriod(SubscriptionPeriod _period, uint256 _priceUSD) external onlyOwner {
        subscriptionPricesUSD[_period] = _priceUSD;
    }


    /**
    * @dev Calculates the NEF token amount required for a subscription based on the period selected.
    *
    * This function takes a subscription period as input and returns the amount of NEF tokens
    * required for that subscription period, based on the current NEF price per USDC from Uniswap.
    * The function scales the USD price of the subscription to USDC's 6 decimal places, calculates
    * the NEF amount equivalent to that USD amount at the current NEF price per USDC, and then
    * scales the result back to NEF's decimal space.
    *
    * @param _period The subscription period for which the price is being calculated.
    *                Can be NinetyDays, OneEightyTwoDays, or ThreeSixtyFiveDays.
    *
    * @return nefAmountForUsdPrice The amount of NEF tokens required for the subscription,
    *         calculated based on the current NEF price per USDC and the USD price of the subscription.
    *         The result takes into account the decimals of both NEF and USDC tokens.
    *
    * Note: The NEF price per USDC is obtained from the Uniswap pool and represents the
    *       amount of NEF you get for 1 USDC, scaled to NEF's decimal space. This function
    *       assumes the usdPrice is in whole dollars and scales it to match USDC's 6 decimals
    *       before performing the calculation. The final amount is adjusted to NEF's decimal
    *       space for accurate pricing.
    */
    function getSubscriptionPriceForPeriod(SubscriptionPeriod _period) public view returns (uint256) {
        uint256 usdPrice = subscriptionPricesUSD[_period]; 
        uint256 nefPricePerUSD = getNEFPrice(); // NEF you get for 1 USDC, in wei
        
        // Assuming usdPrice is in whole dollars, scale it to match USDC's 6 decimals
        uint256 scaledUsdPrice = usdPrice * 10**6; // Scale USD price to USDC's decimals

        // Calculate NEF amount for the USD price
        // Here, you multiply the scaled USD price by the rate (NEF per USDC)
        // and divide by 10**6 to convert from USDC's decimal space to NEF's
        uint256 nefAmountForUsdPrice = (scaledUsdPrice * nefPricePerUSD) / 10**6;

        return nefAmountForUsdPrice;
    }


    /**
    * @dev Retrieves the current price of NEF in terms of USDC from the Uniswap V3 pool.
    *
    * This function fetches the current sqrtPriceX96 from the Uniswap V3 NEF/USDC pool and
    * calculates the NEF price per USDC. The sqrtPriceX96 is a representation of the price
    * that is encoded in a way to facilitate efficient computation within the Uniswap V3 contracts.
    * This function converts that representation back into a regular price format, adjusting
    * for the decimal places of NEF and USDC tokens.
    *
    * The calculation involves squaring the sqrtPriceX96 value to get the actual price
    * (in the format of price per 1 USDC in terms of NEF), adjusting for the fixed-point
    * precision used in the Uniswap pool (2 ** 96), and then scaling the result to account
    * for the decimal differences between NEF (18 decimals) and USDC (6 decimals).
    *
    * @return The price of 1 USDC in terms of NEF, scaled to 18 decimal places. This represents
    *         how many NEF tokens can be bought with 1 USDC according to the current Uniswap
    *         market price. The function ensures that the price is accurately adjusted for the
    *         different decimal places of NEF and USDC, providing a straightforward and
    *         precise value that can be used for calculations in other functions.
    */
    function getNEFPrice() public view returns (uint256) {
        (uint160 sqrtPriceX96, , , , , , ) = uniswapV3Pool.slot0();

        // Ensure we don't lose precision in the calculation
        // Scale up the USDC price before dividing to maintain precision
        uint256 price = uint256(sqrtPriceX96) * uint256(sqrtPriceX96);
        price = price / (2 ** 96) / (2 ** 96); // Adjust for Uniswap's sqrtPriceX96 scaling

        // Now, scale up the USDC price to NEF's 18 decimals before dividing
        return (price * 1e18) / 1e12; // Adjust for USDC's 6 decimals to NEF's 18 decimals
    }


    /**
    * @dev Allows a user to subscribe to a service by paying with NEF tokens.
    *
    * This function enables users to subscribe to a service for a specified period by transferring
    * the necessary amount of NEF tokens from their account to the contract. The subscription price
    * is determined based on the selected subscription period and the current NEF price in USDC.
    *
    * @param _period The subscription period the user is subscribing to. It is one of the predefined
    *                periods (e.g., NinetyDays, OneEightyTwoDays, ThreeSixtyFiveDays) specified by the
    *                SubscriptionPeriod enum.
    *
    * The function performs the following operations:
    * 1. Retrieves the subscription price in NEF tokens for the specified period by calling
    *    `getSubscriptionPriceForPeriod`.
    * 2. Transfers the calculated amount of NEF tokens from the user's account to the contract. This
    *    requires the user to have previously approved the contract to spend the necessary amount of NEF.
    * 3. Updates the subscriber's record in the `subscribers` mapping with the subscription start time,
    *    end time, and period. The start time is set to the current block timestamp, and the end time is
    *    calculated by adding the duration of the subscription period to the start time.
    * 4. Emits a SubscribeEvent with the subscriber's address, subscription start and end times, and the
    *    token used for payment.
    *
    * Requirements:
    * - The function can only be called when the contract is not paused.
    * - The NEF transfer from the user to the contract must succeed.
    *
    * Emits:
    * - A `SubscribeEvent` indicating the user has successfully subscribed to the service.
    */
    function subscribe(SubscriptionPeriod _period) external whenNotPaused {
        uint256 subscriptionPrice = getSubscriptionPriceForPeriod(_period);
        require(NEF.transferFrom(msg.sender, address(this), subscriptionPrice), "Transfer failed");

        Subscriber storage subscriber = subscribers[msg.sender];
        subscriber.start = block.timestamp;
        subscriber.end = block.timestamp + getSubscriptionDuration(_period);
        subscriber.period = _period;

        emit SubscribeEvent(msg.sender, subscriber.start, subscriber.end, address(NEF));
    }


    /**
    * @dev Calculates and returns the duration in seconds for a given subscription period.
    * This is an internal pure function that takes a subscription period enum as input
    * and returns the corresponding duration in seconds.
    *
    * @param _period The subscription period (enum SubscriptionPeriod) for which the duration is requested.
    *                It can be one of the predefined periods: NinetyDays, OneEightyTwoDays, or ThreeSixtyFiveDays.
    *
    * @return The duration in seconds corresponding to the given subscription period.
    *         - NinetyDays returns 90 days in seconds.
    *         - OneEightyTwoDays returns 182 days in seconds.
    *         - ThreeSixtyFiveDays returns 365 days in seconds.
    *
    * Requirements:
    * - The function only accepts valid `SubscriptionPeriod` enum values.
    * 
    * Throws:
    * - If an invalid subscription period is passed, the function reverts with "Invalid subscription period".
    */
    function getSubscriptionDuration(SubscriptionPeriod _period) internal pure returns (uint256) {
        if (_period == SubscriptionPeriod.ThirtyDays) {
            return 30 days;
        } else if (_period == SubscriptionPeriod.NinetyDays) {
            return 90 days;
        } else if (_period == SubscriptionPeriod.OneEightyTwoDays) {
            return 182 days;
        } else if (_period == SubscriptionPeriod.ThreeSixtyFiveDays) {
            return 365 days;
        } else {
            revert("Invalid subscription period");
        }
    }


    /**
    * @dev Checks if a given user is currently a subscriber.
    *
    * This function determines whether a user has an active subscription by checking the subscription
    * details stored in the `subscribers` mapping. An active subscription is indicated by a non-zero
    * end time that is in the future relative to the current block timestamp.
    *
    * @param _user The address of the user to check for an active subscription.
    * @return bool True if the user is currently a subscriber (i.e., has an active subscription),
    *              false otherwise.
    *
    * The function performs the following checks:
    * 1. Retrieves the subscriber's record from the `subscribers` mapping using the user's address.
    * 2. Checks if the subscription end time (`end`) is non-zero and greater than or equal to the current
    *    block timestamp. A non-zero end time indicates that a subscription was started, and if it's still
    *    in the future, the subscription is considered active.
    *
    * This function is view-only, meaning it does not modify the state of the contract or the blockchain.
    * It can be called by anyone to check the subscription status of any user.
    */
    function isSubscriber(address _user) public view returns (bool) {
        Subscriber storage subscriber = subscribers[_user];
        // Check if the subscription has a valid end time and current time is within the subscription period 
        return (subscriber.end != 0 && block.timestamp <= subscriber.end);
    }


    /**
     * @dev Allows the owner of the contract to withdraw a specified amount of ERC20 tokens.
     * This function ensures that only the owner can withdraw tokens from the contract's balance.
     *
     * @param _token The ERC20 token contract address from which tokens are to be withdrawn.
     * @param _amount The amount of tokens to be withdrawn from the contract.
     *
     * Requirements:
     * - The caller must be the owner of the contract.
     * - The amount to be withdrawn must not exceed the contract's current balance of the specified token.
     *
     * Emits a {Transfer} event from the contract address to the owner's address.
     *
     * Throws:
     * - If the transfer operation fails or the contract's balance of the token is insufficient.
     */
    function withdrawToken(IERC20 _token, uint256 _amount) external onlyOwner {
        uint256 balance = _token.balanceOf(address(this));
        require(_amount <= balance, "Not enough balance");
        require(_token.transfer(owner(), _amount), "Transfer failed");
    }


    /**
     * @dev Returns the contract's balance of a specified ERC20 token.
     * This function allows anyone to query the balance of a given ERC20 token held by the contract.
     *
     * @param _token The ERC20 token contract address for which the balance is being queried.
     * @return The amount of the specified token that the contract currently holds.
     *
     * This function is view-only and does not modify the state of the contract or the blockchain.
     */
    function tokenBalance(IERC20 _token) public view returns (uint256){
        return _token.balanceOf(address(this));
    }

}