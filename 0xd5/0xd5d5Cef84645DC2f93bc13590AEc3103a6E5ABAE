// SPDX-License-Identifier: MIT

/*

X: https://x.com/Yescoin_Fam
Telegram: https://t.me/theYescoin_bot
Website: https://yescointon.com

*/

pragma solidity 0.8.0;

contract CrossChain {

    event SendCrossChain(
        uint fromChainId,
        uint toChainId,
        address destContract,
        bytes4 destFuncSig,
        uint64 sendNonce
    );
    event RecvCrossChain(address indexed sender, bytes txData);

    address crossChainVerifyAddr = 0xffFfffFfFFFfFFfffFFFffffFfFfffFfFF030002;
    address chainManagerAddr = 0xffFFFfffFFfFFfFFFFffFFFfFfFFffFFfF020002;



    uint64 crossChainSendNonce;
    uint64 crossChainRecvNonce;

    function getCrossChainSendNonce() public view returns (uint64) {
        return crossChainSendNonce;
    }

    function getCrossChainRecvNonce() public view returns (uint64) {
        return crossChainRecvNonce;
    }

    function getFromChainId() public  returns (uint) {
        address contractAddr = chainManagerAddr;
        bytes4 funcSig = bytes4(keccak256("getChainId()"));
        uint256 chainId;
        // solium-disable-next-line security/no-inline-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, funcSig)
            let result := call(20000, contractAddr, 0, ptr, 0x4, ptr, 0x20)
            if eq(result, 0) { revert(ptr, 0) }
            chainId := mload(ptr)
        }
        return uint(chainId);
    }

    function sendTransaction(
        uint toChainId,
        address destContract,
        bytes4 destFuncSig
    ) internal {
        
        uint fromChainId = getFromChainId();
        emit SendCrossChain(
            fromChainId,
            toChainId,
            destContract,
            destFuncSig,
            crossChainSendNonce
        );
        crossChainSendNonce += 1;
    }

    function verifyTransaction(
        bytes memory txProof,
        uint256 txDataSize
    ) internal returns (
        address sender,
        bytes memory txData
    ) {
        address recvContAddr = address(this);
        bytes4 recvFuncSig;
        // solium-disable-next-line security/no-inline-assembly
        assembly {
            let ptr := mload(0x40)
            calldatacopy(ptr, 0x0, 0x4)
            recvFuncSig := mload(ptr)
        }
        address contractAddr = crossChainVerifyAddr;
        bytes4 nativeFunc = bytes4(
            keccak256("verifyTransaction(address,bytes4,uint64,bytes)")
        );
        uint64 recvNonce = crossChainRecvNonce;
        // bytes len + bytes
        uint txProofSize = 0x20 + txProof.length / 0x20 * 0x20;
        if (txProof.length % 0x20 != 0) {
            txProofSize += 0x20;
        }
        // address + bytes pos + bytes len + bytes
        uint outSize = 0x60 + txDataSize / 0x20 * 0x20;
        if (txDataSize % 0x20 != 0) {
            outSize += 0x20;
        }
        // solium-disable-next-line security/no-inline-assembly
        assembly {
            let ptr := mload(0x40)
            mstore(ptr, nativeFunc)
            mstore(add(ptr, 0x04), recvContAddr)
            mstore(add(ptr, 0x24), recvFuncSig)
            mstore(add(ptr, 0x44), recvNonce)
            mstore(add(ptr, 0x64), 0x80)
            // copy txproof bytes
            let ptrL := add(ptr, 0x84)
            for {
                    let txProofL := txProof
                    let txProofR := add(txProof, txProofSize)
                }
                lt(txProofL, txProofR)
                {
                    txProofL := add(txProofL, 0x20)
                    ptrL := add(ptrL, 0x20)
                }
                {
                mstore(ptrL, mload(txProofL))
            }
            let inSize := sub(ptrL, ptr)
            switch call(100000, contractAddr, 0, ptr, inSize, ptr, outSize)
            case 0 { revert(0, 0) }
            default {
                // return(ptr, outSize)
                sender := mload(ptr)
                txData := add(ptr, 0x40)
            }
        }
        emit RecvCrossChain(sender, txData);
        crossChainRecvNonce += 1;
    }
}

pragma solidity ^0.8.0;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

pragma solidity ^0.8.0;

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

pragma solidity ^0.8.0;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

pragma solidity ^0.8.0;

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

pragma solidity ^0.8.0;

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;

    string private _name;
    string private _symbol;

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 18;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view virtual override returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, amount);
        return true;
    }

    function allowance(address owner, address spender) public view virtual override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public virtual override returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public virtual override returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, amount);
        _transfer(from, to, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = allowance(owner, spender);
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        unchecked {
            _approve(owner, spender, currentAllowance - subtractedValue);
        }

        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);

        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
            _balances[to] += amount;
        }

        emit Transfer(from, to, amount);

        _afterTokenTransfer(from, to, amount);
    }

    function _mint(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: mint to the zero address");

        _beforeTokenTransfer(address(0), account, amount);

        _totalSupply += amount;
        unchecked {
            _balances[account] += amount;
        }
        emit Transfer(address(0), account, amount);

        _afterTokenTransfer(address(0), account, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        unchecked {
            _balances[account] = accountBalance - amount;
            _totalSupply -= amount;
        }

        emit Transfer(account, address(0), amount);

        _afterTokenTransfer(account, address(0), amount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _spendAllowance(
        address owner,
        address spender,
        uint256 amount
    ) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            require(currentAllowance >= amount, "ERC20: insufficient allowance");
            unchecked {
                _approve(owner, spender, currentAllowance - amount);
            }
        }
    }

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}

    function _afterTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

pragma solidity ^0.8.0;

contract Yescoin is ERC20, Ownable, CrossChain {

    uint256 public crossStartAtBlock=99999999;

    constructor() ERC20("Yescoin", "YES") {
        _mint(msg.sender, 10000000 * 10**18);

    }
    
    function setCrossStartTime(uint256 _crossStartAtBlock) external onlyOwner{
        crossStartAtBlock = _crossStartAtBlock;
    }

     uint256 txDataSize = 0x20;

    function sendToTonChain(
        uint toChainId,
        address destContract,
        bytes memory txData
    ) public {
        require(block.number>=crossStartAtBlock,"cross chain not started!");
        
        require(txData.length == txDataSize);
        uint256 value;
        assembly {
            value := mload(add(txData, 0x20))
        }
        require(balanceOf(msg.sender) >= value);
        bytes4 destFuncHasher = bytes4(keccak256("recvFromTonChain(bytes)"));
        sendTransaction(toChainId, destContract, destFuncHasher);
        _burn(msg.sender,value);
    }

     // verify_proof need:
    // check from_chain_id in ChainManager.tonChains
    // check to_chain_id == my chain_id
    // check dest_contract == this
    // check hasher == RECV_FUNC_HASHER
    // check cross_chain_nonce == cross_chain_nonce
    // extract origin tx sender and origin tx data
    function recvFromTonChain(bytes memory txProof) internal  {
        address sender;
        bytes memory txData;
        (sender, txData) = verifyTransaction(txProof, txDataSize);
        uint256 value;
        assembly {
            value := mload(add(txData, 0x20))
        }
        _mint(msg.sender,value);
    }

}