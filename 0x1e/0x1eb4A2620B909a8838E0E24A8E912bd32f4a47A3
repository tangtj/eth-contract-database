// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v4.9/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

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

// File: contracts/MSN_SIMPLE_CLAIM.sol


pragma solidity ^0.8.0;


contract MSN_SIMPLE_CLAIM {
    address public msn_contract_address;

    address public claim_signer;
    mapping(uint256 => uint256) private claim_sig_amount_map; // claim signature id => amount

    address public contract_owner;
    modifier onlyContractOwner() {
        require(msg.sender == contract_owner, "Only contractOwner");
        _;
    }

    constructor(address _msn_contract_addr) {
        msn_contract_address = _msn_contract_addr;
        contract_owner = msg.sender;
    }

    function check_claim_amount_from_signature(uint256 sig_id)
        public
        view
        returns (uint256)
    {
        return claim_sig_amount_map[sig_id];
    }

    event Set_claim_signer_log(address addr);

    function set_claim_signer(address _new_signer)
        external
        onlyContractOwner
    {
        require(address(_new_signer) != address(0)); 
        claim_signer = _new_signer;
        emit Set_claim_signer_log(_new_signer);
    }

    /**
     * Based on https://gist.github.com/axic/5b33912c6f61ae6fd96d6c4a47afde6d
     * @dev Recover signer address from a message by using his signature
     * @param hash bytes32 message, the hash is the signed message. What is recovered is the signer address.
     * @param sig bytes signature, the signature is generated using web3.eth.sign()
     */
    function recover(bytes32 hash, bytes memory sig)
        public
        pure
        returns (address)
    {
        bytes32 r;
        bytes32 s;
        uint8 v;

        //Check the signature length
        if (sig.length != 65) {
            return (address(0));
        }

        // Divide the signature in r, s and v variables
        assembly {
            r := mload(add(sig, 32))
            s := mload(add(sig, 64))
            v := byte(0, mload(add(sig, 96)))
        }

        // Version of signature should be 27 or 28, but 0 and 1 are also possible versions
        if (v < 27) {
            v += 27;
        }

        // If the version is correct return the signer address
        if (v != 27 && v != 28) {
            return (address(0));
        } else {
            return ecrecover(hash, v, r, s);
        }
    }

    function check_claim_sig(
        uint256 sig_id,
        uint256 amount,
        bytes memory sig
    ) private view {
        bytes32 hash = keccak256(abi.encodePacked(sig_id, msg.sender, amount));
        address msg_signer = recover(hash, sig);
        require(msg_signer == claim_signer, "signature error");
    }

    function claim(
        uint256 signature_id,
        uint256 amount,
        bytes memory signature
    ) public {
        require(amount > 0, "claim amount should be bigger then 0");

        require(claim_sig_amount_map[signature_id] == 0, "repeated claim");
        claim_sig_amount_map[signature_id] = amount;

        check_claim_sig(signature_id, amount, signature);

        //transfer
        bool result = IERC20(msn_contract_address).transfer(msg.sender, amount);
        require(result == true, "transfer error");
    }

    function forbid_claim(uint256 signature_id) public onlyContractOwner {
        require(claim_sig_amount_map[signature_id] == 0, "already claimed");
        claim_sig_amount_map[signature_id] = 1;
    }

    function transfer_msn_to_owner(uint256 amount) public onlyContractOwner {
        bool result = IERC20(msn_contract_address).transfer(
            contract_owner,
            amount
        );
        require(result == true, "transfer error");
    }
}