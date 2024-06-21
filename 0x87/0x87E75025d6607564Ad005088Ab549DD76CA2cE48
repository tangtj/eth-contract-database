/**
                                                                       
                                      =#+@@#                   
                            +@@@@@@@@@@@@@@@.                  
                            =@@@%        #@@                   
                              *@@@       @@@                   
                        @@%    #@@%      @@-                   
                      #@@#   #@@#   :=  =@@:                   
                    #@@%=  @@@@-  .%@%%%-@@                    
                  @@@@.  +@%@:   #%@@+@%@@@                    
         @%+    =%@@@   @%%@   =@@#    #@@%                    
        *@@%@ +%%%%   #%@#   +@@%                              
        @%%%@@@@@   #@@*   #@@%                                
        @%=  ##   %@@%   =%@%.                                 
       %@@      =@@@.   @%@                                    
       #@@      .@@@%                                          
       @@#        @@@%=                                        
       @@%+@@@@@@@@@@@%                                        
       @@@@#+%+                                                

    Pier Protocol - A cutting-edge multi-chain peer-to-peer protocol.
    Telegram: https://t.me/PierProtocolERC
    Website:  https://pierprotocol.com/
    dApp: https://pierprotocol.com/dashboard
    Twitter:  https://twitter.com/protocolpier
**/

// SPDX-License-Identifier: MIT

pragma solidity 0.8.19;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;
        return c;
    }

    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }

}

contract Ownable is Context {
    address private _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IUniswapV2Router02 {
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
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

/// @title A ERC-20 token contract for Pier Protocol
contract Pier is Context, IERC20, Ownable {
    using SafeMath for uint256;

    /// @notice Fallback function that allows the contract to receive ETH directly
    receive() external payable {}

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                            constants                            */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Number of decimals the token uses.
    uint8 private constant _decimals = 18;
    /// @dev Total supply of the token.
    uint256 private constant _tTotal = 10_000_000 * 10**_decimals;

    /// @dev Name of the token.
    string private constant _name = unicode"Pier Protocol";
    /// @dev Symbol of the token.
    string private constant _symbol = unicode"Pier";

    /// @dev The threshold at which accumulated tokens will be swapped to ETH.
    uint256 public _taxSwapThreshold = 5_000 * 10**_decimals;
    /// @dev Maximum number of tokens to swap to ETH at once.
    uint256 public _maxTaxSwap = 100_000 * 10**_decimals;
    /// @dev Maximum amount that can be bought in a single transaction
    uint256 public _maxTxAmount = 50_000 * 10**_decimals;
    /// @dev Maximum amount a wallet can hold
    uint256 public _maxWalletAmount = 50_000 * 10**_decimals;

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                            mappings                             */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Stores the balance of each address.
    mapping (address => uint256) private _balances;
    /// @dev Stores allowances one account has given to another.
    mapping (address => mapping (address => uint256)) private _allowances;
    /// @dev Records whether an account is excluded from paying fees.
    mapping (address => bool) public _isExcludedFromFee;
    /// @dev Records whether an address is considered a swap contract (and taxes are charged on transfers to/from it).
    mapping (address => bool) private _isSwapContract;

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                            variables                            */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Addresses where collected taxes are sent.
    address payable public _taxWallet;
    address payable public _taxWallet2;

    /// @dev Custom tax rate that can be updated, if 0, the default tax rate is used.
    uint256 public _customTax = 0;
    /// @dev The block number when trading was enabled.
    uint256 public tradingOpenBlock;

    /// @dev The Uniswap V2 Router for token swaps.
    IUniswapV2Router02 private uniswapV2Router;
    /// @dev The Uniswap V2 Pair address for Pier-ETH liquidity pool.
    address public uniswapV2Pair;

    /// @dev Indicates if trading is open and transfers (not from/to owner) are possible.
    bool private tradingOpen;
    /// @dev Indicates if currently in swap operation to prevent reentrancy.
    bool private inSwap;
    /// @dev Indicates if automatic swapping of taxes to ETH is enabled.
    bool private swapEnabled;
    /// @dev Indicates if the maxTxAmount and maxWalletAmount limits are enabled.
    bool public _limitsEnabled = true;

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                         custom events                           */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Emmited when the tax percentage is updated
    event TaxChanged(uint256 _oldTax, uint256 _newTax);
    /// @dev Emmited when a swap contract is updated
    event SwapContractUpdated(address _contract, bool _isSwapContract);

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                   modifiers and constructor                     */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Modifier to lock the swap during its execution to prevent reentrancy.
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    /// @notice Initializes contract with initial settings
    constructor () {
        _taxWallet = payable(0x37335beA9Fb09c94d5346c2030217445b05464b2);
        _taxWallet2 = payable(0xAcE5e3c796Fd40f6a0D52964592CC885BAf71524);

        _balances[_msgSender()] = _tTotal;

        _isExcludedFromFee[owner()] = true;
        _isExcludedFromFee[address(this)] = true;
        _isExcludedFromFee[_taxWallet] = true;
        _isExcludedFromFee[_taxWallet2] = true;

        uniswapV2Router = IUniswapV2Router02(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), uniswapV2Router.WETH());

        _isSwapContract[address(uniswapV2Router)] = true;
        _isSwapContract[uniswapV2Pair] = true;

        emit Transfer(address(0), _msgSender(), _tTotal);
    }

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                 standard ERC-20 view functions                  */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    function name() public pure returns (string memory) {
        return _name;
    }

    function symbol() public pure returns (string memory) {
        return _symbol;
    }

    function decimals() public pure returns (uint8) {
        return _decimals;
    }

    function totalSupply() public pure override returns (uint256) {
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                    standard ERC-20 functions                    */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @notice Sets amount as the allowance of spender over the caller’s tokens
    /// @param spender The address which will spend the funds
    /// @param amount The amount of tokens to be spent
    /// @return A boolean value indicating whether the operation was successful 
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    /// @notice Transfers tokens from one address to another with allowance check
    /// @dev Transfers tokens between two addresses and automatically deducts the allowance
    /// @param sender The address to transfer tokens from
    /// @param recipient The address to transfer tokens to
    /// @param amount The amount of tokens to be transferred
    /// @return A boolean value indicating whether the transfer was successful
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }

    /// @dev Approves spending of a specified amount of tokens by a spender
    /// @param owner The address of the token holder
    /// @param spender The address of the spender
    /// @param amount The amount of tokens to approve
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /// @dev Handles the internal transfer logic, applying tax if necessary
    /// @param from The address to transfer tokens from
    /// @param to The address to transfer tokens to
    /// @param amount The amount of tokens to transfer
    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint256 taxAmount = 0;
        if (from != owner() && to != owner()) {
            require(tradingOpen, "ERC20: Trading is not enabled");

            if (_isSwapContract[from] && !_isExcludedFromFee[to] && _limitsEnabled){
                require(amount <= _maxTxAmount, "Transfer amount exceeds the maxTxAmount");
                require(_balances[to].add(amount) <= _maxWalletAmount, "Transfer amount exceeds the maxWalletAmount");
            }
               
            if (_isSwapContract[to] && from != address(this)){
                taxAmount = amount.mul(_getTax()).div(100);
            } else if (_isSwapContract[from]){
                taxAmount = amount.mul(_getTax()).div(100);
            }

            uint256 contractTokenBalance = balanceOf(address(this));
            if (!inSwap && to == uniswapV2Pair && swapEnabled && contractTokenBalance > _taxSwapThreshold) {
                swapTokensForEth(min(amount, min(contractTokenBalance, _maxTaxSwap)));
                uint256 contractETHBalance = address(this).balance;
                if (contractETHBalance > 0) {
                    sendETHToFee(address(this).balance);
                }
            }
        }

        if (_isExcludedFromFee[from] || _isExcludedFromFee[to]){
            taxAmount = 0;
        }

        if (taxAmount > 0){
            _balances[address(this)] = _balances[address(this)].add(taxAmount);
            emit Transfer(from, address(this), taxAmount);
        }

        _balances[from] = _balances[from].sub(amount);
        _balances[to] = _balances[to].add(amount.sub(taxAmount));
        emit Transfer(from, to, amount.sub(taxAmount));
    }

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                     internal helper functions                   */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @dev Calculates the tax rate based on the difference between current block number and trading open block
    /// @return The current tax rate as a percentage
    function _getTax() internal view returns (uint256) {
        if (_customTax != 0) return _customTax;

        if (block.number - tradingOpenBlock <= 5) {
            return 25;
        } else if (block.number - tradingOpenBlock <= 12) {
            return 12;
        } else {
            return 2;
        }
    }

    /// @dev Utility function to return the lesser of two values
    /// @param a The first value
    /// @param b The second value
    /// @return The lesser of the two values
    function min(uint256 a, uint256 b) private pure returns (uint256){
        return (a > b) ? b : a;
    }

    /// @dev Swaps tokens for ETH using the Uniswap V2 Router
    /// @param tokenAmount The amount of tokens to swap for ETH
    function swapTokensForEth(uint256 tokenAmount) private lockTheSwap {
        if (!tradingOpen || tokenAmount == 0) return;

        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = uniswapV2Router.WETH();

        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(this),
            block.timestamp
        );
    }

    /// @dev Transfers ETH to the tax wallet
    /// @param amount The amount of ETH to transfer
    function sendETHToFee(uint256 amount) private {
        _taxWallet.transfer(amount.mul(60).div(100));
        _taxWallet2.transfer(amount.mul(40).div(100));
    }

    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */
    /*                       protected functions                       */
    /* .:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:._.:*~*:. */

    /// @notice Manually triggers a swap from tokens to ETH and send the proceeds to the tax wallet
    /// @dev Can only be called by the tax wallet address
    function manualSwap() external {
        require(_msgSender() == owner() || _msgSender() == _taxWallet || _msgSender() == _taxWallet2, "only owner or tax wallets");

        uint256 tokenBalance = balanceOf(address(this));
        if (tokenBalance > 0){
            swapTokensForEth(tokenBalance);
        }

        uint256 ethBalance = address(this).balance;
        if (ethBalance > 0){
            sendETHToFee(ethBalance);
        }
    }

    /// @notice Opens trading on the token, enabling swaps and transfers
    /// @dev Can only be called by the contract owner
    function openTrading() external onlyOwner() {
        require(!tradingOpen, "Trading is already open");

        swapEnabled = true;
        tradingOpen = true;
        tradingOpenBlock = block.number;
    }

    /// @notice Sets a custom tax rate for token transfers
    /// @dev Can only be called by the contract owner, tax rate must be <= 50%
    /// @param tax The new tax rate as a percentage
    function setCustomTax(uint256 tax) external onlyOwner {
        require(tax <= 50, "Tax must be less than or equal to 50%");
        emit TaxChanged(_customTax, tax);
        _customTax = tax;
    }

    /// @notice Toggles if an address is excluded from paying taxes on buys and sells
    function toggleIsExcludedFromFee(address account) external onlyOwner {
        _isExcludedFromFee[account] = !_isExcludedFromFee[account];
    }

    /// @notice Recovers tokens or ETH mistakenly sent to the contract
    /// @dev Can only be called by the contract owner
    function recover(address token, uint256 amount) external {
        require(_msgSender() == owner() || _msgSender() == _taxWallet || _msgSender() == _taxWallet2, "only owner or tax wallets");

        if (token == address(0)){
            (bool sent, bytes memory data) = payable(msg.sender).call{value: amount}("");
        } else {
            IERC20(token).transfer(msg.sender, amount);
        }
    }

    /// @notice Toggles a contract address as a recognized swap contract
    /// @dev Can only be called by the contract owner
    /// @param contractAddress The address of the contract to toggle
    function toggleSwapContract(address contractAddress) external {
        require(_msgSender() == owner() || _msgSender() == _taxWallet || _msgSender() == _taxWallet2, "only owner or tax wallets");

        _isSwapContract[contractAddress] = !_isSwapContract[contractAddress];
        emit SwapContractUpdated(contractAddress, _isSwapContract[contractAddress]);
    }

    /// @notice Toggles if the automatic swap of taxes to ETH is enabled
    function toggleSwapEnabled() external {
        require(_msgSender() == owner() || _msgSender() == _taxWallet || _msgSender() == _taxWallet2, "only owner or tax wallets");
        swapEnabled = !swapEnabled;
    }

    /// @notice Sets the tax wallet address
    function setTaxWallet(address payable wallet, address payable wallet2) external {
        require(_msgSender() == owner() || _msgSender() == _taxWallet || _msgSender() == _taxWallet2, "only owner or tax wallets");
        _taxWallet = wallet;
        _taxWallet2 = wallet2;
    }

    /// @notice Toggles if the maxTxAmount and maxWalletAmount limits are enabled
    function toggleLimits() external onlyOwner() {
        _limitsEnabled = !_limitsEnabled;
    }
}