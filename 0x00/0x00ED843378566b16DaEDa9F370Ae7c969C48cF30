//SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

// helper methods for interacting with ERC20 tokens and sending ETH that do not consistently return true/false
library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::safeApprove: approve failed'
        );
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::safeTransfer: transfer failed'
        );
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            'TransferHelper::transferFrom: transferFrom failed'
        );
    }

    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, 'TransferHelper::safeTransferETH: ETH transfer failed');
    }
}

/**
 * @dev https://eips.ethereum.org/EIPS/eip-1167[EIP 1167] is a standard for
 * deploying minimal proxy contracts, also known as "clones".
 *
 */
contract Cloneable {

    /**
        @dev Deploys and returns the address of a clone of address(this
        Created by DeFi Mark To Allow Clone Contract To Easily Create Clones Of Itself
        Without redundancy
     */
    function clone() external returns(address) {
        return _clone(address(this));
    }

    /**
     * @dev Deploys and returns the address of a clone that mimics the behaviour of `implementation`.
     *
     * This function uses the create opcode, which should never revert.
     */
    function _clone(address implementation) internal returns (address instance) {
        /// @solidity memory-safe-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, 0x3d602d80600a3d3981f3363d3d373d3d3d363d73000000000000000000000000)
            mstore(add(ptr, 0x14), shl(0x60, implementation))
            mstore(add(ptr, 0x28), 0x5af43d82803e903d91602b57fd5bf30000000000000000000000000000000000)
            instance := create(0, ptr, 0x37)
        }
        require(instance != address(0), "ERC1167: create failed");
    }
}

/**
 * @title Owner
 * @dev Set & change owner
 */
contract Ownable {

    address private owner;
    
    // event for EVM logging
    event OwnerSet(address indexed oldOwner, address indexed newOwner);
    
    // modifier to check if caller is owner
    modifier onlyOwner() {
        // If the first argument of 'require' evaluates to 'false', execution terminates and all
        // changes to the state and to Ether balances are reverted.
        // This used to consume all gas in old EVM versions, but not anymore.
        // It is often a good idea to use 'require' to check if functions are called correctly.
        // As a second argument, you can also provide an explanation about what went wrong.
        require(msg.sender == owner, "Caller is not owner");
        _;
    }
    
    /**
     * @dev Set contract deployer as owner
     */
    constructor() {
        owner = msg.sender; // 'msg.sender' is sender of current call, contract deployer for a constructor
        emit OwnerSet(address(0), owner);
    }

    /**
     * @dev Change owner
     * @param newOwner address of new owner
     */
    function changeOwner(address newOwner) public onlyOwner {
        emit OwnerSet(owner, newOwner);
        owner = newOwner;
    }

    /**
     * @dev Return owner address 
     * @return address of owner
     */
    function getOwner() external view returns (address) {
        return owner;
    }
}

interface IERC20 {

    function totalSupply() external view returns (uint256);
    
    function symbol() external view returns(string memory);
    
    function name() external view returns(string memory);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);
    
    /**
     * @dev Returns the number of decimal places
     */
    function decimals() external view returns (uint8);

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
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

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
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 {
    /**
     * @dev Emitted when `bagId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed bagId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `bagId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed bagId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `bagId` token.
     *
     * Requirements:
     *
     * - `bagId` must exist.
     */
    function ownerOf(uint256 bagId) external view returns (address owner);

    /**
     * @dev Safely transfers `bagId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 bagId
    ) external;

    /**
     * @dev Transfers `bagId` token from `from` to `to`.
     *
     * WARNING: Usage of this method is discouraged, use {safeTransferFrom} whenever possible.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 bagId
    ) external;

    /**
     * @dev Gives permission to `to` to transfer `bagId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `bagId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 bagId) external;

    /**
     * @dev Returns the account approved for `bagId` token.
     *
     * Requirements:
     *
     * - `bagId` must exist.
     */
    function getApproved(uint256 bagId) external view returns (address operator);

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);

    /**
     * @dev Safely transfers `bagId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 bagId,
        bytes calldata data
    ) external;
}

interface IERC721Metadata {
    function tokenURI(uint256 bagId) external view returns (string memory);
}

contract BaggageClaim is Ownable, IERC721, IERC721Metadata {

    uint256 internal constant PRECISION = 10**18;

    // BAG Info
    address public BAG;
    string public name;
    string public symbol;
    uint256 public lockTime;

    // reward info
    address public rewardToken;
    uint256 public lastRewardTime;
    uint256 public rewardsPerSecond;

    // NFT Type
    struct NFTType {
        uint256 dividendsPerBag;
        uint256 numBagsChecked;
        uint256 rewardAllocation;
        mapping ( address => uint256 ) userQTY;
        mapping ( address => uint256 ) userTotalExcluded;
    }

    // Reward Allocation total
    uint256 public rewardAllocationTotal;

    // Maps a NFTType Id to an NFTType
    mapping ( uint256 => NFTType ) public nftType;
    
    // Maps a tokenId to an NFTType
    mapping ( uint256 => uint256 ) public tokenIdToNFTType;

    // Bans a tokenId from being able to stake
    mapping ( uint256 => bool ) public tokenIdBlocked;

    struct UserInfo {
        uint256[] bagIds;
        uint256 balance;
        uint256 totalRewardsClaimed;
    }

    struct CheckedBagId {
        uint256 index;      // index in user token id array
        uint256 timeLocked; // time the id was locked
        address owner;
    }

    mapping ( address => UserInfo ) public userInfo;
    mapping ( uint256 => CheckedBagId ) public bagInfo;

    uint256 internal constant _NOT_ENTERED = 1;
    uint256 internal constant _ENTERED = 2;
    uint256 internal _status;

    modifier nonReentrant() {
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
        _status = _ENTERED;
        _;
        _status = _NOT_ENTERED;
    }

    constructor(
        address BAG_,
        address rewardToken_,
        uint256 lockTime_,
        uint256 rewardsPerDay,
        uint256 rewardAllocation0,
        uint256 rewardAllocation1,
        uint256 rewardAllocation2,
        string memory name_,
        string memory symbol_
    ) {
        // ensure data is correct and this function is only called once
        require(
            BAG_ != address(0),
            'Invalid Bag Address'
        );
        require(
            lockTime_ <= 365 days,
            'Lock Time Too Long'
        );

        // set data
        BAG = BAG_;
        rewardToken = rewardToken_;
        lockTime = lockTime_;
        rewardsPerSecond = rewardsPerDay / 1 days;
        name = name_;
        symbol = symbol_;

        // reset reentrancy
        _status = _NOT_ENTERED;

        // set NFT Reward Data per Type
        nftType[0].rewardAllocation = rewardAllocation0;
        nftType[1].rewardAllocation = rewardAllocation1;
        nftType[2].rewardAllocation = rewardAllocation2;

        // set total allocation
        rewardAllocationTotal = rewardAllocation0 + rewardAllocation1 + rewardAllocation2;
    }

    function setRewardAllocations(
        uint256 rewardAllocation0,
        uint256 rewardAllocation1,
        uint256 rewardAllocation2
    ) external onlyOwner {
        nftType[0].rewardAllocation = rewardAllocation0;
        nftType[1].rewardAllocation = rewardAllocation1;
        nftType[2].rewardAllocation = rewardAllocation2;
        rewardAllocationTotal = rewardAllocation0 + rewardAllocation1 + rewardAllocation2;
    }

    function setTokenIdBlocked(uint256[] calldata tokenIds, bool isBlocked) external onlyOwner {
        uint len = tokenIds.length;
        for (uint i = 0; i < len;) {
            tokenIdBlocked[tokenIds[i]] = isBlocked;
            unchecked { ++i; }
        }
    }

    function setLockTime(uint256 newLockTime) external onlyOwner nonReentrant {
        require(
            newLockTime <= 365 days,
            'Lock Time Too Long'
        );
        lockTime = newLockTime;
    }

    function setNFTType(uint256[] calldata tokenIds, uint256[] calldata types) external onlyOwner {
        uint len = tokenIds.length;
        for (uint i = 0; i < len;) {
            require(
                bagInfo[tokenIds[i]].owner == address(0),
                'Bag Already Checked'
            );
            tokenIdToNFTType[tokenIds[i]] = types[i];
            unchecked { ++i; }
        }
    }

    function setRewardsPerDay(uint256 newRewardsPerSecond) external onlyOwner {
        rewardsPerSecond = newRewardsPerSecond / 1 days;
    }



    function checkBag(uint256 bagId) external nonReentrant {
        _checkBag(bagId);
    }

    function checkMultipleBags(uint256[] calldata bagIds) external nonReentrant {
        uint len = bagIds.length;
        for (uint i = 0; i < len;) {
            _checkBag(bagIds[i]);
            unchecked { ++i; }
        }
    }

    function pickUpBag(uint256 bagId) external nonReentrant {
        _pickUpBag(bagId);
    }

    function pickUpMultipleBags(uint256[] calldata bagIds) external nonReentrant {
        uint256 len = bagIds.length;
        for (uint i = 0; i < len;) {
            _pickUpBag(bagIds[i]);
            unchecked { ++i; }
        }
    }

    function claimRewards() external nonReentrant {
        _updateRewards();
        _claimRewards(msg.sender);
    }

    function updateRewards() external nonReentrant {
        _updateRewards();
    }

    function _initRewards() internal {
        lastRewardTime = block.timestamp;
    }

    function _updateRewards() internal {

        // calculate the time since the last reward was claimed
        uint256 timeSince = timeSinceLastReward();
        if (timeSince == 0) {
            return;
        }

        // calculate the amount of rewards to add
        uint256 toReward = rewardsPerSecond * timeSince;

        // reset the last reward time
        lastRewardTime = block.timestamp;

        // get temp reward allocation total, subtract from it if certain rarities have no assets staked
        uint256 tempRewardAllocationTotal = rewardAllocationTotal;

        if (nftType[2].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[2].rewardAllocation;
        }
        if (nftType[1].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[1].rewardAllocation;
        }
        if (nftType[0].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[0].rewardAllocation;
        }
        if (tempRewardAllocationTotal == 0) {
            // something went wrong, exit
            return;
        }

        // calculate rewards for each rarity
        if (nftType[2].numBagsChecked > 0) {
            uint256 reward = ( nftType[2].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            nftType[2].dividendsPerBag += ( reward * PRECISION ) / nftType[2].numBagsChecked;
        }

        if (nftType[1].numBagsChecked > 0) {
            uint256 reward = ( nftType[1].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            nftType[1].dividendsPerBag += ( reward * PRECISION ) / nftType[1].numBagsChecked;
        }

        if (nftType[0].numBagsChecked > 0) {
            uint256 reward = ( nftType[0].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            nftType[0].dividendsPerBag += ( reward * PRECISION ) / nftType[0].numBagsChecked;
        }
    }

    function _checkBag(uint256 bagId) internal {

        // ensure message sender is owner of BAG
        require(
            isOwner(bagId, msg.sender),
            'Sender Not BAG Owner'
        );
        require(
            bagInfo[bagId].owner == address(0),
            'Already Checked'
        );
        require(
            tokenIdBlocked[bagId] == false,
            'Token Blocked'
        );

        // if first bag check, reset reward timer
        if (totalSupply() == 0) {
            _initRewards();
        } else {
            _updateRewards();
        }

        // claim rewards if applicable
        _claimRewards(msg.sender);
        
        // send BAG to self
        IERC721(BAG).transferFrom(msg.sender, address(this), bagId);

        // ensure BAG is now owned by `this`
        require(
            isOwner(bagId, address(this)),
            'BAG Ownership Not Transferred'
        );

        // fetch type based on bagId
        uint256 _type = tokenIdToNFTType[bagId];

        // increment total bags checked and user balance
        unchecked {
            ++nftType[_type].numBagsChecked;
            ++nftType[_type].userQTY[msg.sender];
            ++userInfo[msg.sender].balance;
        }

        // reset total rewards
        nftType[0].userTotalExcluded[msg.sender] = getCumulativeDividends(0, msg.sender);
        nftType[1].userTotalExcluded[msg.sender] = getCumulativeDividends(1, msg.sender);
        nftType[2].userTotalExcluded[msg.sender] = getCumulativeDividends(2, msg.sender);

        // set current bagId index to length of user id array
        bagInfo[bagId].index = userInfo[msg.sender].bagIds.length;
        bagInfo[bagId].timeLocked = block.timestamp;
        bagInfo[bagId].owner = msg.sender;

        // push new token id to user id array
        userInfo[msg.sender].bagIds.push(bagId);

        emit Transfer(address(0), msg.sender, bagId);
    }

    function _pickUpBag(uint256 bagId) internal {
        require(
            isOwner(bagId, address(this)),
            'BAG Is Not Checked'
        );
        require(
            bagInfo[bagId].owner == msg.sender,
            'Only Owner Can Pick Up Bag'
        );
        require(
            hasCheckedBag(msg.sender, bagId),
            'User Has Not Checked bagId'
        );
        require(
            timeUntilUnlock(bagId) == 0,
            'Token Still Locked'
        );

        // update rewards
        _updateRewards();

        // claim pending rewards if any
        _claimRewards(msg.sender);

        // fetch type based on bagId
        uint256 _type = tokenIdToNFTType[bagId];
        
        // decrement balances
        userInfo[msg.sender].balance       -= 1;
        nftType[_type].numBagsChecked      -= 1;
        nftType[_type].userQTY[msg.sender] -= 1;

        // reset total rewards
        nftType[0].userTotalExcluded[msg.sender] = getCumulativeDividends(0, msg.sender);
        nftType[1].userTotalExcluded[msg.sender] = getCumulativeDividends(1, msg.sender);
        nftType[2].userTotalExcluded[msg.sender] = getCumulativeDividends(2, msg.sender);

        // remove BAG from user array
        _removeBAG(msg.sender, bagId);
        
        // send BAG to caller
        IERC721(BAG).transferFrom(address(this), msg.sender, bagId);

        emit Transfer(msg.sender, address(0), bagId);
    }

    
    /**
        Claims Reward For User
     */
    function _claimRewards(address user) internal {

        // return if zero balance
        if (userInfo[user].balance == 0) {
            return;
        }

        // fetch pending rewards
        uint pending = pendingRewards(user);
        uint max = rewardBalanceOf();
        if (pending > max) {
            pending = max;
        }
        
        // reset total rewards
        nftType[0].userTotalExcluded[user] = getCumulativeDividends(0, user);
        nftType[1].userTotalExcluded[user] = getCumulativeDividends(1, user);
        nftType[2].userTotalExcluded[user] = getCumulativeDividends(2, user);

        // return if no rewards
        if (pending == 0) {
            return;
        }

        // incremenet total rewards claimed
        unchecked {
            userInfo[user].totalRewardsClaimed += pending;
        }

        // transfer reward to user
        if (rewardToken == address(0)) {
            TransferHelper.safeTransferETH(user, pending);
        } else {
            TransferHelper.safeTransfer(rewardToken, user, pending);
        }
    }

    function _removeBAG(address user, uint256 bagId) internal {
        
        uint lastElement = userInfo[user].bagIds[userInfo[user].bagIds.length - 1];
        uint removeIndex = bagInfo[bagId].index;

        userInfo[user].bagIds[removeIndex] = lastElement;
        bagInfo[lastElement].index = removeIndex;
        userInfo[user].bagIds.pop();

        delete bagInfo[bagId];
    }

    /**
        Pending Token Rewards For `account` based on the last time _updateRewards() was called
     */
    function pendingRewards(address account) public view returns (uint256) {
        if(userInfo[account].balance == 0){ return 0; }

        // pending rewards for each reward tier
        uint256 amount0 = 0;
        uint256 amount1 = 0;
        uint256 amount2 = 0;

        if (nftType[0].userQTY[account] > 0) {
            uint256 accountTotalDividends = getCumulativeDividends(0, account);
            uint256 accountTotalExcluded = nftType[0].userTotalExcluded[account];
            amount0 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }
        if (nftType[1].userQTY[account] > 0) {
            uint256 accountTotalDividends = getCumulativeDividends(1, account);
            uint256 accountTotalExcluded = nftType[1].userTotalExcluded[account];
            amount1 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }
        if (nftType[2].userQTY[account] > 0) {
            uint256 accountTotalDividends = getCumulativeDividends(2, account);
            uint256 accountTotalExcluded = nftType[2].userTotalExcluded[account];
            amount2 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }
        return amount0 + amount1 + amount2;
    }

    /**
        Pending rewards including the current state of _updateRewards()
        This result will match the actual payout of claimRewards() assuming the same timestamp, unless:
            - `rewardToken` is a tax on transfer token
            - insufficient `rewardToken`s are supplied by contract owner
     */
    function currentPendingRewards(address account) external view returns (uint256) {
        if(userInfo[account].balance == 0){ return 0; }

        // calculate the time since the last reward was claimed
        uint256 timeSince = timeSinceLastReward();
        if (timeSince == 0) {
            return 0;
        }

        // calculate the amount of rewards to add
        uint256 toReward = rewardsPerSecond * timeSince;

        uint256 tempRewardAllocationTotal = rewardAllocationTotal;
        if (nftType[2].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[2].rewardAllocation;
        }
        if (nftType[1].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[1].rewardAllocation;
        }
        if (nftType[0].numBagsChecked == 0) {
            tempRewardAllocationTotal -= nftType[0].rewardAllocation;
        }
        if (tempRewardAllocationTotal == 0) {
            // something went wrong, exit
            return 0;
        }

        uint256 dividendsPerBAGCurrent0 = 0;
        uint256 dividendsPerBAGCurrent1 = 0;
        uint256 dividendsPerBAGCurrent2 = 0;

        // calculate rewards for each rarity
        if (nftType[2].numBagsChecked > 0) {
            uint256 reward2 = ( nftType[2].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            dividendsPerBAGCurrent2 = nftType[2].dividendsPerBag + ( ( reward2 * PRECISION ) / nftType[2].numBagsChecked );
        }

        if (nftType[1].numBagsChecked > 0) {
            uint256 reward1 = ( nftType[1].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            dividendsPerBAGCurrent1 = nftType[1].dividendsPerBag + ( ( reward1 * PRECISION ) / nftType[1].numBagsChecked );
        }

        if (nftType[0].numBagsChecked > 0) {
            uint256 reward0 = ( nftType[0].rewardAllocation * toReward ) / tempRewardAllocationTotal;
            dividendsPerBAGCurrent0 = nftType[0].dividendsPerBag + ( ( reward0 * PRECISION ) / nftType[0].numBagsChecked );
        }

        // pending rewards for each reward tier
        uint256 amount0 = 0;
        uint256 amount1 = 0;
        uint256 amount2 = 0;

        if (nftType[0].userQTY[account] > 0) {
            uint256 accountTotalDividends = ( nftType[0].userQTY[account] * dividendsPerBAGCurrent0 ) / PRECISION;
            uint256 accountTotalExcluded = nftType[0].userTotalExcluded[account];
            amount0 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }
        if (nftType[1].userQTY[account] > 0) {
            uint256 accountTotalDividends = ( nftType[1].userQTY[account] * dividendsPerBAGCurrent1 ) / PRECISION;
            uint256 accountTotalExcluded = nftType[1].userTotalExcluded[account];
            amount1 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }
        if (nftType[2].userQTY[account] > 0) {
            uint256 accountTotalDividends = ( nftType[2].userQTY[account] * dividendsPerBAGCurrent2 ) / PRECISION;
            uint256 accountTotalExcluded = nftType[2].userTotalExcluded[account];
            amount2 = accountTotalDividends <= accountTotalExcluded ? 0 : accountTotalDividends - accountTotalExcluded;
        }

        return amount0 + amount1 + amount2;
    }

    function timeSinceLastReward() public view returns (uint256) {
        if (lastRewardTime == 0 || totalSupply() == 0) {
            return 0;
        }
        return block.timestamp > lastRewardTime ? block.timestamp - lastRewardTime : 0;
    }

    /**
        Cumulative Dividends For A Number Of Tokens
     */
    function getCumulativeDividends(uint256 _type, address user) internal view returns (uint256) {
        uint256 share = nftType[_type].userQTY[user];
        return ( share * nftType[_type].dividendsPerBag ) / PRECISION;
    }

    function timeUntilUnlock(uint256 bagId) public view returns (uint256) {
        uint unlockTime = bagInfo[bagId].timeLocked + lockTime;
        return unlockTime <= block.timestamp ? 0 : unlockTime - block.timestamp;
    }

    function isOwner(uint256 bagId, address user) public view returns (bool) {
        return IERC721(BAG).ownerOf(bagId) == user;
    }

    function listUserCheckedBAGs(address user) public view returns (uint256[] memory) {
        return userInfo[user].bagIds;
    }

    function fetchBalancePendingAndTotalRewards(address user) public view returns (uint256, uint256, uint256) {
        return (userInfo[user].balance, pendingRewards(user), userInfo[user].totalRewardsClaimed);
    }

    function getNFTTypeInfo() external view returns (
        uint256[3] memory dividendsPerBags, 
        uint256[3] memory numBagsChecked, 
        uint256[3] memory rewardAllocations
    ) {
        for (uint i = 0; i < 3;) {
            dividendsPerBags[i] = nftType[i].dividendsPerBag;
            numBagsChecked[i] = nftType[i].numBagsChecked;
            rewardAllocations[i] = nftType[i].rewardAllocation;
            unchecked { ++i; }
        }
    }

    function getUserQuantities() external view returns (uint256[3] memory userQTY) {
        for (uint i = 0; i < 3;) {
            userQTY[i] = nftType[i].userQTY[msg.sender];
            unchecked { ++i; }
        }
    }
    
    function listUserCheckedBAGsAndURIs(address user) public view returns (uint256[] memory, string[] memory) {
        
        uint len = userInfo[user].bagIds.length;
        string[] memory uris = new string[](len);
        for (uint i = 0; i < len;) {
            uris[i] = IERC721Metadata(BAG).tokenURI(userInfo[user].bagIds[i]);
            unchecked {
                ++i;
            }
        }
        return (userInfo[user].bagIds, uris);
    }

    function batchFetchTokenType(uint256[] calldata tokenIds) external view returns (uint256[] memory) {
        uint len = tokenIds.length;
        uint256[] memory types = new uint256[](len);
        for (uint i = 0; i < len;) {
            types[i] = tokenIdToNFTType[tokenIds[i]];
            unchecked { ++i; }
        }
        return types;
    }

    function listUserCheckedBAGsURIsAndRemainingLockTimes(address user) public view returns (
        uint256[] memory, 
        string[] memory,
        uint256[] memory
    ) {
        
        uint len = userInfo[user].bagIds.length;
        string[] memory uris = new string[](len);
        uint256[] memory remainingLocks = new uint256[](len);
        for (uint i = 0; i < len;) {
            uris[i] = IERC721Metadata(BAG).tokenURI(userInfo[user].bagIds[i]);
            remainingLocks[i] = timeUntilUnlock(userInfo[user].bagIds[i]);
            unchecked {
                ++i;
            }
        }
        return (userInfo[user].bagIds, uris, remainingLocks);
    }

    function listUserTotalBAGs(address user, uint min, uint max) public view returns (uint256[] memory) {
        
        IERC721 BAG_ = IERC721(BAG);
        uint len = BAG_.balanceOf(user);

        uint256[] memory ids = new uint256[](len);
        uint count = 0;

        for (uint i = min; i < max;) {

            if (BAG_.ownerOf(i) == user) {
                ids[count] = i;
                count++;
            }
            
            unchecked {++i;}
        }
        return (ids);
    }

    function listUserTotalBAGsAndUris(address user, uint min, uint max) public view returns (uint256[] memory, string[] memory) {
        
        IERC721 BAG_ = IERC721(BAG);
        uint len = BAG_.balanceOf(user);

        uint256[] memory ids = new uint256[](len);
        string[] memory uris = new string[](len);
        uint count = 0;

        for (uint i = min; i < max;) {

            if (BAG_.ownerOf(i) == user) {
                ids[count] = i;
                uris[count] = IERC721Metadata(BAG).tokenURI(i);
                count++;
            }
            
            unchecked {++i;}
        }
        return (ids, uris);
    }

    function hasCheckedBag(address user, uint256 bagId) public view returns (bool) {
        if (userInfo[user].bagIds.length <= bagInfo[bagId].index || bagInfo[bagId].owner != user) {
            return false;
        }
        return userInfo[user].bagIds[bagInfo[bagId].index] == bagId;
    }

    function hasCheckedBags(address user, uint256[] calldata bagId) public view returns (bool[] memory) {
        uint len = bagId.length;
        bool[] memory hasChecked = new bool[](len);
        for (uint i = 0; i < len;) {
            hasChecked[i] = userInfo[user].bagIds[bagInfo[bagId[i]].index] == bagId[i];
            unchecked {
                ++i;
            }
        }
        return hasChecked;
    }

    function rewardBalanceOf() public view returns (uint256) {
        return rewardToken == address(0) ? address(this).balance : IERC20(rewardToken).balanceOf(address(this));
    }

    function totalSupply() public view returns (uint256 total) {
        for (uint i = 0; i < 3;) {
            total += nftType[i].numBagsChecked;
            unchecked { ++i; }
        }
    }

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view override returns (uint256 balance) {
        return userInfo[owner].balance;
    }

    /**
     * @dev Returns the owner of the `bagId` token.
     *
     * Requirements:
     *
     * - `bagId` must exist.
     */
    function ownerOf(uint256 bagId) external view override returns (address owner) {
        return bagInfo[bagId].owner;
    }

    /**
     * @dev Safely transfers `bagId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address,
        address,
        uint256
    ) external override {

    }

    /**
     * @dev Transfers `bagId` token from `from` to `to`.
     *
     * WARNING: Usage of this method is discouraged, use {safeTransferFrom} whenever possible.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address,
        address,
        uint256
    ) external override {

    }

    /**
     * @dev Gives permission to `to` to transfer `bagId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `bagId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address, uint256) external override {
        emit Approval(address(0), address(0), 0);
        return;
    }

    /**
     * @dev Returns the account approved for `bagId` token.
     *
     * Requirements:
     *
     * - `bagId` must exist.
     */
    function getApproved(uint256 a) external view override returns (address operator) {
        return a == uint(uint160(msg.sender)) ? address(0) : msg.sender;
    }

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address, bool) external override {
        emit Approval(address(0), address(0), 0);
        return;
    }

    function isApprovedForAll(address, address) external pure override returns (bool) {
        return false;
    }

    /**
     * @dev Safely transfers `bagId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `bagId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address,
        address,
        uint256,
        bytes calldata
    ) external override {}

    function tokenURI(uint256 bagId) external view override returns (string memory) {
        return IERC721Metadata(BAG).tokenURI(bagId);
    }

}