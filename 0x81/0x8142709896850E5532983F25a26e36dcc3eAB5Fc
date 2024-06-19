// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BatchTransfer {
    function distribute(address[] calldata recipients, uint256[] calldata amounts) external payable {
        require(recipients.length == amounts.length, "Recipients and amounts arrays must be the same length");

        for (uint256 i = 0; i < recipients.length; i++) {
            require(address(this).balance >= amounts[i], "Insufficient contract balance");
            payable(recipients[i]).transfer(amounts[i]);
        }
    }

    // Fallback function to receive ETH
    receive() external payable {}
}