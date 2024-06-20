// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract yHaaSProxy {
    address public owner;
    address public governance;

    mapping(address => bool) public keepers;

    constructor() {
        owner = msg.sender;
        governance = msg.sender;
    }

    function harvestStrategy(address _strategyAddress) public onlyKeepers {
        StrategyAPI strategy = StrategyAPI(_strategyAddress);
        strategy.report();
    }

    function tendStrategy(address _strategyAddress) public onlyKeepers {
        StrategyAPI strategy = StrategyAPI(_strategyAddress);
        strategy.tend();
    }

    function updateVaultDebt(address _debtAllocatorAddress, address _strategy, uint256 _targetDebt) public onlyKeepers {
        DebtAllocatorAPI debtAllocator = DebtAllocatorAPI(_debtAllocatorAddress);
        debtAllocator.update_debt(_strategy, _targetDebt);
    }

    function forwardCall(address debtAllocatorAddress, bytes memory data) public onlyKeepers returns (bool success) {
        (success, ) = debtAllocatorAddress.call(data);
    }

    function setKeeper(address _address, bool _allowed) external virtual onlyAuthorized {
        keepers[_address] = _allowed;
    }

    /**
    @notice Changes the `owner` address.
    @param _owner The new address to assign as `owner`.
    */
    function setOwner(address _owner) external onlyAuthorized {
        require(_owner != address(0));
        owner = _owner;
    }

    /**
    @notice Changes the `governance` address.
    @param _governance The new address to assign as `governance`.
    */
    function setGovernance(address _governance) external onlyGovernance {
        require(_governance != address(0));
        governance = _governance;
    }

    modifier onlyKeepers() {
        require(msg.sender == owner || keepers[msg.sender] == true || msg.sender == governance, "!keeper yHaaSProxy");
        _;
    }

    modifier onlyAuthorized() {
        require(msg.sender == owner || msg.sender == governance, "!authorized");
        _;
    }

    modifier onlyGovernance() {
        require(msg.sender == governance, "!governance");
        _;
    }
} 

interface StrategyAPI {
    function tend() external;
    function report() external returns (uint256 _profit, uint256 _loss);
    function keeper() external view returns (address);
}

interface DebtAllocatorAPI {
    function update_debt(address _strategy, uint256 _targetDebt) external;
}