// SPDX-License-Identifier: MIT

/*
MIT License

Copyright (c) 2024 Cat Church LLC (see CCC.meme)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
*/

/*
This software is based in part on OpenZeppelin Contracts (https://github.com/OpenZeppelin/openzeppelin-contracts),
which is licensed under the MIT License (https://opensource.org/licenses/MIT).

Copyright (c) 2024 Cat Church LLC
*/

/*
OpenZeppelin license:
The MIT License (MIT)

Copyright (c) 2016-2023 zOS Global Limited and contributors

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
*/

// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.2/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

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

// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.2/contracts/token/ERC20/IERC20.sol


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

// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.2/contracts/token/ERC20/extensions/IERC20Metadata.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)

pragma solidity ^0.8.0;


/**
 * @dev Interface for the optional metadata functions from the ERC20 standard.
 *
 * _Available since v4.1._
 */
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

// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.2/contracts/token/ERC20/ERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/ERC20.sol)

pragma solidity ^0.8.0;




/**
 * @dev Implementation of the {IERC20} interface.
 *
 * This implementation is agnostic to the way tokens are created. This means
 * that a supply mechanism has to be added in a derived contract using {_mint}.
 * For a generic mechanism see {ERC20PresetMinterPauser}.
 *
 * TIP: For a detailed writeup see our guide
 * https://forum.openzeppelin.com/t/how-to-implement-erc20-supply-mechanisms/226[How
 * to implement supply mechanisms].
 *
 * The default value of {decimals} is 18. To change this, you should override
 * this function so it returns a different value.
 *
 * We have followed general OpenZeppelin Contracts guidelines: functions revert
 * instead returning `false` on failure. This behavior is nonetheless
 * conventional and does not conflict with the expectations of ERC20
 * applications.
 *
 * Additionally, an {Approval} event is emitted on calls to {transferFrom}.
 * This allows applications to reconstruct the allowance for all accounts just
 * by listening to said events. Other implementations of the EIP may not emit
 * these events, as it isn't required by the specification.
 *
 * Finally, the non-standard {decreaseAllowance} and {increaseAllowance}
 * functions have been added to mitigate the well-known issues around setting
 * allowances. See {IERC20-approve}.
 */
contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    /**
     * @dev Sets the values for {name} and {symbol}.
     *
     * All two of these values are immutable: they can only be set once during
     * construction.
     */
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    /**
     * @dev Returns the name of the token.
     */
    function name() public view virtual override returns (string memory) {
        return _name;
    }

    /**
     * @dev Returns the symbol of the token, usually a shorter version of the
     * name.
     */
    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    /**
     * @dev Returns the number of decimals used to get its user representation.
     * For example, if `decimals` equals `2`, a balance of `505` tokens should
     * be displayed to a user as `5.05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the default value returned by this function, unless
     * it's overridden.
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    /**
     * @dev See {IERC20-balanceOf}.
     */
    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    /**
     * @dev See {IERC20-transfer}.
     *
     * Requirements:
     *
     * - `to` cannot be the zero address.
     * - the caller must have a balance of at least `amount`.
     */
    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    /**
     * @dev See {IERC20-allowance}.
     */
    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    /**
     * @dev See {IERC20-approve}.
     *
     * NOTE: If `amount` is the maximum `uint256`, the allowance is not updated on
     * `transferFrom`. This is semantically equivalent to an infinite approval.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    /**
     * @dev See {IERC20-transferFrom}.
     *
     * Emits an {Approval} event indicating the updated allowance. This is not
     * required by the EIP. See the note at the beginning of {ERC20}.
     *
     * NOTE: Does not update the allowance if the current allowance
     * is the maximum `uint256`.
     *
     * Requirements:
     *
     * - `from` and `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     * - the caller must have allowance for ``from``'s tokens of at least
     * `amount`.
     */
    function transferFrom(address from, address to, uint256 amount) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Atomically increases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     */
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    /**
     * @dev Atomically decreases the allowance granted to `spender` by the caller.
     *
     * This is an alternative to {approve} that can be used as a mitigation for
     * problems described in {IERC20-approve}.
     *
     * Emits an {Approval} event indicating the updated allowance.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `spender` must have allowance for the caller of at least
     * `subtractedValue`.
     */
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    /**
     * @dev Moves `amount` of tokens from `from` to `to`.
     *
     * This internal function is equivalent to {transfer}, and can be used to
     * e.g. implement automatic token fees, slashing mechanisms, etc.
     *
     * Emits a {Transfer} event.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `from` must have a balance of at least `amount`.
     */
    function _transfer(address from, address to, uint256 amount) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    /** @dev Creates `amount` tokens and assigns them to `account`, increasing
     * the total supply.
     *
     * Emits a {Transfer} event with `from` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     */
    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            // Overflow not possible: balance + amount is at most totalSupply + amount, which is checked above.
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, reducing the
     * total supply.
     *
     * Emits a {Transfer} event with `to` set to the zero address.
     *
     * Requirements:
     *
     * - `account` cannot be the zero address.
     * - `account` must have at least `amount` tokens.
     */
    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            // Overflow not possible: amount <= accountBalance <= totalSupply.
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    /**
     * @dev Sets `amount` as the allowance of `spender` over the `owner` s tokens.
     *
     * This internal function is equivalent to `approve`, and can be used to
     * e.g. set automatic allowances for certain subsystems, etc.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `owner` cannot be the zero address.
     * - `spender` cannot be the zero address.
     */
    function _approve(address owner, address spender, uint256 amount) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Updates `owner` s allowance for `spender` based on spent `amount`.
     *
     * Does not update the allowance amount in case of infinite allowance.
     * Revert if not enough allowance is available.
     *
     * Might emit an {Approval} event.
     */
    function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    /**
     * @dev Hook that is called before any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * will be transferred to `to`.
     * - when `from` is zero, `amount` tokens will be minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens will be burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal virtual {}

    /**
     * @dev Hook that is called after any transfer of tokens. This includes
     * minting and burning.
     *
     * Calling conditions:
     *
     * - when `from` and `to` are both non-zero, `amount` of ``from``'s tokens
     * has been transferred to `to`.
     * - when `from` is zero, `amount` tokens have been minted for `to`.
     * - when `to` is zero, `amount` of ``from``'s tokens have been burned.
     * - `from` and `to` are never both zero.
     *
     * To learn more about hooks, head to xref:ROOT:extending-contracts.adoc#using-hooks[Using Hooks].
     */
    function _afterTokenTransfer(address from, address to, uint256 amount) internal virtual {}
}

// File: https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.2/contracts/token/ERC20/extensions/ERC20Burnable.sol


// OpenZeppelin Contracts (last updated v4.5.0) (token/ERC20/extensions/ERC20Burnable.sol)

pragma solidity ^0.8.0;



/**
 * @dev Extension of {ERC20} that allows token holders to destroy both their own
 * tokens and those that they have an allowance for, in a way that can be
 * recognized off-chain (via event analysis).
 */
abstract contract ERC20Burnable is Context, ERC20 {
    /**
     * @dev Destroys `amount` tokens from the caller.
     *
     * See {ERC20-_burn}.
     */
    function burn(uint256 amount) public virtual {
        _burn(_msgSender(), amount);
    }

    /**
     * @dev Destroys `amount` tokens from `account`, deducting from the caller's
     * allowance.
     *
     * See {ERC20-_burn} and {ERC20-allowance}.
     *
     * Requirements:
     *
     * - the caller must have allowance for ``accounts``'s tokens of at least
     * `amount`.
     */
    function burnFrom(address account, uint256 amount) public virtual {
        _spendAllowance(account, _msgSender(), amount);
        _burn(account, amount);
    }
}

// File: contracts/CCC.sol



/*
MIT License

Copyright (c) 2024 Cat Church LLC (see CCC.meme)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
*/

/*
This software is based in part on OpenZeppelin Contracts (https://github.com/OpenZeppelin/openzeppelin-contracts),
which is licensed under the MIT License (https://opensource.org/licenses/MIT).

Copyright (c) 2024 Cat Church LLC
*/

pragma solidity ^0.8.0;



// PRECOMPILER_TARGET = PRODUCTION_TEST //Comments will show DEBUG_TEST vs PRODUCTION_TEST vs PRODUCTION_LAUNCH.

//import "OpenZeppelin/openzeppelin-contracts@4.7.3/contracts/token/ERC20/ERC20.sol";

contract CCC is ERC20, ERC20Burnable {
    // On ethereum, Timestamps are non-decresasing as blocks increase, which is all we require for security
    // So we use timestamps instead of blocks which means the start and end times of our Mint and Multiply
    // stages are predictable with very good accuracy.

    // PRECOMPILER_TARGET

    // DEBUG_TEST:
    // uint256 MintOneStart = 1807433200; //long time in future, use functions to adjust after deployment
    // uint256 MintOneEnd = 1907433200; // long time in future, use functions to adjust after deployment
    // uint256 MintTwoEnd = 2007692400; // long time in future, use functions to adjust after deployment

    // PRODUCTION_TEST1:
    // official public test #1 timestamps:
    // uint256 constant MintOneStart = 1707001200;
    // uint256 constant MintOneEnd = 1707433200;
    // uint256 constant MintTwoEnd = 1707692400;

    // PRODUCTION_TEST2:
    // official public test #2 timestamps:
    // Averaging period start: 1706482800, end: 1707951600 (17 days)
    // uint256 constant MintOneStart = 1708297200;
    //uint256 constant MintOneEnd = 1708642800;
    // uint256 constant MintTwoEnd = 1708902000;

    // PRODUCTION_TEST3:
    // official public test #3 timestamps:
    // Averaging period start: 1706482800, end: 1708902000 (28 days)
    // uint256 constant MintOneStart = 1708935300;
    // uint256 constant MintOneEnd = 1708935600;
    // uint256 constant MintTwoEnd = 1708935900;

    // PRODUCTION_LAUNCH::
    uint256 constant MintOneStart = 1709506800;
    uint256 constant MintOneEnd = 1711317600;
    uint256 constant MintTwoEnd = 1712527200;

    uint256 private totSupply;

    uint256 constant targetSupply = 1_000000000_000000000_000000000; // one billion, 18 zeros

    uint256 constant eighteenZeros = 1_000000000_000000000;

    mapping(bytes32 => uint256) public validHashes;
    string public dataUri;

    address public owner; // Will be set to zero by removeOwnerForever before minting starts.
    event OwnerSet(address newOwner);

    constructor(string memory name, string memory symbol) ERC20(name, symbol) {
        owner = msg.sender;
        emit OwnerSet(msg.sender);
    }

    // We include explcit dissalowance of address(0) for noobs to make things extra clear
    // Exercise for reader: why is "owner != address(0)" not needed?
    modifier onlyOwner() {
        require((msg.sender == owner) && (owner != address(0)), "Not owner.");
        _;
    }

    // removeOwnerForever()
    // In production mode this is only called from RegisterHashPricesSupplies,
    // which is guarded by onlyOwner.  So it should be private in production mode.
    // onlyOwner can Therefore be removed from production mode.

    // PRECOMPILER_TARGET

    // DEBUG_TEST:
    // function removeOwnerForever() external onlyOwner {
    // PRODUCTION_TEST:
    // PRODUCTION_LAUNCH:
    function removeOwnerForever() private {
        owner = address(0);
        emit OwnerSet(address(0)); // Emit event when owner is removed forever
    }

    mapping(address => uint256) public addressStates;

    function GetMintStatus(
        address user
    )
        public
        view
        returns (uint256 mintStage, uint256 accountStage, uint256 timeRemaining)
    {
        if (block.timestamp < MintOneStart) {
            mintStage = 0;
            timeRemaining = MintOneStart - block.timestamp;
        } else if (block.timestamp < MintOneEnd) {
            mintStage = 1;

            // We subtract 1 because a valid time to mint in first stage
            // must have a timestamp strictly less than
            timeRemaining = (MintOneEnd - block.timestamp) - 1;
        } else if (block.timestamp < MintTwoEnd) {
            mintStage = 2;
            // We subtract 1 because a valid time to mint in first stage
            // must have a timestamp strictly less than
            timeRemaining = (MintTwoEnd - block.timestamp) - 1;
        } else {
            mintStage = 3;
            timeRemaining = 0;
        }
        accountStage = GetMintstate(addressStates[user]);
        // return (mintStage, accountStage, timeRemaining); // <-- not needed, but useful reminder
    }

    // mapping(address => mapping(uint256 => VoteWeightTrack)) voteWeightTracks; // for vote weight tracking, not yet implemented.

    // user input added for testing, remove when done testing
    // msg.sender is address
    // proof.length is the proof height, i.e. height of the merkle tree root
    // that must be equal to the validHashes value indexed by the root hash.
    function MintOne(
        uint96 xone_amount,
        uint128 xen_amount,
        uint128 vmpx_amount,
        uint32 leafIndexUint32,
        bytes32[] memory proof
    ) public {
        require(
            tx.origin == msg.sender,
            "Contract addresses are not allowed to mint"
        );

        require(
            (block.timestamp >= uint256(MintOneStart)) &&
                (block.timestamp < uint256(MintOneEnd)),
            "Only works during MintOne window"
        );

        uint256 mintOneAmount = CalculateMintOneAmount(
            xone_amount,
            xen_amount,
            vmpx_amount
        );
        require(mintOneAmount > 0, "Can't mint zero");

        uint256 word = addressStates[msg.sender];
        uint256 accountStage = GetMintstate(word);

        require(accountStage == 0, "You already minted your first mint");

        // It would be slightly more gas efficient for ValidateProof to use msg.sender internally I think
        // But this is more general and we may want to make it public so we leave like this for now.
        require(
            ValidateProof(
                msg.sender,
                xone_amount,
                xen_amount,
                vmpx_amount,
                leafIndexUint32,
                proof
            ),
            "Proof failed"
        );

        // Set Mint State to 1
        // Set vote tracking enabled = 1 in both MintOne and MintTwo.
        // This allows transfers from address 0, (which are not allowed to be 0 amount), to always
        // be interpreted as enabling vote tracking.  Transfers events from address(1) (which are
        // also not allowed to be 0) are intepreted as disabling vote tracking.
        addressStates[msg.sender] = SetTrackenabled(SetMintstate(word, 1), 1);

        _mint(msg.sender, mintOneAmount);
    }
    
    function SetVoteTracking(bool enable) external {
        uint256 word = addressStates[msg.sender];
        uint256 balanceToTrack;
        
        uint256 trackenabled = GetTrackenabled(word);
        if (enable) {
            require(trackenabled != 1, "Already enabled.");
            balanceToTrack = GetBalance(word);
            trackenabled = 1;
            emit Transfer(address(0), msg.sender, 0); // Any mint is an enablement of tracking.
        } else {
            require(trackenabled != 0, "Already disabled.");
            balanceToTrack = 0;
            trackenabled = 0;
            emit Transfer(address(1), msg.sender, 0); // We encode a pseudo-mint of 0 quantity from address(1) as disabling tracking
                                                        // This enables easy sorting of transfer events with vote tracking enable/disable 
                                                        // events for off-chain reconstruction of voteWeight records.
                                                        // Works because nobody can derive private key for given public key such as
                                                        // address(1), and we prohibit transfer of amount 0 from addresses 0 and 1.
                                                        // It seems crazy that in standard ERC20, anyone can transfer 0 from any address,
                                                        // to themselves, including the mint address (0), which in any case
                                                        // pollutes event logs, enabling a sort of event log DOS for someone searching
                                                        // for all transfer events where their address is the receiver.
        }
        
        (uint256 tracklen, uint256 tracktime) = GetTracklenTracktime(word);
        if (tracktime != block.timestamp) {
            // Consider disabling vote tracking if high frequency transfers are
            // required on blockchains that break monotonically increasing timestamps
            require(
                block.timestamp > tracktime,
                "Chain broke monotonic timestamp"
            );
        } else {
            // We increase it by 1 below, so this has the effect of keeping it the same.
            tracklen -= 1;
        }
        SetVoteWeight(msg.sender, tracklen, balanceToTrack);

        addressStates[msg.sender] = SetTrackenabled(SetTracklen(SetTracktime(word, block.timestamp),tracklen+1),trackenabled);
        
    }

    // SafeTestTransactionFunction
    // This function is actually super useful. When a transaction is stuck, the way to get it
    // unstuck is to send a transaction with same nonce but much higher gas AND priority (don't forget priority).
    // But simple gas token transfers (e.g. transfering eth on ethereum) do not always have the option
    // in metamask to set custom high gas.  So a different transaction is needed that metamask doesn't hide
    // the gas customization settings for.  Hence this function which we know is safe.
    // We set a public storage value in case the solidity compiler would try to optimize it out (maybe not
    // necessary but some alternative compilers could need it) - and it prevents a warning that it could be a
    // view function even in standard compilers.
    uint256 public test_value;

    function SafeTestTransactionFunction(uint8 _test_value) external {
        test_value = uint256(_test_value);
    }

    // This is the "Multiply", or "Phase Two Mint", or "Second Mint Stage" function.
    function MintTwo() public {
        // This require is not needed because a contract account
        // will not have mintStageCompleted == 1 (see below)
        // require(
        //    tx.origin == msg.sender,
        //    "Contract addresses are not allowed to mint/multiply"
        // );

        require(
            (block.timestamp >= uint256(MintOneEnd)) &&
                (block.timestamp < uint256(MintTwoEnd)),
            "Only works during MintTwo (Multiply) window"
        );

        uint256 word = addressStates[msg.sender];
        uint256 accountStage = GetMintstate(word);

        require(
            accountStage == 1,
            "You already minted your second mint or you didn't mint your first mint"
        );

        // Calculate and store mintTwoMultiplier if it hasn't been initialized.
        if (mintTwoMultiplier == 0) {
            if (createdMintTwoMultiplier == false) {
                createdMintTwoMultiplier = true;
                uint256 currentSupply = totalSupply();
                mintTwoMultiplier =
                    ((targetSupply - currentSupply) * eighteenZeros) /
                    currentSupply;
            }
        }

        uint256 mintTwoAmount = CalculateMintTwoAmount(msg.sender);
        require(mintTwoAmount > 0, "Can't mint zero");

        // Set Mint State to 2
        // We force track enabled for each mint.  This allows transfer events from
        // 0 address to be interpreted as tracking enabled.
        addressStates[msg.sender] = SetTrackenabled(SetMintstate(word, 2), 1);

        _mint(msg.sender, mintTwoAmount);
    }

    event RegisteredHashPricesSuppliesUri(
        bytes32 rootHash,
        uint8 proofLength,
        uint256 xonePrice,
        uint256 xoneApplicableSupply,
        uint256 xenPrice,
        uint256 xenApplicableSupply,
        uint256 vmpxPrice,
        uint256 vmpxApplicableSupply,
        uint256 multiplier,
        string dataUri
    );

    uint256 public xonePrice;
    uint256 public xoneApplicableSupply;
    uint256 public xenPrice;
    uint256 public xenApplicableSupply;
    uint256 public vmpxPrice;
    uint256 public vmpxApplicableSupply;

    uint256 public mintOneMultiplier;
    uint256 public mintTwoMultiplier;
    bool public createdMintTwoMultiplier;

    // PRECOMPILER_TARGET

    // DEBUG_TEST:
    // function AdvanceToStartOfMintOne() external onlyOwner {
    //     MintOneStart = uint32(block.timestamp);
    // }

    // function AdvanceToEndOfMintOne() external onlyOwner {
    //     MintOneEnd = uint32(block.timestamp);
    // }

    // function AdvanceToEndOfMintTwo() external onlyOwner {
    //     MintTwoEnd = uint32(block.timestamp);
    // }

    // PRODUCTION_TEST:
    // PRODUCTION_LAUNCH:
    // // Do not add any time Advance functions

    function RegisterHashPricesSupplies(
        bytes32 _rootHash,
        uint8 _proofLength,
        uint256 _xonePrice,
        uint256 _xoneApplicableSupply,
        uint256 _xenPrice,
        uint256 _xenApplicableSupply,
        uint256 _vmpxPrice,
        uint256 _vmpxApplicableSupply,
        string memory _dataUri
    ) external onlyOwner {
        //removed for test.
        // Made the function public and onlyOwner in case we forget to put it back

        // PRECOMPILER_TARGET
        // DEBUG_TEST:
        // Do nothing.  We leave owner alone in debug mode.

        // PRODUCTION_TEST:
        // PRODUCTION_LAUNCH:
        // removeOwnerForever();

        validHashes[_rootHash] = _proofLength;
        xonePrice = _xonePrice;
        xoneApplicableSupply = _xoneApplicableSupply;
        xenPrice = _xenPrice;
        xenApplicableSupply = _xenApplicableSupply;
        vmpxPrice = _vmpxPrice;
        vmpxApplicableSupply = _vmpxApplicableSupply;

        // The price is per 10**18 (i.e. one full token) of applicable supply (in eth)
        // We divide by eighteen zeros at the end.  Then totalValue
        // is equal to total eth valuation of the all the tokens
        // with all their supply.
        uint256 totalValue = (_xonePrice *
            _xoneApplicableSupply +
            _xenPrice *
            _xenApplicableSupply +
            _vmpxPrice *
            _vmpxApplicableSupply) / eighteenZeros;
        uint256 multiplier = (eighteenZeros * targetSupply) / totalValue;
        mintOneMultiplier = multiplier;
        dataUri = _dataUri;

        emit RegisteredHashPricesSuppliesUri(
            _rootHash,
            _proofLength,
            _xonePrice,
            _xoneApplicableSupply,
            _xenPrice,
            _xenApplicableSupply,
            _vmpxPrice,
            _vmpxApplicableSupply,
            multiplier,
            dataUri
        );
    }

    function CalculateMintOneAmount(
        uint256 xoneAmount,
        uint256 xenAmount,
        uint256 vmpxAmount
    ) public view returns (uint256) {
        return
            (((xoneAmount *
                xonePrice +
                xenAmount *
                xenPrice +
                vmpxAmount *
                vmpxPrice) / eighteenZeros) * mintOneMultiplier) /
            eighteenZeros;
    }

    // This function is only applicable during MintTwo window for accounts
    // that haven't completed the mintTwo
    // It calculates the mintTwoMultiplier if mintTwoMultiplier hasn't been
    // set yet, so that it can be a view function.
    function CalculateMintTwoAmount(
        address user
    ) public view returns (uint256) {
        // Avoid reading the createdMintTwoMultiplier flag unless really needed
        uint256 mem_mintTwoMultiplier = mintTwoMultiplier;
        // Don't read createdMintTwoMultiplier right away becuse that would be
        // unnecessary gas.
        if (mem_mintTwoMultiplier == 0) {
            //redo these calculations to enable read-only function
            if (createdMintTwoMultiplier == false) {
                uint256 currentSupply = totalSupply();
                mem_mintTwoMultiplier =
                    ((targetSupply - currentSupply) * eighteenZeros) /
                    currentSupply;
            }
        }
        return (balanceOf(user) * mem_mintTwoMultiplier) / eighteenZeros;
    }

    // Returns true if proof is valid, otherwise false.
    // Reverts if proof length is zero.
    function ValidateProof(
        address user,
        uint96 xone_amount,
        uint128 xen_amount,
        uint128 vmpx_amount,
        uint32 leafIndexUint32,
        bytes32[] memory proof
    ) public view returns (bool) {
        // This check that proof length is greater than 0 is absolutely essential.
        // Because we search validHashes which is filled with zero values for every
        // hash, so searching for 0 height proof would always return true (eek!)
        require(proof.length > 0, "Proof length cannot be zero");
        //gas optimization to avoid small integer math
        uint256 leafIndex = uint256(leafIndexUint32);
        // Calculate the hash of the leaf node
        bytes32 computedHash = keccak256(
            abi.encodePacked(user, xone_amount, xen_amount, vmpx_amount)
        );

        // Compute the Merkle root from the proof and leaf
        // data_uri points to all the 512-bit leaf data, sorted by address.  The tree is binary
        // and to fill out empty leaves, a hash value (not leaf value, but leaf post-hash value)
        // is 0x0123456789abcdeffedcba987654321000112233445566778899aabbccddeeff
        // See how it is sequential and not the output of a keccak256.  If it were the output of
        // a keccak256, that is how one could sneak in a secret allocation.
        for (uint256 i = 0; i < proof.length; i++) {
            bytes32 proofElement = proof[i];

            //if we are odd at this level, proof element goes first since it is on left
            if (((leafIndex >> i) & 0x1) != 0) {
                computedHash = keccak256(
                    abi.encodePacked(proofElement, computedHash)
                );
            } else {
                computedHash = keccak256(
                    abi.encodePacked(computedHash, proofElement)
                );
            }
        }

        // Check if the computed root hash exists in the validHashes mapping
        // with the appropriate required proof length.  The proof length is
        // required to match the height _exactly_.  It's an exercise for the reader as to
        // what exploit is possible if it were allowed to be any non-zero number.
        // Hint: our leaves are 512-bits and the hash at each branch is of two 
        // lower level hashes each 256-bits, totalling 512-bits...
        // We don't require first mint not to be done because the mint stages are checked
        // in the mint functions, and this public function is useful for someone trying to
        // check their own implementation of proof creation.
        if (validHashes[computedHash] == proof.length) {
            return true;
        } else {
            return false;
        }
    }

    // Validates the proof and if valid it returns the amount of CCC the user could mint
    // in the first mint.
    // This is a helper test function that could be removed since ValidateProof is public,
    // and CalculateMintOneAmount is public.  It's kind of handy to do both at once though.
    function TestMintOne(
        address user,
        uint96 xone_amount,
        uint128 xen_amount,
        uint128 vmpx_amount,
        uint32 leafIndexUint32,
        bytes32[] memory proof
    ) public view returns (uint256) {
        if (
            ValidateProof(
                user,
                xone_amount,
                xen_amount,
                vmpx_amount,
                leafIndexUint32,
                proof
            )
        ) {
            return CalculateMintOneAmount(xone_amount, xen_amount, vmpx_amount);
        } else {
            return 0;
        }
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual override {
        super._beforeTokenTransfer(from, to, amount); // Call parent hook if there is any
        //Maybe we should just require all transfer be greater than 0, but we avoid changing typical Open Zeppelelin behavior too much.
        if(from == address(0) || from == address(1)) { 
            // It seems very strange to me that anyone can call TransferFrom, without prior approval, from any address and transfer 0 to themselves.
            require(amount > 0, "address 0 and 1 cannot xfer 0, their events are coopted to indicate vote tracking enabled/disabled respectively");
        }
        require(
            ((block.timestamp >= MintTwoEnd) || (from == address(0))),
            "Multiply stage is not done yet."
        );
    }

    // We override these functions to use our balances storage addressStates.  This packs lots more data, saves on minting gas,
    // and saves on vote tracking.
    function _transfer(
        address sender,
        address recipient,
        uint256 amt
    ) internal override {
        require(address(0) != sender, "0 address xfer");
        require(address(0) != recipient, "0 address xfer");

        // Custom logic using addressStates for balances.

        _beforeTokenTransfer(sender, recipient, amt);

        uint256 senderWord = addressStates[sender];
        uint256 senderBalance = GetBalance(senderWord);

        require(senderBalance >= amt, "amt > sender balance");

        AppendVoteWeightIAAndSaveAccountState(
            sender,
            senderWord,
            senderBalance - amt
        );

        // Overflow is impossible.

        _increaseBalance(recipient, amt);

        // Ensure to emit Transfer event
        emit Transfer(sender, recipient, amt);

        _afterTokenTransfer(sender, recipient, amt);
    }

    function _increaseBalance(address recipient, uint256 amt) private {
        uint256 word = addressStates[recipient];
        uint256 balance = GetBalance(word);

        AppendVoteWeightIAAndSaveAccountState(recipient, word, balance + amt);
    }

    function _mint(address my_address, uint256 amt) internal override {
        require(address(0) != my_address, "0 addr mint");

        _beforeTokenTransfer(address(0), my_address, amt);

        totSupply += amt;

        _increaseBalance(my_address, amt);

        emit Transfer(address(0), my_address, amt);

        _afterTokenTransfer(address(0), my_address, amt);
    }

    function _burn(address my_address, uint256 amt) internal override {
        require(address(0) != my_address, "0 addr burn");

        _beforeTokenTransfer(my_address, address(0), amt);

        uint256 word = addressStates[my_address];
        uint256 balance = GetBalance(word);

        require(balance >= amt, "amt > burner balance");

        AppendVoteWeightIAAndSaveAccountState(my_address, word, balance - amt);

        totSupply -= amt;

        emit Transfer(my_address, address(0), amt);

        _afterTokenTransfer(my_address, address(0), amt);
    }

    function balanceOf(
        address my_address
    ) public view override returns (uint256) {
        return GetBalance(addressStates[my_address]);
        //return ExtractField(addressStates[my_address], balance_index, balance_mask);
    }

    function totalSupply() public view override returns (uint256) {
        return totSupply;
    }

    // How great would it be if structs in storage could be copied to memory,
    // read and manipulated in memory, and then WRITTEN BACK in one shot from memory
    // to storage.  Alas Solidity does not allow it. You must instead edit structs field
    // by field directly operating on the storage variable, which costs 100 gas per field
    // that is edited (and another 100 to read it in a separate function if necessary).
    // Fuck that.  Vitalik et al. please enable this to be efficient:
    // struct AddressStateStruct {
    //     uint96 balance;     // restricted to uint90 by code
    //     uint64 tracklen;
    //     uint40 tracktime;   // restricted to uint38 by code
    //     bool trackenabled;
    //     uint8 minstate;     // restricted to 2 bits, values 0,1 or 2, by code.
    // }

    uint256 constant balance_index = 0;
    uint256 constant balance_length = 90;
    uint256 constant balance_mask = (1 << balance_length) - 1;

    // Length of the accountTracks for the address.  This is virtual length because each index in accountTracks represents
    //  two entries.
    uint256 constant tracklen_index = 90;
    uint256 constant tracklen_length = 64;
    uint256 constant tracklen_mask = (1 << tracklen_length) - 1;

    // Most recent time that was tracked.
    // This is equal to the timestamp of GetTracks[numtracks_length-1] but we don't have to do an extra storage read
    // to check if we should just be updating the last track (since it has the same timestamp) rather than appending.
    uint256 constant tracktime_index = 154;
    uint256 constant tracktime_length = 38; // matches number of bits of track timestamp of addressTracks entries
    uint256 constant tracktime_mask = (1 << tracktime_length) - 1;

    uint256 constant trackenabled_index = 192;
    uint256 constant trackenabled_length = 1;
    uint256 constant trackenabled_mask = (1 << trackenabled_length) - 1;

    uint256 constant mintstate_index = 193;
    uint256 constant mintstate_length = 2;
    uint256 constant mintstate_mask = (1 << mintstate_length) - 1;

    // Requires balance, tracklen, and tracktime to be the first 3 values in word.
    uint256 constant balanceTracklenTracktime_reversemask =
        ~((1 << (balance_length + tracklen_length + tracktime_length)) - 1);

    function ExtractField(
        uint256 word,
        uint256 index,
        uint256 mask
    ) private pure returns (uint256) {
        return (word >> index) & mask;
    }

    function SetField(
        uint256 word,
        uint256 index,
        uint256 mask,
        uint256 value
    ) private pure returns (uint256) {
        return (word & (~(mask << index))) | ((value & mask) << index);
    }

    // Requires balance to be the first value in the word
    // DOES NOT WRITE THE RESULT TO STORAGE.
    function SetBalance(
        uint256 word,
        uint256 new_balance
    ) private pure returns (uint256 new_word) {
        new_word = ((word) & (~balance_mask)) | (new_balance & balance_mask);
    }

    // Requires balance to be the first value in the word
    function GetBalance(uint256 word) private pure returns (uint256 balance) {
        balance = word & balance_mask;
    }

    function GetTracklenTracktime(
        uint256 word
    ) private pure returns (uint256 tracklen, uint256 tracktime) {
        tracklen = ExtractField(word, tracklen_index, tracklen_mask);
        tracktime = ExtractField(word, tracktime_index, tracktime_mask);
    }

    function GetTrackenabled(
        uint256 word
    ) private pure returns (uint256 trackenabled) {
        trackenabled = ExtractField(
            word,
            trackenabled_index,
            trackenabled_mask
        );
    }

    // DOES NOT WRITE THE RESULT TO STORAGE.
    function SetTrackenabled(
        uint256 word,
        uint256 trackenabled
    ) private pure returns (uint256 new_word) {
        new_word = SetField(
            word,
            trackenabled_index,
            trackenabled_mask,
            trackenabled
        );
    }

    // DOES NOT WRITE THE RESULT TO STORAGE.
    function SetTracklen( uint256 word, uint256 tracklen) private pure returns (uint256 new_word){
        new_word = SetField(
            word,
            tracklen_index,
            tracklen_mask,
            tracklen
        );
    }

    function SetTracktime( uint256 word, uint256 tracktime) private pure returns (uint256 new_word){
        new_word = SetField(
            word,
            tracktime_index,
            tracktime_mask,
            tracktime
        );
    }

    function GetMintstate(
        uint256 word
    ) private pure returns (uint256 mintstate) {
        return ExtractField(word, mintstate_index, mintstate_mask);
    }

    // DOES NOT WRITE THE RESULT TO STORAGE.
    function SetMintstate(
        uint256 word,
        uint256 mintstate
    ) private pure returns (uint256 new_word) {
        new_word = SetField(word, mintstate_index, mintstate_mask, mintstate);
    }

    function GetAddressState(address user) external view returns(uint96 balance, uint64 tracklen, uint40 tracktime, bool trackenabled, uint8 mintstate) {
        uint256 word = addressStates[user];
        balance = uint96(GetBalance(word));
        (uint256 tracklenUint256, uint256 tracktimeUint256) = GetTracklenTracktime(word);
        tracklen = uint64(tracklenUint256);
        tracktime = uint40(tracktimeUint256);
        trackenabled = false;
        if(GetTrackenabled(word) == 1) {
            trackenabled = true;
        }
        mintstate = uint8(GetMintstate(word));
    }

    function StorageSetStateBalanceTracklenNow(
        address my_address,
        uint256 word,
        uint256 balance,
        uint256 tracklen
    ) private {
        word &= balanceTracklenTracktime_reversemask;
        word |= balance & balance_mask;
        word |= (tracklen & tracklen_mask) << tracklen_index;
        word |= ((block.timestamp) & tracktime_mask) << tracktime_index; // In 8,000 years we're going to wish we used more bits.
        addressStates[my_address] = word;
    }

    //AppendVoteWeightIAAndSaveAccountState:
    //  We append vote weight, if applicable, and save account state.
    // We check if vote tracking is enabled, and if so we will store the new_balance to the appropriate location in addressTracks.
    // If the timestamp is the same as tracktime, then we know the most recent track has the current timestamp and we can just
    // overwrite it.  If not equal, then we append a new addressTrack and increment tracklen, and set the new tracktime.
    function AppendVoteWeightIAAndSaveAccountState(
        address my_address,
        uint256 word,
        uint256 new_balance
    ) private {
        uint256 trackenabled = GetTrackenabled(word);
        if (trackenabled != 0) {
            (uint256 tracklen, uint256 tracktime) = GetTracklenTracktime(word);
            if (tracktime != block.timestamp) {
                // Consider disabling vote tracking if high frequency transfers are
                // required on blockchains that break monotonically increasing timestamps
                require(
                    block.timestamp > tracktime,
                    "Chain broke monotonic timestamp"
                );
                // Append index is tracklen.  Append new balance there.  Timestamp will be now
                SetVoteWeight(my_address, tracklen, new_balance);
                // Increment tracklen, set new balance, and set tracktime to now.
                StorageSetStateBalanceTracklenNow(
                    my_address,
                    word,
                    new_balance,
                    tracklen + 1
                );
            } else {
                // Update the previous end of the list (instead of append).  This does not have a conflict for the
                // first append possibly being negative/out-of-bounds, because tracktime will be 0
                // so as long as block timestamp is greater than 0 (which it should always be) then
                // we won't end up in this condition when adding the first track.
                SetVoteWeight(my_address, tracklen - 1, new_balance);
                // We don't have to update tracklen or tracktime since we didn't append and tracktime is already block.timestamp
                addressStates[my_address] = SetBalance(word, new_balance);
            }
        } else {
            // Just set the balance, tracking is not enabled
            addressStates[my_address] = SetBalance(word, new_balance);
        }
    }

    struct PackedVoteWeightRecord {
        uint128 record0; // Lower 128 bits: 90 bits of voteWeight + 38 bits of timestamp
        uint128 record1; // Upper 128 bits: 90 bits of voteWeight + 38 bits of timestamp
    }

    // Mapping from an address and index to a PackedVoteWeightRecord
    mapping(address => mapping(uint256 => PackedVoteWeightRecord))
        public voteWeights;

    // 90-bit voteWeight are the high 90 bits in a 128-bit record.
    // 38-bit timestamp is bottom (least signficant) 38 bits.
    uint256 constant voteWeightMask = balance_mask;
    uint256 constant voteWeightShift = 38;
    uint256 constant voteWeightTimestampMask = (1 << voteWeightShift) - 1;

    // Function to set vote weight
    // This function writes to storage.
    function SetVoteWeight(
        address user,
        uint256 index,
        uint256 voteWeight
    ) private {
        // Mask the voteWeight to 90 bits
        uint128 combinedRecord = uint128(
            ((voteWeight & voteWeightMask) << voteWeightShift) |
                (block.timestamp & voteWeightTimestampMask)
        );

        // Calculate the storage index
        uint256 storageIndex = index / 2;

        // Directly reference the storage location to avoid multiple indexing
        PackedVoteWeightRecord storage record = voteWeights[user][storageIndex];

        if (index % 2 == 0) {
            // If the least significant bit of index is 0, store in record0
            record.record0 = combinedRecord;
        } else {
            // If the least significant bit of index is 1, store in record1
            record.record1 = combinedRecord;
        }
    }

    // Function to retrieve the vote weight and timestamp for a given index
    function GetVoteWeightAndTimestamp(
        address user,
        uint256 index
    ) public view returns (uint256 voteWeight, uint256 timestamp) {
        uint256 storageIndex = index / 2;

        PackedVoteWeightRecord storage record = voteWeights[user][storageIndex];
        uint256 specificRecord;
        if (index % 2 == 0) {
            specificRecord = record.record0;
        } else {
            specificRecord = record.record1;
        }
        timestamp = specificRecord & voteWeightTimestampMask; // The first 38 bits are the timestamp
        voteWeight = specificRecord >> voteWeightShift; // Shift right to get the vote weight
    }

    // This is a low gas alterantive to the function GetIndexAndVoteWeight
    // The idea is that a user will first pre-calculate the proper index using a call (zero gas)
    // and then pass the derived index to the voting functions which need only be validated
    // by calling this function to get vote weight.  This keeps the binary search out of the
    // transactions and moves them into the free calls.
    // Uses the fact that if next index is beyond the end of valid voteWeights (i..e not less than tracklen)
    // it can be inferred from timestamp == 0, which is a gas savings to not check tracklen itself.
    // This function validates the vote weight based on timestamps and passed index and retrieves the vote weight if valid
    function GetAndValidateVoteWeight(
        address user,
        uint64 indexUint64,
        uint40 inputTimestampUint40
    ) external view returns (bool isValid, uint96 voteWeight) {
        uint256 index = indexUint64;
        uint256 inputTimestamp = inputTimestampUint40;

        // Get the vote weight and timestamp for the given index
        (
            uint256 voteWeightAtIndex,
            uint256 timestampAtIndex
        ) = GetVoteWeightAndTimestamp(user, index);
        // Get the timestamp for the next index to compare
        (, uint256 timestampAtNextIndex) = GetVoteWeightAndTimestamp(
            user,
            index + 1
        );

        // Validate the input timestamp against the stored timestamps
        // Infer if timestampAtNextIndex == 0 then there is no next timestamp (beyond
        // length of valid records).
        if (
            timestampAtIndex <= inputTimestamp &&
            ((inputTimestamp < timestampAtNextIndex) ||
                (timestampAtNextIndex == 0))
        ) {
            // Since valid, return true for isValid and the voteWeight
            return (true, uint96(voteWeightAtIndex));
        } else {
            // If not valid, return false for isValid and 0 for voteWeight
            return (false, 0);
        }
    }

    // If zero vote weight returned, it may be out of bounds (before first vote weight entry)
    // or it may be an entry with 0 vote weight that was found.  Thus, a call to validate 0 vote
    // weight at index 0 could fail, it's not possible to know if it would succeed from this
    // call.  But if there is zero vote weight that should tell the user everything they should
    // want to know - i.e. you shouldn't need to later verify 0 vote weight.  If this was really
    // needed, the public function to read the first tracked voteweight entry could be used,
    // to check if the timestamp is before that.  If not, then the validate function above will work.
    function GetIndexAndVoteWeight(
        address user,
        uint40 inputTimestamp
    ) external view returns ( uint64 index, uint256 voteWeight) {
        (uint256 length, ) = GetTracklenTracktime(addressStates[user]);
        if (length == 0) {
            return (0, 0); // Early return for length 0.
        }

        uint256 low = 0;
        uint256 high = length - 1;
        uint256 mid;
        uint256 timestampAtMid;

        // Get the timestamp of the first entry to check if inputTimestamp is less than it
        (, uint256 firstTimestamp) = GetVoteWeightAndTimestamp(user, 0);
        if (inputTimestamp < firstTimestamp) {
            return (0, 0); // Early return if inputTimestamp is less than the first entry's timestamp
        }

        // Binary search to find the highest index with a timestamp less than or equal to inputTimestamp
        while (low < high) {
            mid = (low + high + 1) / 2; // Use upper mid to lean towards the right in case of equality
            (, timestampAtMid) = GetVoteWeightAndTimestamp(user, mid);

            if (timestampAtMid <= inputTimestamp) {
                low = mid; // Move right
            } else {
                high = mid - 1; // Move left
            }
        }

        // At this point, low should be at the correct index
        (uint256 foundVoteWeight, ) = GetVoteWeightAndTimestamp(user, low);
        return (uint64(low), uint96(foundVoteWeight));
    }
}