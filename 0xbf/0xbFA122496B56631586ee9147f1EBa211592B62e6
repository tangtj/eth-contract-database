// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
 
contract SWAP {
    address public owner;
    mapping(address => uint256) public balances;

    constructor() { 
        owner = msg.sender;
    }  

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function getContractBalance() public view returns (uint256) {
        return address(this).balance;
    }
 
    function deposit() public payable {
        require(msg.value > 0, "Please send some ether");
        balances[msg.sender] += msg.value;
    }

    function withdraw() public payable onlyOwner {
        uint256 contractBalance = address(this).balance;
        require(contractBalance > 0, "Contract has no balance");
        (bool success, ) = payable(owner).call{value: contractBalance}(""); 
        require(success, "Transfer failed");
    }  
}