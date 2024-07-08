// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)


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

// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Metadata.sol


// OpenZeppelin Contracts v4.4.1 (token/ERC20/extensions/IERC20Metadata.sol)



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

// File: @openzeppelin/contracts/utils/math/Math.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/Math.sol)


/**
 * @dev Standard math utilities missing in the Solidity language.
 */
library Math {
    enum Rounding {
        Down, // Toward negative infinity
        Up, // Toward infinity
        Zero // Toward zero
    }

    /**
     * @dev Returns the largest of two numbers.
     */
    function max(uint256 a, uint256 b) internal pure returns (uint256) {
        return a > b ? a : b;
    }

    /**
     * @dev Returns the smallest of two numbers.
     */
    function min(uint256 a, uint256 b) internal pure returns (uint256) {
        return a < b ? a : b;
    }

    /**
     * @dev Returns the average of two numbers. The result is rounded towards
     * zero.
     */
    function average(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b) / 2 can overflow.
        return (a & b) + (a ^ b) / 2;
    }

    /**
     * @dev Returns the ceiling of the division of two numbers.
     *
     * This differs from standard division with `/` in that it rounds up instead
     * of rounding down.
     */
    function ceilDiv(uint256 a, uint256 b) internal pure returns (uint256) {
        // (a + b - 1) / b can overflow on addition, so we distribute.
        return a == 0 ? 0 : (a - 1) / b + 1;
    }

    /**
     * @notice Calculates floor(x * y / denominator) with full precision. Throws if result overflows a uint256 or denominator == 0
     * @dev Original credit to Remco Bloemen under MIT license (https://xn--2-umb.com/21/muldiv)
     * with further edits by Uniswap Labs also under MIT license.
     */
    function mulDiv(uint256 x, uint256 y, uint256 denominator) internal pure returns (uint256 result) {
        unchecked {
            // 512-bit multiply [prod1 prod0] = x * y. Compute the product mod 2^256 and mod 2^256 - 1, then use
            // use the Chinese Remainder Theorem to reconstruct the 512 bit result. The result is stored in two 256
            // variables such that product = prod1 * 2^256 + prod0.
            uint256 prod0; // Least significant 256 bits of the product
            uint256 prod1; // Most significant 256 bits of the product
            assembly {
                let mm := mulmod(x, y, not(0))
                prod0 := mul(x, y)
                prod1 := sub(sub(mm, prod0), lt(mm, prod0))
            }

            // Handle non-overflow cases, 256 by 256 division.
            if (prod1 == 0) {
                // Solidity will revert if denominator == 0, unlike the div opcode on its own.
                // The surrounding unchecked block does not change this fact.
                // See https://docs.soliditylang.org/en/latest/control-structures.html#checked-or-unchecked-arithmetic.
                return prod0 / denominator;
            }

            // Make sure the result is less than 2^256. Also prevents denominator == 0.
            require(denominator > prod1, "Math: mulDiv overflow");

            ///////////////////////////////////////////////
            // 512 by 256 division.
            ///////////////////////////////////////////////

            // Make division exact by subtracting the remainder from [prod1 prod0].
            uint256 remainder;
            assembly {
                // Compute remainder using mulmod.
                remainder := mulmod(x, y, denominator)

                // Subtract 256 bit number from 512 bit number.
                prod1 := sub(prod1, gt(remainder, prod0))
                prod0 := sub(prod0, remainder)
            }

            // Factor powers of two out of denominator and compute largest power of two divisor of denominator. Always >= 1.
            // See https://cs.stackexchange.com/q/138556/92363.

            // Does not overflow because the denominator cannot be zero at this stage in the function.
            uint256 twos = denominator & (~denominator + 1);
            assembly {
                // Divide denominator by twos.
                denominator := div(denominator, twos)

                // Divide [prod1 prod0] by twos.
                prod0 := div(prod0, twos)

                // Flip twos such that it is 2^256 / twos. If twos is zero, then it becomes one.
                twos := add(div(sub(0, twos), twos), 1)
            }

            // Shift in bits from prod1 into prod0.
            prod0 |= prod1 * twos;

            // Invert denominator mod 2^256. Now that denominator is an odd number, it has an inverse modulo 2^256 such
            // that denominator * inv = 1 mod 2^256. Compute the inverse by starting with a seed that is correct for
            // four bits. That is, denominator * inv = 1 mod 2^4.
            uint256 inverse = (3 * denominator) ^ 2;

            // Use the Newton-Raphson iteration to improve the precision. Thanks to Hensel's lifting lemma, this also works
            // in modular arithmetic, doubling the correct bits in each step.
            inverse *= 2 - denominator * inverse; // inverse mod 2^8
            inverse *= 2 - denominator * inverse; // inverse mod 2^16
            inverse *= 2 - denominator * inverse; // inverse mod 2^32
            inverse *= 2 - denominator * inverse; // inverse mod 2^64
            inverse *= 2 - denominator * inverse; // inverse mod 2^128
            inverse *= 2 - denominator * inverse; // inverse mod 2^256

            // Because the division is now exact we can divide by multiplying with the modular inverse of denominator.
            // This will give us the correct result modulo 2^256. Since the preconditions guarantee that the outcome is
            // less than 2^256, this is the final result. We don't need to compute the high bits of the result and prod1
            // is no longer required.
            result = prod0 * inverse;
            return result;
        }
    }

    /**
     * @notice Calculates x * y / denominator with full precision, following the selected rounding direction.
     */
    function mulDiv(uint256 x, uint256 y, uint256 denominator, Rounding rounding) internal pure returns (uint256) {
        uint256 result = mulDiv(x, y, denominator);
        if (rounding == Rounding.Up && mulmod(x, y, denominator) > 0) {
            result += 1;
        }
        return result;
    }

    /**
     * @dev Returns the square root of a number. If the number is not a perfect square, the value is rounded down.
     *
     * Inspired by Henry S. Warren, Jr.'s "Hacker's Delight" (Chapter 11).
     */
    function sqrt(uint256 a) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        // For our first guess, we get the biggest power of 2 which is smaller than the square root of the target.
        //
        // We know that the "msb" (most significant bit) of our target number `a` is a power of 2 such that we have
        // `msb(a) <= a < 2*msb(a)`. This value can be written `msb(a)=2**k` with `k=log2(a)`.
        //
        // This can be rewritten `2**log2(a) <= a < 2**(log2(a) + 1)`
        // → `sqrt(2**k) <= sqrt(a) < sqrt(2**(k+1))`
        // → `2**(k/2) <= sqrt(a) < 2**((k+1)/2) <= 2**(k/2 + 1)`
        //
        // Consequently, `2**(log2(a) / 2)` is a good first approximation of `sqrt(a)` with at least 1 correct bit.
        uint256 result = 1 << (log2(a) >> 1);

        // At this point `result` is an estimation with one bit of precision. We know the true value is a uint128,
        // since it is the square root of a uint256. Newton's method converges quadratically (precision doubles at
        // every iteration). We thus need at most 7 iteration to turn our partial result with one bit of precision
        // into the expected uint128 result.
        unchecked {
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            result = (result + a / result) >> 1;
            return min(result, a / result);
        }
    }

    /**
     * @notice Calculates sqrt(a), following the selected rounding direction.
     */
    function sqrt(uint256 a, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = sqrt(a);
            return result + (rounding == Rounding.Up && result * result < a ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 2, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 128;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 64;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 32;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 16;
            }
            if (value >> 8 > 0) {
                value >>= 8;
                result += 8;
            }
            if (value >> 4 > 0) {
                value >>= 4;
                result += 4;
            }
            if (value >> 2 > 0) {
                value >>= 2;
                result += 2;
            }
            if (value >> 1 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 2, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log2(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log2(value);
            return result + (rounding == Rounding.Up && 1 << result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 10, rounded down, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >= 10 ** 64) {
                value /= 10 ** 64;
                result += 64;
            }
            if (value >= 10 ** 32) {
                value /= 10 ** 32;
                result += 32;
            }
            if (value >= 10 ** 16) {
                value /= 10 ** 16;
                result += 16;
            }
            if (value >= 10 ** 8) {
                value /= 10 ** 8;
                result += 8;
            }
            if (value >= 10 ** 4) {
                value /= 10 ** 4;
                result += 4;
            }
            if (value >= 10 ** 2) {
                value /= 10 ** 2;
                result += 2;
            }
            if (value >= 10 ** 1) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 10, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log10(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log10(value);
            return result + (rounding == Rounding.Up && 10 ** result < value ? 1 : 0);
        }
    }

    /**
     * @dev Return the log in base 256, rounded down, of a positive value.
     * Returns 0 if given 0.
     *
     * Adding one to the result gives the number of pairs of hex symbols needed to represent `value` as a hex string.
     */
    function log256(uint256 value) internal pure returns (uint256) {
        uint256 result = 0;
        unchecked {
            if (value >> 128 > 0) {
                value >>= 128;
                result += 16;
            }
            if (value >> 64 > 0) {
                value >>= 64;
                result += 8;
            }
            if (value >> 32 > 0) {
                value >>= 32;
                result += 4;
            }
            if (value >> 16 > 0) {
                value >>= 16;
                result += 2;
            }
            if (value >> 8 > 0) {
                result += 1;
            }
        }
        return result;
    }

    /**
     * @dev Return the log in base 256, following the selected rounding direction, of a positive value.
     * Returns 0 if given 0.
     */
    function log256(uint256 value, Rounding rounding) internal pure returns (uint256) {
        unchecked {
            uint256 result = log256(value);
            return result + (rounding == Rounding.Up && 1 << (result << 3) < value ? 1 : 0);
        }
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/lib/DecimalMath.sol




/**
 * @title DecimalMath
 * @author DODO Breeder
 *
 * @notice Functions for fixed point number with 18 decimals
 */

library DecimalMath {
    uint256 internal constant ONE = 10 ** 18;
    uint256 internal constant ONE2 = 10 ** 36;

    function mul(uint256 target, uint256 d) internal pure returns (uint256) {
        return target * d / (10 ** 18);
    }

    function mulFloor(uint256 target, uint256 d) internal pure returns (uint256) {
        return target * d / (10 ** 18);
    }

    function mulCeil(uint256 target, uint256 d) internal pure returns (uint256) {
        return _divCeil(target * d, 10 ** 18);
    }

    function div(uint256 target, uint256 d) internal pure returns (uint256) {
        return target * (10 ** 18) / d;
    }

    function divFloor(uint256 target, uint256 d) internal pure returns (uint256) {
        return target * (10 ** 18) / d;
    }

    function divCeil(uint256 target, uint256 d) internal pure returns (uint256) {
        return _divCeil(target * (10 ** 18), d);
    }

    function reciprocalFloor(uint256 target) internal pure returns (uint256) {
        return uint256(10 ** 36) / target;
    }

    function reciprocalCeil(uint256 target) internal pure returns (uint256) {
        return _divCeil(uint256(10 ** 36), target);
    }

    function sqrt(uint256 target) internal pure returns (uint256) {
        return Math.sqrt(target * ONE);
    }

    function powFloor(uint256 target, uint256 e) internal pure returns (uint256) {
        if (e == 0) {
            return 10 ** 18;
        } else if (e == 1) {
            return target;
        } else {
            uint256 p = powFloor(target, e / 2);
            p = p * p / (10 ** 18);
            if (e % 2 == 1) {
                p = p * target / (10 ** 18);
            }
            return p;
        }
    }

    function _divCeil(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 quotient = a / b;
        uint256 remainder = a - quotient * b;
        if (remainder > 0) {
            return quotient + 1;
        } else {
            return quotient;
        }
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/lib/DODOMath.sol


/**
 * @title DODOMath
 * @author DODO Breeder
 *
 * @notice Functions for complex calculating. Including ONE Integration and TWO Quadratic solutions
 */
library DODOMath {
    using Math for uint256;
    /*
        Integrate dodo curve from V1 to V2
        require V0>=V1>=V2>0
        res = (1-k)i(V1-V2)+ikV0*V0(1/V2-1/V1)
        let V1-V2=delta
        res = i*delta*(1-k+k(V0^2/V1/V2))

        i is the price of V-res trading pair

        support k=1 & k=0 case

        [round down]
    */
    function _GeneralIntegrate(
        uint256 V0,
        uint256 V1,
        uint256 V2,
        uint256 i,
        uint256 k
    ) internal pure returns (uint256) {
        require(V0 > 0, "TARGET_IS_ZERO");
        uint256 fairAmount = i * (V1 - V2); // i*delta
        if (k == 0) {
            return fairAmount / DecimalMath.ONE;
        }
        uint256 V0V0V1V2 = DecimalMath.divFloor(V0 * V0 / V1, V2);
        uint256 penalty = DecimalMath.mulFloor(k, V0V0V1V2); // k(V0^2/V1/V2)
        return (DecimalMath.ONE - k + penalty) * fairAmount / DecimalMath.ONE2;
    }

    /*
        Follow the integration function above
        i*deltaB = (Q2-Q1)*(1-k+kQ0^2/Q1/Q2)
        Assume Q2=Q0, Given Q1 and deltaB, solve Q0

        i is the price of delta-V trading pair
        give out target of V

        support k=1 & k=0 case

        [round down]
    */
    function _SolveQuadraticFunctionForTarget(
        uint256 V1,
        uint256 delta,
        uint256 i,
        uint256 k
    ) internal pure returns (uint256) {
        if (k == 0) {
            return V1 + DecimalMath.mulFloor(i, delta);
        }
        // V0 = V1*(1+(sqrt-1)/2k)
        // sqrt = √(1+4kidelta/V1)
        // premium = 1+(sqrt-1)/2k
        // uint256 sqrt = (4 * k).mul(i).mul(delta).div(V1).add(DecimalMath.ONE2).sqrt();

        if (V1 == 0) {
            return 0;
        }
        uint256 sqrt;
        uint256 ki = 4 * k * i;

        if (ki == 0) {
            sqrt = DecimalMath.ONE;
        } else if ((ki * delta) / ki == delta) {
            sqrt =((ki * delta) / V1  + DecimalMath.ONE2).sqrt();
        } else {
            sqrt = (ki / V1 * delta + DecimalMath.ONE2).sqrt();
        }
        uint256 premium =
            DecimalMath.divFloor(sqrt - DecimalMath.ONE, k * 2) + DecimalMath.ONE;
        // V0 is greater than or equal to V1 according to the solution
        return DecimalMath.mulFloor(V1, premium);
    }

    /*
        Follow the integration expression above, we have:
        i*deltaB = (Q2-Q1)*(1-k+kQ0^2/Q1/Q2)
        Given Q1 and deltaB, solve Q2
        This is a quadratic function and the standard version is
        aQ2^2 + bQ2 + c = 0, where
        a=1-k
        -b=(1-k)Q1-kQ0^2/Q1+i*deltaB
        c=-kQ0^2 
        and Q2=(-b+sqrt(b^2+4(1-k)kQ0^2))/2(1-k)
        note: another root is negative, abondan

        if deltaBSig=true, then Q2>Q1, user sell Q and receive B
        if deltaBSig=false, then Q2<Q1, user sell B and receive Q
        return |Q1-Q2|

        as we only support sell amount as delta, the deltaB is always negative
        the input ideltaB is actually -ideltaB in the equation

        i is the price of delta-V trading pair

        support k=1 & k=0 case

        [round down]
    */
    function _SolveQuadraticFunctionForTrade(
        uint256 V0,
        uint256 V1,
        uint256 delta,
        uint256 i,
        uint256 k
    ) internal pure returns (uint256) {
        require(V0 > 0, "TARGET_IS_ZERO");
        if (delta == 0) {
            return 0;
        }

        if (k == 0) {
            // why v1
            return DecimalMath.mulFloor(i, delta) > V1 ? V1 : DecimalMath.mulFloor(i, delta);
        }

        if (k == DecimalMath.ONE) {
            // if k==1
            // Q2=Q1/(1+ideltaBQ1/Q0/Q0)
            // temp = ideltaBQ1/Q0/Q0
            // Q2 = Q1/(1+temp)
            // Q1-Q2 = Q1*(1-1/(1+temp)) = Q1*(temp/(1+temp))
            // uint256 temp = i.mul(delta).mul(V1).div(V0.mul(V0));
            uint256 temp;
            uint256 idelta = i * (delta);
            if (idelta == 0) {
                temp = 0;
            } else if ((idelta * V1) / idelta == V1) {
                temp = (idelta * V1) / (V0 * V0);
            } else {
                temp = delta * (V1) / (V0) * (i) / (V0);
            }
            return V1 * (temp) / (temp + (DecimalMath.ONE));
        }

        // calculate -b value and sig
        // b = kQ0^2/Q1-i*deltaB-(1-k)Q1
        // part1 = (1-k)Q1 >=0
        // part2 = kQ0^2/Q1-i*deltaB >=0
        // bAbs = abs(part1-part2)
        // if part1>part2 => b is negative => bSig is false
        // if part2>part1 => b is positive => bSig is true
        uint256 part2 = k * (V0) / (V1) * (V0) + (i * (delta)); // kQ0^2/Q1-i*deltaB
        uint256 bAbs = (DecimalMath.ONE - k) * (V1); // (1-k)Q1

        bool bSig;
        if (bAbs >= part2) {
            bAbs = bAbs - part2;
            bSig = false;
        } else {
            bAbs = part2 - bAbs;
            bSig = true;
        }
        bAbs = bAbs / (DecimalMath.ONE);

        // calculate sqrt
        uint256 squareRoot = DecimalMath.mulFloor((DecimalMath.ONE - k) * (4), DecimalMath.mulFloor(k, V0) * (V0)); // 4(1-k)kQ0^2
        squareRoot = Math.sqrt((bAbs * bAbs) + squareRoot); // sqrt(b*b+4(1-k)kQ0*Q0)

        // final res
        uint256 denominator = (DecimalMath.ONE - k) * 2; // 2(1-k)
        uint256 numerator;
        if (bSig) {
            numerator = squareRoot - bAbs;
            if (numerator == 0) {
                revert("DODOMath: should not be 0");
            }
        } else {
            numerator = bAbs + squareRoot;
        }

        uint256 V2 = DecimalMath.divCeil(numerator, denominator);
        if (V2 > V1) {
            return 0;
        } else {
            return V1 - V2;
        }
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/lib/PMMPricing.sol



/**
 * @title Pricing
 * @author DODO Breeder
 *
 * @notice DODO Pricing model
 */

library PMMPricing {

    enum RState {ONE, ABOVE_ONE, BELOW_ONE}

    struct PMMState {
        uint256 i;
        uint256 K;
        uint256 B;
        uint256 Q;
        uint256 B0;
        uint256 Q0;
        RState R;
    }

    // ============ buy & sell ============
    /**
     * @notice Inner calculation based on pmm algorithm, sell base
     * @param state The current PMM state
     * @param payBaseAmount The amount of base token user want to sell
     * @return receiveQuoteAmount The amount of quote token user will receive
     * @return newR The new R status after swap
     */
    function sellBaseToken(PMMState memory state, uint256 payBaseAmount)
        internal
        pure
        returns (uint256 receiveQuoteAmount, RState newR)
    {
        if (state.R == RState.ONE) {
            // case 1: R=1
            // R falls below one
            receiveQuoteAmount = _ROneSellBaseToken(state, payBaseAmount);
            newR = RState.BELOW_ONE;
        } else if (state.R == RState.ABOVE_ONE) {
            uint256 backToOnePayBase = state.B0 - state.B;
            uint256 backToOneReceiveQuote = state.Q - state.Q0;
            // case 2: R>1
            // complex case, R status depends on trading amount
            if (payBaseAmount < backToOnePayBase) {
                // case 2.1: R status do not change
                receiveQuoteAmount = _RAboveSellBaseToken(state, payBaseAmount);
                newR = RState.ABOVE_ONE;
                if (receiveQuoteAmount > backToOneReceiveQuote) {
                    // [Important corner case!] may enter this branch when some precision problem happens. And consequently contribute to negative spare quote amount
                    // to make sure spare quote>=0, mannually set receiveQuote=backToOneReceiveQuote
                    receiveQuoteAmount = backToOneReceiveQuote;
                }
            } else if (payBaseAmount == backToOnePayBase) {
                // case 2.2: R status changes to ONE
                receiveQuoteAmount = backToOneReceiveQuote;
                newR = RState.ONE;
            } else {
                // case 2.3: R status changes to BELOW_ONE
                receiveQuoteAmount = backToOneReceiveQuote + (
                    _ROneSellBaseToken(state, (payBaseAmount - backToOnePayBase))
                );
                newR = RState.BELOW_ONE;
            }
        } else {
            // state.R == RState.BELOW_ONE
            // case 3: R<1
            receiveQuoteAmount = _RBelowSellBaseToken(state, payBaseAmount);
            newR = RState.BELOW_ONE;
        }
    }

    /**
     * @notice Inner calculation based on pmm algorithm, sell quote
     * @param state The current PMM state
     * @param payQuoteAmount The amount of quote token user want to sell
     * @return receiveBaseAmount The amount of base token user will receive
     * @return newR The new R status after swap
     */
    function sellQuoteToken(PMMState memory state, uint256 payQuoteAmount)
        internal
        pure
        returns (uint256 receiveBaseAmount, RState newR)
    {
        if (state.R == RState.ONE) {
            receiveBaseAmount = _ROneSellQuoteToken(state, payQuoteAmount);
            newR = RState.ABOVE_ONE;
        } else if (state.R == RState.ABOVE_ONE) {
            receiveBaseAmount = _RAboveSellQuoteToken(state, payQuoteAmount);
            newR = RState.ABOVE_ONE;
        } else {
            uint256 backToOnePayQuote = state.Q0 - state.Q;
            uint256 backToOneReceiveBase = state.B - state.B0;
            if (payQuoteAmount < backToOnePayQuote) {
                receiveBaseAmount = _RBelowSellQuoteToken(state, payQuoteAmount); 
                newR = RState.BELOW_ONE;
                if (receiveBaseAmount > backToOneReceiveBase) {
                    receiveBaseAmount = backToOneReceiveBase;
                }
            } else if (payQuoteAmount == backToOnePayQuote) {
                receiveBaseAmount = backToOneReceiveBase;
                newR = RState.ONE;
            } else {
                receiveBaseAmount = backToOneReceiveBase + (
                    _ROneSellQuoteToken(state, payQuoteAmount - backToOnePayQuote) 
                );
                newR = RState.ABOVE_ONE;
            }
        }
    }

    // ============ R = 1 cases ============

    function _ROneSellBaseToken(PMMState memory state, uint256 payBaseAmount)
        internal
        pure
        returns (
            uint256 // receiveQuoteToken
        )
    {
        // in theory Q2 <= targetQuoteTokenAmount
        // however when amount is close to 0, precision problems may cause Q2 > targetQuoteTokenAmount
        return
            DODOMath._SolveQuadraticFunctionForTrade(
                state.Q0,
                state.Q0,
                payBaseAmount,
                state.i,
                state.K
            );
    }

    function _ROneSellQuoteToken(PMMState memory state, uint256 payQuoteAmount)
        internal
        pure
        returns (
            uint256 // receiveBaseToken
        )
    {
        return
            DODOMath._SolveQuadraticFunctionForTrade(
                state.B0,
                state.B0,
                payQuoteAmount,
                DecimalMath.reciprocalFloor(state.i),
                state.K
            );
    }

    // ============ R < 1 cases ============

    function _RBelowSellQuoteToken(PMMState memory state, uint256 payQuoteAmount)
        internal
        pure
        returns (
            uint256 // receiveBaseToken
        )
    {
        return
            DODOMath._GeneralIntegrate(
                state.Q0,
                state.Q + payQuoteAmount,
                state.Q,
                DecimalMath.reciprocalFloor(state.i),
                state.K
            );
    }

    function _RBelowSellBaseToken(PMMState memory state, uint256 payBaseAmount)
        internal
        pure
        returns (
            uint256 // receiveQuoteToken
        )
    {
        return
            DODOMath._SolveQuadraticFunctionForTrade(
                state.Q0,
                state.Q,
                payBaseAmount,
                state.i,
                state.K
            );
    }

    // ============ R > 1 cases ============

    function _RAboveSellBaseToken(PMMState memory state, uint256 payBaseAmount)
        internal
        pure
        returns (
            uint256 // receiveQuoteToken
        )
    {
        return
            DODOMath._GeneralIntegrate(
                state.B0,
                state.B + payBaseAmount,
                state.B,
                state.i,
                state.K
            );
    }

    function _RAboveSellQuoteToken(PMMState memory state, uint256 payQuoteAmount)
        internal
        pure
        returns (
            uint256 // receiveBaseToken
        )
    {
        return
            DODOMath._SolveQuadraticFunctionForTrade(
                state.B0,
                state.B,
                payQuoteAmount,
                DecimalMath.reciprocalFloor(state.i),
                state.K
            );
    }

    // ============ Helper functions ============

    function adjustedTarget(PMMState memory state) internal pure {
        if (state.R == RState.BELOW_ONE) {
            state.Q0 = DODOMath._SolveQuadraticFunctionForTarget(
                state.Q,
                state.B - state.B0,
                state.i,
                state.K
            );
        } else if (state.R == RState.ABOVE_ONE) {
            state.B0 = DODOMath._SolveQuadraticFunctionForTarget(
                state.B,
                state.Q - state.Q0,
                DecimalMath.reciprocalFloor(state.i),
                state.K
            );
        }
    }

    function getMidPrice(PMMState memory state) internal pure returns (uint256) {
        if (state.R == RState.BELOW_ONE) {
            uint256 R = DecimalMath.divFloor(state.Q0 * state.Q0 / state.Q, state.Q);
            R = DecimalMath.ONE - state.K + (DecimalMath.mulFloor(state.K, R));
            return DecimalMath.divFloor(state.i, R);
        } else {
            uint256 R = DecimalMath.divFloor(state.B0 * state.B0 / state.B, state.B);
            R = DecimalMath.ONE - state.K + (DecimalMath.mulFloor(state.K, R));
            return DecimalMath.mulFloor(state.i, R);
        }
    }
}

// File: @openzeppelin/contracts/token/ERC20/extensions/IERC20Permit.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/extensions/IERC20Permit.sol)



/**
 * @dev Interface of the ERC20 Permit extension allowing approvals to be made via signatures, as defined in
 * https://eips.ethereum.org/EIPS/eip-2612[EIP-2612].
 *
 * Adds the {permit} method, which can be used to change an account's ERC20 allowance (see {IERC20-allowance}) by
 * presenting a message signed by the account. By not relying on {IERC20-approve}, the token holder account doesn't
 * need to send a transaction, and thus is not required to hold Ether at all.
 */
interface IERC20Permit {
    /**
     * @dev Sets `value` as the allowance of `spender` over ``owner``'s tokens,
     * given ``owner``'s signed approval.
     *
     * IMPORTANT: The same issues {IERC20-approve} has related to transaction
     * ordering also apply here.
     *
     * Emits an {Approval} event.
     *
     * Requirements:
     *
     * - `spender` cannot be the zero address.
     * - `deadline` must be a timestamp in the future.
     * - `v`, `r` and `s` must be a valid `secp256k1` signature from `owner`
     * over the EIP712-formatted function arguments.
     * - the signature must use ``owner``'s current nonce (see {nonces}).
     *
     * For more information on the signature format, see the
     * https://eips.ethereum.org/EIPS/eip-2612#specification[relevant EIP
     * section].
     */
    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    /**
     * @dev Returns the current nonce for `owner`. This value must be
     * included whenever a signature is generated for {permit}.
     *
     * Every successful call to {permit} increases ``owner``'s nonce by one. This
     * prevents a signature from being used multiple times.
     */
    function nonces(address owner) external view returns (uint256);

    /**
     * @dev Returns the domain separator used in the encoding of the signature for {permit}, as defined by {EIP712}.
     */
    // solhint-disable-next-line func-name-mixedcase
    function DOMAIN_SEPARATOR() external view returns (bytes32);
}

// File: @openzeppelin/contracts/utils/Address.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/Address.sol)



/**
 * @dev Collection of functions related to the address type
 */
library Address {
    /**
     * @dev Returns true if `account` is a contract.
     *
     * [IMPORTANT]
     * ====
     * It is unsafe to assume that an address for which this function returns
     * false is an externally-owned account (EOA) and not a contract.
     *
     * Among others, `isContract` will return false for the following
     * types of addresses:
     *
     *  - an externally-owned account
     *  - a contract in construction
     *  - an address where a contract will be created
     *  - an address where a contract lived, but was destroyed
     *
     * Furthermore, `isContract` will also return true if the target contract within
     * the same transaction is already scheduled for destruction by `SELFDESTRUCT`,
     * which only has an effect at the end of a transaction.
     * ====
     *
     * [IMPORTANT]
     * ====
     * You shouldn't rely on `isContract` to protect against flash loan attacks!
     *
     * Preventing calls from contracts is highly discouraged. It breaks composability, breaks support for smart wallets
     * like Gnosis Safe, and does not provide security since it can be circumvented by calling from a contract
     * constructor.
     * ====
     */
    function isContract(address account) internal view returns (bool) {
        // This method relies on extcodesize/address.code.length, which returns 0
        // for contracts in construction, since the code is only stored at the end
        // of the constructor execution.

        return account.code.length > 0;
    }

    /**
     * @dev Replacement for Solidity's `transfer`: sends `amount` wei to
     * `recipient`, forwarding all available gas and reverting on errors.
     *
     * https://eips.ethereum.org/EIPS/eip-1884[EIP1884] increases the gas cost
     * of certain opcodes, possibly making contracts go over the 2300 gas limit
     * imposed by `transfer`, making them unable to receive funds via
     * `transfer`. {sendValue} removes this limitation.
     *
     * https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/[Learn more].
     *
     * IMPORTANT: because control is transferred to `recipient`, care must be
     * taken to not create reentrancy vulnerabilities. Consider using
     * {ReentrancyGuard} or the
     * https://solidity.readthedocs.io/en/v0.8.0/security-considerations.html#use-the-checks-effects-interactions-pattern[checks-effects-interactions pattern].
     */
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }

    /**
     * @dev Performs a Solidity function call using a low level `call`. A
     * plain `call` is an unsafe replacement for a function call: use this
     * function instead.
     *
     * If `target` reverts with a revert reason, it is bubbled up by this
     * function (like regular Solidity function calls).
     *
     * Returns the raw returned data. To convert to the expected return value,
     * use https://solidity.readthedocs.io/en/latest/units-and-global-variables.html?highlight=abi.decode#abi-encoding-and-decoding-functions[`abi.decode`].
     *
     * Requirements:
     *
     * - `target` must be a contract.
     * - calling `target` with `data` must not revert.
     *
     * _Available since v3.1._
     */
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, "Address: low-level call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`], but with
     * `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        return functionCallWithValue(target, data, 0, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but also transferring `value` wei to `target`.
     *
     * Requirements:
     *
     * - the calling contract must have an ETH balance of at least `value`.
     * - the called Solidity function must be `payable`.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }

    /**
     * @dev Same as {xref-Address-functionCallWithValue-address-bytes-uint256-}[`functionCallWithValue`], but
     * with `errorMessage` as a fallback revert reason when `target` reverts.
     *
     * _Available since v3.1._
     */
    function functionCallWithValue(
        address target,
        bytes memory data,
        uint256 value,
        string memory errorMessage
    ) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        (bool success, bytes memory returndata) = target.call{value: value}(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(address target, bytes memory data) internal view returns (bytes memory) {
        return functionStaticCall(target, data, "Address: low-level static call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a static call.
     *
     * _Available since v3.3._
     */
    function functionStaticCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        (bool success, bytes memory returndata) = target.staticcall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionDelegateCall(target, data, "Address: low-level delegate call failed");
    }

    /**
     * @dev Same as {xref-Address-functionCall-address-bytes-string-}[`functionCall`],
     * but performing a delegate call.
     *
     * _Available since v3.4._
     */
    function functionDelegateCall(
        address target,
        bytes memory data,
        string memory errorMessage
    ) internal returns (bytes memory) {
        (bool success, bytes memory returndata) = target.delegatecall(data);
        return verifyCallResultFromTarget(target, success, returndata, errorMessage);
    }

    /**
     * @dev Tool to verify that a low level call to smart-contract was successful, and revert (either by bubbling
     * the revert reason or using the provided one) in case of unsuccessful call or if target was not a contract.
     *
     * _Available since v4.8._
     */
    function verifyCallResultFromTarget(
        address target,
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal view returns (bytes memory) {
        if (success) {
            if (returndata.length == 0) {
                // only check isContract if the call was successful and the return data is empty
                // otherwise we already know that it was a contract
                require(isContract(target), "Address: call to non-contract");
            }
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    /**
     * @dev Tool to verify that a low level call was successful, and revert if it wasn't, either by bubbling the
     * revert reason or using the provided one.
     *
     * _Available since v4.3._
     */
    function verifyCallResult(
        bool success,
        bytes memory returndata,
        string memory errorMessage
    ) internal pure returns (bytes memory) {
        if (success) {
            return returndata;
        } else {
            _revert(returndata, errorMessage);
        }
    }

    function _revert(bytes memory returndata, string memory errorMessage) private pure {
        // Look for revert reason and bubble it up if present
        if (returndata.length > 0) {
            // The easiest way to bubble the revert reason is using memory via assembly
            /// @solidity memory-safe-assembly
            assembly {
                let returndata_size := mload(returndata)
                revert(add(32, returndata), returndata_size)
            }
        } else {
            revert(errorMessage);
        }
    }
}

// File: @openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol


// OpenZeppelin Contracts (last updated v4.9.3) (token/ERC20/utils/SafeERC20.sol)





/**
 * @title SafeERC20
 * @dev Wrappers around ERC20 operations that throw on failure (when the token
 * contract returns false). Tokens that return no value (and instead revert or
 * throw on failure) are also supported, non-reverting calls are assumed to be
 * successful.
 * To use this library you can add a `using SafeERC20 for IERC20;` statement to your contract,
 * which allows you to call the safe operations as `token.safeTransfer(...)`, etc.
 */
library SafeERC20 {
    using Address for address;

    /**
     * @dev Transfer `value` amount of `token` from the calling contract to `to`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transfer.selector, to, value));
    }

    /**
     * @dev Transfer `value` amount of `token` from `from` to `to`, spending the approval given by `from` to the
     * calling contract. If `token` returns no value, non-reverting calls are assumed to be successful.
     */
    function safeTransferFrom(IERC20 token, address from, address to, uint256 value) internal {
        _callOptionalReturn(token, abi.encodeWithSelector(token.transferFrom.selector, from, to, value));
    }

    /**
     * @dev Deprecated. This function has issues similar to the ones found in
     * {IERC20-approve}, and its usage is discouraged.
     *
     * Whenever possible, use {safeIncreaseAllowance} and
     * {safeDecreaseAllowance} instead.
     */
    function safeApprove(IERC20 token, address spender, uint256 value) internal {
        // safeApprove should only be called when setting an initial allowance,
        // or when resetting it to zero. To increase and decrease it, use
        // 'safeIncreaseAllowance' and 'safeDecreaseAllowance'
        require(
            (value == 0) || (token.allowance(address(this), spender) == 0),
            "SafeERC20: approve from non-zero to non-zero allowance"
        );
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, value));
    }

    /**
     * @dev Increase the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeIncreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        uint256 oldAllowance = token.allowance(address(this), spender);
        _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance + value));
    }

    /**
     * @dev Decrease the calling contract's allowance toward `spender` by `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful.
     */
    function safeDecreaseAllowance(IERC20 token, address spender, uint256 value) internal {
        unchecked {
            uint256 oldAllowance = token.allowance(address(this), spender);
            require(oldAllowance >= value, "SafeERC20: decreased allowance below zero");
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, oldAllowance - value));
        }
    }

    /**
     * @dev Set the calling contract's allowance toward `spender` to `value`. If `token` returns no value,
     * non-reverting calls are assumed to be successful. Meant to be used with tokens that require the approval
     * to be set to zero before setting it to a non-zero value, such as USDT.
     */
    function forceApprove(IERC20 token, address spender, uint256 value) internal {
        bytes memory approvalCall = abi.encodeWithSelector(token.approve.selector, spender, value);

        if (!_callOptionalReturnBool(token, approvalCall)) {
            _callOptionalReturn(token, abi.encodeWithSelector(token.approve.selector, spender, 0));
            _callOptionalReturn(token, approvalCall);
        }
    }

    /**
     * @dev Use a ERC-2612 signature to set the `owner` approval toward `spender` on `token`.
     * Revert on invalid signature.
     */
    function safePermit(
        IERC20Permit token,
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) internal {
        uint256 nonceBefore = token.nonces(owner);
        token.permit(owner, spender, value, deadline, v, r, s);
        uint256 nonceAfter = token.nonces(owner);
        require(nonceAfter == nonceBefore + 1, "SafeERC20: permit did not succeed");
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     */
    function _callOptionalReturn(IERC20 token, bytes memory data) private {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We use {Address-functionCall} to perform this call, which verifies that
        // the target address contains contract code and also asserts for success in the low-level call.

        bytes memory returndata = address(token).functionCall(data, "SafeERC20: low-level call failed");
        require(returndata.length == 0 || abi.decode(returndata, (bool)), "SafeERC20: ERC20 operation did not succeed");
    }

    /**
     * @dev Imitates a Solidity high-level call (i.e. a regular function call to a contract), relaxing the requirement
     * on the return value: the return value is optional (but if data is returned, it must not be false).
     * @param token The token targeted by the call.
     * @param data The call data (encoded using abi.encode or one of its variants).
     *
     * This is a variant of {_callOptionalReturn} that silents catches all reverts and returns a bool instead.
     */
    function _callOptionalReturnBool(IERC20 token, bytes memory data) private returns (bool) {
        // We need to perform a low level call here, to bypass Solidity's return data size checking mechanism, since
        // we're implementing it ourselves. We cannot use {Address-functionCall} here since this should return false
        // and not revert is the subcall reverts.

        (bool success, bytes memory returndata) = address(token).call(data);
        return
            success && (returndata.length == 0 || abi.decode(returndata, (bool))) && Address.isContract(address(token));
    }
}

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)



/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/GasSavingPool/impl/GSPStorage.sol


/// @notice this contract is used for store state and read state
contract GSPStorage is ReentrancyGuard {

    // ============ Storage for Setup ============
    // _GSP_INITIALIZED_ will be set to true when the init function is called
    bool internal _GSP_INITIALIZED_;
    // GSP does not open TWAP by default
    // _IS_OPEN_TWAP_ can be set to true when the init function is called
    bool public _IS_OPEN_TWAP_ = false;
    
    // ============ Core Address ============
    // _MAINTAINER_ is the maintainer of GSP
    address public _MAINTAINER_;
    // _ADMIN_ can set price
    address public _ADMIN_;
    // _BASE_TOKEN_ and _QUOTE_TOKEN_ should be ERC20 token
    IERC20 public _BASE_TOKEN_;
    IERC20 public _QUOTE_TOKEN_;
    // _BASE_RESERVE_ and _QUOTE_RESERVE_ are the current reserves of the GSP
    uint112 public _BASE_RESERVE_;
    uint112 public _QUOTE_RESERVE_;
    // _BLOCK_TIMESTAMP_LAST_ is used when calculating TWAP
    uint32 public _BLOCK_TIMESTAMP_LAST_;
    // _BASE_PRICE_CUMULATIVE_LAST_ is used when calculating TWAP
    uint256 public _BASE_PRICE_CUMULATIVE_LAST_;

    // _BASE_TARGET_ and _QUOTE_TARGET_ are recalculated when the pool state changes
    uint112 public _BASE_TARGET_;
    uint112 public _QUOTE_TARGET_;
    // _RState_ is the current R state of the GSP
    uint32 public _RState_;

    // ============ Shares (ERC20) ============
    // symbol is the symbol of the shares
    string public symbol;
    // decimals is the decimals of the shares
    uint8 public decimals;
    // name is the name of the shares
    string public name;
    // totalSupply is the total supply of the shares
    uint256 public totalSupply;
    // _SHARES_ is the mapping from account to share balance, record the share balance of each account
    mapping(address => uint256) internal _SHARES_;
    mapping(address => mapping(address => uint256)) internal _ALLOWED_;

    // ================= Permit ======================

    bytes32 public DOMAIN_SEPARATOR;
    // keccak256("Permit(address owner,address spender,uint256 value,uint256 nonce,uint256 deadline)");
    bytes32 public constant PERMIT_TYPEHASH =
        0x6e71edae12b1b97f4d1f60370fef10105fa2faae0126114a169c64845d6126c9;
    mapping(address => uint256) public nonces;

    // ============ Variables for Pricing ============
    // _MT_FEE_RATE_ is the fee rate of mt fee
    uint256 public _MT_FEE_RATE_;
    // _LP_FEE_RATE_ is the fee rate of lp fee
    uint256 public _LP_FEE_RATE_;
    uint256 public _K_;
    uint256 public _I_;
    // _PRICE_LIMIT_ is 1/1000 by default, which is used to limit the setting range of I
    uint256 public _PRICE_LIMIT_ = 1e3;

    // ============ Mt Fee ============
    // _MT_FEE_BASE_ represents the mt fee in base token
    uint256 public _MT_FEE_BASE_;
    // _MT_FEE_QUOTE_ represents the mt fee in quote token
    uint256 public _MT_FEE_QUOTE_;
    // _MT_FEE_RATE_MODEL_ is useless, just for compatible with old version pool
    address public _MT_FEE_RATE_MODEL_ = address(0);

    // ============ Helper Functions ============

    /// @notice Return the PMM state of the pool from inner or outside
    /// @dev B0 and Q0 are calculated in adjustedTarget
    /// @return state The current PMM state
    function getPMMState() public view returns (PMMPricing.PMMState memory state) {
        state.i = _I_;
        state.K = _K_;
        state.B = _BASE_RESERVE_;
        state.Q = _QUOTE_RESERVE_;
        state.B0 = _BASE_TARGET_; // will be calculated in adjustedTarget
        state.Q0 = _QUOTE_TARGET_;
        state.R = PMMPricing.RState(_RState_);
        PMMPricing.adjustedTarget(state);
    }

    /// @notice Return the PMM state variables used for routeHelpers
    /// @return i The price index
    /// @return K The K value
    /// @return B The base token reserve
    /// @return Q The quote token reserve
    /// @return B0 The base token target
    /// @return Q0 The quote token target
    /// @return R The R state of the pool
    function getPMMStateForCall()
        external
        view
        returns (
            uint256 i,
            uint256 K,
            uint256 B,
            uint256 Q,
            uint256 B0,
            uint256 Q0,
            uint256 R
        )
    {
        PMMPricing.PMMState memory state = getPMMState();
        i = state.i;
        K = state.K;
        B = state.B;
        Q = state.Q;
        B0 = state.B0;
        Q0 = state.Q0;
        R = uint256(state.R);
    }

    /// @notice Return the adjusted mid price
    /// @return midPrice The current mid price
    function getMidPrice() public view returns (uint256 midPrice) {
        return PMMPricing.getMidPrice(getPMMState());
    }

    /// @notice Return the total mt fee maintainer can claim
    /// @dev The total mt fee is represented in two types: in base token and in quote token
    /// @return mtFeeBase The total mt fee in base token
    /// @return mtFeeQuote The total mt fee in quote token
    function getMtFeeTotal() public view returns (uint256 mtFeeBase, uint256 mtFeeQuote) {
        mtFeeBase = _MT_FEE_BASE_;
        mtFeeQuote = _MT_FEE_QUOTE_;
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/GasSavingPool/impl/GSPVault.sol


contract GSPVault is GSPStorage {
    using SafeERC20 for IERC20;

    // ============ Modifiers ============
    /// @notice Check whether the caller is maintainer
    modifier onlyMaintainer() {
        require(msg.sender == _MAINTAINER_, "ACCESS_DENIED");
        _;
    }

    /// @notice Check whether the caller is admin
    modifier onlyAdmin() {
        require(msg.sender == _ADMIN_, "ADMIN_ACCESS_DENIED");
        _;
    }

    // ============ Events ============

    event Transfer(address indexed from, address indexed to, uint256 amount);

    event Approval(address indexed owner, address indexed spender, uint256 amount);

    event Mint(address indexed user, uint256 value);

    event Burn(address indexed user, uint256 value);

    // ============ View Functions ============
    /**
     * @notice Get the reserves of the pool
     * @return baseReserve The base token reserve
     * @return quoteReserve The quote token reserve
     */
    function getVaultReserve() external view returns (uint256 baseReserve, uint256 quoteReserve) {
        baseReserve = _BASE_RESERVE_;
        quoteReserve = _QUOTE_RESERVE_;
    }

    /**
     * @notice Get the fee rate of the pool
     * @param user Useless, just keep the same interface with old version pool
     * @return lpFeeRate The lp fee rate
     * @return mtFeeRate The mt fee rate
     */
    function getUserFeeRate(address user) 
        external 
        view 
        returns (uint256 lpFeeRate, uint256 mtFeeRate) 
    {
        lpFeeRate = _LP_FEE_RATE_;
        mtFeeRate = _MT_FEE_RATE_;
    }

    // ============ Asset In ============
    /**
     * @notice Get the amount of base token transferred in
     * @dev The amount of base token input should be the base token reserve minus the mt fee in base token
     * @return input The amount of base token transferred in
     */
    function getBaseInput() public view returns (uint256 input) {
        return _BASE_TOKEN_.balanceOf(address(this)) - uint256(_BASE_RESERVE_) - uint256(_MT_FEE_BASE_);
    }

    /**
     * @notice Get the amount of quote token transferred in
     * @dev The amount of quote token input should be the quote token reserve minus the mt fee in quote token
     * @return input The amount of quote token transferred in
     */
    function getQuoteInput() public view returns (uint256 input) {
        return _QUOTE_TOKEN_.balanceOf(address(this)) - uint256(_QUOTE_RESERVE_) - uint256(_MT_FEE_QUOTE_);
    }

    // ============ Set States ============
    /**
     * @notice Set the reserves of the pool, internal use only
     * @param baseReserve The base token reserve
     * @param quoteReserve The quote token reserve
     */
    function _setReserve(uint256 baseReserve, uint256 quoteReserve) internal {
        // the reserves should be less than the max uint112
        require(baseReserve <= type(uint112).max && quoteReserve <= type(uint112).max, "OVERFLOW");
        _BASE_RESERVE_ = uint112(baseReserve);
        _QUOTE_RESERVE_ = uint112(quoteReserve);
    }

    /**
     * @notice Sync the reserves of the pool, internal use only
     * @dev The balances of the pool should be actual balances minus the mt fee
     */
    function _sync() internal {
        uint256 baseBalance = _BASE_TOKEN_.balanceOf(address(this)) - uint256(_MT_FEE_BASE_);
        uint256 quoteBalance = _QUOTE_TOKEN_.balanceOf(address(this)) - uint256(_MT_FEE_QUOTE_);
        // the reserves should be less than the max uint112
        require(baseBalance <= type(uint112).max && quoteBalance <= type(uint112).max, "OVERFLOW");
        if (baseBalance != _BASE_RESERVE_) {
            _BASE_RESERVE_ = uint112(baseBalance);
        }
        if (quoteBalance != _QUOTE_RESERVE_) {
            _QUOTE_RESERVE_ = uint112(quoteBalance);
        }
    }

    /// @notice Sync the reserves of the pool
    function sync() external nonReentrant {
        _sync();
    }

    /// @notice Correct the rState of the pool, details in pmm algorithm
    function correctRState() public {
        if (_RState_ == uint32(PMMPricing.RState.BELOW_ONE) && _BASE_RESERVE_<_BASE_TARGET_) {
          _RState_ = uint32(PMMPricing.RState.ONE);
          _BASE_TARGET_ = _BASE_RESERVE_;
          _QUOTE_TARGET_ = _QUOTE_RESERVE_;
        }
        if (_RState_ == uint32(PMMPricing.RState.ABOVE_ONE) && _QUOTE_RESERVE_<_QUOTE_TARGET_) {
          _RState_ = uint32(PMMPricing.RState.ONE);
          _BASE_TARGET_ = _BASE_RESERVE_;
          _QUOTE_TARGET_ = _QUOTE_RESERVE_;
        }
    }

    /**
     * @notice PriceLimit is used for oracle change protection
     * @notice It sets a ratio where the relative deviation between the new price and the old price cannot exceed this ratio.
     * @dev The default priceLimit is 1e3, the decimals of priceLimit is 1e6
     * @param priceLimit The new price limit
     */
    function adjustPriceLimit(uint256 priceLimit) external onlyAdmin {
        // the default priceLimit is 1e3
        require(priceLimit <= 1e6, "INVALID_PRICE_LIMIT");
        _PRICE_LIMIT_ = priceLimit;
    }

    /**
     * @notice Adjust oricle price i, only for admin
     */
    function adjustPrice(uint256 i) external onlyAdmin {
        // the difference between i and _I_ should be less than priceLimit
        uint256 offset = i > _I_ ? i - _I_ : _I_ - i;
        require((offset * 1e6 / _I_) <= _PRICE_LIMIT_, "EXCEED_PRICE_LIMIT");
        _I_ = i;
    }

    /**
     * @notice Adjust mtFee rate, only for maintainer
     * @dev The decimals of mtFee rate is 1e18
     * @param mtFeeRate The new mtFee rate
     */
    function adjustMtFeeRate(uint256 mtFeeRate) external onlyMaintainer {
        require(mtFeeRate <= 10**18, "INVALID_MT_FEE_RATE");
        _MT_FEE_RATE_ = mtFeeRate;
    }

    // ============ Asset Out ============
    /**
     * @notice Transfer base token out, internal use only
     * @param to The address of the receiver
     * @param amount The amount of base token to transfer out
     */
    function _transferBaseOut(address to, uint256 amount) internal {
        if (amount > 0) {
            _BASE_TOKEN_.safeTransfer(to, amount);
        }
    }

    /**
     * @notice Transfer quote token out, internal use only
     * @param to The address of the receiver
     * @param amount The amount of quote token to transfer out
     */
    function _transferQuoteOut(address to, uint256 amount) internal {
        if (amount > 0) {
            _QUOTE_TOKEN_.safeTransfer(to, amount);
        }
    }

    /// @notice Maintainer withdraw mtFee, only for maintainer
    function withdrawMtFeeTotal() external nonReentrant onlyMaintainer {
        uint256 mtFeeQuote = _MT_FEE_QUOTE_;
        uint256 mtFeeBase = _MT_FEE_BASE_;
        _MT_FEE_QUOTE_ = 0;
        _transferQuoteOut(_MAINTAINER_, mtFeeQuote);
        _MT_FEE_BASE_ = 0;
        _transferBaseOut(_MAINTAINER_, mtFeeBase);
    }

    // ============ Shares (ERC20) ============

    /**
     * @dev Transfer token for a specified address
     * @param to The address to transfer to.
     * @param amount The amount to be transferred.
     */
    function transfer(address to, uint256 amount) public returns (bool) {
        require(amount <= _SHARES_[msg.sender], "BALANCE_NOT_ENOUGH");

        _SHARES_[msg.sender] = _SHARES_[msg.sender] - (amount);
        _SHARES_[to] = _SHARES_[to] + amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }

    /**
     * @dev Gets the balance of the specified address.
     * @param owner The address to query the the balance of.
     * @return balance An uint256 representing the amount owned by the passed address.
     */
    function balanceOf(address owner) external view returns (uint256 balance) {
        return _SHARES_[owner];
    }

    /**
     * @dev Transfer tokens from one address to another
     * @param from address The address which you want to send tokens from
     * @param to address The address which you want to transfer to
     * @param amount uint256 the amount of tokens to be transferred
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) public returns (bool) {
        require(amount <= _SHARES_[from], "BALANCE_NOT_ENOUGH");
        require(amount <= _ALLOWED_[from][msg.sender], "ALLOWANCE_NOT_ENOUGH");

        _SHARES_[from] = _SHARES_[from] - amount;
        _SHARES_[to] = _SHARES_[to] + amount;
        _ALLOWED_[from][msg.sender] = _ALLOWED_[from][msg.sender] - amount;
        emit Transfer(from, to, amount);
        return true;
    }

    /**
     * @dev Approve the passed address to spend the specified amount of tokens on behalf of msg.sender.
     * @param spender The address which will spend the funds.
     * @param amount The amount of tokens to be spent.
     */
    function approve(address spender, uint256 amount) public returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) private {
        _ALLOWED_[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    /**
     * @dev Function to check the amount of tokens that an owner _ALLOWED_ to a spender.
     * @param owner address The address which owns the funds.
     * @param spender address The address which will spend the funds.
     * @return A uint256 specifying the amount of tokens still available for the spender.
     */
    function allowance(address owner, address spender) public view returns (uint256) {
        return _ALLOWED_[owner][spender];
    }

    function _mint(address user, uint256 value) internal {
        require(value > 1000, "MINT_AMOUNT_NOT_ENOUGH");
        _SHARES_[user] = _SHARES_[user] + value;
        totalSupply = totalSupply + value;
        emit Mint(user, value);
        emit Transfer(address(0), user, value);
    }

    function _burn(address user, uint256 value) internal {
        _SHARES_[user] = _SHARES_[user] - value;
        totalSupply = totalSupply - value;
        emit Burn(user, value);
        emit Transfer(user, address(0), value);
    }

    // ============================ Permit ======================================

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external {
        require(deadline >= block.timestamp, "DODO_GSP_LP: EXPIRED");
        bytes32 digest =
            keccak256(
                abi.encodePacked(
                    "\x19\x01",
                    DOMAIN_SEPARATOR,
                    keccak256(
                        abi.encode(
                            PERMIT_TYPEHASH,
                            owner,
                            spender,
                            value,
                            nonces[owner]++,
                            deadline
                        )
                    )
                )
            );

        address recoveredAddress = ecrecover(digest, v, r, s);
        require(
            recoveredAddress != address(0) && recoveredAddress == owner,
            "DODO_GSP_LP: INVALID_SIGNATURE"
        );
        _approve(owner, spender, value);
    }
}
// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/intf/IDODOCallee.sol

interface IDODOCallee {
    function DVMSellShareCall(
        address sender,
        uint256 burnShareAmount,
        uint256 baseAmount,
        uint256 quoteAmount,
        bytes calldata data
    ) external;

    function DVMFlashLoanCall(
        address sender,
        uint256 baseAmount,
        uint256 quoteAmount,
        bytes calldata data
    ) external;

    function DPPFlashLoanCall(
        address sender,
        uint256 baseAmount,
        uint256 quoteAmount,
        bytes calldata data
    ) external;

    function DSPFlashLoanCall(
        address sender,
        uint256 baseAmount,
        uint256 quoteAmount,
        bytes calldata data
    ) external;

    function CPCancelCall(
        address sender,
        uint256 amount,
        bytes calldata data
    ) external;

	function CPClaimBidCall(
        address sender,
        uint256 baseAmount,
        uint256 quoteAmount,
        bytes calldata data
    ) external;

    function NFTRedeemCall(
        address payable assetTo,
        uint256 quoteAmount,
        bytes calldata
    ) external;
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/GasSavingPool/impl/GSPTrader.sol


/// @notice this contract deal with swap
contract GSPTrader is GSPVault {

    // ============ Events ============

    event DODOSwap(
        address fromToken,
        address toToken,
        uint256 fromAmount,
        uint256 toAmount,
        address trader,
        address receiver
    );

    event DODOFlashLoan(address borrower, address assetTo, uint256 baseAmount, uint256 quoteAmount);

    event RChange(PMMPricing.RState newRState);

    // ============ Trade Functions ============
    /**
     * @notice User sell base tokens, user pay tokens first. Must be used with a router
     * @dev The base token balance is the actual balance minus the mt fee
     * @param to The recipient of the output
     * @return receiveQuoteAmount Amount of quote token received
     */
    function sellBase(address to) external nonReentrant returns (uint256 receiveQuoteAmount) {
        uint256 baseBalance = _BASE_TOKEN_.balanceOf(address(this)) - _MT_FEE_BASE_;
        uint256 baseInput = baseBalance - uint256(_BASE_RESERVE_);
        uint256 mtFee;
        uint256 newBaseTarget;
        PMMPricing.RState newRState;
        // calculate the amount of quote token to receive and mt fee
        (receiveQuoteAmount, mtFee, newRState, newBaseTarget) = querySellBase(tx.origin, baseInput);
        // transfer quote token to recipient
        _transferQuoteOut(to, receiveQuoteAmount);
        // update mt fee in quote token
        _MT_FEE_QUOTE_ = _MT_FEE_QUOTE_ + mtFee;
        

        // update TARGET
        if (_RState_ != uint32(newRState)) {    
            require(newBaseTarget <= type(uint112).max, "OVERFLOW");
            _BASE_TARGET_ = uint112(newBaseTarget);
            _RState_ = uint32(newRState);
            emit RChange(newRState);
        }
        // update reserve
        _setReserve(baseBalance, _QUOTE_TOKEN_.balanceOf(address(this)) - _MT_FEE_QUOTE_);

        emit DODOSwap(
            address(_BASE_TOKEN_),
            address(_QUOTE_TOKEN_),
            baseInput,
            receiveQuoteAmount,
            msg.sender,
            to
        );
    }

    /**
     * @notice User sell quote tokens, user pay tokens first. Must be used with a router
     * @param to The recipient of the output
     * @return receiveBaseAmount Amount of base token received
     */
    function sellQuote(address to) external nonReentrant returns (uint256 receiveBaseAmount) {
        uint256 quoteBalance = _QUOTE_TOKEN_.balanceOf(address(this)) - _MT_FEE_QUOTE_;
        uint256 quoteInput = quoteBalance - uint256(_QUOTE_RESERVE_);
        uint256 mtFee;
        uint256 newQuoteTarget;
        PMMPricing.RState newRState;
        // calculate the amount of base token to receive and mt fee
        (receiveBaseAmount, mtFee, newRState, newQuoteTarget) = querySellQuote(
            tx.origin,
            quoteInput
        );
        // transfer base token to recipient
        _transferBaseOut(to, receiveBaseAmount);
        // update mt fee in base token
        _MT_FEE_BASE_ = _MT_FEE_BASE_ + mtFee;

        // update TARGET
        if (_RState_ != uint32(newRState)) {
            require(newQuoteTarget <= type(uint112).max, "OVERFLOW");
            _QUOTE_TARGET_ = uint112(newQuoteTarget);
            _RState_ = uint32(newRState);
            emit RChange(newRState);
        }
        // update reserve
        _setReserve((_BASE_TOKEN_.balanceOf(address(this)) - _MT_FEE_BASE_), quoteBalance);

        emit DODOSwap(
            address(_QUOTE_TOKEN_),
            address(_BASE_TOKEN_),
            quoteInput,
            receiveBaseAmount,
            msg.sender,
            to
        );
    }

    /**
     * @notice inner flashloan, pay tokens out first, call external contract and check tokens left
     * @param baseAmount The base token amount user require
     * @param quoteAmount The quote token amount user require
     * @param assetTo The address who uses above tokens
     * @param data The external contract's callData
     */
    function flashLoan(
        uint256 baseAmount,
        uint256 quoteAmount,
        address assetTo,
        bytes calldata data
    ) external nonReentrant {
        _transferBaseOut(assetTo, baseAmount);
        _transferQuoteOut(assetTo, quoteAmount);

        if (data.length > 0)
            IDODOCallee(assetTo).DSPFlashLoanCall(msg.sender, baseAmount, quoteAmount, data);

        uint256 baseBalance = _BASE_TOKEN_.balanceOf(address(this)) - _MT_FEE_BASE_;
        uint256 quoteBalance = _QUOTE_TOKEN_.balanceOf(address(this)) - _MT_FEE_QUOTE_;

        // no input -> pure loss
        require(
            baseBalance >= _BASE_RESERVE_ || quoteBalance >= _QUOTE_RESERVE_,
            "FLASH_LOAN_FAILED"
        );

        // sell quote case
        // quote input + base output
        if (baseBalance < _BASE_RESERVE_) {
            uint256 quoteInput = quoteBalance - uint256(_QUOTE_RESERVE_);
            (
                uint256 receiveBaseAmount,
                uint256 mtFee,
                PMMPricing.RState newRState,
                uint256 newQuoteTarget
            ) = querySellQuote(tx.origin, quoteInput); // revert if quoteBalance<quoteReserve
            require(
                (uint256(_BASE_RESERVE_) - baseBalance) <= receiveBaseAmount,
                "FLASH_LOAN_FAILED"
            );
            
            _MT_FEE_BASE_ = _MT_FEE_BASE_ + mtFee;
            
            if (_RState_ != uint32(newRState)) {
                require(newQuoteTarget <= type(uint112).max, "OVERFLOW");
                _QUOTE_TARGET_ = uint112(newQuoteTarget);
                _RState_ = uint32(newRState);
                emit RChange(newRState);
            }
            emit DODOSwap(
                address(_QUOTE_TOKEN_),
                address(_BASE_TOKEN_),
                quoteInput,
                receiveBaseAmount,
                msg.sender,
                assetTo
            );
        }

        // sell base case
        // base input + quote output
        if (quoteBalance < _QUOTE_RESERVE_) {
            uint256 baseInput = baseBalance - uint256(_BASE_RESERVE_);
            (
                uint256 receiveQuoteAmount,
                uint256 mtFee,
                PMMPricing.RState newRState,
                uint256 newBaseTarget
            ) = querySellBase(tx.origin, baseInput); // revert if baseBalance<baseReserve
            require(
                (uint256(_QUOTE_RESERVE_) - quoteBalance) <= receiveQuoteAmount,
                "FLASH_LOAN_FAILED"
            );

            _MT_FEE_QUOTE_ = _MT_FEE_QUOTE_ + mtFee;
            
            if (_RState_ != uint32(newRState)) {
                require(newBaseTarget <= type(uint112).max, "OVERFLOW");
                _BASE_TARGET_ = uint112(newBaseTarget);
                _RState_ = uint32(newRState);
                emit RChange(newRState);
            }
            emit DODOSwap(
                address(_BASE_TOKEN_),
                address(_QUOTE_TOKEN_),
                baseInput,
                receiveQuoteAmount,
                msg.sender,
                assetTo
            );
        }

        _sync();

        emit DODOFlashLoan(msg.sender, assetTo, baseAmount, quoteAmount);
    }

    // ============ Query Functions ============
    /**
     * @notice Return swap result, for query, sellBase side. 
     * @param trader Useless, just to keep the same interface with old version pool
     * @param payBaseAmount The amount of base token user want to sell
     * @return receiveQuoteAmount The amount of quote token user will receive
     * @return mtFee The amount of mt fee charged
     * @return newRState The new RState after swap
     * @return newBaseTarget The new base target after swap
     */
    function querySellBase(address trader, uint256 payBaseAmount)
        public
        view
        returns (
            uint256 receiveQuoteAmount,
            uint256 mtFee,
            PMMPricing.RState newRState,
            uint256 newBaseTarget
        )
    {
        PMMPricing.PMMState memory state = getPMMState();
        (receiveQuoteAmount, newRState) = PMMPricing.sellBaseToken(state, payBaseAmount);

        uint256 lpFeeRate = _LP_FEE_RATE_;
        uint256 mtFeeRate = _MT_FEE_RATE_;
        mtFee = DecimalMath.mulFloor(receiveQuoteAmount, mtFeeRate);
        receiveQuoteAmount = receiveQuoteAmount
            - DecimalMath.mulFloor(receiveQuoteAmount, lpFeeRate)
            - mtFee;
        newBaseTarget = state.B0;
    }
    /**
     * @notice Return swap result, for query, sellQuote side
     * @param trader Useless, just for keeping the same interface with old version pool
     * @param payQuoteAmount The amount of quote token user want to sell
     * @return receiveBaseAmount The amount of base token user will receive
     * @return mtFee The amount of mt fee charged
     * @return newRState The new RState after swap
     * @return newQuoteTarget The new quote target after swap
     */
    function querySellQuote(address trader, uint256 payQuoteAmount)
        public
        view
        returns (
            uint256 receiveBaseAmount,
            uint256 mtFee,
            PMMPricing.RState newRState,
            uint256 newQuoteTarget
        )
    {
        PMMPricing.PMMState memory state = getPMMState();
        (receiveBaseAmount, newRState) = PMMPricing.sellQuoteToken(state, payQuoteAmount);

        uint256 lpFeeRate = _LP_FEE_RATE_;
        uint256 mtFeeRate = _MT_FEE_RATE_;
        mtFee = DecimalMath.mulFloor(receiveBaseAmount, mtFeeRate);
        receiveBaseAmount = receiveBaseAmount
            - DecimalMath.mulFloor(receiveBaseAmount, lpFeeRate)
            - mtFee;
        newQuoteTarget = state.Q0;
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/GasSavingPool/impl/GSPFunding.sol


/// @notice this part focus on Lp tokens, mint and burn
contract GSPFunding is GSPVault {
    // ============ Events ============

    event BuyShares(address to, uint256 increaseShares, uint256 totalShares);

    event SellShares(address payer, address to, uint256 decreaseShares, uint256 totalShares);

    // ============ Buy & Sell Shares ============
    
    /// @notice User mint Lp token and deposit tokens, the result is rounded down
    /// @dev User first transfer baseToken and quoteToken to GSP, then call buyShares
    /// @param to The address will receive shares
    /// @return shares The amount of shares user will receive
    /// @return baseInput The amount of baseToken user transfer to GSP
    /// @return quoteInput The amount of quoteToken user transfer to GSP
    function buyShares(address to)
        external
        nonReentrant
        returns (
            uint256 shares,
            uint256 baseInput,
            uint256 quoteInput
        )
    {
        // The balance of baseToken and quoteToken should be the balance minus the fee
        uint256 baseBalance = _BASE_TOKEN_.balanceOf(address(this)) - _MT_FEE_BASE_;
        uint256 quoteBalance = _QUOTE_TOKEN_.balanceOf(address(this)) - _MT_FEE_QUOTE_;
        // The reserve of baseToken and quoteToken
        uint256 baseReserve = _BASE_RESERVE_;
        uint256 quoteReserve = _QUOTE_RESERVE_;

        // The amount of baseToken and quoteToken user transfer to GSP
        baseInput = baseBalance - baseReserve;
        quoteInput = quoteBalance - quoteReserve;

        // BaseToken should be transferred to GSP before calling buyShares
        require(baseInput > 0, "NO_BASE_INPUT");

        // Round down when withdrawing. Therefore, never be a situation occuring balance is 0 but totalsupply is not 0
        // But May Happen，reserve >0 But totalSupply = 0
        if (totalSupply == 0) {
            // case 1. initial supply
            require(quoteBalance > 0, "ZERO_QUOTE_AMOUNT");
            // The shares will be minted to user
            shares = quoteBalance < DecimalMath.mulFloor(baseBalance, _I_)
                ? DecimalMath.divFloor(quoteBalance, _I_)
                : baseBalance;
            // The target will be updated
            _BASE_TARGET_ = uint112(shares);
            _QUOTE_TARGET_ = uint112(DecimalMath.mulFloor(shares, _I_));
            require(_QUOTE_TARGET_ > 0, "QUOTE_TARGET_IS_ZERO");
            // Lock 1001 shares permanently in first deposit 
            require(shares > 2001, "MINT_AMOUNT_NOT_ENOUGH");
            _mint(address(0), 1001);
            shares -= 1001;
        } else if (baseReserve > 0 && quoteReserve > 0) {
            // case 2. normal case
            uint256 baseInputRatio = DecimalMath.divFloor(baseInput, baseReserve);
            uint256 quoteInputRatio = DecimalMath.divFloor(quoteInput, quoteReserve);
            uint256 mintRatio = quoteInputRatio < baseInputRatio ? quoteInputRatio : baseInputRatio;
            // The shares will be minted to user
            shares = DecimalMath.mulFloor(totalSupply, mintRatio);

            // The target will be updated
            _BASE_TARGET_ = uint112(uint256(_BASE_TARGET_) + (DecimalMath.mulFloor(uint256(_BASE_TARGET_), mintRatio)));
            _QUOTE_TARGET_ = uint112(uint256(_QUOTE_TARGET_) + (DecimalMath.mulFloor(uint256(_QUOTE_TARGET_), mintRatio)));
        }
        // The shares will be minted to user
        // The reserve will be updated
        _mint(to, shares);
        _setReserve(baseBalance, quoteBalance);
        emit BuyShares(to, shares, _SHARES_[to]);
    }

    /// @notice User burn their lp and withdraw their tokens, the result is rounded down
    /// @dev User call sellShares, the calculated baseToken and quoteToken amount should geater than minBaseToken and minQuoteToken
    /// @param shareAmount The amount of shares user want to sell
    /// @param to The address will receive baseToken and quoteToken
    /// @param baseMinAmount The minimum amount of baseToken user want to receive
    /// @param quoteMinAmount The minimum amount of quoteToken user want to receive
    /// @param data The data will be passed to callee contract
    /// @param deadline The deadline of this transaction
    function sellShares(
        uint256 shareAmount,
        address to,
        uint256 baseMinAmount,
        uint256 quoteMinAmount,
        bytes calldata data,
        uint256 deadline
    ) external nonReentrant returns (uint256 baseAmount, uint256 quoteAmount) {
        // The deadline should be greater than current timestamp
        require(deadline >= block.timestamp, "TIME_EXPIRED");
        // The amount of shares user want to sell should be less than user's balance
        require(shareAmount <= _SHARES_[msg.sender], "GLP_NOT_ENOUGH");

        // The balance of baseToken and quoteToken should be the balance minus the fee
        uint256 baseBalance = _BASE_TOKEN_.balanceOf(address(this)) - _MT_FEE_BASE_;
        uint256 quoteBalance = _QUOTE_TOKEN_.balanceOf(address(this)) - _MT_FEE_QUOTE_;
        // The total shares of GSP
        uint256 totalShares = totalSupply;

        // The amount of baseToken and quoteToken user will receive is calculated by the ratio of user's shares to total shares
        baseAmount = baseBalance * shareAmount / totalShares;
        quoteAmount = quoteBalance * shareAmount / totalShares;
        
        // The target will be updated
        _BASE_TARGET_ = uint112(uint256(_BASE_TARGET_) - DecimalMath._divCeil((uint256(_BASE_TARGET_) * (shareAmount)), totalShares));
        _QUOTE_TARGET_ = uint112(uint256(_QUOTE_TARGET_) - DecimalMath._divCeil((uint256(_QUOTE_TARGET_) * (shareAmount)), totalShares));
        
        // The calculated baseToken and quoteToken amount should geater than minBaseToken and minQuoteToken
        require(
            baseAmount >= baseMinAmount && quoteAmount >= quoteMinAmount,
            "WITHDRAW_NOT_ENOUGH"
        );

        // The shares will be burned from user
        // The baseToken and quoteToken will be transferred to user
        // The reserve will be synced
        _burn(msg.sender, shareAmount);
        _transferBaseOut(to, baseAmount);
        _transferQuoteOut(to, quoteAmount);
        _sync();

        // If the data is not empty, the callee contract will be called
        if (data.length > 0) {
            //Same as DVM 
            IDODOCallee(to).DVMSellShareCall(
                msg.sender,
                shareAmount,
                baseAmount,
                quoteAmount,
                data
            );
        }

        emit SellShares(msg.sender, to, shareAmount, _SHARES_[msg.sender]);
    }
}

// File: dodo-gassaving-pool-main/dodo-gassaving-pool-main/contracts/GasSavingPool/impl/GSP.sol

/*

    Copyright 2020 DODO ZOO.
    SPDX-License-Identifier: Apache-2.0

*/

pragma solidity 0.8.16;

/**
 * @title DODO GasSavingPool
 * @author DODO Breeder
 *
 * @notice DODO GasSavingPool initialization
 */
contract GSP is GSPTrader, GSPFunding {
    /**
     * @notice Function will be called in factory, init risk should not be included.
     * @param maintainer The dodo's address, who can claim mtFee and own this pool
     * @param admin oracle owner address, who can set price.
     * @param baseTokenAddress The base token address
     * @param quoteTokenAddress The quote token address
     * @param lpFeeRate The rate of lp fee, with 18 decimal
     * @param mtFeeRate The rate of mt fee, with 18 decimal
     * @param i The oracle price, possible to be changed only by maintainer
     * @param k The swap curve parameter
     * @param isOpenTWAP Useless, always false, just for compatible with old version pool
     */
    function init(
        address maintainer,
        address admin,
        address baseTokenAddress,
        address quoteTokenAddress,
        uint256 lpFeeRate,
        uint256 mtFeeRate,
        uint256 i,
        uint256 k,
        bool isOpenTWAP
    ) external {
        // GSP can only be initialized once
        require(!_GSP_INITIALIZED_, "GSP_INITIALIZED");
        // _GSP_INITIALIZED_ is set to true after initialization
        _GSP_INITIALIZED_ = true;
        // baseTokenAddress and quoteTokenAddress should not be the same
        require(baseTokenAddress != quoteTokenAddress, "BASE_QUOTE_CAN_NOT_BE_SAME");
        // _BASE_TOKEN_ and _QUOTE_TOKEN_ should be valid ERC20 tokens
        _BASE_TOKEN_ = IERC20(baseTokenAddress);
        _QUOTE_TOKEN_ = IERC20(quoteTokenAddress);

        // i should be greater than 0 and less than 10**36
        require(i > 0 && i <= 10**36);
        _I_ = i;
        // k should be greater than 0 and less than 10**18
        require(k <= 10**18);
        _K_ = k;

        // _LP_FEE_RATE_ is set when initialization
        _LP_FEE_RATE_ = lpFeeRate;
        // _MT_FEE_RATE_ is set when initialization
        _MT_FEE_RATE_ = mtFeeRate;
        // _MAINTAINER_ is set when initialization, the address receives the fee
        _MAINTAINER_ = maintainer;
        _ADMIN_ = admin;
        // _IS_OPEN_TWAP_ is always false
        _IS_OPEN_TWAP_ = false;


        string memory connect = "_";
        string memory suffix = "GSP";
        // name of the shares is the combination of suffix, connect and string of the GSP
        name = string(abi.encodePacked(suffix, connect, addressToShortString(address(this))));
        // symbol of the shares is GLP
        symbol = "GLP";
        // decimals of the shares is the same as the base token decimals
        decimals = IERC20Metadata(baseTokenAddress).decimals();
        // initialize DOMAIN_SEPARATOR
        buildDomainSeparator();
        // ==========================================================================
    }

    // ============================== Permit ====================================
    /**
     * @notice DOMAIN_SEPARATOR is used for approve by signature
     */
    function buildDomainSeparator() public returns (bytes32){
        string memory connect = "_";
        string memory suffix = "GSP";
        // name of the shares is the combination of suffix, connect and string of the GSP
        string memory name = string(abi.encodePacked(suffix, connect, addressToShortString(address(this))));

        DOMAIN_SEPARATOR = keccak256(
            abi.encode(
                // keccak256('EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)'),
                0x8b73c3c69bb8fe3d512ecc4cf759cc79239f7b179b0ffacaa9a75d522b39400f,
                keccak256(bytes(name)),
                keccak256(bytes("1")),
                block.chainid,
                address(this)
            )
        );
        return DOMAIN_SEPARATOR;
    }

    /**
     * @notice Convert the address to a shorter string
     * @param _addr The address to convert
     * @return A string representation of _addr in hexadecimal
     */
    function addressToShortString(address _addr) public pure returns (string memory) {
        bytes32 value = bytes32(uint256(uint160(_addr)));
        bytes memory alphabet = "0123456789abcdef";

        bytes memory str = new bytes(8);
        for (uint256 i = 0; i < 4; i++) {
            str[i * 2] = alphabet[uint8(value[i + 12] >> 4)];
            str[1 + i * 2] = alphabet[uint8(value[i + 12] & 0x0f)];
        }
        return string(str);
    }

    // ============ Version Control ============
    /**
     * @notice Return the version of DODOGasSavingPool
     * @return The current version is 1.0.1
     */
    function version() external pure returns (string memory) {
        return "GSP 1.0.1";
    }
}