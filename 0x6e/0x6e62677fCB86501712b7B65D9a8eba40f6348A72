// SPDX-License-Identifier: MIT
pragma solidity 0.8.24;

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

interface ITellor {
    function balanceOf(
        address _user
    ) external view returns (uint256);

    function transfer(address _to, uint256 _amount) external;

    function mintToOracle() external;

    function approve(address _to, uint256 _amount) external;
}

interface ITellorFlex {
    function submitValue(
        bytes32 _queryId,
        bytes calldata _value,
        uint256 _nonce,
        bytes calldata _queryData
    ) external;

    function requestStakingWithdraw(uint256 _amount) external;

    function withdrawStake() external;

    function depositStake(uint256 _amount) external;

    function addStakingRewards(uint256 _amount) external;

    function getPendingRewardByStaker(
        address _stakerAddress
    ) external returns (uint256);

    function updateStakeAmount() external;

    // View functions

    function getTimeOfLastNewValue() external view returns (uint256);
    function getCurrentValue(
        bytes32 _queryId
    ) external view returns (bytes memory _value);

    function getDataBefore(
        bytes32 _queryId,
        uint256 _timestamp
    )
    external
    view
    returns (
        bool _ifRetrieve,
        bytes memory _value,
        uint256 _timestampRetrieved
    );
}

contract TellorManagementContract {

    ITellorFlex public tellorFlex;
    ITellor public tellorToken;
    address payable public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "This is not owner.");
        _;
    }

    constructor(address _tellor, address _tellorFlex) {
        require(_tellor != address(0), 'TellorMaster address can not be zero.');

        tellorToken = ITellor(_tellor);
        tellorFlex = ITellorFlex(_tellorFlex);
        owner = payable(msg.sender);
    }

    function updateOwner(address _owner) external onlyOwner {
        owner = payable(_owner);
    }

    function submitValue(
        bytes32 queryId,
        bytes calldata price,
        uint256 nonce,
        bytes calldata queryData
    ) public onlyOwner {
        uint256 timeOfLastNewValue = tellorFlex.getTimeOfLastNewValue();
        uint256 offset = block.timestamp - timeOfLastNewValue;
        require(offset > 60, "too close");
        tellorFlex.submitValue(queryId, price, nonce, queryData);
    }

    function setTellorToken(address _newTokenAddress) public onlyOwner {
        tellorToken = ITellor(_newTokenAddress);
    }

    function withdrawTRB() public onlyOwner {
        uint256 balance = tellorToken.balanceOf(address(this));
        tellorToken.transfer(owner, balance);
    }

    function requestStakingWithdraw(uint256 _amount) public onlyOwner {
        tellorFlex.requestStakingWithdraw(_amount);
    }

    function mintToOracle() public onlyOwner {
        tellorToken.mintToOracle();
    }

    function withdrawStake() public onlyOwner {
        tellorFlex.withdrawStake();
    }

    function depositStake(uint256 amount) public onlyOwner {
        tellorToken.approve(address(tellorFlex), amount);
        tellorFlex.depositStake(amount);
    }

    function setTellorFlex(address _newAddress) public onlyOwner {
        tellorFlex = ITellorFlex(_newAddress);
    }

    function getTellorOracle() public view returns (address) {
        return address(tellorFlex);
    }

    function getTellorToken() public view returns (address) {
        return address(tellorToken);
    }

    function mintAndSubmit(
        bytes32 queryId,
        bytes calldata price,
        uint256 nonce,
        bytes calldata queryData
    ) public onlyOwner {
        uint256 timeOfLastNewValue = tellorFlex.getTimeOfLastNewValue();
        uint256 offset = block.timestamp - timeOfLastNewValue;
        require(offset > 60, "too close");
        tellorToken.mintToOracle();
        tellorFlex.submitValue(queryId, price, nonce, queryData);
    }

    function approveTRB(address _to, uint256 _amount) public onlyOwner {
        tellorToken.approve(_to, _amount);
    }

    // Emergency function
    function withdrawERC20(
        address _to,
        address _token,
        uint256 _amount
    ) public onlyOwner {
        IERC20(_token).transfer(_to, _amount);
    }

    // Unnecessary but for the future

    //Write functions
    function addStakingRewards(uint256 _amount) public onlyOwner {
        tellorFlex.addStakingRewards(_amount);
    }

    function getPendingRewardByStaker(
        address _stakerAddress
    ) public onlyOwner returns (uint256 _pendingReward) {
        _pendingReward = tellorFlex.getPendingRewardByStaker(_stakerAddress);
    }

    function updateStakeAmount() public onlyOwner {
        tellorFlex.updateStakeAmount();
    }

    //View functions
    function getCurrentValue(
        bytes32 _queryId
    ) public view onlyOwner returns (bytes memory _value) {
        _value = tellorFlex.getCurrentValue(_queryId);
    }

}