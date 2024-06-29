//SPDX-License-Identifier: MIT
pragma solidity ^0.8.15;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

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

contract ERC20 is Context, IERC20, IERC20Metadata {
    mapping(address => uint256) internal _balances;

    mapping(address => mapping(address => uint256)) internal _allowances;
    uint256 private _totalSupply;
    uint8 private _decimals;

    string private _name;
    string private _symbol;

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
     * be displayed to a user as `5,05` (`505 / 10 ** 2`).
     *
     * Tokens usually opt for a value of 18, imitating the relationship between
     * Ether and Wei. This is the value {ERC20} uses, unless this function is
     * overridden;
     *
     * NOTE: This information is only used for _display_ purposes: it in
     * no way affects any of the arithmetic of the contract, including
     * {IERC20-balanceOf} and {IERC20-transfer}.
     */
    function decimals() public view virtual override returns (uint8) {
        return _decimals;
    }

    /**
     * @dev See {IERC20-totalSupply}.
     */
    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }


    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount)
        public
        virtual
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public virtual override returns (bool) {
        _transfer(sender, recipient, amount);

        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        _approve(sender, _msgSender(), currentAllowance - amount);

        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        virtual
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] + addedValue
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        virtual
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        require(sender != address(0), "ERC20: transfer from the zero address");
        //require(recipient != address(0), "ERC20: transfer to the zero address");

        _beforeTokenTransfer(sender, recipient, amount);
        _basicTransfer(sender, recipient, amount);
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal virtual {
        uint256 senderBalance = _balances[sender];
        require(
            senderBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        _balances[sender] = senderBalance - amount;
        _balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function _burn(address account, uint256 amount) internal virtual {
        require(account != address(0), "ERC20: burn from the zero address");

        _beforeTokenTransfer(account, address(0), amount);

        uint256 accountBalance = _balances[account];
        require(accountBalance >= amount, "ERC20: burn amount exceeds balance");
        _balances[account] = accountBalance - amount;
        _totalSupply -= amount;

        emit Transfer(account, address(0), amount);
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

    function _beforeTokenTransfer(
        address from,
        address to,
        uint256 amount
    ) internal virtual {}
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(
            address(this).balance >= amount,
            "Address: insufficient balance"
        );

        (bool success, ) = recipient.call{value: amount}("");
        require(
            success,
            "Address: unable to send value, recipient may have reverted"
        );
    }
}

contract Ownable {
    address _owner;
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    function renounceOwnership() public onlyOwner {
        _owner = address(0);
    }
    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _owner = newOwner;
    }
}

interface IFactory {
    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address uniswapV2Pair);

    function createPair(address tokenA, address tokenB)
        external
        returns (address uniswapV2Pair);
}

interface IRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Anarchy is ERC20, Ownable {
    using Address for address payable;
    
    IRouter private uniswapV2Router;
    address private uniswapV2Pair;
    address BURN_ADDRESS = 0x0000000000000000000000000000000000000000;

    bool public tradingActive;

    string private _name = "Anarchy";
    string private _symbol = "ANA";
    uint8 private _decimals = 18;
    uint256 private _totalSupply = 1666666666666666666666666666;

    bool private autoHandleFee = true;

    address public daoFeeWallet = 0x7cE7E5975EBc006aC7F3A5daCbDBC5B21545f1CD;
    address private presaleContract = 0x5d267900b71b0170Ccd1C5405EAE6c7fc4A62317;

    uint256 tokenHandleFeeThreshold = (_totalSupply / 1000) * 2;

    uint256 private feeMultiply = 1000;
    uint256 public daoFees = 45;
    uint256 public burnFees = 5;

    mapping(address => bool) public exemptFee;

    constructor(address router_)
    {
        IRouter _router = IRouter(router_);
        // Create a pancake uniswapV2Pair for this new token
        address _pair = IFactory(_router.factory()).createPair(
            address(this),
            _router.WETH()
        );
        _beforeTokenTransfer(address(0), msg.sender, _totalSupply);
        _balances[msg.sender] += _totalSupply;
        _owner = msg.sender;

        uniswapV2Router = _router;
        uniswapV2Pair = _pair;

        exemptFee[msg.sender] = true;
        exemptFee[address(this)] = true;
        exemptFee[presaleContract] = true;

        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    
    function approve(address spender, uint256 amount)
        public
        override
        returns (bool)
    {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function name() public view virtual override returns (string memory) {
        return _name;
    }

    function symbol() public view virtual override returns (string memory) {
        
        return _symbol;
    }

    function decimals() public view virtual override returns (uint8) {
        return _decimals;
    }

    function totalSupply() public view virtual override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account)
        public
        view
        virtual
        override
        returns (uint256)
    {
        return _balances[account];
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][_msgSender()];
        require(
            currentAllowance >= amount,
            "ERC20: transfer amount exceeds allowance"
        );
        _approve(sender, _msgSender(), currentAllowance - amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue)
        public
        override
        returns (bool)
    {
        _approve(
            _msgSender(),
            spender,
            _allowances[_msgSender()][spender] + addedValue
        );
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue)
        public
        override
        returns (bool)
    {
        uint256 currentAllowance = _allowances[_msgSender()][spender];
        require(
            currentAllowance >= subtractedValue,
            "ERC20: decreased allowance below zero"
        );
        _approve(_msgSender(), spender, currentAllowance - subtractedValue);

        return true;
    }

    function transfer(address recipient, uint256 amount)
        public
        override
        returns (bool)
    {
        _transfer(msg.sender, recipient, amount);
        return true;
    }


    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal override {
        require(amount > 0, "Transfer amount must be greater than zero");
        if (!exemptFee[sender] && !exemptFee[recipient]) {
            require(tradingActive, "Trading is not enabled");
        }

        if (
            exemptFee[sender] ||
            exemptFee[recipient] || 
            (sender == uniswapV2Pair)
        ){
        super._transfer(sender, recipient, amount);

        }else{
        uint256 _daoFeeAmount = (amount * daoFees) / feeMultiply;
        uint256 _burnFeeAmount = (amount * burnFees) / feeMultiply;

       
        if (_burnFeeAmount > 0) {
            super._transfer(sender, BURN_ADDRESS, _burnFeeAmount);
        }

        if (_daoFeeAmount > 0) {
            super._transfer(sender, address(this), _daoFeeAmount);
        }

        

        if (
            sender != uniswapV2Pair &&
            autoHandleFee &&
            balanceOf(address(this)) >= tokenHandleFeeThreshold
        ) {
            swapBack();
        }

        //rest to recipient
        super._transfer(sender, recipient, amount - _burnFeeAmount - _daoFeeAmount);
        

        }
    }

    function swapBack() private {
        uint256 contractBalance = balanceOf(address(this));
        if (contractBalance > 0) {
            swapTokensForETH(contractBalance);
        }
        uint256 ethBalance = address(this).balance;

        if(ethBalance > 0){
            payable(daoFeeWallet).sendValue(ethBalance);
        }

    }

    function handleFee(bool _autoHandle) external onlyOwner {
        autoHandleFee = _autoHandle;
        swapBack();
    }

    function swapTokensForETH(uint256 tokenAmount) private {
        // generate the pancake uniswapV2Pair path of token -> weth

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        _approve(address(this), address(uniswapV2Router), tokenAmount);

        // make the swap
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    function updateHandleFeeThreshold(uint256 new_amount) external onlyOwner {
        //update the treshhold
        require(
            tokenHandleFeeThreshold != new_amount * 10**decimals(),
            "You must provide a different amount other than the current value in order to update it"
        );
        tokenHandleFeeThreshold = new_amount * 10**decimals();
    }

    function enableTrading() external onlyOwner {
        tradingActive = true;
    }

    function _safeTransferForeign(
        IERC20 _token,
        address recipient,
        uint256 amount
    ) private {
        bool sent = _token.transfer(recipient, amount);
        require(sent, "Token transfer failed.");
    }

    function withdrawETH(uint256 amount, address receiveAddress)
        external
        onlyOwner
    {
        payable(receiveAddress).transfer(amount);
    }

    function withdrawToken(
        IERC20 _token,
        address receiveAddress,
        uint256 amount
    ) external onlyOwner {
        _safeTransferForeign(_token, receiveAddress, amount);
    }


    function updateExemptFee(address _address, bool flag) external onlyOwner {
        require(
            exemptFee[_address] != flag,
            "You must provide a different exempt address or status other than the current value in order to update it"
        );
        exemptFee[_address] = flag;
    }

    function setRouter(address newRouter)
        external
        onlyOwner
        returns (address _pair)
    {
        require(newRouter != address(0), "newRouter address cannot be 0");
        require(
            uniswapV2Router != IRouter(newRouter),
            "You must provide a different uniswapV2Router other than the current uniswapV2Router address in order to update it"
        );
        IRouter _router = IRouter(newRouter);

        _pair = IFactory(_router.factory()).getPair(
            address(this),
            _router.WETH()
        );
        if (_pair == address(0)) {
            // uniswapV2Pair doesn't exist
            _pair = IFactory(_router.factory()).createPair(
                address(this),
                _router.WETH()
            );
        }

        // Set the uniswapV2Pair of the contract variables
        uniswapV2Pair = _pair;
        // Set the uniswapV2Router of the contract variables
        uniswapV2Router = _router;
    }

    function updateDaoFeeWallet(address _newDaoWallet) external onlyOwner {
        daoFeeWallet = _newDaoWallet;
    }

    function updateFeeRatio(uint256 _daoFee, uint256 _burnFee) external onlyOwner {
        require(_daoFee + _burnFee <= 50, "Must keep fees at 5% or less");
        daoFees = _daoFee;
        burnFees = _burnFee;
    }

    // fallbacks
    receive() external payable {}

}