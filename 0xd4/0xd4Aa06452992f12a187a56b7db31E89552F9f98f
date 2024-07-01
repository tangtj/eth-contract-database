// SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;

interface IPresale {
    function buyTokenFromBSC(
        address buyer,
        uint256 amount,
        uint256 amountBSC,
        string memory buyFrom,
        address refOf,
        string memory hash
    ) external;
}

struct BuyInfo {
    address buyer;
    uint256 amount;
    uint256 amountBSC;
    string buyFrom;
    address refOf;
    string hash;
}

contract BatchBuyWPEPE {
    IPresale ipresale = IPresale(address(0x5049443427037c3af8b4eAd6E4FbF250D372d9Fd));
    address public _brigdeBuyTokenFromBSC = 0xFa4A9861a2A4d30568bC05437E50ED27324CB915;  
    function batchSend(
        BuyInfo[] memory items
    ) public {
        require(msg.sender == _brigdeBuyTokenFromBSC, "Batch Buy Only call by bridge");
        for (uint256 i = 0; i < items.length; i++) {
            ipresale.buyTokenFromBSC(items[i].buyer, items[i].amount, items[i].amountBSC, items[i].buyFrom, items[i].refOf, items[i].hash);
            // token.transferFrom(msg.sender, addrress[i], amounts[i]);
        }
    }
}