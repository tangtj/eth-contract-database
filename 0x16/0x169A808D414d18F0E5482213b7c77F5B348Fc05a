// SPDX-License-Identifier: AGPL-3.0
pragma solidity 0.8.21;


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)
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

// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)
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

contract RewardConfig is Ownable {
    struct RewardInfo {
        address rewardToken;
        uint256 epochStart;
        uint256 epochEnd;
        uint256 amount;
        uint256 decimal;
    }

    struct AggregatorData {
        address aggregator;
        RewardInfo[] rewardInfo;
    }

    address[] private _aggregators;
    // aggregator -> reward token array
    mapping(address => address[]) private _aggregatorToRewardTokens;
    // aggregator -> reward token -> reawrd info
    mapping(address => mapping(address => RewardInfo)) private _aggregatorToRewardInfos;

    event SetRewardInfo(
        address aggregator,
        address rewardToken,
        uint256 epochStart,
        uint256 epochEnd,
        uint256 amount,
        uint256 decimals
    );
    event RemoveAggregator(address aggregator);
    event RemoveRewardToken(address aggregator, address rewardToken);

    /**
     * @dev Set reward info to aggregator
     * - Caller is Admin
     * @param _aggregator The aggregator address.
     * @param _rewardToken The reward token address.
     * @param _epochStart The reward epoch start block number.
     * @param _epochEnd The reward epoch end block number.
     * @param _amount The reward token count amount.
     */
    function setRewardInfo(
        address _aggregator,
        address _rewardToken,
        uint256 _epochStart,
        uint256 _epochEnd,
        uint256 _amount
    ) external payable onlyOwner {
        uint256 decimals = IERC20Metadata(_rewardToken).decimals();
        uint256 count = _aggregators.length;
        bool isExist;

        // check and register aggregator
        for (uint256 i; i < count; ++i) {
            if (_aggregators[i] == _aggregator) {
                isExist = true;
                break;
            }
        }

        if (!isExist) {
            _aggregators.push(_aggregator);
        }

        // check and register reward token
        address[] memory aggregatorRewardTokens = _aggregatorToRewardTokens[_aggregator];
        count = aggregatorRewardTokens.length;
        isExist = false;

        for (uint256 i; i < count; ++i) {
            if (aggregatorRewardTokens[i] == _rewardToken) {
                isExist = true;
                break;
            }
        }

        if (!isExist) {
            _aggregatorToRewardTokens[_aggregator].push(_rewardToken);
        }

        // set reard info
        _aggregatorToRewardInfos[_aggregator][_rewardToken] = RewardInfo(
            _rewardToken,
            _epochStart,
            _epochEnd,
            _amount,
            decimals
        );

        emit SetRewardInfo(_aggregator, _rewardToken, _epochStart, _epochEnd, _amount, decimals);
    }

    /**
     * @dev Remove the aggregator reward info
     * - Caller is Admin
     * @param _aggregator The aggregator address.
     */
    function removeAggregator(address _aggregator) external payable onlyOwner {
        uint256 count = _aggregators.length;

        for (uint256 i; i < count; ++i) {
            if (_aggregators[i] == _aggregator) {
                if (i < count - 1) {
                    _aggregators[i] = _aggregators[count - 1];
                }
                _aggregators.pop();
                break;
            }
        }

        delete _aggregatorToRewardTokens[_aggregator];

        emit RemoveAggregator(_aggregator);
    }

    /**
     * @dev Remove the reward token from aggregator
     * - Caller is Admin
     * @param _aggregator The aggregator address.
     * @param _rewardToken The aggregator address.
     */
    function removeRewardToken(address _aggregator, address _rewardToken) external payable onlyOwner {
        uint256 count = _aggregatorToRewardTokens[_aggregator].length;

        for (uint256 i; i < count; ++i) {
            if (_aggregatorToRewardTokens[_aggregator][i] == _rewardToken) {
                if (i < count - 1) {
                    _aggregatorToRewardTokens[_aggregator][i] = _aggregatorToRewardTokens[_aggregator][count - 1];
                }
                _aggregatorToRewardTokens[_aggregator].pop();
                break;
            }
        }

        delete _aggregatorToRewardInfos[_aggregator][_rewardToken];

        emit RemoveRewardToken(_aggregator, _rewardToken);
    }

    /**
     * @dev Get all reward config
     */
    function getAllRewardInfo() external view returns (AggregatorData[] memory) {
        uint256 aggregatorCount = _aggregators.length;
        AggregatorData[] memory result = new AggregatorData[](aggregatorCount);

        for (uint256 i; i < aggregatorCount; ++i) {
            address aggregator = _aggregators[i];
            uint256 rewardTokenCount = _aggregatorToRewardTokens[aggregator].length;
            RewardInfo[] memory rewardInfo = new RewardInfo[](rewardTokenCount);
            
            for (uint256 j; j < rewardTokenCount; ++j) {
                address rewardToken = _aggregatorToRewardTokens[aggregator][j];

                rewardInfo[j] = _aggregatorToRewardInfos[aggregator][rewardToken];
            }

            result[i] = AggregatorData(
                aggregator,
                rewardInfo
            );
        }

        return result;
    }
}