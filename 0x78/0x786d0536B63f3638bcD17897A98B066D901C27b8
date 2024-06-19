//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// OpenZeppelin Contracts (last updated v5.0.0) (utils/Context.sol)
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

// OpenZeppelin Contracts (last updated v5.0.0) (access/Ownable.sol)
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

// OpenZeppelin Contracts (last updated v5.0.0) (access/Ownable2Step.sol)
/**
 * @dev Contract module which provides access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * The initial owner is specified at deployment time in the constructor for `Ownable`. This
 * can later be changed with {transferOwnership} and {acceptOwnership}.
 *
 * This module is used through inheritance. It will make available all functions
 * from parent (Ownable).
 */
abstract contract Ownable2Step is Ownable {
    address private _pendingOwner;

    event OwnershipTransferStarted(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Returns the address of the pending owner.
     */
    function pendingOwner() public view virtual returns (address) {
        return _pendingOwner;
    }

    /**
     * @dev Starts the ownership transfer of the contract to a new account. Replaces the pending transfer if there is one.
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual override onlyOwner {
        _pendingOwner = newOwner;
        emit OwnershipTransferStarted(owner(), newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`) and deletes any pending owner.
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual override {
        delete _pendingOwner;
        super._transferOwnership(newOwner);
    }

    /**
     * @dev The new owner accepts the ownership transfer.
     */
    function acceptOwnership() public virtual {
        address sender = _msgSender();
        if (pendingOwner() != sender) {
            revert OwnableUnauthorizedAccount(sender);
        }
        _transferOwnership(sender);
    }
}

// OpenZeppelin Contracts (last updated v5.0.0) (token/ERC20/IERC20.sol)
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

// OpenZeppelin Contracts (last updated v5.0.0) (utils/ReentrancyGuard.sol)
/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant NOT_ENTERED = 1;
    uint256 private constant ENTERED = 2;

    uint256 private _status;

    /**
     * @dev Unauthorized reentrant call.
     */
    error ReentrancyGuardReentrantCall();

    constructor() {
        _status = NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be NOT_ENTERED
        if (_status == ENTERED) {
            revert ReentrancyGuardReentrantCall();
        }

        // Any calls to nonReentrant after this point will fail
        _status = ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == ENTERED;
    }
}

// 2 Setp Ownership
/**
 * @title FarmOwner
 * @dev Ownable contract with deployer set as initial owner.
 */
abstract contract FarmOwner is Ownable2Step {
    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() Ownable(_msgSender()) {}
}

// LinqAI LNQFarm
/**
 * @title LNQFarm
 * @dev A contract for staking LNQ tokens in multiple pools with different reward rates and lock times.
//  */
contract LNQFarm is Context, FarmOwner, ReentrancyGuard {
    uint256 private constant REWARDS_PRECISION = 1e18; // A big number to perform mul and div operations
    IERC20 public LNQToken;
    // Staking user for a pool
    struct PoolStaker {
        uint256 amount; // The tokens quantity the user has staked.
        uint256 rewards; // The reward tokens quantity the user can harvest
        uint256 rewardDebt; // The amount relative to accumulatedRewardsPerShare the user can't get as reward
        uint256 earned;
        uint256 startTime;
        uint256 endTime;
    }

    // Staking pool
    struct Pool {
        uint256 tokensStaked; // Total tokens staked
        uint256 lastRewardedBlock; // Last block number the user had their rewards calculated
        uint256 accumulatedRewardsPerShare; // Accumulated rewards per share times REWARDS_PRECISION
    }

    Pool[] public pools; // Staking pools
    uint256[3] public rewardTokensPerBlock = [12, 6, 2]; // Reward tokens per block for each pool
    uint256[3] public lockTime = [180, 90, 60]; // Lock time for each pool

    mapping(uint256 => mapping(address => PoolStaker)) public poolStakers;

    // Events
    event Deposit(address indexed user, uint256 indexed poolId, uint256 amount);
    event Withdraw(address indexed user, uint256 indexed poolId, uint256 amount);
    event HarvestRewards(address indexed user, uint256 indexed poolId, uint256 amount);
    event PoolCreated(uint256 poolId);
    
    /**
     * @dev Constructor function.
     * @param _LNQ The address of the LNQ token contract.
     */
    constructor(address _LNQ) {
        LNQToken = IERC20(_LNQ);
        for(uint256 i = 0; i < 3; i++) {
            createPool();
        }
    }

    /**
     * @dev Create a new staking pool
     */
    function createPool() internal {
        Pool memory pool;
        pools.push(pool);
        uint256 poolId = pools.length - 1;
        emit PoolCreated(poolId);
    }

    /**
     * @dev Deposit tokens to an existing pool.
     * @param _poolId The ID of the pool to deposit into.
     * @param _amount The amount of tokens to deposit.
     */
    function deposit(uint256 _poolId, uint256 _amount) external nonReentrant {
        require(_amount >= 1000 * 1e18, "Minimum deposit amount is 1000 LNQ");
        require(_poolId >= 0 && _poolId < pools.length, "Pool does not exist");
        Pool storage pool = pools[_poolId];
        PoolStaker storage staker = poolStakers[_poolId][msg.sender];
        // Update pool stakers
        updatePoolRewards(_poolId);
        // Update current staker
        if(staker.amount > 0) {
            staker.rewards = staker.rewards + staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION - staker.rewardDebt;
        } else {
            staker.startTime = block.timestamp;
            staker.endTime = block.timestamp + lockTime[_poolId] * 1 days;
        }
        staker.amount = staker.amount + _amount;
        staker.rewardDebt = staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION;

        // Update pool
        pool.tokensStaked = pool.tokensStaked + _amount;
        // Deposit tokens
        emit Deposit(msg.sender, _poolId, _amount);
        LNQToken.transferFrom(
            address(msg.sender),
            address(this),
            _amount
        );
    }

    /**
     * @dev Compound tokens to an existing pool.
     * @param _poolId The ID of the pool to compound rewards in.
     */
    function compound(uint256 _poolId) external nonReentrant {
        Pool storage pool = pools[_poolId];
        PoolStaker storage staker = poolStakers[_poolId][msg.sender];
        uint256 rewards = getClaimableRewards(_poolId, msg.sender);
        require(rewards > 0, "Compound amount can't be zero");
        updatePoolRewards(_poolId);

        staker.rewards = 0;
        staker.amount = staker.amount + rewards;
        staker.rewardDebt = staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION;

        // Update pool
        pool.tokensStaked = pool.tokensStaked + rewards;
    }

     /**
     * @dev Withdraw all tokens from an existing pool.
     * @param _poolId The ID of the pool to withdraw from.
     */
    function withdraw(uint256 _poolId) external nonReentrant {
        Pool storage pool = pools[_poolId];
        PoolStaker storage staker = poolStakers[_poolId][msg.sender];
        uint256 amount = staker.amount;
        require(amount > 0, "Withdraw amount can't be zero");
        require(block.timestamp >= staker.endTime, "Lock time is not passed yet!");

        // Pay rewards
        _harvestRewards(_poolId);

        // Update staker
        staker.amount = 0;
        staker.rewardDebt = 0;
        staker.startTime = 0;
        staker.endTime = 0;
        
        // Update pool
        pool.tokensStaked = pool.tokensStaked - amount;

        // Withdraw tokens
        emit Withdraw(msg.sender, _poolId, amount);
        LNQToken.transfer(
            address(msg.sender),
            amount
        );
    }

    /**
     * @dev Harvest user rewards from a given pool id.
     * @param _poolId The ID of the pool to harvest rewards from.
     */
    
    function harvestRewards(uint256 _poolId) external nonReentrant {
        _harvestRewards(_poolId);
    }

    function _harvestRewards(uint256 _poolId) private {
        updatePoolRewards(_poolId);
        Pool storage pool = pools[_poolId];
        PoolStaker storage staker = poolStakers[_poolId][msg.sender];
        require(block.timestamp >= staker.endTime, "Lock time is not passed yet!");
        uint256 rewardsToHarvest = staker.rewards + (staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION) - staker.rewardDebt;
        if (rewardsToHarvest == 0) {
            staker.rewardDebt = staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION;
            return;
        }
        staker.rewards = 0;
        staker.rewardDebt = staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION;
        staker.earned += rewardsToHarvest;
        emit HarvestRewards(msg.sender, _poolId, rewardsToHarvest);
        LNQToken.transfer(msg.sender, rewardsToHarvest);
    }

    /**
     * @dev Update pool's accumulatedRewardsPerShare and lastRewardedBlock.
     * @param _poolId The ID of the pool to update rewards for.
     */
    function updatePoolRewards(uint256 _poolId) private {
        Pool storage pool = pools[_poolId];
        if (pool.tokensStaked == 0) {
            pool.lastRewardedBlock = block.number;
            return;
        }
        uint256 blocksSinceLastReward = block.number - pool.lastRewardedBlock;
        uint256 rewards = blocksSinceLastReward * rewardTokensPerBlock[_poolId] * 1e18;
        pool.accumulatedRewardsPerShare = pool.accumulatedRewardsPerShare + (rewards * REWARDS_PRECISION / pool.tokensStaked);
        pool.lastRewardedBlock = block.number; 
    }

    /**
     * @dev Get the claimable rewards for a user in a pool.
     * @param _poolId The ID of the pool.
     * @param _user The address of the user.
     * @return The amount of claimable rewards.
     */
    function getClaimableRewards(uint256 _poolId, address _user) public view returns(uint256) {
        Pool storage pool = pools[_poolId];
        PoolStaker storage staker = poolStakers[_poolId][_user];
        uint256 blocksSinceLastReward = block.number - pool.lastRewardedBlock;
        if(pool.tokensStaked == 0) {
            return 0;
        }
        uint256 rewardsToHarvest = staker.rewards + (staker.amount * pool.accumulatedRewardsPerShare / REWARDS_PRECISION) - 
                                    staker.rewardDebt + staker.amount * rewardTokensPerBlock[_poolId] * blocksSinceLastReward * 1e18 / pool.tokensStaked;
        return rewardsToHarvest;       
    }

    /**
     * @dev Get the liquidity in each farm pool.
     * @return An array of liquidity amounts for each pool.
     */
    function getFarmsLiquidity() external view returns(uint256[3] memory) {
        uint256[3] memory farmsLiquidity;
        for(uint8 i = 0; i < 3; i++) {
            Pool storage pool = pools[i];
            farmsLiquidity[i] = pool.tokensStaked;
        }
        return farmsLiquidity;
    }

    /**
     * @dev Get the APR for a user in each pool.
     * @param _user The address of the user.
     * @return An array of APR values for each pool.
     */
    function getAPRForUser(address _user) external view returns(uint256[3] memory) {
        uint256[3] memory APR;
        for(uint8 i = 0; i < 3; i++) {
            Pool storage pool = pools[i];
            PoolStaker storage staker = poolStakers[i][_user];
            if (pool.tokensStaked > 0) {
                APR[i] = staker.amount * 100000 / pool.tokensStaked ;
            } else {
                APR[i] = 0;
            }
        }
        return APR;
    }

    /**
     * @dev Get the staked token amounts for a user in each pool.
     * @param _user The address of the user.
     * @return An array of staked token amounts for each pool.
     */
    function getStakedToken(address _user) external view returns(uint256[3] memory) {
        uint256[3] memory stakedToken;
        for(uint8 i = 0; i < 3; i++) {
            PoolStaker storage staker = poolStakers[i][_user];
            stakedToken[i] = staker.amount;
        }
        return stakedToken;
    }

    /**
     * @dev Get the earned token amounts for a user in each pool.
     * @param _user The address of the user.
     * @return An array of earned token amounts for each pool.
     */
    function getEarnedToken(address _user) external view returns(uint256[3] memory) {
        uint256[3] memory earnedToken;
        for(uint8 i = 0; i < 3; i++) {
            PoolStaker storage staker = poolStakers[i][_user];
            earnedToken[i] = staker.earned;
        }
        return earnedToken;
    }

    /**
     * @dev Get the claimable rewards for a user in each pool.
     * @param _user The address of the user.
     * @return An array of claimable reward amounts for each pool.
     */
    function getClaimableRewards(address _user) external view returns(uint256[3] memory) {
        uint256[3] memory claimableRewards;
        for(uint8 i = 0; i < 3; i++) {
            claimableRewards[i] = getClaimableRewards(i, _user);
        }
        return claimableRewards;
    }

    /**
     * @dev Check if the lock time has passed for a user in each pool.
     * @param _user The address of the user.
     * @return An array of boolean values indicating if the lock time has passed for each pool.
     */
    function checkLockTime(address _user) external view returns(bool[3] memory) {
        bool[3] memory checkLockState;
        for(uint8 i = 0; i < 3; i++) {
            PoolStaker storage staker = poolStakers[i][_user];
            if (staker.endTime == 0) {
                checkLockState[i] = false;
            } else {
                checkLockState[i] = staker.endTime < block.timestamp;
            }            
        }
        return checkLockState;
    }
}