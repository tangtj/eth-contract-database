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

// File: contracts/MSN_MINING_CLAIM.sol


pragma solidity ^0.8.0;


contract MSN_MINING_CLAIM {
    address public msn_contract_address;
    uint256 public mining_claim_base;

    //mining
    address public miner_signer;
    uint256 public mining_start_timestamp;
    uint256 public miner_total_claimed; //total tokens claimed by all miners in the past
    mapping(uint256 => uint256) private mining_sig_amount_map; // mining claim signature id => amount

    //sum(claim rate)*1000 for mining in each year
    uint256[10] public mining_claim_years_limit = [
        50,
        95,
        135,
        170,
        200,
        225,
        245,
        260,
        270,
        275
    ];

    address public contract_owner;
    modifier onlyContractOwner() {
        require(msg.sender == contract_owner, "Only contractOwner");
        _;
    }

    constructor(
        address _msn_contract_addr,
        uint256 _mining_claim_base,
        uint256 _mining_start_time
    ) {
        msn_contract_address = _msn_contract_addr;
        mining_claim_base = _mining_claim_base;
        contract_owner = msg.sender;
        mining_start_timestamp = _mining_start_time;
    }

    function check_amount_from_signature(uint256 sig_id)
        public
        view
        returns (uint256)
    {
        return mining_sig_amount_map[sig_id];
    }

    function transfer_msn_to_owner(uint256 amount) public onlyContractOwner {
        bool result = IERC20(msn_contract_address).transfer(
            contract_owner,
            amount
        );
        require(result == true, "transfer error");
    }

    // sum of all the tokens can be claimed currently
    function mining_claim_limit() public view returns (uint256) {
        uint256 past_years_num = past_years();
        uint256 past_days_num = past_days();

        /////////past years total limit //////////////
        uint256 past_years_claim_limit = 0;
        if (past_years_num == 0) {
            //keeps past_years_claim_limit 0
        } else if (past_years_num < 10)
            past_years_claim_limit =
                (mining_claim_years_limit[past_years_num - 1] *
                    mining_claim_base) /
                1000;
        else {
            past_years_claim_limit =
                (mining_claim_years_limit[9] * mining_claim_base) /
                1000;
        }

        /////////this year total limit /////////////////////
        uint256 claim_ratio_this_year = 0;
        if (past_years_num == 0) {
            claim_ratio_this_year = mining_claim_years_limit[past_years_num];
        } else if (past_years_num < 10) {
            claim_ratio_this_year =
                mining_claim_years_limit[past_years_num] -
                mining_claim_years_limit[past_years_num - 1];
        } else {
            //keeps claim_ratio_this_year 0
        }

        uint256 this_year_claim_limit = ((past_days_num +
            1 -
            365 *
            past_years_num) *
            mining_claim_base *
            claim_ratio_this_year) /
            365 /
            1000;

        ///////////////////////////////////////////////////
        return past_years_claim_limit + this_year_claim_limit;
    }

    event Set_miner_signer_log(address addr);

    function set_miner_signer(address _new_signer) external onlyContractOwner {
        require(address(_new_signer) != address(0)); 
        miner_signer = _new_signer;
        emit Set_miner_signer_log(_new_signer);
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
        require(msg_signer == miner_signer, "signature error");
    }

    function miner_claim(
        uint256 signature_id,
        uint256 amount,
        bytes memory signature
    ) public {
        require(amount > 0, "claim amount should be bigger then 0");

        require(mining_sig_amount_map[signature_id] == 0, "repeated claim");
        mining_sig_amount_map[signature_id] = amount;

        uint256 after_add = miner_total_claimed + amount;
        require(after_add <= mining_claim_limit(), "mining over limit");
        miner_total_claimed = after_add;

        check_claim_sig(signature_id, amount, signature);

        //transfer
        bool result = IERC20(msn_contract_address).transfer(msg.sender, amount);
        require(result == true, "transfer error");
    }

    function forbid_claim(uint256 signature_id) public onlyContractOwner {
        require(mining_sig_amount_map[signature_id] == 0, "already claimed");
        mining_sig_amount_map[signature_id] = 1;
    }


    //how many years passed since initial deployed
    function past_years() public view returns (uint256) {
        return (block.timestamp - mining_start_timestamp) / 365 days;
    }

    //how many days passed since initial deployed
    function past_days() public view returns (uint256) {
        return (block.timestamp - mining_start_timestamp) / 1 days;
    }
}