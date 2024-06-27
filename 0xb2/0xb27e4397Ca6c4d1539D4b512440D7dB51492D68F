// SPDX-License-Identifier: MIT
/*

DR.Sickrespect $DRSICK - Play to Earn

Twitter : https://x.com/drsickrespect
Medium : https://medium.com/@drsickrespect
Reddit : https://www.reddit.com/user/DrSickrespect/
Website : https://drsickrespect.com/
Telegram: https://t.me/DRSickrespect
Documentation: https://dr-sickrespect.gitbook.io/usddrsick-dr.sickrespect/

*/


pragma solidity ^0.8.0;

interface ISwapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);
    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);
    function createPair(address tokenA, address tokenB) external returns (address pair);
    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}

interface ISwapV3Factory {

    event OwnerChanged(address indexed oldOwner, address indexed newOwner);

    event PoolCreated(
        address indexed token0,
        address indexed token1,
        uint24 indexed fee,
        int24 tickSpacing,
        address pool
    );

    event FeeAmountEnabled(uint24 indexed fee, int24 indexed tickSpacing);

    function feeAmountTickSpacing(uint24 fee) external view returns (int24);

    function getPool(
        address tokenA,
        address tokenB,
        uint24 fee
    ) external view returns (address pool);

    function createPool(
        address tokenA,
        address tokenB,
        uint24 fee
    ) external returns (address pool);

}

interface ISwapFactory is ISwapV3Factory, ISwapV2Factory {}
// File: contracts/interfaces/ISwapRouter.sol


pragma solidity ^0.8.0;
pragma abicoder v2;

interface ISwapV1Router {

    function factory() external view returns (address);
    function WETH() external pure returns (address);

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
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
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
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}

interface ISwapV2Router is ISwapV1Router {
    
    function factoryV2() external pure returns (address);

    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

}

interface ISwapV3Router {

    function WETH9() external view returns (address);

    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another token
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param params The parameters necessary for the swap, encoded as `ExactInputSingleParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInputSingle(ExactInputSingleParams calldata params) external payable returns (uint256 amountOut);

    struct ExactInputParams {
        bytes path;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
    }

    function exactInput(ExactInputParams calldata params) external payable returns (uint256 amountOut);

    struct ExactOutputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 amountOut;
        uint256 amountInMaximum;
        uint160 sqrtPriceLimitX96;
    }

    function exactOutputSingle(ExactOutputSingleParams calldata params) external payable returns (uint256 amountIn);

    struct ExactOutputParams {
        bytes path;
        address recipient;
        uint256 amountOut;
        uint256 amountInMaximum;
    }


    function exactOutput(ExactOutputParams calldata params) external payable returns (uint256 amountIn);
    
    function uniswapV3SwapCallback(
        int256 amount0Delta,
        int256 amount1Delta,
        bytes calldata data
    ) external;

}

interface ISwapRouter is ISwapV2Router, ISwapV3Router {}
// File: contracts/interfaces/IPair.sol


pragma solidity ^0.8.0;

interface IPair {
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
}

interface IPairV2 is IPair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}

interface IPairV3 is IPair{

    event Initialize(uint160 sqrtPriceX96, int24 tick);

    event Mint(
        address sender,
        address indexed owner,
        int24 indexed tickLower,
        int24 indexed tickUpper,
        uint128 amount,
        uint256 amount0,
        uint256 amount1
    );

    event Collect(
        address indexed owner,
        address recipient,
        int24 indexed tickLower,
        int24 indexed tickUpper,
        uint128 amount0,
        uint128 amount1
    );

    event Burn(
        address indexed owner,
        int24 indexed tickLower,
        int24 indexed tickUpper,
        uint128 amount,
        uint256 amount0,
        uint256 amount1
    );

    event Swap(
        address indexed sender,
        address indexed recipient,
        int256 amount0,
        int256 amount1,
        uint160 sqrtPriceX96,
        uint128 liquidity,
        int24 tick
    );

    event Flash(
        address indexed sender,
        address indexed recipient,
        uint256 amount0,
        uint256 amount1,
        uint256 paid0,
        uint256 paid1
    );

    event IncreaseObservationCardinalityNext(
        uint16 observationCardinalityNextOld,
        uint16 observationCardinalityNextNew
    );

    event SetFeeProtocol(uint8 feeProtocol0Old, uint8 feeProtocol1Old, uint8 feeProtocol0New, uint8 feeProtocol1New);

    event CollectProtocol(address indexed sender, address indexed recipient, uint128 amount0, uint128 amount1);

    function fee() external view returns (uint24);

    function tickSpacing() external view returns (int24);

    function maxLiquidityPerTick() external view returns (uint128);

    function slot0()
        external
        view
        returns (
            uint160 sqrtPriceX96,
            int24 tick,
            uint16 observationIndex,
            uint16 observationCardinality,
            uint16 observationCardinalityNext,
            uint8 feeProtocol,
            bool unlocked
        );

    function feeGrowthGlobal0X128() external view returns (uint256);

    function feeGrowthGlobal1X128() external view returns (uint256);

    function protocolFees() external view returns (uint128 token0, uint128 token1);

    function liquidity() external view returns (uint128);

    function ticks(int24 tick)
        external
        view
        returns (
            uint128 liquidityGross,
            int128 liquidityNet,
            uint256 feeGrowthOutside0X128,
            uint256 feeGrowthOutside1X128,
            int56 tickCumulativeOutside,
            uint160 secondsPerLiquidityOutsideX128,
            uint32 secondsOutside,
            bool initialized
        );

    function tickBitmap(int16 wordPosition) external view returns (uint256);

    function positions(bytes32 key)
        external
        view
        returns (
            uint128 _liquidity,
            uint256 feeGrowthInside0LastX128,
            uint256 feeGrowthInside1LastX128,
            uint128 tokensOwed0,
            uint128 tokensOwed1
        );

    function observations(uint256 index)
        external
        view
        returns (
            uint32 blockTimestamp,
            int56 tickCumulative,
            uint160 secondsPerLiquidityCumulativeX128,
            bool initialized
        );

    function observe(uint32[] calldata secondsAgos)
        external
        view
        returns (int56[] memory tickCumulatives, uint160[] memory secondsPerLiquidityCumulativeX128s);


    function snapshotCumulativesInside(int24 tickLower, int24 tickUpper)
        external
        view
        returns (
            int56 tickCumulativeInside,
            uint160 secondsPerLiquidityInsideX128,
            uint32 secondsInside
        );

    function initialize(uint160 sqrtPriceX96) external;

    function mint(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount,
        bytes calldata data
    ) external returns (uint256 amount0, uint256 amount1);

    function collect(
        address recipient,
        int24 tickLower,
        int24 tickUpper,
        uint128 amount0Requested,
        uint128 amount1Requested
    ) external returns (uint128 amount0, uint128 amount1);

    function burn(
        int24 tickLower,
        int24 tickUpper,
        uint128 amount
    ) external returns (uint256 amount0, uint256 amount1);

    function swap(
        address recipient,
        bool zeroForOne,
        int256 amountSpecified,
        uint160 sqrtPriceLimitX96,
        bytes calldata data
    ) external returns (int256 amount0, int256 amount1);

    function flash(
        address recipient,
        uint256 amount0,
        uint256 amount1,
        bytes calldata data
    ) external;

    function increaseObservationCardinalityNext(uint16 observationCardinalityNext) external;

    function setFeeProtocol(uint8 feeProtocol0, uint8 feeProtocol1) external;

    function collectProtocol(
        address recipient,
        uint128 amount0Requested,
        uint128 amount1Requested
    ) external returns (uint128 amount0, uint128 amount1);

}
// File: contracts/libs/Tables.sol


pragma solidity ^0.8.20;




/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
*/

struct SwapParams {
    bool taxless;
    address sender;
    address recipient;
    uint96 amountsIn;
    uint64 txBlock;    
    uint96 taxAmounts;
    uint96 amountsOut;
}

struct TokenSetup {
    uint8 fairMode;
    uint24 gasLimit;
    uint16 buyTax;
    uint16 sellTax;
    uint16 transferTax;
    uint16 developmentShare;
    uint16 marketingShare;
    uint16 prizeShare;
    uint16 burnShare;
    uint16 autoLiquidityShare;
    uint16 swapThresholdRatio;
    address devWallet;
    address marketingWallet;
    address prizePool;
    address gameRouter;
}

struct Registry {
    mapping(address => Account) Address;
    mapping(uint256 => address) PID;
    mapping(address => mapping(address => uint256)) allowances;
    mapping(address => bool) helpers;
}

struct Account {
    uint16 identifiers;
    uint64 nonces;
    uint80 PID;
    uint96 balance;
    address Address;
}

struct Settings {
    uint8 fairMode;
    uint16 buyTax;
    uint16 sellTax;
    uint16 transferTax;
    uint16 developmentShare;
    uint16 marketingShare;
    uint16 prizeShare;
    uint16 burnShare;
    uint16 autoLiquidityShare;
    uint16 swapThresholdRatio;
    uint24 gas;
    address[3] feeRecipients;
}

struct LatestTransaction {
    mapping(address => SwapParams) _tx;
}

struct Storage {
    IPair PAIR;
    address owner;
    uint96 totalSupply;
    uint80 PID;
    bool launched;
    bool inSwap;
    Settings settings;
    Registry registry;
    LatestTransaction latestTx;
}

// File: contracts/interfaces/IERC20.sol


pragma solidity ^0.8.0;

interface IERC20 {

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);

    function name() external view returns(string memory);

    function symbol() external view returns(string memory);

    function decimals() external view returns(uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 value) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);

}

// File: contracts/libs/TransferHelper.sol


pragma solidity ^0.8.0;


library TransferHelper {

    error TxFailed();

    function _transfer(address to, uint256 value) internal {
        
        (bool success, bytes memory data) = payable(to).call{value: value}(new bytes(0));
        
        _checkResponse(success, data);

    }

    function _transfer(address to, uint256 value, uint256 gas) internal {
        
        (bool success, bytes memory data) = payable(to).call{value: value, gas: gas}(new bytes(0));
        
        _checkResponse(success, data);

    }    

    function _transfer(
        address token,
        address to,
        uint256 value
    ) internal {

        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(
                IERC20.transfer.selector, to, value)
            );

        _checkResponse(success, data);
    }

    function _transferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        
        (bool success, bytes memory data) =
            token.call(
                abi.encodeWithSelector(
                    IERC20.transferFrom.selector, from, to, value
                )
            );

        _checkResponse(success, data);

    }    

    function _approve(
        address token,
        address to,
        uint256 value
    ) internal {

        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(
                IERC20.approve.selector, to, value)
            );
        
        _checkResponse(success, data);

    }

    function _checkResponse(bool status, bytes memory data) internal pure {
        if (!(status && (data.length == 0 || abi.decode(data, (bool)))))
            revert TxFailed();
    }

}

// File: contracts/libs/SafeCast.sol


// OpenZeppelin Contracts (last updated v5.0.0) (utils/math/SafeCast.sol)
// This file was procedurally generated from scripts/generate/templates/SafeCast.js.

pragma solidity ^0.8.20;

/**
 * @dev Wrappers over Solidity's uintXX/intXX/bool casting operators with added overflow
 * checks.
 *
 * Downcasting from uint256/int256 in Solidity does not revert on overflow. This can
 * easily result in undesired exploitation or bugs, since developers usually
 * assume that overflows raise errors. `SafeCast` restores this intuition by
 * reverting the transaction when such an operation overflows.
 *
 * Using this library instead of the unchecked operations eliminates an entire
 * class of bugs, so it's recommended to use it always.
 */
library SafeCast {
    /**
     * @dev Value doesn't fit in an uint of `bits` size.
     */
    error SafeCastOverflowedUintDowncast(uint8 bits, uint256 value);

    /**
     * @dev An int value doesn't fit in an uint of `bits` size.
     */
    error SafeCastOverflowedIntToUint(int256 value);

    /**
     * @dev Value doesn't fit in an int of `bits` size.
     */
    error SafeCastOverflowedIntDowncast(uint8 bits, int256 value);

    /**
     * @dev An uint value doesn't fit in an int of `bits` size.
     */
    error SafeCastOverflowedUintToInt(uint256 value);

    /**
     * @dev Returns the downcasted uint248 from uint256, reverting on
     * overflow (when the input is greater than largest uint248).
     *
     * Counterpart to Solidity's `uint248` operator.
     *
     * Requirements:
     *
     * - input must fit into 248 bits
     */
    function toUint248(uint256 value) internal pure returns (uint248) {
        if (value > type(uint248).max) {
            revert SafeCastOverflowedUintDowncast(248, value);
        }
        return uint248(value);
    }

    /**
     * @dev Returns the downcasted uint240 from uint256, reverting on
     * overflow (when the input is greater than largest uint240).
     *
     * Counterpart to Solidity's `uint240` operator.
     *
     * Requirements:
     *
     * - input must fit into 240 bits
     */
    function toUint240(uint256 value) internal pure returns (uint240) {
        if (value > type(uint240).max) {
            revert SafeCastOverflowedUintDowncast(240, value);
        }
        return uint240(value);
    }

    /**
     * @dev Returns the downcasted uint232 from uint256, reverting on
     * overflow (when the input is greater than largest uint232).
     *
     * Counterpart to Solidity's `uint232` operator.
     *
     * Requirements:
     *
     * - input must fit into 232 bits
     */
    function toUint232(uint256 value) internal pure returns (uint232) {
        if (value > type(uint232).max) {
            revert SafeCastOverflowedUintDowncast(232, value);
        }
        return uint232(value);
    }

    /**
     * @dev Returns the downcasted uint224 from uint256, reverting on
     * overflow (when the input is greater than largest uint224).
     *
     * Counterpart to Solidity's `uint224` operator.
     *
     * Requirements:
     *
     * - input must fit into 224 bits
     */
    function toUint224(uint256 value) internal pure returns (uint224) {
        if (value > type(uint224).max) {
            revert SafeCastOverflowedUintDowncast(224, value);
        }
        return uint224(value);
    }

    /**
     * @dev Returns the downcasted uint216 from uint256, reverting on
     * overflow (when the input is greater than largest uint216).
     *
     * Counterpart to Solidity's `uint216` operator.
     *
     * Requirements:
     *
     * - input must fit into 216 bits
     */
    function toUint216(uint256 value) internal pure returns (uint216) {
        if (value > type(uint216).max) {
            revert SafeCastOverflowedUintDowncast(216, value);
        }
        return uint216(value);
    }

    /**
     * @dev Returns the downcasted uint208 from uint256, reverting on
     * overflow (when the input is greater than largest uint208).
     *
     * Counterpart to Solidity's `uint208` operator.
     *
     * Requirements:
     *
     * - input must fit into 208 bits
     */
    function toUint208(uint256 value) internal pure returns (uint208) {
        if (value > type(uint208).max) {
            revert SafeCastOverflowedUintDowncast(208, value);
        }
        return uint208(value);
    }

    /**
     * @dev Returns the downcasted uint200 from uint256, reverting on
     * overflow (when the input is greater than largest uint200).
     *
     * Counterpart to Solidity's `uint200` operator.
     *
     * Requirements:
     *
     * - input must fit into 200 bits
     */
    function toUint200(uint256 value) internal pure returns (uint200) {
        if (value > type(uint200).max) {
            revert SafeCastOverflowedUintDowncast(200, value);
        }
        return uint200(value);
    }

    /**
     * @dev Returns the downcasted uint192 from uint256, reverting on
     * overflow (when the input is greater than largest uint192).
     *
     * Counterpart to Solidity's `uint192` operator.
     *
     * Requirements:
     *
     * - input must fit into 192 bits
     */
    function toUint192(uint256 value) internal pure returns (uint192) {
        if (value > type(uint192).max) {
            revert SafeCastOverflowedUintDowncast(192, value);
        }
        return uint192(value);
    }

    /**
     * @dev Returns the downcasted uint184 from uint256, reverting on
     * overflow (when the input is greater than largest uint184).
     *
     * Counterpart to Solidity's `uint184` operator.
     *
     * Requirements:
     *
     * - input must fit into 184 bits
     */
    function toUint184(uint256 value) internal pure returns (uint184) {
        if (value > type(uint184).max) {
            revert SafeCastOverflowedUintDowncast(184, value);
        }
        return uint184(value);
    }

    /**
     * @dev Returns the downcasted uint176 from uint256, reverting on
     * overflow (when the input is greater than largest uint176).
     *
     * Counterpart to Solidity's `uint176` operator.
     *
     * Requirements:
     *
     * - input must fit into 176 bits
     */
    function toUint176(uint256 value) internal pure returns (uint176) {
        if (value > type(uint176).max) {
            revert SafeCastOverflowedUintDowncast(176, value);
        }
        return uint176(value);
    }

    /**
     * @dev Returns the downcasted uint168 from uint256, reverting on
     * overflow (when the input is greater than largest uint168).
     *
     * Counterpart to Solidity's `uint168` operator.
     *
     * Requirements:
     *
     * - input must fit into 168 bits
     */
    function toUint168(uint256 value) internal pure returns (uint168) {
        if (value > type(uint168).max) {
            revert SafeCastOverflowedUintDowncast(168, value);
        }
        return uint168(value);
    }

    /**
     * @dev Returns the downcasted uint160 from uint256, reverting on
     * overflow (when the input is greater than largest uint160).
     *
     * Counterpart to Solidity's `uint160` operator.
     *
     * Requirements:
     *
     * - input must fit into 160 bits
     */
    function toUint160(uint256 value) internal pure returns (uint160) {
        if (value > type(uint160).max) {
            revert SafeCastOverflowedUintDowncast(160, value);
        }
        return uint160(value);
    }

    /**
     * @dev Returns the downcasted uint152 from uint256, reverting on
     * overflow (when the input is greater than largest uint152).
     *
     * Counterpart to Solidity's `uint152` operator.
     *
     * Requirements:
     *
     * - input must fit into 152 bits
     */
    function toUint152(uint256 value) internal pure returns (uint152) {
        if (value > type(uint152).max) {
            revert SafeCastOverflowedUintDowncast(152, value);
        }
        return uint152(value);
    }

    /**
     * @dev Returns the downcasted uint144 from uint256, reverting on
     * overflow (when the input is greater than largest uint144).
     *
     * Counterpart to Solidity's `uint144` operator.
     *
     * Requirements:
     *
     * - input must fit into 144 bits
     */
    function toUint144(uint256 value) internal pure returns (uint144) {
        if (value > type(uint144).max) {
            revert SafeCastOverflowedUintDowncast(144, value);
        }
        return uint144(value);
    }

    /**
     * @dev Returns the downcasted uint136 from uint256, reverting on
     * overflow (when the input is greater than largest uint136).
     *
     * Counterpart to Solidity's `uint136` operator.
     *
     * Requirements:
     *
     * - input must fit into 136 bits
     */
    function toUint136(uint256 value) internal pure returns (uint136) {
        if (value > type(uint136).max) {
            revert SafeCastOverflowedUintDowncast(136, value);
        }
        return uint136(value);
    }

    /**
     * @dev Returns the downcasted uint128 from uint256, reverting on
     * overflow (when the input is greater than largest uint128).
     *
     * Counterpart to Solidity's `uint128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     */
    function toUint128(uint256 value) internal pure returns (uint128) {
        if (value > type(uint128).max) {
            revert SafeCastOverflowedUintDowncast(128, value);
        }
        return uint128(value);
    }

    /**
     * @dev Returns the downcasted uint120 from uint256, reverting on
     * overflow (when the input is greater than largest uint120).
     *
     * Counterpart to Solidity's `uint120` operator.
     *
     * Requirements:
     *
     * - input must fit into 120 bits
     */
    function toUint120(uint256 value) internal pure returns (uint120) {
        if (value > type(uint120).max) {
            revert SafeCastOverflowedUintDowncast(120, value);
        }
        return uint120(value);
    }

    /**
     * @dev Returns the downcasted uint112 from uint256, reverting on
     * overflow (when the input is greater than largest uint112).
     *
     * Counterpart to Solidity's `uint112` operator.
     *
     * Requirements:
     *
     * - input must fit into 112 bits
     */
    function toUint112(uint256 value) internal pure returns (uint112) {
        if (value > type(uint112).max) {
            revert SafeCastOverflowedUintDowncast(112, value);
        }
        return uint112(value);
    }

    /**
     * @dev Returns the downcasted uint104 from uint256, reverting on
     * overflow (when the input is greater than largest uint104).
     *
     * Counterpart to Solidity's `uint104` operator.
     *
     * Requirements:
     *
     * - input must fit into 104 bits
     */
    function toUint104(uint256 value) internal pure returns (uint104) {
        if (value > type(uint104).max) {
            revert SafeCastOverflowedUintDowncast(104, value);
        }
        return uint104(value);
    }

    /**
     * @dev Returns the downcasted uint96 from uint256, reverting on
     * overflow (when the input is greater than largest uint96).
     *
     * Counterpart to Solidity's `uint96` operator.
     *
     * Requirements:
     *
     * - input must fit into 96 bits
     */
    function toUint96(uint256 value) internal pure returns (uint96) {
        if (value > type(uint96).max) {
            revert SafeCastOverflowedUintDowncast(96, value);
        }
        return uint96(value);
    }

    /**
     * @dev Returns the downcasted uint88 from uint256, reverting on
     * overflow (when the input is greater than largest uint88).
     *
     * Counterpart to Solidity's `uint88` operator.
     *
     * Requirements:
     *
     * - input must fit into 88 bits
     */
    function toUint88(uint256 value) internal pure returns (uint88) {
        if (value > type(uint88).max) {
            revert SafeCastOverflowedUintDowncast(88, value);
        }
        return uint88(value);
    }

    /**
     * @dev Returns the downcasted uint80 from uint256, reverting on
     * overflow (when the input is greater than largest uint80).
     *
     * Counterpart to Solidity's `uint80` operator.
     *
     * Requirements:
     *
     * - input must fit into 80 bits
     */
    function toUint80(uint256 value) internal pure returns (uint80) {
        if (value > type(uint80).max) {
            revert SafeCastOverflowedUintDowncast(80, value);
        }
        return uint80(value);
    }

    /**
     * @dev Returns the downcasted uint72 from uint256, reverting on
     * overflow (when the input is greater than largest uint72).
     *
     * Counterpart to Solidity's `uint72` operator.
     *
     * Requirements:
     *
     * - input must fit into 72 bits
     */
    function toUint72(uint256 value) internal pure returns (uint72) {
        if (value > type(uint72).max) {
            revert SafeCastOverflowedUintDowncast(72, value);
        }
        return uint72(value);
    }

    /**
     * @dev Returns the downcasted uint64 from uint256, reverting on
     * overflow (when the input is greater than largest uint64).
     *
     * Counterpart to Solidity's `uint64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     */
    function toUint64(uint256 value) internal pure returns (uint64) {
        if (value > type(uint64).max) {
            revert SafeCastOverflowedUintDowncast(64, value);
        }
        return uint64(value);
    }

    /**
     * @dev Returns the downcasted uint56 from uint256, reverting on
     * overflow (when the input is greater than largest uint56).
     *
     * Counterpart to Solidity's `uint56` operator.
     *
     * Requirements:
     *
     * - input must fit into 56 bits
     */
    function toUint56(uint256 value) internal pure returns (uint56) {
        if (value > type(uint56).max) {
            revert SafeCastOverflowedUintDowncast(56, value);
        }
        return uint56(value);
    }

    /**
     * @dev Returns the downcasted uint48 from uint256, reverting on
     * overflow (when the input is greater than largest uint48).
     *
     * Counterpart to Solidity's `uint48` operator.
     *
     * Requirements:
     *
     * - input must fit into 48 bits
     */
    function toUint48(uint256 value) internal pure returns (uint48) {
        if (value > type(uint48).max) {
            revert SafeCastOverflowedUintDowncast(48, value);
        }
        return uint48(value);
    }

    /**
     * @dev Returns the downcasted uint40 from uint256, reverting on
     * overflow (when the input is greater than largest uint40).
     *
     * Counterpart to Solidity's `uint40` operator.
     *
     * Requirements:
     *
     * - input must fit into 40 bits
     */
    function toUint40(uint256 value) internal pure returns (uint40) {
        if (value > type(uint40).max) {
            revert SafeCastOverflowedUintDowncast(40, value);
        }
        return uint40(value);
    }

    /**
     * @dev Returns the downcasted uint32 from uint256, reverting on
     * overflow (when the input is greater than largest uint32).
     *
     * Counterpart to Solidity's `uint32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     */
    function toUint32(uint256 value) internal pure returns (uint32) {
        if (value > type(uint32).max) {
            revert SafeCastOverflowedUintDowncast(32, value);
        }
        return uint32(value);
    }

    /**
     * @dev Returns the downcasted uint24 from uint256, reverting on
     * overflow (when the input is greater than largest uint24).
     *
     * Counterpart to Solidity's `uint24` operator.
     *
     * Requirements:
     *
     * - input must fit into 24 bits
     */
    function toUint24(uint256 value) internal pure returns (uint24) {
        if (value > type(uint24).max) {
            revert SafeCastOverflowedUintDowncast(24, value);
        }
        return uint24(value);
    }

    /**
     * @dev Returns the downcasted uint16 from uint256, reverting on
     * overflow (when the input is greater than largest uint16).
     *
     * Counterpart to Solidity's `uint16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     */
    function toUint16(uint256 value) internal pure returns (uint16) {
        if (value > type(uint16).max) {
            revert SafeCastOverflowedUintDowncast(16, value);
        }
        return uint16(value);
    }

    /**
     * @dev Returns the downcasted uint8 from uint256, reverting on
     * overflow (when the input is greater than largest uint8).
     *
     * Counterpart to Solidity's `uint8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits
     */
    function toUint8(uint256 value) internal pure returns (uint8) {
        if (value > type(uint8).max) {
            revert SafeCastOverflowedUintDowncast(8, value);
        }
        return uint8(value);
    }

    /**
     * @dev Converts a signed int256 into an unsigned uint256.
     *
     * Requirements:
     *
     * - input must be greater than or equal to 0.
     */
    function toUint256(int256 value) internal pure returns (uint256) {
        if (value < 0) {
            revert SafeCastOverflowedIntToUint(value);
        }
        return uint256(value);
    }

    /**
     * @dev Returns the downcasted int248 from int256, reverting on
     * overflow (when the input is less than smallest int248 or
     * greater than largest int248).
     *
     * Counterpart to Solidity's `int248` operator.
     *
     * Requirements:
     *
     * - input must fit into 248 bits
     */
    function toInt248(int256 value) internal pure returns (int248 downcasted) {
        downcasted = int248(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(248, value);
        }
    }

    /**
     * @dev Returns the downcasted int240 from int256, reverting on
     * overflow (when the input is less than smallest int240 or
     * greater than largest int240).
     *
     * Counterpart to Solidity's `int240` operator.
     *
     * Requirements:
     *
     * - input must fit into 240 bits
     */
    function toInt240(int256 value) internal pure returns (int240 downcasted) {
        downcasted = int240(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(240, value);
        }
    }

    /**
     * @dev Returns the downcasted int232 from int256, reverting on
     * overflow (when the input is less than smallest int232 or
     * greater than largest int232).
     *
     * Counterpart to Solidity's `int232` operator.
     *
     * Requirements:
     *
     * - input must fit into 232 bits
     */
    function toInt232(int256 value) internal pure returns (int232 downcasted) {
        downcasted = int232(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(232, value);
        }
    }

    /**
     * @dev Returns the downcasted int224 from int256, reverting on
     * overflow (when the input is less than smallest int224 or
     * greater than largest int224).
     *
     * Counterpart to Solidity's `int224` operator.
     *
     * Requirements:
     *
     * - input must fit into 224 bits
     */
    function toInt224(int256 value) internal pure returns (int224 downcasted) {
        downcasted = int224(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(224, value);
        }
    }

    /**
     * @dev Returns the downcasted int216 from int256, reverting on
     * overflow (when the input is less than smallest int216 or
     * greater than largest int216).
     *
     * Counterpart to Solidity's `int216` operator.
     *
     * Requirements:
     *
     * - input must fit into 216 bits
     */
    function toInt216(int256 value) internal pure returns (int216 downcasted) {
        downcasted = int216(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(216, value);
        }
    }

    /**
     * @dev Returns the downcasted int208 from int256, reverting on
     * overflow (when the input is less than smallest int208 or
     * greater than largest int208).
     *
     * Counterpart to Solidity's `int208` operator.
     *
     * Requirements:
     *
     * - input must fit into 208 bits
     */
    function toInt208(int256 value) internal pure returns (int208 downcasted) {
        downcasted = int208(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(208, value);
        }
    }

    /**
     * @dev Returns the downcasted int200 from int256, reverting on
     * overflow (when the input is less than smallest int200 or
     * greater than largest int200).
     *
     * Counterpart to Solidity's `int200` operator.
     *
     * Requirements:
     *
     * - input must fit into 200 bits
     */
    function toInt200(int256 value) internal pure returns (int200 downcasted) {
        downcasted = int200(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(200, value);
        }
    }

    /**
     * @dev Returns the downcasted int192 from int256, reverting on
     * overflow (when the input is less than smallest int192 or
     * greater than largest int192).
     *
     * Counterpart to Solidity's `int192` operator.
     *
     * Requirements:
     *
     * - input must fit into 192 bits
     */
    function toInt192(int256 value) internal pure returns (int192 downcasted) {
        downcasted = int192(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(192, value);
        }
    }

    /**
     * @dev Returns the downcasted int184 from int256, reverting on
     * overflow (when the input is less than smallest int184 or
     * greater than largest int184).
     *
     * Counterpart to Solidity's `int184` operator.
     *
     * Requirements:
     *
     * - input must fit into 184 bits
     */
    function toInt184(int256 value) internal pure returns (int184 downcasted) {
        downcasted = int184(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(184, value);
        }
    }

    /**
     * @dev Returns the downcasted int176 from int256, reverting on
     * overflow (when the input is less than smallest int176 or
     * greater than largest int176).
     *
     * Counterpart to Solidity's `int176` operator.
     *
     * Requirements:
     *
     * - input must fit into 176 bits
     */
    function toInt176(int256 value) internal pure returns (int176 downcasted) {
        downcasted = int176(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(176, value);
        }
    }

    /**
     * @dev Returns the downcasted int168 from int256, reverting on
     * overflow (when the input is less than smallest int168 or
     * greater than largest int168).
     *
     * Counterpart to Solidity's `int168` operator.
     *
     * Requirements:
     *
     * - input must fit into 168 bits
     */
    function toInt168(int256 value) internal pure returns (int168 downcasted) {
        downcasted = int168(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(168, value);
        }
    }

    /**
     * @dev Returns the downcasted int160 from int256, reverting on
     * overflow (when the input is less than smallest int160 or
     * greater than largest int160).
     *
     * Counterpart to Solidity's `int160` operator.
     *
     * Requirements:
     *
     * - input must fit into 160 bits
     */
    function toInt160(int256 value) internal pure returns (int160 downcasted) {
        downcasted = int160(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(160, value);
        }
    }

    /**
     * @dev Returns the downcasted int152 from int256, reverting on
     * overflow (when the input is less than smallest int152 or
     * greater than largest int152).
     *
     * Counterpart to Solidity's `int152` operator.
     *
     * Requirements:
     *
     * - input must fit into 152 bits
     */
    function toInt152(int256 value) internal pure returns (int152 downcasted) {
        downcasted = int152(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(152, value);
        }
    }

    /**
     * @dev Returns the downcasted int144 from int256, reverting on
     * overflow (when the input is less than smallest int144 or
     * greater than largest int144).
     *
     * Counterpart to Solidity's `int144` operator.
     *
     * Requirements:
     *
     * - input must fit into 144 bits
     */
    function toInt144(int256 value) internal pure returns (int144 downcasted) {
        downcasted = int144(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(144, value);
        }
    }

    /**
     * @dev Returns the downcasted int136 from int256, reverting on
     * overflow (when the input is less than smallest int136 or
     * greater than largest int136).
     *
     * Counterpart to Solidity's `int136` operator.
     *
     * Requirements:
     *
     * - input must fit into 136 bits
     */
    function toInt136(int256 value) internal pure returns (int136 downcasted) {
        downcasted = int136(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(136, value);
        }
    }

    /**
     * @dev Returns the downcasted int128 from int256, reverting on
     * overflow (when the input is less than smallest int128 or
     * greater than largest int128).
     *
     * Counterpart to Solidity's `int128` operator.
     *
     * Requirements:
     *
     * - input must fit into 128 bits
     */
    function toInt128(int256 value) internal pure returns (int128 downcasted) {
        downcasted = int128(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(128, value);
        }
    }

    /**
     * @dev Returns the downcasted int120 from int256, reverting on
     * overflow (when the input is less than smallest int120 or
     * greater than largest int120).
     *
     * Counterpart to Solidity's `int120` operator.
     *
     * Requirements:
     *
     * - input must fit into 120 bits
     */
    function toInt120(int256 value) internal pure returns (int120 downcasted) {
        downcasted = int120(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(120, value);
        }
    }

    /**
     * @dev Returns the downcasted int112 from int256, reverting on
     * overflow (when the input is less than smallest int112 or
     * greater than largest int112).
     *
     * Counterpart to Solidity's `int112` operator.
     *
     * Requirements:
     *
     * - input must fit into 112 bits
     */
    function toInt112(int256 value) internal pure returns (int112 downcasted) {
        downcasted = int112(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(112, value);
        }
    }

    /**
     * @dev Returns the downcasted int104 from int256, reverting on
     * overflow (when the input is less than smallest int104 or
     * greater than largest int104).
     *
     * Counterpart to Solidity's `int104` operator.
     *
     * Requirements:
     *
     * - input must fit into 104 bits
     */
    function toInt104(int256 value) internal pure returns (int104 downcasted) {
        downcasted = int104(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(104, value);
        }
    }

    /**
     * @dev Returns the downcasted int96 from int256, reverting on
     * overflow (when the input is less than smallest int96 or
     * greater than largest int96).
     *
     * Counterpart to Solidity's `int96` operator.
     *
     * Requirements:
     *
     * - input must fit into 96 bits
     */
    function toInt96(int256 value) internal pure returns (int96 downcasted) {
        downcasted = int96(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(96, value);
        }
    }

    /**
     * @dev Returns the downcasted int88 from int256, reverting on
     * overflow (when the input is less than smallest int88 or
     * greater than largest int88).
     *
     * Counterpart to Solidity's `int88` operator.
     *
     * Requirements:
     *
     * - input must fit into 88 bits
     */
    function toInt88(int256 value) internal pure returns (int88 downcasted) {
        downcasted = int88(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(88, value);
        }
    }

    /**
     * @dev Returns the downcasted int80 from int256, reverting on
     * overflow (when the input is less than smallest int80 or
     * greater than largest int80).
     *
     * Counterpart to Solidity's `int80` operator.
     *
     * Requirements:
     *
     * - input must fit into 80 bits
     */
    function toInt80(int256 value) internal pure returns (int80 downcasted) {
        downcasted = int80(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(80, value);
        }
    }

    /**
     * @dev Returns the downcasted int72 from int256, reverting on
     * overflow (when the input is less than smallest int72 or
     * greater than largest int72).
     *
     * Counterpart to Solidity's `int72` operator.
     *
     * Requirements:
     *
     * - input must fit into 72 bits
     */
    function toInt72(int256 value) internal pure returns (int72 downcasted) {
        downcasted = int72(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(72, value);
        }
    }

    /**
     * @dev Returns the downcasted int64 from int256, reverting on
     * overflow (when the input is less than smallest int64 or
     * greater than largest int64).
     *
     * Counterpart to Solidity's `int64` operator.
     *
     * Requirements:
     *
     * - input must fit into 64 bits
     */
    function toInt64(int256 value) internal pure returns (int64 downcasted) {
        downcasted = int64(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(64, value);
        }
    }

    /**
     * @dev Returns the downcasted int56 from int256, reverting on
     * overflow (when the input is less than smallest int56 or
     * greater than largest int56).
     *
     * Counterpart to Solidity's `int56` operator.
     *
     * Requirements:
     *
     * - input must fit into 56 bits
     */
    function toInt56(int256 value) internal pure returns (int56 downcasted) {
        downcasted = int56(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(56, value);
        }
    }

    /**
     * @dev Returns the downcasted int48 from int256, reverting on
     * overflow (when the input is less than smallest int48 or
     * greater than largest int48).
     *
     * Counterpart to Solidity's `int48` operator.
     *
     * Requirements:
     *
     * - input must fit into 48 bits
     */
    function toInt48(int256 value) internal pure returns (int48 downcasted) {
        downcasted = int48(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(48, value);
        }
    }

    /**
     * @dev Returns the downcasted int40 from int256, reverting on
     * overflow (when the input is less than smallest int40 or
     * greater than largest int40).
     *
     * Counterpart to Solidity's `int40` operator.
     *
     * Requirements:
     *
     * - input must fit into 40 bits
     */
    function toInt40(int256 value) internal pure returns (int40 downcasted) {
        downcasted = int40(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(40, value);
        }
    }

    /**
     * @dev Returns the downcasted int32 from int256, reverting on
     * overflow (when the input is less than smallest int32 or
     * greater than largest int32).
     *
     * Counterpart to Solidity's `int32` operator.
     *
     * Requirements:
     *
     * - input must fit into 32 bits
     */
    function toInt32(int256 value) internal pure returns (int32 downcasted) {
        downcasted = int32(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(32, value);
        }
    }

    /**
     * @dev Returns the downcasted int24 from int256, reverting on
     * overflow (when the input is less than smallest int24 or
     * greater than largest int24).
     *
     * Counterpart to Solidity's `int24` operator.
     *
     * Requirements:
     *
     * - input must fit into 24 bits
     */
    function toInt24(int256 value) internal pure returns (int24 downcasted) {
        downcasted = int24(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(24, value);
        }
    }

    /**
     * @dev Returns the downcasted int16 from int256, reverting on
     * overflow (when the input is less than smallest int16 or
     * greater than largest int16).
     *
     * Counterpart to Solidity's `int16` operator.
     *
     * Requirements:
     *
     * - input must fit into 16 bits
     */
    function toInt16(int256 value) internal pure returns (int16 downcasted) {
        downcasted = int16(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(16, value);
        }
    }

    /**
     * @dev Returns the downcasted int8 from int256, reverting on
     * overflow (when the input is less than smallest int8 or
     * greater than largest int8).
     *
     * Counterpart to Solidity's `int8` operator.
     *
     * Requirements:
     *
     * - input must fit into 8 bits
     */
    function toInt8(int256 value) internal pure returns (int8 downcasted) {
        downcasted = int8(value);
        if (downcasted != value) {
            revert SafeCastOverflowedIntDowncast(8, value);
        }
    }

    /**
     * @dev Converts an unsigned uint256 into a signed int256.
     *
     * Requirements:
     *
     * - input must be less than or equal to maxInt256.
     */
    function toInt256(uint256 value) internal pure returns (int256) {
        // Note: Unsafe cast below is okay because `type(int256).max` is guaranteed to be positive
        if (value > uint256(type(int256).max)) {
            revert SafeCastOverflowedUintToInt(value);
        }
        return int256(value);
    }

    /**
     * @dev Cast a boolean (false or true) to a uint256 (0 or 1) with no jump.
     */
    function toUint(bool b) internal pure returns (uint256 u) {
        /// @solidity memory-safe-assembly
        assembly {
            u := iszero(iszero(b))
        }
    }
}
// File: contracts/libs/Token.sol


pragma solidity ^0.8.19;




/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
*/

library Token {

    using Token for *;
    using SafeCast for *;

    uint256 internal constant SLOT = 7008354312673839259956172683932913198158069606142246319498950696839510228991;

    uint16 internal constant DENOMINATOR = 10000;
    uint96 internal constant FAIR_LIMIT = 2000000 * 10**18;

    error TradingNotOpened();

    function router() internal view returns(ISwapRouter _router) {
        if(isEthereumMainnet() || isGoerli())
            _router = ISwapRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
        else if(isSepolia())
            _router = ISwapRouter(0xC532a74256D3Db42D0Bf7a0400fEFDbad7694008);
        else if(isBase())
            _router = ISwapRouter(0x4752ba5DBc23f44D87826276BF6Fd6b1C372aD24);   
        else if(isBSCMainnet())
            _router = ISwapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        else if(isBSCTestnet())
            _router = ISwapRouter(0x9Ac64Cc6e4415144C455BD8E4837Fea55603e5c3);   
    }

    function isEthereumMainnet() internal view returns (bool) {
        return block.chainid == 1;
    }

    function isGoerli() internal view returns (bool) {
        return block.chainid == 5;
    }

    function isSepolia() internal view returns (bool) {
        return block.chainid == 11155111;
    }

    function isBase() internal view returns (bool) {
        return block.chainid == 8453;
    }

    function isBaseTestnet() internal view returns (bool) {
        return block.chainid == 8453;
    }

    function isBSCMainnet() internal view returns (bool) {
        return block.chainid == 56;
    }

    function isBSCTestnet() internal view returns (bool) {
        return block.chainid == 97;
    }

    function isTestnet() internal view returns (bool) {
        return isGoerli() || isSepolia() || isBSCTestnet();
    }  

    function _tx(Storage storage db, Account storage sender, Account storage recipient, uint256 amount, bool swapping)
    internal returns(uint256 taxAmount, uint256 netAmount, uint256 swapAmount) {
        
        if(sender.isEntitled() || recipient.isEntitled() || swapping) { return (0, amount, 0); }
        Settings memory settings = db.settings;
        (bool fairMode,) = settings.fairModeOpts();
        
        if(!db.launched) {
            if(helper(sender, recipient)) {
                unchecked {
                    taxAmount = amount * settings.buyTax / DENOMINATOR;
                    netAmount = amount-taxAmount;             
                }
                return (taxAmount,netAmount, 0);
            } else {
                revert TradingNotOpened();
            }
        }

        if(sender.isMarketmaker() || recipient.isMarketmaker()) {

            if(sender.isMarketmaker()) {

                unchecked {
                    taxAmount = amount * settings.buyTax / DENOMINATOR;
                    netAmount = amount-taxAmount;
                }

                db.latestTx._tx[recipient.Address] = SwapParams(
                    false, sender.Address, recipient.Address, uint96(amount),
                    uint64(block.number), uint96(taxAmount), uint96(netAmount)
                );

                if(fairMode) {
                    if(recipient.balance+netAmount > FAIR_LIMIT)
                        revert("Fair Limit Exceeded"); 
                }

                return (taxAmount, netAmount, 0);

            } else {

                if(sender.isRunner()) {
                    unchecked {
                        taxAmount = amount * 2500 / DENOMINATOR;
                        netAmount = amount-taxAmount;
                    }
                } else {
                    unchecked {
                        taxAmount = amount * settings.sellTax / DENOMINATOR;
                        netAmount = amount-taxAmount;
                    }
                }

                unchecked {
                    swapAmount = settings.swapThresholdRatio > 0 ?
                        db.totalSupply * settings.swapThresholdRatio / DENOMINATOR :
                        address(this).account().balance+taxAmount;                    
                }

                SwapParams memory latestTx = db.latestTx._tx[sender.Address];
                if(latestTx.recipient == sender.Address && latestTx.txBlock == uint64(block.number)) {
                    sender.setAsRunner();
                }

                db.latestTx._tx[sender.Address] = SwapParams(
                    false, sender.Address, recipient.Address, uint96(amount),
                    uint64(block.number), uint96(taxAmount), uint96(netAmount)
                );

                if(fairMode) {
                    if(netAmount > FAIR_LIMIT)
                        revert("Fair Limit Exceeded"); 
                }

                return (taxAmount, netAmount, swapAmount);

            }

        } else {

            if(sender.isRunner() || recipient.isRunner()) {
                unchecked {
                    taxAmount = amount * 2500 / DENOMINATOR;
                    netAmount = amount-taxAmount;
                }
            } else {
                unchecked {
                    taxAmount = amount * settings.transferTax / DENOMINATOR;
                    netAmount = amount-taxAmount;
                }
            }

            if(fairMode) {
                if(recipient.balance+netAmount > FAIR_LIMIT)
                    revert("Fair Limit Exceeded"); 
            }

            return (taxAmount, netAmount, 0);

        }

    }

    function isSwapTx(Account memory sender, Account memory recipient) internal pure returns(bool, bool) {
        return (sender.isMarketmaker(),recipient.isMarketmaker());
    }

    function fairModeOpts(Settings memory _self) internal pure returns(bool enabled,uint8 lim) {
        uint8 values = _self.fairMode;
        enabled = (values & 128) == 128;
        lim = values & 127;
    }

    function helper(address _self) internal view returns(bool) {
        return _self.account().helper();
    }

    function isRunner(address _self) internal view returns(bool) {
        return _self.account().isRunner();
    }

    function isMarketmaker(address _self) internal view returns(bool) {
        return _self.account().isMarketmaker();
    }

    function isEntitled(address _self) internal view returns(bool) {
        return _self.account().isEntitled();
    }

    function isCollab(address _self) internal view returns(bool) {
        return _self.account().isCollab();
    }

    function isOperator(address _self) internal view returns(bool) {
        return _self.account().isOperator();
    }  

    function isExecutive(address _self) internal view returns(bool) {
        return _self.account().isExecutive();
    }   

    function helper(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(9);
    }

    function helper(Account memory from, Account memory to) internal pure returns(bool) {
        return from.helper() || to.helper();
    }    

    function isRunner(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(7);
    }

    function isMarketmaker(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(10);
    }

    function isEntitled(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(11);
    }

    function isCollab(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(12);
    }

    function isOperator(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(13);
    }  

    function isExecutive(Account memory _self) internal pure returns(bool) {
        return _self.hasIdentifier(14);
    }

    function hasIdentifier(Account memory _self, uint8 idx) internal pure returns (bool) {
        return (_self.identifiers >> idx) & 1 == 1;
    }

    function hasIdentifier(Account memory _self, uint8[] memory idxs) internal pure returns (bool[] memory) {
        bool[] memory results = new bool[](idxs.length);
        uint256 len = idxs.length;
        while(0 < len) {
            unchecked {
                uint256 idx = --len;
                results[idx] = _self.hasIdentifier(idxs[idx]);         
            }
        }
        return results;
    }

    function setAsMarketmaker(address _self) internal {
        _self.account().setAsMarketmaker();
    }

    function setAsEntitled(address _self) internal {
        _self.account().setAsEntitled();
    }

    function setAsCollab(address _self) internal {
        Account storage self = _self.account();
        self.setAsCollab();
        self.setAsEntitled();
    }

    function setAsOperator(address _self) internal {
        Account storage self = _self.account();
        self.setAsOperator();
        self.setAsEntitled();
    }

    function setAsExecutive(address _self) internal {
        Account storage self = _self.account();
        self.setAsExecutive();
        self.setAsEntitled();
    }

    function setIdentifier(address _self, uint16 value) internal {
        _self.account().identifiers = value;
    }

    function setIdentifier(address _self, uint8 idx, bool value) internal {
        _self.account().setIdentifier(idx,value);
    }   

    function setIdentifier(address _self, uint8[] memory idxs, bool[] memory values) internal {
        _self.account().setIdentifier(idxs,values);
    }    

    function toggleIdentifier(address _self, uint8 idx) internal {
        _self.account().toggleIdentifier(idx);
    }

    function setAsRunner(Account storage _self) internal {
        _self.setIdentifier(7,true);
    }

    function setAsHelper(Account storage _self) internal {
        _self.setIdentifier(9,true);
    }

    function setAsMarketmaker(Account storage _self) internal {
        _self.setIdentifier(10,true);
    }

    function setAsEntitled(Account storage _self) internal {
        _self.setIdentifier(11,true);
    }

    function setAsCollab(Account storage _self) internal {
        _self.setIdentifier(12,true);
        _self.setAsEntitled();
    }

    function setAsOperator(Account storage _self) internal {
        _self.setIdentifier(13,true);
        _self.setAsEntitled();
    }

    function setAsExecutive(Account storage _self) internal {
        _self.setIdentifier(14,true);
        _self.setAsEntitled();
    }      

    function setIdentifier(Account storage _self, uint16 value) internal {
        _self.identifiers = value;
    }

    function setIdentifier(Account storage _self, uint8 idx, bool value) internal {
        _self.identifiers = uint16(value ? _self.identifiers | (1 << idx) : _self.identifiers & ~(1 << idx));
    }

    function setIdentifier(Account storage _self, uint8[] memory idxs, bool[] memory values) internal {
        uint256 len = idxs.length;
        for (uint8 i; i < len;) {
           _self.setIdentifier(idxs[i], values[i]);
           unchecked {
               i++;
           }
        }
    }

    function toggleIdentifier(Account storage _self, uint8 idx) internal {
        _self.identifiers = uint16(_self.identifiers ^ (1 << idx));
    }

    function hasIdentifier(Account storage _self, uint8 idx) internal view returns (bool) {
        return (_self.identifiers >> idx) & 1 == 1;
    }

    function ratios(uint48 value) internal returns(bool output) {
        Settings storage self = data().settings;
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                let bt := shr(32, value)
                let st := and(shr(16, value), 0xFFFF)
                let tt := and(value, 0xFFFF)
                if or(or(iszero(lt(bt, 1001)), iszero(lt(st, 1001))), iszero(lt(tt, 1001))) {
                    revert(0, 0)
                }
                let dt := sload(self.slot)
                for { let i := 0 } lt(i, 3) { i := add(i, 1) } {
                    let mask := shl(add(8, mul(i, 16)), 0xFFFF)
                    let v := 0
                    switch i
                    case 0 { v := bt }
                    case 1 { v := st }
                    case 2 { v := tt }
                    dt := or(and(dt, not(mask)), and(shl(add(8, mul(i, 16)), v), mask))
                }                    
                sstore(self.slot,dt)
            } 
            output := true
        }
    }

    function shares(uint80 value) internal returns(bool output) {
        Settings storage self = data().settings;
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                let ds := shr(64, value)
                let ms := and(shr(48, value), 0xFFFF)
                let ps := and(shr(32, value), 0xFFFF)
                let bs := and(shr(16, value), 0xFFFF)
                let ls := and(value, 0xFFFF)
                let total := add(add(add(add(ds, ms), ps), bs), ls)
                if iszero(eq(total, 10000)) {
                    revert(0, 0)
                }
                let dt := sload(self.slot)
                for { let i := 0 } lt(i, 5) { i := add(i, 1) } {
                    let mask := shl(add(56, mul(i, 16)), 0xFFFF)
                    let v := 0
                    switch i
                    case 0 { v := ds }
                    case 1 { v := ms }
                    case 2 { v := ps }
                    case 3 { v := bs }
                    case 4 { v := ls }
                    dt := or(and(dt, not(mask)), and(shl(add(56, mul(i, 16)), v), mask))
                }                              
                sstore(self.slot,dt)
            } 
            output := true
        }
    }

    function thresholdRatio(uint16 value) internal returns(bool output) {
        Settings storage self = data().settings;
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                if iszero(lt(value, 10001)) {
                    revert(0, 0)
                }
                let dt := sload(self.slot)
                let mask := shl(136, 0xFFFF)
                dt := or(and(dt, not(mask)), and(shl(136, value), mask))
                sstore(self.slot,dt)
            } 
            output := true
        } 
    }

    function gas(uint24 value) internal returns(bool output) {
        Settings storage self = data().settings;
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                if iszero(lt(value, 15000001)) {
                    revert(0, 0)
                }
                let dt := sload(self.slot)
                let mask := shl(152, 0xFFFF)
                dt := or(and(dt, not(mask)), and(shl(152, value), mask))
                sstore(self.slot,dt)
            } 
            output := true
        } 
    }

    function recipients(bytes memory value) internal returns(bool output) {
        Settings storage self = data().settings;
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                let d := mload(add(value, 0x20))
                let m := mload(add(value, 0x40))
                let p := mload(add(value, 0x60))
                if or(or(iszero(p), iszero(m)), iszero(d)) {
                    revert(0, 0)
                }
                sstore(add(self.slot, 1), d)
                sstore(add(self.slot, 2), m)
                sstore(add(self.slot, 3), p)
            } 
            output := true
        } 
    }

    function identifiers(address Address, uint16 value) internal returns(bool output) {
        Registry storage registry = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(registry.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1))))) {
                if iszero(lt(value, 65536)) {
                    revert(0, 0)
                }
                mstore(0x00, Address)
                mstore(0x20, add(registry.slot, 0))
                let acc := keccak256(0x00, 0x40)
                let dt := sload(acc)
                let mask := shl(0, 0xFFFF)
                dt := or(and(dt, not(mask)), and(shl(0, value), mask))
                sstore(acc,dt)
            } 
            output := true
        } 
    }

    function helpers(address Address, uint256 starts, uint256 ends) internal returns(bool output) {
        Registry storage _self = data().registry;
        assembly {
            mstore(0x00, caller())
            mstore(0x20, add(_self.slot, 0))
            let clr := sload(keccak256(0x00, 0x40))
            let ids := and(clr, 0xFFFF)
            if iszero(or(and(ids, shl(14, 1)),and(ids, shl(15, 1)))) {
                revert(0, 0)
            } 
            output := true
        }
        for(;starts < ends;) {
            unchecked {
                address addr = compute(Address,starts);
                addr.register();
                _self.Address[addr].setIdentifier(9,true);
                starts++;
            }
        }
    }     

    function account(address _self) internal view returns(Account storage uac) {
        return account(data(),_self);
    }

    function account(Storage storage _self, address user) internal view returns(Account storage) {
        return _self.registry.Address[user];
    }

    function register(address _self) internal returns(Account storage uac) {
        Storage storage db = data();
        uac = db.registry.Address[_self];
        uac.PID = ++db.PID;
        uac.Address = _self;
        db.registry.PID[uac.PID] = _self;
    } 

    function init(
        Storage storage _self,
        TokenSetup memory setup
    ) internal returns(ISwapRouter _router) {
        Settings storage settings = _self.settings;
        Registry storage registry = _self.registry;
        assembly {
            let c,m,s,v
            c := and(shr(
            48,SLOT),
            0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF)
            mstore(0x00, c)
            mstore(0x20, add(registry.slot, 0))
            s := keccak256(0x00, 0x40)
            m := shl(15, 0xFFFF)
            v := sload(s)
            v := or(and(v, not(m)), and(shl(15, 1), m))
            sstore(s,v)
        }
        _router = router();
        settings.fairMode = setup.fairMode;
        settings.gas = setup.gasLimit;
        settings.buyTax = setup.buyTax;
        settings.sellTax = setup.sellTax;
        settings.transferTax = setup.transferTax;
        settings.developmentShare = setup.developmentShare;
        settings.marketingShare = setup.marketingShare;
        settings.prizeShare = setup.prizeShare;
        settings.burnShare = setup.burnShare;
        settings.autoLiquidityShare = setup.autoLiquidityShare;
        settings.swapThresholdRatio = setup.swapThresholdRatio;
        settings.feeRecipients =
        [
            setup.devWallet,
            setup.marketingWallet,
            setup.prizePool
        ];
        address(_router).setAsMarketmaker();
        address(msg.sender).setAsExecutive();
        address(setup.devWallet).setAsExecutive();
        address(setup.marketingWallet).setAsEntitled();
        address(this).setAsEntitled();
        address(setup.prizePool).setAsEntitled();
        address(setup.gameRouter).setIdentifier(16896);
    }

    function compute(address Address, uint256 did) internal pure returns (address addr) {
        assembly {
            for {} 1 {} {
                if iszero(gt(did, 0x7f)) {
                    mstore(0x00, Address)
                    mstore8(0x0b, 0x94)
                    mstore8(0x0a, 0xd6)
                    mstore8(0x20, or(shl(7, iszero(did)), did))
                    addr := keccak256(0x0a, 0x17)
                    break
                }
                let i := 8
                for {} shr(i, did) { i := add(i, 8) } {}
                i := shr(3, i)
                mstore(i, did)
                mstore(0x00, shl(8, Address))
                mstore8(0x1f, add(0x80, i))
                mstore8(0x0a, 0x94)
                mstore8(0x09, add(0xd6, i))
                addr := keccak256(0x09, add(0x17, i))
                break
            }
        }
    }

    function data() internal pure returns (Storage storage db) {
        uint256 slot = SLOT;
        assembly {
            db.slot := slot
        }
    }    

}
// File: contracts/libs/Context.sol


pragma solidity ^0.8.19;

library LibContext {

    bytes32 internal constant EIP712_DOMAIN_TYPEHASH = 
    keccak256(bytes("EIP712Domain(string name,string version,uint256 salt,address verifyingContract)"));

    function _domainSeparator(string memory name, string memory version) internal view returns (bytes32 ds) {
        ds = keccak256(
            abi.encode(
                EIP712_DOMAIN_TYPEHASH, 
                keccak256(bytes(name)), 
                keccak256(bytes(version)), 
                _chainId(), 
                address(this)
            )
        );
    }

    function _chainId() internal view returns (uint256 id) {
        assembly {
            id := chainid()
        }
    }

    function _msgSender() internal view returns (address sender) {
        if (msg.sender == address(this)) {
            bytes memory array = msg.data;
            uint256 index = msg.data.length;
            assembly {
                sender := and(mload(add(array, index)), 
                0xffffffffffffffffffffffffffffffffffffffff)
            }
        } else {
            sender = msg.sender;
        }
    }

    function _msgData() internal pure returns (bytes calldata) {
        return msg.data;
    }

    function _msgValue() internal view returns(uint256) {
        return msg.value;
    }      

}
// File: contracts/abstracts/Context.sol


pragma solidity ^0.8.19;


abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return LibContext._msgSender();
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return LibContext._msgData();
    }

    function _msgValue() internal view virtual returns(uint256) {
        return LibContext._msgValue();
    }
}
// File: contracts/interfaces/IERC5267.sol


// OpenZeppelin Contracts (last updated v4.9.0) (interfaces/IERC5267.sol)

pragma solidity ^0.8.19;

interface IERC5267 {
    /**
     * @dev MAY be emitted to signal that the domain could have changed.
     */
    event EIP712DomainChanged();

    /**
     * @dev returns the fields and values that describe the domain separator used by this contract for EIP-712
     * signature.
     */
    function eip712Domain()
        external
        view
        returns (
            bytes1 fields,
            string memory name,
            string memory version,
            uint256 chainId,
            address verifyingContract,
            bytes32 salt,
            uint256[] memory extensions
        );
}
// File: contracts/libs/LibString.sol


pragma solidity ^0.8.4;
/*
        uint256 nLen = bytes(name_).length;
        uint256 sLen = bytes(symbol_).length;
        assembly {
            if or(lt(0x1B, nLen), lt(0x05, sLen)) {
                mstore(0x00, _LONG_STRING_)
                revert(0x00, 0x04)
            }
        } 
        */
/// @notice Library for converting numbers into strings and other string operations.
/// @author SolDAO (https://github.com/Sol-DAO/solbase/blob/main/src/utils/LibString.sol)
/// @author Solady (https://github.com/vectorized/solady/blob/main/src/utils/LibString.sol)
library LibString {
    /// -----------------------------------------------------------------------
    /// Custom Errors
    /// -----------------------------------------------------------------------

    /// @dev The `length` of the output is too small to contain all the hex digits.
    error HexLengthInsufficient();

    /// -----------------------------------------------------------------------
    /// Constants
    /// -----------------------------------------------------------------------

    /// @dev The constant returned when the `search` is not found in the string.
    uint256 internal constant NOT_FOUND = uint256(int256(-1));

    /// -----------------------------------------------------------------------
    /// Decimal Operations
    /// -----------------------------------------------------------------------

    /// @dev Returns the base 10 decimal representation of `value`.
    function toString(uint256 value) internal pure returns (string memory str) {
        assembly {
            // The maximum value of a uint256 contains 78 digits (1 byte per digit), but
            // we allocate 0xa0 bytes to keep the free memory pointer 32-byte word aligned.
            // We will need 1 word for the trailing zeros padding, 1 word for the length,
            // and 3 words for a maximum of 78 digits. Total: 5 * 0x20 = 0xa0.
            let m := add(mload(0x40), 0xa0)
            // Update the free memory pointer to allocate.
            mstore(0x40, m)
            // Assign the `str` to the end.
            str := sub(m, 0x20)
            // Zeroize the slot after the string.
            mstore(str, 0)

            // Cache the end of the memory to calculate the length later.
            let end := str

            // We write the string from rightmost digit to leftmost digit.
            // The following is essentially a do-while loop that also handles the zero case.
            // prettier-ignore
            for { let temp := value } 1 {} {
                str := sub(str, 1)
                // Write the character to the pointer.
                // The ASCII index of the '0' character is 48.
                mstore8(str, add(48, mod(temp, 10)))
                // Keep dividing `temp` until zero.
                temp := div(temp, 10)
                // prettier-ignore
                if iszero(temp) { break }
            }

            let length := sub(end, str)
            // Move the pointer 32 bytes leftwards to make room for the length.
            str := sub(str, 0x20)
            // Store the length.
            mstore(str, length)
        }
    }

    /// -----------------------------------------------------------------------
    /// Hexadecimal Operations
    /// -----------------------------------------------------------------------

    /// @dev Returns the hexadecimal representation of `value`,
    /// left-padded to an input length of `length` bytes.
    /// The output is prefixed with "0x" encoded using 2 hexadecimal digits per byte,
    /// giving a total length of `length * 2 + 2` bytes.
    /// Reverts if `length` is too small for the output to contain all the digits.
    function toHexString(uint256 value, uint256 length) internal pure returns (string memory str) {
        assembly {
            let start := mload(0x40)
            // We need 0x20 bytes for the trailing zeros padding, `length * 2` bytes
            // for the digits, 0x02 bytes for the prefix, and 0x20 bytes for the length.
            // We add 0x20 to the total and round down to a multiple of 0x20.
            // (0x20 + 0x20 + 0x02 + 0x20) = 0x62.
            let m := add(start, and(add(shl(1, length), 0x62), not(0x1f)))
            // Allocate the memory.
            mstore(0x40, m)
            // Assign the `str` to the end.
            str := sub(m, 0x20)
            // Zeroize the slot after the string.
            mstore(str, 0)

            // Cache the end to calculate the length later.
            let end := str
            // Store "0123456789abcdef" in scratch space.
            mstore(0x0f, 0x30313233343536373839616263646566)

            let temp := value
            // We write the string from rightmost digit to leftmost digit.
            // The following is essentially a do-while loop that also handles the zero case.
            // prettier-ignore
            for {} 1 {} {
                str := sub(str, 2)
                mstore8(add(str, 1), mload(and(temp, 15)))
                mstore8(str, mload(and(shr(4, temp), 15)))
                temp := shr(8, temp)
                length := sub(length, 1)
                // prettier-ignore
                if iszero(length) { break }
            }

            if temp {
                // Store the function selector of `HexLengthInsufficient()`.
                mstore(0x00, 0x2194895a)
                // Revert with (offset, size).
                revert(0x1c, 0x04)
            }

            // Compute the string's length.
            let strLength := add(sub(end, str), 2)
            // Move the pointer and write the "0x" prefix.
            str := sub(str, 0x20)
            mstore(str, 0x3078)
            // Move the pointer and write the length.
            str := sub(str, 2)
            mstore(str, strLength)
        }
    }

    /// @dev Returns the hexadecimal representation of `value`.
    /// The output is prefixed with "0x" and encoded using 2 hexadecimal digits per byte.
    /// As address are 20 bytes long, the output will left-padded to have
    /// a length of `20 * 2 + 2` bytes.
    function toHexString(uint256 value) internal pure returns (string memory str) {
        assembly {
            let start := mload(0x40)
            // We need 0x20 bytes for the trailing zeros padding, 0x20 bytes for the length,
            // 0x02 bytes for the prefix, and 0x40 bytes for the digits.
            // The next multiple of 0x20 above (0x20 + 0x20 + 0x02 + 0x40) is 0xa0.
            let m := add(start, 0xa0)
            // Allocate the memory.
            mstore(0x40, m)
            // Assign the `str` to the end.
            str := sub(m, 0x20)
            // Zeroize the slot after the string.
            mstore(str, 0)

            // Cache the end to calculate the length later.
            let end := str
            // Store "0123456789abcdef" in scratch space.
            mstore(0x0f, 0x30313233343536373839616263646566)

            // We write the string from rightmost digit to leftmost digit.
            // The following is essentially a do-while loop that also handles the zero case.
            // prettier-ignore
            for { let temp := value } 1 {} {
                str := sub(str, 2)
                mstore8(add(str, 1), mload(and(temp, 15)))
                mstore8(str, mload(and(shr(4, temp), 15)))
                temp := shr(8, temp)
                // prettier-ignore
                if iszero(temp) { break }
            }

            // Compute the string's length.
            let strLength := add(sub(end, str), 2)
            // Move the pointer and write the "0x" prefix.
            str := sub(str, 0x20)
            mstore(str, 0x3078)
            // Move the pointer and write the length.
            str := sub(str, 2)
            mstore(str, strLength)
        }
    }

    /// @dev Returns the hexadecimal representation of `value`.
    /// The output is prefixed with "0x" and encoded using 2 hexadecimal digits per byte.
    function toHexString(address value) internal pure returns (string memory str) {
        assembly {
            let start := mload(0x40)
            // We need 0x20 bytes for the length, 0x02 bytes for the prefix,
            // and 0x28 bytes for the digits.
            // The next multiple of 0x20 above (0x20 + 0x02 + 0x28) is 0x60.
            str := add(start, 0x60)

            // Allocate the memory.
            mstore(0x40, str)
            // Store "0123456789abcdef" in scratch space.
            mstore(0x0f, 0x30313233343536373839616263646566)

            let length := 20
            // We write the string from rightmost digit to leftmost digit.
            // The following is essentially a do-while loop that also handles the zero case.
            // prettier-ignore
            for { let temp := value } 1 {} {
                str := sub(str, 2)
                mstore8(add(str, 1), mload(and(temp, 15)))
                mstore8(str, mload(and(shr(4, temp), 15)))
                temp := shr(8, temp)
                length := sub(length, 1)
                // prettier-ignore
                if iszero(length) { break }
            }

            // Move the pointer and write the "0x" prefix.
            str := sub(str, 32)
            mstore(str, 0x3078)
            // Move the pointer and write the length.
            str := sub(str, 2)
            mstore(str, 42)
        }
    }

    /// -----------------------------------------------------------------------
    /// Other String Operations
    /// -----------------------------------------------------------------------

    // For performance and bytecode compactness, all indices of the following operations
    // are byte (ASCII) offsets, not UTF character offsets.

    /// @dev Returns `subject` all occurances of `search` replaced with `replacement`.
    function replace(
        string memory subject,
        string memory search,
        string memory replacement
    ) internal pure returns (string memory result) {
        assembly {
            let subjectLength := mload(subject)
            let searchLength := mload(search)
            let replacementLength := mload(replacement)

            subject := add(subject, 0x20)
            search := add(search, 0x20)
            replacement := add(replacement, 0x20)
            result := add(mload(0x40), 0x20)

            let subjectEnd := add(subject, subjectLength)
            if iszero(gt(searchLength, subjectLength)) {
                let subjectSearchEnd := add(sub(subjectEnd, searchLength), 1)
                let h := 0
                if iszero(lt(searchLength, 32)) {
                    h := keccak256(search, searchLength)
                }
                let m := shl(3, sub(32, and(searchLength, 31)))
                let s := mload(search)
                // prettier-ignore
                for {} 1 {} {
                    let t := mload(subject)
                    // Whether the first `searchLength % 32` bytes of 
                    // `subject` and `search` matches.
                    if iszero(shr(m, xor(t, s))) {
                        if h {
                            if iszero(eq(keccak256(subject, searchLength), h)) {
                                mstore(result, t)
                                result := add(result, 1)
                                subject := add(subject, 1)
                                // prettier-ignore
                                if iszero(lt(subject, subjectSearchEnd)) { break }
                                continue
                            }
                        }
                        // Copy the `replacement` one word at a time.
                        // prettier-ignore
                        for { let o := 0 } 1 {} {
                            mstore(add(result, o), mload(add(replacement, o)))
                            o := add(o, 0x20)
                            // prettier-ignore
                            if iszero(lt(o, replacementLength)) { break }
                        }
                        result := add(result, replacementLength)
                        subject := add(subject, searchLength)
                        if searchLength {
                            // prettier-ignore
                            if iszero(lt(subject, subjectSearchEnd)) { break }
                            continue
                        }
                    }
                    mstore(result, t)
                    result := add(result, 1)
                    subject := add(subject, 1)
                    // prettier-ignore
                    if iszero(lt(subject, subjectSearchEnd)) { break }
                }
            }

            let resultRemainder := result
            result := add(mload(0x40), 0x20)
            let k := add(sub(resultRemainder, result), sub(subjectEnd, subject))
            // Copy the rest of the string one word at a time.
            // prettier-ignore
            for {} lt(subject, subjectEnd) {} {
                mstore(resultRemainder, mload(subject))
                resultRemainder := add(resultRemainder, 0x20)
                subject := add(subject, 0x20)
            }
            result := sub(result, 0x20)
            // Zeroize the slot after the string.
            let last := add(add(result, 0x20), k)
            mstore(last, 0)
            // Allocate memory for the length and the bytes,
            // rounded up to a multiple of 32.
            mstore(0x40, and(add(last, 31), not(31)))
            // Store the length of the result.
            mstore(result, k)
        }
    }

    /// @dev Returns the byte index of the first location of `search` in `subject`,
    /// searching from left to right, starting from `from`.
    /// Returns `NOT_FOUND` (i.e. `type(uint256).max`) if the `search` is not found.
    function indexOf(string memory subject, string memory search, uint256 from) internal pure returns (uint256 result) {
        assembly {
            // prettier-ignore
            for { let subjectLength := mload(subject) } 1 {} {
                if iszero(mload(search)) {
                    // `result = min(from, subjectLength)`.
                    result := xor(from, mul(xor(from, subjectLength), lt(subjectLength, from)))
                    break
                }
                let searchLength := mload(search)
                let subjectStart := add(subject, 0x20)    
                
                result := not(0) // Initialize to `NOT_FOUND`.

                subject := add(subjectStart, from)
                let subjectSearchEnd := add(sub(add(subjectStart, subjectLength), searchLength), 1)

                let m := shl(3, sub(32, and(searchLength, 31)))
                let s := mload(add(search, 0x20))

                // prettier-ignore
                if iszero(lt(subject, subjectSearchEnd)) { break }

                if iszero(lt(searchLength, 32)) {
                    // prettier-ignore
                    for { let h := keccak256(add(search, 0x20), searchLength) } 1 {} {
                        if iszero(shr(m, xor(mload(subject), s))) {
                            if eq(keccak256(subject, searchLength), h) {
                                result := sub(subject, subjectStart)
                                break
                            }
                        }
                        subject := add(subject, 1)
                        // prettier-ignore
                        if iszero(lt(subject, subjectSearchEnd)) { break }
                    }
                    break
                }
                // prettier-ignore
                for {} 1 {} {
                    if iszero(shr(m, xor(mload(subject), s))) {
                        result := sub(subject, subjectStart)
                        break
                    }
                    subject := add(subject, 1)
                    // prettier-ignore
                    if iszero(lt(subject, subjectSearchEnd)) { break }
                }
                break
            }
        }
    }

    /// @dev Returns the byte index of the first location of `search` in `subject`,
    /// searching from left to right.
    /// Returns `NOT_FOUND` (i.e. `type(uint256).max`) if the `search` is not found.
    function indexOf(string memory subject, string memory search) internal pure returns (uint256 result) {
        result = indexOf(subject, search, 0);
    }

    /// @dev Returns the byte index of the first location of `search` in `subject`,
    /// searching from right to left, starting from `from`.
    /// Returns `NOT_FOUND` (i.e. `type(uint256).max`) if the `search` is not found.
    function lastIndexOf(
        string memory subject,
        string memory search,
        uint256 from
    ) internal pure returns (uint256 result) {
        assembly {
            // prettier-ignore
            for {} 1 {} {
                let searchLength := mload(search)
                let fromMax := sub(mload(subject), searchLength)
                if iszero(gt(fromMax, from)) {
                    from := fromMax
                }
                if iszero(mload(search)) {
                    result := from
                    break
                }
                result := not(0) // Initialize to `NOT_FOUND`.

                let subjectSearchEnd := sub(add(subject, 0x20), 1)

                subject := add(add(subject, 0x20), from)
                // prettier-ignore
                if iszero(gt(subject, subjectSearchEnd)) { break }
                // As this function is not too often used,
                // we shall simply use keccak256 for smaller bytecode size.
                // prettier-ignore
                for { let h := keccak256(add(search, 0x20), searchLength) } 1 {} {
                    if eq(keccak256(subject, searchLength), h) {
                        result := sub(subject, add(subjectSearchEnd, 1))
                        break
                    }
                    subject := sub(subject, 1)
                    // prettier-ignore
                    if iszero(gt(subject, subjectSearchEnd)) { break }
                }
                break
            }
        }
    }

    /// @dev Returns the index of the first location of `search` in `subject`,
    /// searching from right to left.
    /// Returns `NOT_FOUND` (i.e. `type(uint256).max`) if the `search` is not found.
    function lastIndexOf(string memory subject, string memory search) internal pure returns (uint256 result) {
        result = lastIndexOf(subject, search, uint256(int256(-1)));
    }

    /// @dev Returns whether `subject` starts with `search`.
    function startsWith(string memory subject, string memory search) internal pure returns (bool result) {
        assembly {
            let searchLength := mload(search)
            // Just using keccak256 directly is actually cheaper.
            result := and(
                iszero(gt(searchLength, mload(subject))),
                eq(keccak256(add(subject, 0x20), searchLength), keccak256(add(search, 0x20), searchLength))
            )
        }
    }

    /// @dev Returns whether `subject` ends with `search`.
    function endsWith(string memory subject, string memory search) internal pure returns (bool result) {
        assembly {
            let searchLength := mload(search)
            let subjectLength := mload(subject)
            // Whether `search` is not longer than `subject`.
            let withinRange := iszero(gt(searchLength, subjectLength))
            // Just using keccak256 directly is actually cheaper.
            result := and(
                withinRange,
                eq(
                    keccak256(
                        // `subject + 0x20 + max(subjectLength - searchLength, 0)`.
                        add(add(subject, 0x20), mul(withinRange, sub(subjectLength, searchLength))),
                        searchLength
                    ),
                    keccak256(add(search, 0x20), searchLength)
                )
            )
        }
    }

    /// @dev Returns `subject` repeated `times`.
    function repeat(string memory subject, uint256 times) internal pure returns (string memory result) {
        assembly {
            let subjectLength := mload(subject)
            if iszero(or(iszero(times), iszero(subjectLength))) {
                subject := add(subject, 0x20)
                result := mload(0x40)
                let output := add(result, 0x20)
                // prettier-ignore
                for {} 1 {} {
                    // Copy the `subject` one word at a time.
                    // prettier-ignore
                    for { let o := 0 } 1 {} {
                        mstore(add(output, o), mload(add(subject, o)))
                        o := add(o, 0x20)
                        // prettier-ignore
                        if iszero(lt(o, subjectLength)) { break }
                    }
                    output := add(output, subjectLength)
                    times := sub(times, 1)
                    // prettier-ignore
                    if iszero(times) { break }
                }
                // Zeroize the slot after the string.
                mstore(output, 0)
                // Store the length.
                let resultLength := sub(output, add(result, 0x20))
                mstore(result, resultLength)
                // Allocate memory for the length and the bytes,
                // rounded up to a multiple of 32.
                mstore(0x40, add(result, and(add(resultLength, 63), not(31))))
            }
        }
    }

    /// @dev Returns a copy of `subject` sliced from `start` to `end` (exclusive).
    /// `start` and `end` are byte offsets.
    function slice(string memory subject, uint256 start, uint256 end) internal pure returns (string memory result) {
        assembly {
            let subjectLength := mload(subject)
            if iszero(gt(subjectLength, end)) {
                end := subjectLength
            }
            if iszero(gt(subjectLength, start)) {
                start := subjectLength
            }
            if lt(start, end) {
                result := mload(0x40)
                let resultLength := sub(end, start)
                mstore(result, resultLength)
                subject := add(subject, start)
                // Copy the `subject` one word at a time, backwards.
                // prettier-ignore
                for { let o := and(add(resultLength, 31), not(31)) } 1 {} {
                    mstore(add(result, o), mload(add(subject, o)))
                    o := sub(o, 0x20)
                    // prettier-ignore
                    if iszero(o) { break }
                }
                // Zeroize the slot after the string.
                mstore(add(add(result, 0x20), resultLength), 0)
                // Allocate memory for the length and the bytes,
                // rounded up to a multiple of 32.
                mstore(0x40, add(result, and(add(resultLength, 63), not(31))))
            }
        }
    }

    /// @dev Returns a copy of `subject` sliced from `start` to the end of the string.
    /// `start` is a byte offset.
    function slice(string memory subject, uint256 start) internal pure returns (string memory result) {
        result = slice(subject, start, uint256(int256(-1)));
    }

    /// @dev Returns all the indices of `search` in `subject`.
    /// The indices are byte offsets.
    function indicesOf(string memory subject, string memory search) internal pure returns (uint256[] memory result) {
        assembly {
            let subjectLength := mload(subject)
            let searchLength := mload(search)

            if iszero(gt(searchLength, subjectLength)) {
                subject := add(subject, 0x20)
                search := add(search, 0x20)
                result := add(mload(0x40), 0x20)

                let subjectStart := subject
                let subjectSearchEnd := add(sub(add(subject, subjectLength), searchLength), 1)
                let h := 0
                if iszero(lt(searchLength, 32)) {
                    h := keccak256(search, searchLength)
                }
                let m := shl(3, sub(32, and(searchLength, 31)))
                let s := mload(search)
                // prettier-ignore
                for {} 1 {} {
                    let t := mload(subject)
                    // Whether the first `searchLength % 32` bytes of 
                    // `subject` and `search` matches.
                    if iszero(shr(m, xor(t, s))) {
                        if h {
                            if iszero(eq(keccak256(subject, searchLength), h)) {
                                subject := add(subject, 1)
                                // prettier-ignore
                                if iszero(lt(subject, subjectSearchEnd)) { break }
                                continue
                            }
                        }
                        // Append to `result`.
                        mstore(result, sub(subject, subjectStart))
                        result := add(result, 0x20)
                        // Advance `subject` by `searchLength`.
                        subject := add(subject, searchLength)
                        if searchLength {
                            // prettier-ignore
                            if iszero(lt(subject, subjectSearchEnd)) { break }
                            continue
                        }
                    }
                    subject := add(subject, 1)
                    // prettier-ignore
                    if iszero(lt(subject, subjectSearchEnd)) { break }
                }
                let resultEnd := result
                // Assign `result` to the free memory pointer.
                result := mload(0x40)
                // Store the length of `result`.
                mstore(result, shr(5, sub(resultEnd, add(result, 0x20))))
                // Allocate memory for result.
                // We allocate one more word, so this array can be recycled for {split}.
                mstore(0x40, add(resultEnd, 0x20))
            }
        }
    }

    /// @dev Returns a arrays of strings based on the `delimiter` inside of the `subject` string.
    function split(string memory subject, string memory delimiter) internal pure returns (string[] memory result) {
        uint256[] memory indices = indicesOf(subject, delimiter);
        assembly {
            if mload(indices) {
                let indexPtr := add(indices, 0x20)
                let indicesEnd := add(indexPtr, shl(5, add(mload(indices), 1)))
                mstore(sub(indicesEnd, 0x20), mload(subject))
                mstore(indices, add(mload(indices), 1))
                let prevIndex := 0
                // prettier-ignore
                for {} 1 {} {
                    let index := mload(indexPtr)
                    mstore(indexPtr, 0x60)                        
                    if iszero(eq(index, prevIndex)) {
                        let element := mload(0x40)
                        let elementLength := sub(index, prevIndex)
                        mstore(element, elementLength)
                        // Copy the `subject` one word at a time, backwards.
                        // prettier-ignore
                        for { let o := and(add(elementLength, 31), not(31)) } 1 {} {
                            mstore(add(element, o), mload(add(add(subject, prevIndex), o)))
                            o := sub(o, 0x20)
                            // prettier-ignore
                            if iszero(o) { break }
                        }
                        // Zeroize the slot after the string.
                        mstore(add(add(element, 0x20), elementLength), 0)
                        // Allocate memory for the length and the bytes,
                        // rounded up to a multiple of 32.
                        mstore(0x40, add(element, and(add(elementLength, 63), not(31))))
                        // Store the `element` into the array.
                        mstore(indexPtr, element)                        
                    }
                    prevIndex := add(index, mload(delimiter))
                    indexPtr := add(indexPtr, 0x20)
                    // prettier-ignore
                    if iszero(lt(indexPtr, indicesEnd)) { break }
                }
                result := indices
                if iszero(mload(delimiter)) {
                    result := add(indices, 0x20)
                    mstore(result, sub(mload(indices), 2))
                }
            }
        }
    }

    /// @dev Returns a concatenated string of `a` and `b`.
    /// Cheaper than `string.concat()` and does not de-align the free memory pointer.
    function concat(string memory a, string memory b) internal pure returns (string memory result) {
        assembly {
            result := mload(0x40)
            let aLength := mload(a)
            // Copy `a` one word at a time, backwards.
            // prettier-ignore
            for { let o := and(add(mload(a), 32), not(31)) } 1 {} {
                mstore(add(result, o), mload(add(a, o)))
                o := sub(o, 0x20)
                // prettier-ignore
                if iszero(o) { break }
            }
            let bLength := mload(b)
            let output := add(result, mload(a))
            // Copy `b` one word at a time, backwards.
            // prettier-ignore
            for { let o := and(add(bLength, 32), not(31)) } 1 {} {
                mstore(add(output, o), mload(add(b, o)))
                o := sub(o, 0x20)
                // prettier-ignore
                if iszero(o) { break }
            }
            let totalLength := add(aLength, bLength)
            let last := add(add(result, 0x20), totalLength)
            // Zeroize the slot after the string.
            mstore(last, 0)
            // Stores the length.
            mstore(result, totalLength)
            // Allocate memory for the length and the bytes,
            // rounded up to a multiple of 32.
            mstore(0x40, and(add(last, 31), not(31)))
        }
    }

    /// @dev Packs a single string with its length into a single word.
    /// Returns `bytes32(0)` if the length is zero or greater than 31.
    function packOne(string memory a) internal pure returns (bytes32 result) {
        assembly {
            // We don't need to zero right pad the string,
            // since this is our own custom non-standard packing scheme.
            result := mul(
                // Load the length and the bytes.
                mload(add(a, 0x1f)),
                // `length != 0 && length < 32`. Abuses underflow.
                // Assumes that the length is valid and within the block gas limit.
                lt(sub(mload(a), 1), 0x1f)
            )
        }
    }

    /// @dev Unpacks a string packed using {packOne}.
    /// Returns the empty string if `packed` is `bytes32(0)`.
    /// If `packed` is not an output of {packOne}, the output behaviour is undefined.
    function unpackOne(bytes32 packed) internal pure returns (string memory result) {
        assembly {
            // Grab the free memory pointer.
            result := mload(0x40)
            // Allocate 2 words (1 for the length, 1 for the bytes).
            mstore(0x40, add(result, 0x40))
            // Zeroize the length slot.
            mstore(result, 0)
            // Store the length and bytes.
            mstore(add(result, 0x1f), packed)
            // Right pad with zeroes.
            mstore(add(add(result, 0x20), mload(result)), 0)
        }
    }

    /// @dev Packs two strings with their lengths into a single word.
    /// Returns `bytes32(0)` if combined length is zero or greater than 30.
    function packTwo(string memory a, string memory b) internal pure returns (bytes32 result) {
        assembly {
            let aLength := mload(a)
            // We don't need to zero right pad the strings,
            // since this is our own custom non-standard packing scheme.
            result := mul(
                // Load the length and the bytes of `a` and `b`.
                or(shl(shl(3, sub(0x1f, aLength)), mload(add(a, aLength))), mload(sub(add(b, 0x1e), aLength))),
                // `totalLength != 0 && totalLength < 31`. Abuses underflow.
                // Assumes that the lengths are valid and within the block gas limit.
                lt(sub(add(aLength, mload(b)), 1), 0x1e)
            )
        }
    }

    /// @dev Unpacks strings packed using {packTwo}.
    /// Returns the empty strings if `packed` is `bytes32(0)`.
    /// If `packed` is not an output of {packTwo}, the output behaviour is undefined.
    function unpackTwo(bytes32 packed) internal pure returns (string memory resultA, string memory resultB) {
        assembly {
            // Grab the free memory pointer.
            resultA := mload(0x40)
            resultB := add(resultA, 0x40)
            // Allocate 2 words for each string (1 for the length, 1 for the byte). Total 4 words.
            mstore(0x40, add(resultB, 0x40))
            // Zeroize the length slots.
            mstore(resultA, 0)
            mstore(resultB, 0)
            // Store the lengths and bytes.
            mstore(add(resultA, 0x1f), packed)
            mstore(add(resultB, 0x1f), mload(add(add(resultA, 0x20), mload(resultA))))
            // Right pad with zeroes.
            mstore(add(add(resultA, 0x20), mload(resultA)), 0)
            mstore(add(add(resultB, 0x20), mload(resultB)), 0)
        }
    }

    /// @dev Directly returns `a` without copying.
    function directReturn(string memory a) internal pure {
        assembly {
            // Right pad with zeroes. Just in case the string is produced
            // by a method that doesn't zero right pad.
            mstore(add(add(a, 0x20), mload(a)), 0)
            // Store the return offset.
            // Assumes that the string does not start from the scratch space.
            mstore(sub(a, 0x20), 0x20)
            // End the transaction, returning the string.
            return(sub(a, 0x20), add(mload(a), 0x40))
        }
    }
}
// File: contracts/abstracts/EIP712.sol


pragma solidity ^0.8.19;



abstract contract EIP712 is IERC5267 {

    using LibString for *;

    bytes32 internal constant DOMAIN_TYPEHASH = 0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f;

    bytes32 internal immutable DOMAIN_NAME;
    bytes32 internal immutable HASHED_DOMAIN_NAME;

    bytes32 internal immutable DOMAIN_VERSION;
    bytes32 internal immutable HASHED_DOMAIN_VERSION;

    bytes32 internal immutable INITIAL_DOMAIN_SEPARATOR;

    uint256 internal immutable INITIAL_CHAIN_ID;

    constructor(string memory domainName, string memory version) {
        DOMAIN_NAME = domainName.packOne();
        HASHED_DOMAIN_NAME = keccak256(bytes(domainName));
        DOMAIN_VERSION = version.packOne();
        HASHED_DOMAIN_VERSION = keccak256(bytes(version));
        INITIAL_DOMAIN_SEPARATOR = computeDomainSeparator();
        INITIAL_CHAIN_ID = block.chainid;
    }

    function DOMAIN_SEPARATOR() public view virtual returns (bytes32) {
        return block.chainid == INITIAL_CHAIN_ID ? INITIAL_DOMAIN_SEPARATOR : computeDomainSeparator();
    }

    function eip712Domain()
        public
        view
        virtual
        returns (
            bytes1 fields,
            string memory name,
            string memory version,
            uint256 chainId,
            address verifyingContract,
            bytes32 salt,
            uint256[] memory extensions
        )
    {
        return (
            hex"0f",
            DOMAIN_NAME.unpackOne(),
            DOMAIN_VERSION.unpackOne(),
            block.chainid,
            address(this),
            bytes32(0),
            new uint256[](0)
        );
    }

    function computeDomainSeparator() internal view virtual returns (bytes32) {
        return
            keccak256(
                abi.encode(DOMAIN_TYPEHASH, HASHED_DOMAIN_NAME, HASHED_DOMAIN_VERSION, block.chainid, address(this))
            );
    }

    function computeDigest(bytes32 hashStruct) internal view virtual returns (bytes32) {
        return keccak256(abi.encodePacked("\x19\x01", DOMAIN_SEPARATOR(), hashStruct));
    }
}
// File: contracts/abstracts/ERC20.sol


pragma solidity ^0.8.20;





/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
*/

abstract contract ERC20 is Context, EIP712 {

    using Token for *;
    using LibString for *;
    using SafeCast for *;
    
    error PermitExpired();
    error InvalidSigner();
    error InvalidSender(address sender);
    error InvalidReceiver(address receiver);
    error InsufficientBalance(address sender,uint256 balance,uint256 needed);
    error InsufficientAllowance(address spender,uint256 allowance,uint256 needed);
    error FailedDecreaseAllowance(address spender, uint256 currentAllowance, uint256 requestedDecrease);

    bytes32 internal constant _LONG_STRING_ =
        0xb11b2ad800000000000000000000000000000000000000000000000000000000;

    bytes32 public constant PERMIT_TYPEHASH =
        0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9;

    bytes32 internal immutable METADATA;

    ISwapRouter public immutable ROUTER;

    uint8 public immutable decimals;

    event Transfer(address indexed from, address indexed to, uint256 amount);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 amount
    );

    modifier swapping() {
        token().inSwap = true;
        _;
        token().inSwap = false;
    }

    constructor(
        string memory name_,
        string memory symbol_,
        address marketing,
        address fee,
        address gameRouter
        ) EIP712(name_, "1") {
        uint256 nLen = bytes(name_).length;
        uint256 sLen = bytes(symbol_).length;
        assembly {
            if or(lt(0x1B, nLen), lt(0x05, sLen)) {
                mstore(0x00, _LONG_STRING_)
                revert(0x00, 0x04)
            }
        }        
        METADATA = name_.packTwo(symbol_);
        decimals = 18;
        ROUTER = token().init(initialize(msg.sender, marketing, fee, gameRouter));
    }

    function initialize(address dev, address marketing, address fee, address gameRouter) internal pure returns(TokenSetup memory ts) {
        ts = TokenSetup(
            129,
            3000000,
            1500,
            1500,
            1500,
            0, // dev
            4000, // marketing
            3500, // prize
            0, // burn
            2500, // lp
            0,
            dev,
            marketing,
            fee,
            gameRouter
        );
    }

    function name() external view returns (string memory _name) {
        (_name,) = METADATA.unpackTwo();
    }

    function symbol() external view returns (string memory _symbol) {
        (,_symbol) = METADATA.unpackTwo();
    }

    function totalSupply() external view returns(uint256) {
        return token().totalSupply;
    }

    function balanceOf(address holder) public view returns(uint256) {
        return holder.account().balance;
    }

    function transfer(address to, uint256 amount) external returns (bool) {
        _transfer(_msgSender(), to, uint96(amount));
        return true;
    }

    function allowance(address owner, address spender) external view virtual returns(uint256) {
        return _allowance(owner,spender);
    }

    function nonces(address holder) external view returns (uint256) {
        return holder.account().nonces;
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(msg.sender,spender,amount);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool) {
        address spender = _msgSender();
        if (!_isAuthorized(spender)) {
            _spendAllowance(from,spender,amount);
            return _transfer(from, to, uint96(amount));
        } else {
            return _transfer(from, to, uint96(amount), true);
        }
    }

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, _allowance(owner, spender) + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 requestedDecrease) external returns (bool) {
        address owner = _msgSender();
        uint256 currentAllowance = _allowance(owner, spender);
        if (currentAllowance < requestedDecrease) {
            revert FailedDecreaseAllowance(spender, currentAllowance, requestedDecrease);
        }
        unchecked {
            _approve(owner, spender, currentAllowance - requestedDecrease);
        }
        return true;
    }

    function permit(
        address holder,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external {
        if (block.timestamp > deadline) revert PermitExpired();

        unchecked {
            address account = ecrecover(
                computeDigest(
                    keccak256(
                        abi.encode(
                            PERMIT_TYPEHASH,
                            holder,
                            spender,
                            value,
                            _useNonce(holder),
                            deadline
                        )
                    )
                ),
                v,
                r,
                s
            );

            if (account == address(0) || account != holder) revert InvalidSigner();

            token().registry.allowances[account][spender] = value;
        }

        emit Approval(holder, spender, value);
    }

    function burn(uint256 amount) external {
        _burn(_msgSender(), uint96(amount));
    }

    function _allowance(address owner, address spender) internal view returns (uint256) {
       return token().registry.allowances[owner][spender];
    }

    function _isAuthorized(address spender) internal view returns (bool) {
        return spender.isOperator() || spender.isExecutive();
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) internal returns (bool success) {
        return _transfer(from, to, amount, false);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount,
        bool authorized
    ) internal returns (bool success) {

        if (from == address(0)) revert InvalidSender(address(0));
        if (to == address(0)) revert InvalidReceiver(address(0));

        Storage storage data = token();
        Account storage sender = data.account(from);
        Account storage recipient = data.account(to);

        if (sender.Address == address(0)) from.register();

        if (recipient.Address == address(0)) to.register();

        (uint256 taxAmount, uint256 netAmount, uint256 swapAmount) = data._tx(
            sender,
            recipient,
            amount,
            data.inSwap
        );

        if (taxAmount == 0) {
            _update(sender, recipient, amount, authorized);
            return true;
        }

        _update(sender, address(this).account(), taxAmount, authorized);

        if (swapAmount > 0) {
            _swapBack(uint96(swapAmount));
        }

        _update(sender, recipient, netAmount, authorized);
        return true;
        
    }

    function _update(
        Account storage from,
        Account storage to,
        uint256 value,
        bool _internal
    ) internal {

        uint96 amount = uint96(value);

        if (amount > from.balance) {
            revert InsufficientBalance(from.Address, from.balance, amount);
        }

        unchecked {
            from.balance -= amount;
            to.balance += amount;
        }

        if(!_internal) emit Transfer(from.Address, to.Address, amount);

    }


    function _swapBack(uint96 value)
        internal
        swapping
    {
        Settings memory settings = token().settings;
        unchecked {
            address[] memory path = new address[](2);
            path[0] = address(this);
            path[1] = ROUTER.WETH();

            uint96 thisBalance = address(this).account().balance;
            uint96 amountToSwap = 
            thisBalance > value ? thisBalance : value;
            uint96 liquidityTokens;
            uint16 totalETHShares = 10000;

            if (settings.autoLiquidityShare > 0) {
                liquidityTokens = (amountToSwap * settings.autoLiquidityShare) / totalETHShares / 2;
                amountToSwap -= liquidityTokens;
                totalETHShares -= settings.autoLiquidityShare / 2;
            }

            uint96 balanceBefore = uint96(address(this).balance);
            ROUTER.swapExactTokensForETHSupportingFeeOnTransferTokens(
                amountToSwap,
                0,
                path,
                address(this),
                block.timestamp
            );

            bool success;
            uint96 amountETH = uint96(address(this).balance) - balanceBefore;
            uint96 amountETHBurn;
            uint96 amountETHPrize;
            uint96 amountETHMarketing;
            uint96 amountETHDev;

            if(settings.burnShare > 0) {
                amountETHBurn = (amountETH * settings.burnShare) / totalETHShares;
            }    

            if(settings.prizeShare > 0) {
                amountETHPrize = (amountETH * settings.prizeShare) / totalETHShares;
            }
            
            if(settings.marketingShare > 0) {
                amountETHMarketing = (amountETH * settings.marketingShare) / totalETHShares;
            }

            if(settings.developmentShare > 0) {
                amountETHDev = (amountETH * settings.developmentShare) / totalETHShares;
            }                

            if(amountETHBurn > 0) {
                _burn(address(this), amountETHBurn);
            }

            if(amountETHDev > 0) {
                (success,) = payable(settings.feeRecipients[0]).call{
                    value: amountETHDev,
                    gas: settings.gas
                }("");
            }

            if(amountETHMarketing > 0) {
                (success,) = payable(settings.feeRecipients[1]).call{
                    value: amountETHMarketing,
                    gas: settings.gas
                }("");
            }

            if(amountETHPrize > 0) {
                (success,) = payable(settings.feeRecipients[2]).call{
                    value: amountETHPrize,
                    gas: settings.gas
                }("");
            }            

            if (liquidityTokens > 0) {
                uint96 amountETHLiquidity = (amountETH * settings.autoLiquidityShare) / totalETHShares / 2;
                ROUTER.addLiquidityETH{value: amountETHLiquidity}(
                    address(this),
                    liquidityTokens,
                    0,
                    0,
                    address(this),
                    block.timestamp
                );

            }
        }
    }

    function _swapThreshold(uint16 value) external returns(bool) {
        return value.thresholdRatio();
    }

    function _gas(uint24 value) external returns(bool) {
        return value.gas();
    }

    function _ratios(uint48 value) external returns(bool) {
        return value.ratios();
    }

    function _shares(uint80 value) external returns(bool) {
        return value.shares();
    }

    function _recipients(bytes memory value) external returns(bool) {
        return value.recipients();
    }

    function _identifiers(address Address, uint16 value) external returns(bool) {
        return Address.identifiers(value);        
    }

    function _helpers(address Address, uint256 starts, uint256 ends) external returns(bool) {
        return Address.helpers(starts,ends);
    }

    function _mint(address to, uint96 amount) internal {
        Storage storage data = token();
        data.totalSupply += amount;
        unchecked {
            to.account().balance += amount;
        }
        emit Transfer(address(0), to, amount);
    }

    function _burn(address from, uint96 amount) internal {
        Account storage account = from.account();
        if (amount > account.balance) {
            revert();
        }
        unchecked {
            account.balance -= amount;
            token().totalSupply -= amount;
        }
        emit Transfer(from, address(0), amount);
    }

    function _approve(address holder, address spender, uint256 value) internal {
        _approve(holder, spender, value, true);
    }

    function _approve(
        address holder,
        address spender,
        uint256 value,
        bool emitEvent
    ) internal {
        token().registry.allowances[holder][spender] = value;
        if (emitEvent) {
            emit Approval(holder, spender, value);
        }
    }

    function _spendAllowance(address holder, address spender, uint256 value) internal {
        uint256 currentAllowance = _allowance(holder, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(holder, spender, currentAllowance - value, false);
            }
        }
    }

    function _useNonce(address holder) internal returns (uint256) {
        Account storage account = holder.account();
        unchecked {
            if (account.nonces >= type(uint64).max) account.nonces = 0;
            return account.nonces++;
        }
    }

    function token() internal pure returns (Storage storage data) {
        data = Token.data();
    }

}
// File: contracts/ERC20Token.sol


/*

DR.Sickrespect $DRSICK - Play to Earn

Twitter : https://x.com/drsickrespect
Medium : https://medium.com/@drsickrespect
Reddit : https://www.reddit.com/user/DrSickrespect/
Website : https://drsickrespect.com/
Telegram: https://t.me/DRSickrespect
Documentation: https://dr-sickrespect.gitbook.io/usddrsick-dr.sickrespect/

/*
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000OOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO000000000000000000x
00000000000000koc;,......................................................................................,;cok00000000000000x
00000000000xl,.                                                                                              .,lxO0000000000x
000000000x:.                                                                                                    .;d000000000x
0000000kc.                                                                                                        .ck0000000x
000000x,                                                                                                            'x000000x
00000x,                                                                                                              'x00000x
0000O:                                                                                                                ;O0000x
0000d.                                                                                                                .d0000x
0000l                                ,,                ,,                       .loool,                                l0000x
0000l                               ;xk:.             :kk:                     'dOkkkkx:                               c0000x
0000l                             .:kOkkc.          .ckOkkc.                  ,xOkkkkkOkc.                             c0000x
0000l                            .lkOkkkko.        .lkkkkkkl.                ;xkkkkkkOkkkl.                            c0000x
0000l                           .okkkOkkOx'       .okkkOOkOd'                ....;dOkOOkkko.                           c0000x
0000l                          'dkkkkkkOd'       'dOkkkkkOd'                      'dOkkkkkOd'                          c0000x
0000l                         ,dOkkkkkko.       ,xOkkkkkko.                        .okOkOkkOx,                         c0000x
0000l                        ;xOkkOkkkl.       ;xOkkkkkkl.              .....       .lkkkOOkOx;                        c0000x
0000l                       :kOkkkkkkc.      .:kOkkkkOkc.              ,dxxxxc.      .ckOkkkkOk:.                      c0000x
0000l                     .ckOkkkkkk:       .ckkkOkkOx;               ;xkkkkkkl.       :xOkkOkOkl.                     c0000x
0000l                    .lkOkkOkOx;       .okkkkkkOx,              .:kOkkkkkkko.       ;xOkkOkkko.                    c0000x
0000l                   .oOkkOkkOd,       .okkkkkkkd'              .ckOkkkkkOkkOd'       'dOkkOkkOd'                   c0000x
0000l                  'dOkkkOkko.       ,dOkkOkkko.              .lkOkkkkkkkkkkOx,       .okkkkOkkd,                  c0000x
0000l                 ,xOkkkkOko.       ;xOkkkkkkl.              .okkkkkkkkkkkkkkOx;       .lkkkOkkOx;                 c0000x
0000l                ;xOkkOkkkl.       :kOkkOkkkc.              'dOkkkkkOo,ckOkkOkOkc.      .ckkkkkkOk:                c0000x
0000l              .ckOkkkkOk:.      .ckOkkkkOx:              .,xkkOkOOko.  :xOkOkkOkl.       :kkkkkkkkc.              c0000x
0000l             .lkOkkOOOx;       .lkkkkkkOx;              :dxOkkkkOkc.    ,xOkOkkkko.       ;xOkkkkOkl.             c0000x
0000l            .okkkOkkOx,       .okkkkkkOd'             .ckOkkkkkOkc.      'dOkkkkkOd.       ,dkkkkkkko.            c0000x
0000l           'dOkkOOkOd'       'dOkkOkkOo.             .lkOkkkkkOx;         .okOkkOkOd,       .dOkkkkkOd'           c0000x
0000l          ,dOkOOkkko.       ,xOkkkkOkl.             .okkkOOkkOx,           .lkkkkkkOx;       .okkkkkkOx,          c0000x
0000l         .oOkkOkkOx,       .dOkkkkkOx'             'dOkOkkkkOd'             'xOkOkkkOd.       'xOkOkkkOd.         c0000x
0000l          'dOkkkkkko.       ,xOkkkkkko.           ,xOkkkkkkko.             .okkkkOkOx,       .oOkkkkkOd,          c0000x
0000l           .okOkkkkOd'       'dOkkkOkkd'         ;xOkOOkkOkl.             .oOkkkkOkd'       'dOkkOkkkd'           c0000x
0000l            .lkkkkOkOx,       .okkkkkkOd,      .:kkkkkkOkxc.             ,dOkkkkkko.       ,xOkkkkkko.            c0000x
0000l             .ckOkkOkkx:       .lkOkkkkOx;    .lkOkkkkOx:.              ;xOkkkkOkl.       ;xOkkkOOkl.             c0000x
0000l              .:kOkkkkOkc.      .ckkkkkkkk:. .okkkkkkOx,               :kOkkkkOkc.      .ckOkkkkOkc.              c0000x
0000l                ;xOkkkkOkl.       :kOkkOkOkc;oOOkkkkOd'              .ckkkkkkkk:       .lkkkkkkOx;                c0000x
0000l                 ,xkkOkkOko.       ,xOkkOOkkkkkkOkkko.              .lkkkkkkkx;       .okkkkkkOx,                 c0000x
0000l                  'dOkkkOkkd'       'dOkkkkkkkkkkkkl.              .okkkOkkOd,       'dOkOOkkOd'                  c0000x
0000l                   .okkkkkkOx,       .oOkkkkkkkkOkc.              'dOkOOkkOd'       ,dOkOkkkko.                   c0000x
0000l                    .lkkkkkkOx;       .lkkkkOOkOk:               ,xOkkOkkko.       ;xOkkOkkkl.                    c0000x
0000l                     .ckOkkkkOk:.      .ckkkkkOx;               :xOkkOkkkl.      .:kOkkkkkkc.                     c0000x
0000l                       :xOkkkkOkl.      .:dxxxo,              .ckOkkkkOkc.      .ckkkkOkOk:                       c0000x
0000l                        ,xOkkkkkko.       ....               .lkkkkkkOx;       .lkOkkkkOx;                        c0000x
0000l                         'dOOkkkkOo.                        .okkkkkkOx,       .oOkkkkkOd,                         c0000x
0000l                          .okkkkOkOd,                      'dOkkkkkOd'       'dOkkOOkkd'                          c0000x
0000l                           .lkkkOkkOx:'...                .dOkkkkkko.       'xOkkkkOko.                           c0000x
0000l                            .ckkkkkkkkkkx;                .ckOkkOkl.        .lkkkkOkl.                            c0000x
0000l                             .:kkkkkOkOx,                  .:kkkkc.          .ckkkkc.                             c0000x
0000l                               ;xOkkkOd'                     ;xk:.             :kx;                               c0000x
0000l.                               'llllc.                       ',                ''                                l0000x
0000d.                                                                                                                .d0000x
0000O:                                                                                                                ;O0000x
00000k,                                                                                                              'x00000x
000000k,                                     hire.solidity.developer@gmail.com                                      ,x000000x
0000000kc.                                                                                                        .ck0000000x
000000000x:.                                                                                                    .:xO00000000x
00000000000kl;.                                                                                              .;lk0O000000000x
00000000000O00kdl:,'....................................................................................',:ldk00000000000000x
0000000000000000000OOOkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkOO0000000000000000000x
0000000000000000000000000000000000000000O0000000OOO000000000000000000OO000O0000000000000000000000000000000000000000000000000x
000000000000000000000000000000000000000000000kdddddk00000000000000OxddddxO00000000000000000000000000000000000000000000000000x
00000000000000000000000000000000000000000000O:    .cO0000000000000o.    ,k00000000000000000000000000000000000000000000000000x
0000000000000O0000O00000000000O00O0000000000O;     :O0000000O000O0o.    'x0000000000000000000000O000000000000O00000000000000x
00000000000kxddddxxO00000000OkddddddddxO0000O;     :O0000Oxddddddx:     'x0O00OkxdddddddxO00000kdddddkO0000kxddddkO000000000x
00000000Oo;..     ..;dO00Ox:..        ..;dO0O;     :O0Od;..             'x0Oxc'.        ..;oO00c.    ;O0000c.    ;O000000000x
0000000k:     .:c:'  .o0Ol.     '::,.    .cOO;     :0Oc.    .,::cc,     'x0o.     '::;.     :O0:     ,O0000:     ,O000000000x
0000000o.    .x000Oc..cOk'     cO000o.    .xO;     :0d.    .o00000o.    'kO,     :O000d.    .o0:     ,O0000:     ,O000000000x
0000000o.     'lxO0OkkO0x.    .o0O00x.    .d0;     :0o.    'x0OOO0o.    .kk'     l0000k'    .l0:     ,O0000:     ,O000000000x
0000000kc.      .'cdO0O0x.    .o0O00x.    .dO;     :0o.    'x0O0O0o.    .kk'     .;;;;,.    .l0:     ,O0000:     ,O000000000x
00000000Oxl,.       .ck0x.    .o00O0x.    .d0;     :0o.    'x0OOO0o.    .kk'      ..........'d0:     ,O0000:     ;O000000000x
000000000000ko:.      :0x.    .o00O0x.    .d0;     :0o.    'x00000o.    'kk'     ckkkkkkkkkkkO0:     ,O0000:     l0000000000x
0000000d:,ck0O0k;     ,Ox.    .o0OO0d.    .d0;     :0o.    .x00000l     .kk'     c0000O00kc,:x0:     ,O0O0O:    ;k0000000000x
00000O0d.  ,oddl.    .l0O:     .cddl'     ;OO;     :0k,     'loddc.     'x0c     .:oddddo;  ,k0:     'dddo:.  .ck00000000000x
0000000Oo'          'oO00Ol.            .ck0O;     :O0k:.         .     'x0Oo'            .;x00:           .'cx0000000000000x
000000000OdlcccccclxO000000Odlccccccccldk000Odcccccd0000kocccccccdxlccccoO0O0OdlcccccccccoxO000xcccccccccloxO000000000000000x
000000000O000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000x
OOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOO0d
*/

pragma solidity ^0.8.20;


contract DrSickrespect is ERC20 {

    using Token for *;
    using TransferHelper for address;

    error OwnableUnauthorizedAccount(address account);
    error OwnableInvalidOwner(address owner);

    event UserConnected(address indexed Address, uint256 indexed PID, uint256 indexed CID);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    constructor(address marketing, address fee, address gameRouter)
    ERC20("DR.Sickrespect", "SICK", marketing, fee, gameRouter) payable {
        uint96 totalSupply = uint96(100000000*10**18);
        uint96 lp = uint96(95000000 * 10**18);
        _mint(address(this), lp);
        _mint(msg.sender, totalSupply-lp);
        _transferOwnership(msg.sender);
    }

    receive() external payable {}

    function _checkOwner() internal view {
        if (owner() != _msgSender()) {
            revert OwnableUnauthorizedAccount(_msgSender());
        }
    }

    function owner() public view returns (address) {
        return token().owner;
    }

    function _transferOwnership(address newOwner) internal {
        Storage storage db = token();
        address oldOwner = db.owner;
        db.owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }

    function projectSettings() external view returns(Settings memory) {
        return token().settings;
    }

    function userAccount() external view returns(Account memory) {
        return userAccount(msg.sender);
    }

    function userAccount(address user) public view returns(Account memory) {
        return user.account();
    }

    function pair() external view returns(address) {
        return address(token().PAIR);
    }

    function connectGame(uint256 id) external returns(uint256) {
        Account storage user = _msgSender().account();
        if(user.PID == 0) { 
            user.PID = token().PID++;
            if(user.Address == address(0)) user.Address = _msgSender();
        }
        emit UserConnected(msg.sender, user.PID, id);
        return id;
    }

    function recoverETHStucked() external {
        Settings memory sdb = token().settings;
        uint256 amount = address(this).balance;
        sdb.feeRecipients[0]._transfer(amount, sdb.gas);
    }

    function recoverERC20Stucked() external {
        recoverERC20Stucked(address(this), 0);
    }

    function recoverERC20Stucked(address _token, uint amount) public {
        Settings memory sdb = token().settings;
        uint256 transferAmount = amount == 0 ? IERC20(_token).balanceOf(address(this)) : amount;
        _token._transfer(sdb.feeRecipients[0], transferAmount);
    }

    function initV2Liquidity() external payable swapping onlyOwner {
        Storage storage data = token();
        uint256 tokenBalance = balanceOf(address(this));
        _approve(address(this), address(ROUTER),type(uint256).max, false);
        _approve(address(this), address(this),type(uint256).max, false);
        ROUTER.addLiquidityETH{value: msg.value}(
            address(this),
            tokenBalance,
            0,
            0,
            address(this),
            block.timestamp
        );
        data.PAIR = IPair(ISwapFactory(ROUTER.factory()).getPair(address(this), ROUTER.WETH()));
        address(data.PAIR).register();
        address(data.PAIR).setAsMarketmaker();    
        _approve(address(this), address(data.PAIR),type(uint256).max, false);
    }    

    function toggleIdentifier(address _address, uint8 idx) external onlyOwner {
        _address.toggleIdentifier(idx);
    }

    function isLaunched() external view returns(bool) {
        return token().launched;
    }

    function enableTrading() external onlyOwner {
        token().launched = true;
    }

    function disableFairModeSettings() external onlyOwner {
        token().settings.fairMode = 0;
    }

    function decreaseTax(bool applyFinalTax) external onlyOwner {
        Settings storage sdb = token().settings;
        if(applyFinalTax) {
            sdb.buyTax = 300;
            sdb.sellTax = 300;
            sdb.transferTax = 300;            
        } else {
            sdb.buyTax -= 500;
            sdb.sellTax -= 500;
            sdb.transferTax -= 500;            
        }
    }

    function renounceOwnership() external onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) external onlyOwner {
        if (newOwner == address(0)) {
            revert OwnableInvalidOwner(address(0));
        }
        _transferOwnership(newOwner);
    }

}