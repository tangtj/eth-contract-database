# @version 0.3.10

"""
@title Zharta Renting Contract
@author [Zharta](https://zharta.io/)
@notice This contract manages the renting process for NFTs in the LOTM Renting Protocol.
@dev This contract is the single user-facing contract for each Renting Market. It does not hold any NFTs, although it holds the rentals values and the protocol fees (payment tokens). It also manages the creation of vaults (as minimal proxies to the vault implementation) and implements the rental logic. The delegation and staking functionality are implemented in the vaults.
The information regarding listings and rentals was externalized in order to reduce the gas costs while using the protocol. That requires the state to be passed as an argument to each function and validated by matching its hash against the one stored in the contract. Conversly, changes to the state are hashed and stored, and the resulting state variables are either published as events or returned directly to the user.
The information that hold the state (`TokenContext`) consist of the token id, the owner of the NFT and the active rental (`Rental`), which are required to keep the integrity of the contract.
The listings (`SignedListing`) are required arguments for the relevant functions and must be signed by both the owner (EIP-712 type 3) and the protocol admin (EIP-712 type 0). The signature is validated by the contract and requires the signature timestamp to be within 2 minutes of the current timestamp
"""


# Interfaces

from vyper.interfaces import ERC20 as IERC20
from vyper.interfaces import ERC721 as IERC721

interface IVault:
    def initialise(): nonpayable
    def deposit(token_id: uint256, nft_owner: address, delegate: address): nonpayable
    def withdraw(token_id: uint256, wallet: address): nonpayable
    def delegate_to_wallet(delegate: address, expiration: uint256): nonpayable
    def staking_deposit(sender: address, amount: uint256, token_id: uint256, staking_addr: address, pool_method_id: bytes4): nonpayable
    def staking_withdraw(wallet: address, amount: uint256, token_id: uint256, staking_addr: address, pool_method_id: bytes4): nonpayable
    def staking_claim(wallet: address, token_id: uint256, staking_addr: address, pool_method_id: bytes4): nonpayable
    def staking_compound(token_id: uint256, staking_addr: address, pool_claim_method_id: bytes4, pool_deposit_method_id: bytes4): nonpayable


interface ERC721Receiver:
    def onERC721Received(_operator: address, _from: address, _tokenId: uint256, _data: Bytes[1024]) -> bytes4: view

interface RentingERC721:
    def initialise(): nonpayable
    def mint(tokens: DynArray[TokenAndWallet, 32]): nonpayable
    def burn(tokens: DynArray[TokenAndWallet, 32]): nonpayable
    def ownerOf(tokenId: uint256) -> address: view
    def owner_of(tokenId: uint256) -> address: view


# Structs

struct TokenContext:
    token_id: uint256
    nft_owner: address
    active_rental: Rental

struct Rental:
    id: bytes32 # keccak256 of the renter, token_id, start and expiration
    owner: address
    renter: address
    delegate: address
    token_id: uint256
    start: uint256
    min_expiration: uint256
    expiration: uint256
    amount: uint256
    protocol_fee: uint256

struct Listing:
    token_id: uint256
    price: uint256 # price per hour, 0 means not listed
    min_duration: uint256 # min duration in hours
    max_duration: uint256 # max duration in hours, 0 means unlimited
    timestamp: uint256

struct Signature:
    v: uint256
    r: uint256
    s: uint256

struct SignedListing:
    listing: Listing
    owner_signature: Signature
    admin_signature: Signature

struct TokenContextAndListing:
    token_context: TokenContext
    signed_listing: SignedListing
    duration: uint256

struct TokenContextAndAmount:
    token_context: TokenContext
    amount: uint256

struct RentalLog:
    id: bytes32
    vault: address
    owner: address
    token_id: uint256
    start: uint256
    min_expiration: uint256
    expiration: uint256
    amount: uint256
    protocol_fee: uint256

struct RentalExtensionLog:
    id: bytes32
    vault: address
    owner: address
    token_id: uint256
    start: uint256
    min_expiration: uint256
    expiration: uint256
    amount_settled: uint256
    extension_amount: uint256
    protocol_fee: uint256


struct RewardLog:
    token_id: uint256
    active_rental_amount: uint256

struct WithdrawalLog:
    vault: address
    token_id: uint256

struct VaultLog:
    vault: address
    token_id: uint256

struct StakingLog:
    token_id: uint256
    amount: uint256

struct TokenAndWallet:
    token_id: uint256
    wallet: address

# Events

event NftsDeposited:
    owner: address
    nft_contract: address
    vaults: DynArray[VaultLog, 32]
    delegate: address

event NftsWithdrawn:
    owner: address
    nft_contract: address
    total_rewards: uint256
    withdrawals: DynArray[WithdrawalLog, 32]

event DelegatedToWallet:
    owner: address
    delegate: address
    nft_contract: address
    vaults: DynArray[VaultLog, 32]

event RenterDelegatedToWallet:
    renter: address
    delegate: address
    nft_contract: address
    vaults: DynArray[VaultLog, 32]

event ListingsRevoked:
    owner: address
    timestamp: uint256
    token_ids: DynArray[uint256, 32]

event RentalStarted:
    renter: address
    delegate: address
    nft_contract: address
    rentals: DynArray[RentalLog, 32]

event RentalClosed:
    renter: address
    nft_contract: address
    rentals: DynArray[RentalLog, 32]

event RentalExtended:
    renter: address
    nft_contract: address
    rentals: DynArray[RentalExtensionLog, 32]

event RewardsClaimed:
    owner: address
    nft_contract: address
    amount: uint256
    protocol_fee_amount: uint256
    rewards: DynArray[RewardLog, 32]

event TokenOwnershipChanged:
    new_owner: address
    nft_contract: address
    tokens: DynArray[uint256, 32]

event ProtocolFeeSet:
    old_fee: uint256
    new_fee: uint256
    fee_wallet: address

event ProtocolWalletChanged:
    old_wallet: address
    new_wallet: address

event StakingAddressSet:
    old_value: address
    new_value: address

event AdminProposed:
    admin: address
    proposed_admin: address

event OwnershipTransferred:
    old_admin: address
    new_admin: address

event StakingDeposit:
    owner: address
    nft_contract: address
    tokens: DynArray[StakingLog, 32]

event StakingWithdraw:
    owner: address
    nft_contract: address
    recipient: address
    tokens: DynArray[StakingLog, 32]

event StakingClaim:
    owner: address
    nft_contract: address
    recipient: address
    tokens: DynArray[uint256, 32]

event StakingCompound:
    owner: address
    nft_contract: address
    tokens: DynArray[uint256, 32]

event FeesClaimed:
    fee_wallet: address
    amount: uint256


event PauseStateSet:
    old_value: bool
    new_value: bool


# Global Variables

ZHARTA_DOMAIN_NAME: constant(String[6]) = "Zharta"
ZHARTA_DOMAIN_VERSION: constant(String[1]) = "1"

DOMAIN_TYPE_HASH: constant(bytes32) = keccak256("EIP712Domain(string name,string version,uint256 chainId,address verifyingContract)")
LISTING_TYPE_HASH: constant(bytes32) = keccak256("Listing(uint256 token_id,uint256 price,uint256 min_duration,uint256 max_duration,uint256 timestamp)")

_COLLISION_OFFSET: constant(bytes1) = 0xFF
_DEPLOYMENT_CODE: constant(bytes9) = 0x602D3D8160093D39F3
_PRE: constant(bytes10) = 0x363d3d373d3d3d363d73
_POST: constant(bytes15) = 0x5af43d82803e903d91602b57fd5bf3

SUPPORTED_INTERFACES: constant(bytes4[2]) = [0x01ffc9a7, 0x80ac58cd] # ERC165, ERC721

LISTINGS_SIGNATURE_VALID_PERIOD: constant(uint256) = 120

listing_sig_domain_separator: immutable(bytes32)
vault_impl_addr: public(immutable(address))
payment_token: public(immutable(IERC20))
nft_contract_addr: public(immutable(address))
delegation_registry_addr: public(immutable(address))
staking_addr: public(address)
renting_erc721: public(immutable(RentingERC721))
max_protocol_fee: public(immutable(uint256))

protocol_wallet: public(address)
protocol_fee: public(uint256)
protocol_admin: public(address)
proposed_admin: public(address)

rental_states: public(HashMap[uint256, bytes32]) # token_id -> hash(token_context)
listing_revocations: public(HashMap[uint256, uint256]) # token_id -> timestamp

unclaimed_rewards: public(HashMap[address, uint256]) # wallet -> amount
protocol_fees_amount: public(uint256)
paused: public(bool)

##### EXTERNAL METHODS - WRITE #####


@external
def __init__(
    _vault_impl_addr: address,
    _payment_token_addr: address,
    _nft_contract_addr: address,
    _delegation_registry_addr: address,
    _renting_erc721: address,
    _staking_addr: address,
    _max_protocol_fee: uint256,
    _protocol_fee: uint256,
    _protocol_wallet: address,
    _protocol_admin: address
):
    """
    @notice Initialize the renting contract with necessary parameters and addresses.
    @dev Sets up the contract by initializing various addresses and fees.
    @param _vault_impl_addr The address of the vault implementation.
    @param _payment_token_addr The address of the payment token.
    @param _nft_contract_addr The address of the NFT contract.
    @param _delegation_registry_addr The address of the delegation registry.
    @param _renting_erc721 The address of the renting ERC721 contract.
    @param _max_protocol_fee The maximum protocol fee that can be set.
    @param _protocol_fee The initial protocol fee.
    @param _protocol_wallet The wallet to receive protocol fees.
    @param _protocol_admin The administrator of the protocol.
    """

    assert _vault_impl_addr != empty(address), "vault impl is the zero addr"
    assert _payment_token_addr != empty(address), "payment token is the zero addr"
    assert _nft_contract_addr != empty(address), "nft contract is the zero addr"
    assert _delegation_registry_addr != empty(address), "deleg registry is the zero addr"
    assert _renting_erc721 != empty(address), "renting_erc721 is the zero addr"
    assert _max_protocol_fee <= 10000, "max protocol fee > 100%"
    assert _protocol_fee <= _max_protocol_fee, "protocol fee > max fee"
    assert _protocol_wallet != empty(address), "protocol wallet not set"
    assert _protocol_admin != empty(address), "admin wallet not set"

    vault_impl_addr = _vault_impl_addr
    payment_token = IERC20(_payment_token_addr)
    nft_contract_addr = _nft_contract_addr
    delegation_registry_addr = _delegation_registry_addr
    max_protocol_fee = _max_protocol_fee
    renting_erc721 = RentingERC721(_renting_erc721)

    self.staking_addr = _staking_addr
    self.protocol_wallet = _protocol_wallet
    self.protocol_fee = _protocol_fee
    self.protocol_admin = _protocol_admin
    self.paused = False

    listing_sig_domain_separator = keccak256(
        _abi_encode(
            DOMAIN_TYPE_HASH,
            keccak256(ZHARTA_DOMAIN_NAME),
            keccak256(ZHARTA_DOMAIN_VERSION),
            chain.id,
            self
        )
    )

    renting_erc721.initialise()


@external
def delegate_to_wallet(token_contexts: DynArray[TokenContext, 32], delegate: address):

    """
    @notice Delegates multiple NFTs to a wallet while not rented
    @dev Iterates over token contexts to delegate NFTs to a wallet
    @param token_contexts An array of token contexts, each containing the vault state for an NFT.
    @param delegate The address to delegate the NFTs to.
    """

    vault_logs: DynArray[VaultLog, 32] = empty(DynArray[VaultLog, 32])

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert not self._is_rental_active(token_context.active_rental), "active rental"
        assert msg.sender == token_context.nft_owner, "not owner"
        vault: IVault = self._get_vault(token_context.token_id)

        vault.delegate_to_wallet(delegate, max_value(uint256))

        vault_logs.append(VaultLog({vault: vault.address, token_id: token_context.token_id}))

    log DelegatedToWallet(msg.sender, delegate, nft_contract_addr, vault_logs)



@external
def renter_delegate_to_wallet(token_contexts: DynArray[TokenContext, 32], delegate: address):

    """
    @notice Delegates multiple NFTs to a wallet while rented
    @dev Iterates over token contexts to delegate NFTs to a wallet
    @param token_contexts An array of token contexts, each containing the vault state for an NFT.
    @param delegate The address to delegate the NFTs to.
    """

    vault_logs: DynArray[VaultLog, 32] = empty(DynArray[VaultLog, 32])

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert self._is_rental_active(token_context.active_rental), "no active rental"
        assert msg.sender == token_context.active_rental.renter, "not renter"

        vault: IVault = self._get_vault(token_context.token_id)
        vault.delegate_to_wallet(delegate, token_context.active_rental.expiration)

        self._store_token_state(
            token_context.token_id,
            token_context.nft_owner,
            Rental({
                id: token_context.active_rental.id,
                owner: token_context.active_rental.owner,
                renter: token_context.active_rental.renter,
                delegate: delegate,
                token_id: token_context.active_rental.token_id,
                start: token_context.active_rental.start,
                min_expiration: token_context.active_rental.min_expiration,
                expiration: token_context.active_rental.expiration,
                amount: token_context.active_rental.amount,
                protocol_fee: token_context.active_rental.protocol_fee,
            })
        )

        vault_logs.append(VaultLog({vault: vault.address, token_id: token_context.token_id}))

    log RenterDelegatedToWallet(msg.sender, delegate, nft_contract_addr, vault_logs)


@external
def deposit(token_ids: DynArray[uint256, 32], delegate: address):

    """
    @notice Deposits a set of NFTs in vaults (creating them if needed) and sets up delegations
    @dev Iterates over a list of token ids, creating vaults if not needed, transfering the NFTs to the vaults and setting the delegations
    @param token_ids An array of NFT token ids to deposit.
    @param delegate Address to delegate the NFT to while listed.
    """

    self._check_not_paused()
    vault_logs: DynArray[VaultLog, 32] = empty(DynArray[VaultLog, 32])

    for token_id in token_ids:
        assert self.rental_states[token_id] == empty(bytes32), "invalid state"
        vault: IVault = self._create_vault(token_id)
        vault.deposit(token_id, msg.sender, delegate)

        self._store_token_state(token_id, msg.sender, empty(Rental))

        vault_logs.append(VaultLog({
            vault: vault.address,
            token_id: token_id
        }))

    log NftsDeposited(msg.sender, nft_contract_addr, vault_logs, delegate)


@external
def mint(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Mints ERC721 renting tokens for a set of NFTs
    @dev Iterates over a list of token contexts, creating ERC721 renting tokens with matching ids for each NFT
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    """

    tokens: DynArray[TokenAndWallet, 32] = empty(DynArray[TokenAndWallet, 32])

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"

        tokens.append(TokenAndWallet({
            token_id: token_context.token_id,
            wallet: token_context.nft_owner
        }))

    renting_erc721.mint(tokens)


@external
def revoke_listing(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Revokes any existing listings for a set of NFTs
    @dev Iterates over a list of token contexts, revoking listings for each NFT created before the current block timestamp
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    """

    token_ids: DynArray[uint256, 32] = empty(DynArray[uint256, 32])
    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert token_context.nft_owner == msg.sender, "not owner"
        self.listing_revocations[token_context.token_id] = block.timestamp
        token_ids.append(token_context.token_id)
    log ListingsRevoked(msg.sender, block.timestamp, token_ids)


@external
def start_rentals(token_contexts: DynArray[TokenContextAndListing, 32], delegate: address, signature_timestamp: uint256):

    """
    @notice Start rentals for multiple NFTs for the specified duration and delegate them to a wallet
    @dev Iterates over token contexts to begin rentals for each NFT. The rental conditions are evaluated against the matching listing, signed by the owner and the protocol admin. The rental amount is computed and transferred to the protocol wallet and the delegation is created for the given wallet.
    @param token_contexts An array of token contexts, each containing the rental state and signed listing for an NFT.
    @param delegate The address to delegate the NFT to during the rental period.
    @param signature_timestamp The timestamp of the protocol admin signature.
    """

    self._check_not_paused()

    rental_logs: DynArray[RentalLog, 32] = []
    rental_amounts: uint256 = 0

    for context in token_contexts:
        rental_amounts += self._compute_rental_amount(block.timestamp, block.timestamp + context.duration * 3600, context.signed_listing.listing.price)

    self._receive_payment_token(msg.sender, rental_amounts)

    for context in token_contexts:
        vault: IVault = self._get_vault(context.token_context.token_id)
        assert self._is_context_valid(context.token_context), "invalid context"
        assert not self._is_rental_active(context.token_context.active_rental), "active rental"
        assert self._is_within_duration_range(context.signed_listing.listing, context.duration), "duration not respected"
        assert context.signed_listing.listing.price > 0, "listing not active"
        self._check_valid_listing(context.token_context.token_id, context.signed_listing, signature_timestamp, context.token_context.nft_owner)

        expiration: uint256 = block.timestamp + context.duration * 3600
        vault.delegate_to_wallet(delegate if delegate != empty(address) else msg.sender, expiration)

        # store unclaimed rewards
        self._consolidate_claims(context.token_context.token_id, context.token_context.nft_owner, context.token_context.active_rental)

        # create rental
        rental_id: bytes32 = self._compute_rental_id(msg.sender, context.token_context.token_id, block.timestamp, expiration)

        new_rental: Rental = Rental({
            id: rental_id,
            owner: context.token_context.nft_owner,
            renter: msg.sender,
            delegate: delegate,
            token_id: context.token_context.token_id,
            start: block.timestamp,
            min_expiration: block.timestamp + context.signed_listing.listing.min_duration * 3600,
            expiration: expiration,
            amount: self._compute_rental_amount(block.timestamp, expiration, context.signed_listing.listing.price),
            protocol_fee: self.protocol_fee,
        })

        self._store_token_state(context.token_context.token_id, context.token_context.nft_owner, new_rental)

        rental_logs.append(RentalLog({
            id: rental_id,
            vault: vault.address,
            owner: context.token_context.nft_owner,
            token_id: context.token_context.token_id,
            start: block.timestamp,
            min_expiration: new_rental.min_expiration,
            expiration: expiration,
            amount: new_rental.amount,
            protocol_fee: new_rental.protocol_fee,
        }))

    log RentalStarted(msg.sender, delegate, nft_contract_addr, rental_logs)


@external
def close_rentals(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Close rentals for multiple NFTs and claim rewards
    @dev Iterates over token contexts to close rentals for each NFT. The new rental amount is computed pro-rata (considering the minimum duration) and any payback amount transferred to the renter. The protocol fee is computed and accrued and the delegation is revoked.
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    """

    rental_logs: DynArray[RentalLog, 32] = []
    protocol_fees_amount: uint256 = 0
    payback_amounts: uint256 = 0

    for token_context in token_contexts:
        vault: IVault = self._get_vault(token_context.token_id)
        assert self._is_context_valid(token_context), "invalid context"
        assert self._is_rental_active(token_context.active_rental), "active rental does not exist"
        assert msg.sender == token_context.active_rental.renter, "not renter of active rental"

        real_expiration_adjusted: uint256 = block.timestamp
        if block.timestamp < token_context.active_rental.min_expiration:
            real_expiration_adjusted = token_context.active_rental.min_expiration

        pro_rata_rental_amount: uint256 = self._compute_real_rental_amount(
            token_context.active_rental.expiration - token_context.active_rental.start,
            real_expiration_adjusted - token_context.active_rental.start,
            token_context.active_rental.amount
        )
        payback_amount: uint256 = token_context.active_rental.amount - pro_rata_rental_amount
        payback_amounts += payback_amount

        protocol_fee_amount: uint256 = pro_rata_rental_amount * token_context.active_rental.protocol_fee / 10000
        protocol_fees_amount += protocol_fee_amount

        # clear active rental
        self._store_token_state(token_context.token_id, token_context.nft_owner, empty(Rental))

        # set unclaimed rewards
        self.unclaimed_rewards[token_context.nft_owner] += pro_rata_rental_amount - protocol_fee_amount

        # revoke delegation
        vault.delegate_to_wallet(empty(address), 0)

        rental_logs.append(RentalLog({
            id: token_context.active_rental.id,
            vault: vault.address,
            owner: token_context.active_rental.owner,
            token_id: token_context.active_rental.token_id,
            start: token_context.active_rental.start,
            min_expiration: token_context.active_rental.min_expiration,
            expiration: block.timestamp,
            amount: pro_rata_rental_amount,
            protocol_fee: token_context.active_rental.protocol_fee,
        }))

    assert payment_token.transfer(msg.sender, payback_amounts), "transfer failed"

    if protocol_fees_amount > 0:
        self.protocol_fees_amount += protocol_fees_amount

    log RentalClosed(msg.sender, nft_contract_addr, rental_logs)


@external
def extend_rentals(token_contexts: DynArray[TokenContextAndListing, 32], signature_timestamp: uint256):

    """
    @notice Extend rentals for multiple NFTs for the specified duration
    @dev Iterates over token contexts to extend rentals for each NFT. The rental amount is computed pro-rata (considering the minimum duration) and the new rental amount is computed. The difference between the new rental amount and the payback amount is transferred from / to the renter and the new rental protocol fee is computed and accrued.
    @param token_contexts An array of token contexts, each containing the rental state and signed listing for an NFT.
    @param signature_timestamp The timestamp of the protocol admin signature.
    """

    rental_logs: DynArray[RentalExtensionLog, 32] = []
    protocol_fees_amount: uint256 = 0
    payback_amounts: uint256 = 0
    extension_amounts: uint256 = 0

    for context in token_contexts:
        vault: IVault = self._get_vault(context.token_context.token_id)
        assert self._is_context_valid(context.token_context), "invalid context"
        assert self._is_rental_active(context.token_context.active_rental), "no active rental"
        assert msg.sender == context.token_context.active_rental.renter, "not renter of active rental"

        assert self._is_within_duration_range(context.signed_listing.listing, context.duration), "duration not respected"
        assert context.signed_listing.listing.price > 0, "listing not active"
        self._check_valid_listing(context.token_context.token_id, context.signed_listing, signature_timestamp, context.token_context.nft_owner)

        expiration: uint256 = block.timestamp + context.duration * 3600
        real_expiration_adjusted: uint256 = block.timestamp
        if block.timestamp < context.token_context.active_rental.min_expiration:
            real_expiration_adjusted = context.token_context.active_rental.min_expiration

        pro_rata_rental_amount: uint256 = self._compute_real_rental_amount(
            context.token_context.active_rental.expiration - context.token_context.active_rental.start,
            real_expiration_adjusted - context.token_context.active_rental.start,
            context.token_context.active_rental.amount
        )
        new_rental_amount: uint256 = self._compute_rental_amount(block.timestamp, expiration, context.signed_listing.listing.price)
        extension_amounts += new_rental_amount

        payback_amount: uint256 = context.token_context.active_rental.amount - pro_rata_rental_amount
        payback_amounts += payback_amount

        protocol_fee_amount: uint256 = pro_rata_rental_amount * context.token_context.active_rental.protocol_fee / 10000
        protocol_fees_amount += protocol_fee_amount

        new_rental: Rental = Rental({
            id: context.token_context.active_rental.id,
            owner: context.token_context.nft_owner,
            renter: msg.sender,
            delegate: context.token_context.active_rental.delegate,
            token_id: context.token_context.token_id,
            start: block.timestamp,
            min_expiration: block.timestamp + context.signed_listing.listing.min_duration * 3600,
            expiration: expiration,
            amount: new_rental_amount,
            protocol_fee: self.protocol_fee,
        })
        # clear active rental
        self._store_token_state(context.token_context.token_id, context.token_context.nft_owner, new_rental)

        # set unclaimed rewards
        self.unclaimed_rewards[context.token_context.nft_owner] += pro_rata_rental_amount - protocol_fee_amount

        rental_logs.append(RentalExtensionLog({
            id: context.token_context.active_rental.id,
            vault: vault.address,
            owner: context.token_context.active_rental.owner,
            token_id: context.token_context.active_rental.token_id,
            start: block.timestamp,
            min_expiration: block.timestamp + context.signed_listing.listing.min_duration * 3600,
            expiration: expiration,
            amount_settled: pro_rata_rental_amount,
            extension_amount: new_rental_amount,
            protocol_fee: context.token_context.active_rental.protocol_fee,
        }))

    if payback_amounts > extension_amounts:
        self._transfer_payment_token(msg.sender, payback_amounts - extension_amounts)
    elif payback_amounts < extension_amounts:
        self._receive_payment_token(msg.sender, extension_amounts - payback_amounts)

    if protocol_fees_amount > 0:
        self.protocol_fees_amount += protocol_fees_amount

    log RentalExtended(msg.sender, nft_contract_addr, rental_logs)


@external
def withdraw(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Withdraw multiple NFTs and claim rewards
    @dev Iterates over token contexts to withdraw NFTs from their vaults and claim any unclaimed rewards, while also burning the matching ERC721 renting token.
    @param token_contexts An array of token contexts, each containing the vault state for an NFT.
    """


    withdrawal_log: DynArray[WithdrawalLog, 32] = empty(DynArray[WithdrawalLog, 32])
    tokens: DynArray[TokenAndWallet, 32] = empty(DynArray[TokenAndWallet, 32])
    total_rewards: uint256 = 0

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert not self._is_rental_active(token_context.active_rental), "active rental"
        token_owner: address = renting_erc721.owner_of(token_context.token_id)
        if token_owner != empty(address):
            assert msg.sender == token_owner, "not owner"
        else:
            assert msg.sender == token_context.nft_owner, "not owner"

        vault: IVault = self._get_vault(token_context.token_id)

        self._consolidate_claims(token_context.token_id, token_context.nft_owner, token_context.active_rental, False)

        self._clear_token_state(token_context.token_id)

        tokens.append(TokenAndWallet({
            token_id: token_context.token_id,
            wallet: token_context.nft_owner
        }))

        vault.withdraw(token_context.token_id, msg.sender)
        self.listing_revocations[token_context.token_id] = block.timestamp

        withdrawal_log.append(WithdrawalLog({
            vault: vault.address,
            token_id: token_context.token_id,
        }))

    renting_erc721.burn(tokens)

    rewards_to_claim: uint256 = self.unclaimed_rewards[msg.sender]

    # transfer reward to nft owner
    if rewards_to_claim > 0:
        self.unclaimed_rewards[msg.sender] = 0
        self._transfer_payment_token(msg.sender, rewards_to_claim)

    log NftsWithdrawn(
        msg.sender,
        nft_contract_addr,
        rewards_to_claim,
        withdrawal_log
    )


@external
def stake_deposit(token_contexts: DynArray[TokenContextAndAmount, 32], pool_method_id: bytes4):

    """
    @notice Deposit the given amounts for multiple NFTs in the configured staking pool
    @dev Iterates over token contexts to deposit the given amounts for each NFT in the staking pool
    @param token_contexts An array of token contexts paired with amounts, each containing the rental state for an NFT.
    @param pool_method_id The method id to call on the staking pool to deposit the given amounts.
    """

    self._check_not_paused()
    staking_addr: address = self.staking_addr
    assert staking_addr != empty(address), "staking not supported"

    staking_log: DynArray[StakingLog, 32] = empty(DynArray[StakingLog, 32])

    for context in token_contexts:
        assert msg.sender == context.token_context.nft_owner, "not owner"
        assert self._is_context_valid(context.token_context), "invalid context"

        vault: IVault = self._get_vault(context.token_context.token_id)
        assert payment_token.transferFrom(msg.sender, vault.address, context.amount), "transferFrom failed"
        vault.staking_deposit(msg.sender, context.amount, context.token_context.token_id, staking_addr, pool_method_id)
        staking_log.append(StakingLog({
            token_id: context.token_context.token_id,
            amount: context.amount
        }))

    log StakingDeposit(msg.sender, nft_contract_addr, staking_log)


@external
def stake_withdraw(token_contexts: DynArray[TokenContextAndAmount, 32], recipient: address, pool_method_id: bytes4):

    """
    @notice Withdraw the given amounts for multiple NFTs from the configured staking pool
    @dev Iterates over token contexts to withdraw the given amounts for each NFT from the staking pool
    @param token_contexts An array of token contexts paired with amounts, each containing the rental state for an NFT.
    @param recipient The address to receive the withdrawn amounts.
    @param pool_method_id The method id to call on the staking pool to withdraw the given amounts.
    """

    staking_addr: address = self.staking_addr
    assert staking_addr != empty(address), "staking not supported"

    staking_log: DynArray[StakingLog, 32] = empty(DynArray[StakingLog, 32])

    for context in token_contexts:
        assert msg.sender == context.token_context.nft_owner, "not owner"
        assert self._is_context_valid(context.token_context), "invalid context"

        self._get_vault(context.token_context.token_id).staking_withdraw(recipient, context.amount, context.token_context.token_id, staking_addr, pool_method_id)
        staking_log.append(StakingLog({
            token_id: context.token_context.token_id,
            amount: context.amount
        }))

    log StakingWithdraw(msg.sender, nft_contract_addr, recipient, staking_log)


@external
def stake_claim(token_contexts: DynArray[TokenContextAndAmount, 32], recipient: address, pool_method_id: bytes4):

    """
    @notice Claim the rewards for multiple NFTs from the configured staking pool
    @dev Iterates over token contexts to claim the rewards for each NFT from the staking pool
    @param token_contexts An array of token contexts paired with amounts, each containing the rental state for an NFT.
    @param recipient The address to receive the claimed rewards.
    @param pool_method_id The method id to call on the staking pool to claim the rewards.
    """

    staking_addr: address = self.staking_addr
    assert staking_addr != empty(address), "staking not supported"
    tokens: DynArray[uint256, 32] = empty(DynArray[uint256, 32])

    for context in token_contexts:
        assert msg.sender == context.token_context.nft_owner, "not owner"
        assert self._is_context_valid(context.token_context), "invalid context"
        self._get_vault(context.token_context.token_id).staking_claim(recipient, context.token_context.token_id, staking_addr, pool_method_id)
        tokens.append(context.token_context.token_id)

    log StakingClaim(msg.sender, nft_contract_addr, recipient, tokens)


@external
def stake_compound(token_contexts: DynArray[TokenContextAndAmount, 32], pool_claim_method_id: bytes4, pool_deposit_method_id: bytes4):

    """
    @notice Compound the rewards for multiple NFTs in the configured staking pool
    @dev Iterates over token contexts to compound the rewards for each NFT in the staking pool
    @param token_contexts An array of token contexts paired with amounts, each containing the rental state for an NFT.
    @param pool_claim_method_id The method id to call on the staking pool to claim the rewards.
    @param pool_deposit_method_id The method id to call on the staking pool to deposit the rewards.
    """

    self._check_not_paused()
    staking_addr: address = self.staking_addr
    assert staking_addr != empty(address), "staking not supported"
    tokens: DynArray[uint256, 32] = empty(DynArray[uint256, 32])

    for context in token_contexts:
        assert msg.sender == context.token_context.nft_owner, "not owner"
        assert self._is_context_valid(context.token_context), "invalid context"

        self._get_vault(context.token_context.token_id).staking_compound(context.token_context.token_id, staking_addr, pool_claim_method_id, pool_deposit_method_id)
        tokens.append(context.token_context.token_id)

    log StakingCompound(msg.sender, nft_contract_addr, tokens)


@external
def claim(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Claim the rental rewards for multiple NFTs
    @dev Iterates over token contexts to claim rewards for each expired rental. The rental rewards and any previous unclaimed rewards are transferred to the NFT owner and the protocol fees are accrued.
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    """

    reward_logs: DynArray[RewardLog, 32] = []

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert token_context.nft_owner == msg.sender, "not owner"

        result_active_rental: Rental = self._consolidate_claims(token_context.token_id, token_context.nft_owner, token_context.active_rental)

        reward_logs.append(RewardLog({
            token_id: token_context.token_id,
            active_rental_amount: result_active_rental.amount
        }))

    rewards_to_claim: uint256 = self.unclaimed_rewards[msg.sender]

    # transfer reward to nft owner
    assert rewards_to_claim > 0, "no rewards to claim"
    assert payment_token.transfer(msg.sender, rewards_to_claim), "transfer failed"
    self.unclaimed_rewards[msg.sender] = 0

    log RewardsClaimed(msg.sender, nft_contract_addr, rewards_to_claim, self.protocol_fees_amount, reward_logs)


@view
@external
def claimable_rewards(nft_owner: address, token_contexts: DynArray[TokenContext, 32]) -> uint256:

    """
    @notice Compute the claimable rewards for a given NFT owner
    @dev Iterates over token contexts to compute the claimable rewards for each expired rental, wich are then summed up to any previous unclaimed rewards.
    @param nft_owner The address of the NFT owner.
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    @return The claimable rewards for the given NFT owner.
    """

    rewards: uint256 = self.unclaimed_rewards[nft_owner]
    for context in token_contexts:
        assert self._is_context_valid(context), "invalid context"
        assert context.nft_owner == nft_owner, "not owner"
        if context.active_rental.expiration < block.timestamp:
            rewards += context.active_rental.amount * (10000 - context.active_rental.protocol_fee) / 10000
    return rewards


@external
def claim_token_ownership(token_contexts: DynArray[TokenContext, 32]):

    """
    @notice Allow the owner of rental ERC721 tokens to claim the ownership of the underlying NFTs
    @dev Iterates over token contexts to claim the ownership of each NFT. The ownership is transferred to the NFT owner.
    @param token_contexts An array of token contexts, each containing the rental state for an NFT.
    """

    tokens: DynArray[uint256, 32] = empty(DynArray[uint256, 32])

    for token_context in token_contexts:
        assert self._is_context_valid(token_context), "invalid context"
        assert renting_erc721.ownerOf(token_context.token_id) == msg.sender, "not owner"
        self._store_token_state(token_context.token_id, msg.sender, token_context.active_rental)
        tokens.append(token_context.token_id)

    log TokenOwnershipChanged(msg.sender, nft_contract_addr, tokens)


@external
def claim_fees():

    """
    @notice Claim the accrued protocol fees
    @dev Transfers the accrued protocol fees to the protocol wallet and logs the event.
    """

    assert msg.sender == self.protocol_admin, "not admin"
    protocol_fees_amount: uint256 = self.protocol_fees_amount
    self.protocol_fees_amount = 0
    self._transfer_payment_token(self.protocol_wallet, protocol_fees_amount)
    log FeesClaimed(self.protocol_wallet, protocol_fees_amount)


@external
def set_protocol_fee(protocol_fee: uint256):

    """
    @notice Set the protocol fee
    @dev Sets the protocol fee to the given value and logs the event. Admin function.
    @param protocol_fee The new protocol fee.
    """

    assert msg.sender == self.protocol_admin, "not protocol admin"
    assert protocol_fee <= max_protocol_fee, "protocol fee > max fee"

    log ProtocolFeeSet(self.protocol_fee, protocol_fee, self.protocol_wallet)
    self.protocol_fee = protocol_fee


@external
def change_protocol_wallet(new_protocol_wallet: address):

    """
    @notice Change the protocol wallet
    @dev Changes the protocol wallet to the given address and logs the event. Admin function.
    @param new_protocol_wallet The new protocol wallet.
    """

    assert msg.sender == self.protocol_admin, "not protocol admin"
    assert new_protocol_wallet != empty(address), "wallet is the zero address"

    log ProtocolWalletChanged(self.protocol_wallet, new_protocol_wallet)
    self.protocol_wallet = new_protocol_wallet


@external
def set_paused(paused: bool):

    """
    @notice Pause or unpause the contract
    @dev Pauses or unpauses the contract and logs the event. Admin function.
    @param paused The new paused state.
    """

    assert msg.sender == self.protocol_admin, "not protocol admin"

    log PauseStateSet(self.paused, paused)

    self.paused = paused


@external
def set_staking_addr(staking_addr: address):

    """
    @notice Set the staking pool address
    @dev Sets the staking pool address to the given value and logs the event. Admin function.
    @param staking_addr The new staking pool address.
    """

    assert msg.sender == self.protocol_admin, "not protocol admin"
    log StakingAddressSet(self.staking_addr, staking_addr)
    self.staking_addr = staking_addr


@external
def propose_admin(_address: address):

    """
    @notice Propose a new admin
    @dev Proposes a new admin and logs the event. Admin function.
    @param _address The address of the proposed admin.
    """

    assert msg.sender == self.protocol_admin, "not the admin"
    assert _address != empty(address), "_address is the zero address"

    log AdminProposed(self.protocol_admin, _address)
    self.proposed_admin = _address


@external
def claim_ownership():

    """
    @notice Claim the ownership of the contract
    @dev Claims the ownership of the contract and logs the event. Requires the caller to be the proposed admin.
    """

    assert msg.sender == self.proposed_admin, "not the proposed"

    log OwnershipTransferred(self.protocol_admin, self.proposed_admin)
    self.protocol_admin = self.proposed_admin
    self.proposed_admin = empty(address)


@view
@external
def tokenid_to_vault(token_id: uint256) -> address:

    """
    @notice Get the vault address for a given token id
    @dev Computes the vault address for the given token id and returns it.
    @param token_id The token id.
    @return The vault address for the given token id.
    """

    return self._tokenid_to_vault(token_id)


@view
@internal
def _tokenid_to_vault(token_id: uint256) -> address:
    return self._compute_address(
        convert(token_id, bytes32),
        keccak256(concat(
            _DEPLOYMENT_CODE,
            _PRE,
            convert(vault_impl_addr, bytes20),
            _POST
        )),
        self
    )


@pure
@internal
def _state_hash(token_id: uint256, nft_owner: address, rental: Rental) -> bytes32:
    return keccak256(
        concat(
            convert(token_id, bytes32),
            convert(nft_owner, bytes32),
            rental.id,
            convert(rental.owner, bytes32),
            convert(rental.renter, bytes32),
            convert(rental.delegate, bytes32),  #should this be part of state?
            convert(rental.token_id, bytes32),
            convert(rental.start, bytes32),
            convert(rental.min_expiration, bytes32),
            convert(rental.expiration, bytes32),
            convert(rental.amount, bytes32),
            convert(rental.protocol_fee, bytes32),
        )
    )


@pure
@internal
def _compute_address(salt: bytes32, bytecode_hash: bytes32, deployer: address) -> address:
    data: bytes32 = keccak256(concat(_COLLISION_OFFSET, convert(deployer, bytes20), salt, bytecode_hash))
    return self._convert_keccak256_2_address(data)


@pure
@internal
def _convert_keccak256_2_address(digest: bytes32) -> address:
    return convert(convert(digest, uint256) & convert(max_value(uint160), uint256), address)


@view
@internal
def _is_rental_active(rental: Rental) -> bool:
    return rental.expiration > block.timestamp


@view
@internal
def _is_context_valid(context: TokenContext) -> bool:
    """ Check if the context is valid, also meaning that the token is deposited """
    return self.rental_states[context.token_id] == self._state_hash(context.token_id, context.nft_owner, context.active_rental)


@internal
def _store_token_state(token_id: uint256, nft_owner: address, rental: Rental):
    self.rental_states[token_id] = self._state_hash(token_id, nft_owner, rental)


@internal
def _clear_token_state(token_id: uint256):
    self.rental_states[token_id] = empty(bytes32)


@internal
def _get_vault(token_id: uint256) -> IVault:
    vault: address = self._tokenid_to_vault(token_id)
    assert vault.is_contract, "no vault exists for token_id"
    return IVault(vault)


@internal
def _create_vault(token_id: uint256) -> IVault:
    # only creates a vault if needed
    vault: address = self._tokenid_to_vault(token_id)
    if not vault.is_contract:
        vault = create_minimal_proxy_to(vault_impl_addr, salt=convert(token_id, bytes32))
        IVault(vault).initialise()

    return IVault(vault)


@internal
def _transfer_payment_token(_to: address, _amount: uint256):
    assert payment_token.transfer(_to, _amount), "transferFrom failed"


@internal
def _receive_payment_token(_from: address, _amount: uint256):
    assert payment_token.transferFrom(_from, self, _amount), "transferFrom failed"


@pure
@internal
def _compute_rental_id(renter: address, token_id: uint256, start: uint256, expiration: uint256) -> bytes32:
    return keccak256(concat(convert(renter, bytes32), convert(token_id, bytes32), convert(start, bytes32), convert(expiration, bytes32)))

@pure
@internal
def _compute_rental_amount(start: uint256, expiration: uint256, price: uint256) -> uint256:
    return (expiration - start) * price / 3600


@pure
@internal
def _compute_real_rental_amount(duration: uint256, real_duration: uint256, rental_amount: uint256) -> uint256:
    return rental_amount * real_duration / duration


@internal
def _check_not_paused():
    assert not self.paused, "paused"


@internal
def _consolidate_claims(token_id: uint256, nft_owner: address, active_rental: Rental, store_state: bool = True) -> Rental:
    if active_rental.amount == 0 or active_rental.expiration >= block.timestamp:
        return active_rental
    else:
        protocol_fee_amount: uint256 = active_rental.amount * active_rental.protocol_fee / 10000

        self.unclaimed_rewards[active_rental.owner] += active_rental.amount - protocol_fee_amount
        self.protocol_fees_amount += protocol_fee_amount

        new_rental: Rental = Rental({
            id: active_rental.id,
            owner: active_rental.owner,
            renter: active_rental.renter,
            delegate: active_rental.delegate,
            token_id: token_id,
            start: active_rental.start,
            min_expiration: active_rental.min_expiration,
            expiration: active_rental.expiration,
            amount: 0,
            protocol_fee: active_rental.protocol_fee,
        })

        if store_state:
            self._store_token_state(token_id, nft_owner, new_rental)

        return new_rental


@internal
def _check_valid_listing(token_id: uint256, signed_listing: SignedListing, signature_timestamp:uint256, nft_owner: address):
    assert token_id == signed_listing.listing.token_id, "invalid token_id"
    assert self._is_listing_signed_by_owner(signed_listing, nft_owner), "invalid owner signature"
    assert self._is_listing_signed_by_admin(signed_listing, signature_timestamp), "invalid admin signature"
    assert signature_timestamp + LISTINGS_SIGNATURE_VALID_PERIOD > block.timestamp, "listing expired"
    assert self.listing_revocations[signed_listing.listing.token_id] < signed_listing.listing.timestamp, "listing revoked"


@internal
def _is_within_duration_range(listing: Listing, duration: uint256) -> bool:
    return duration >= listing.min_duration and (listing.max_duration == 0 or duration <= listing.max_duration)


@internal
def _is_listing_signed_by_owner(signed_listing: SignedListing, owner: address) -> bool:
    return ecrecover(
        keccak256(
            concat(
                convert("\x19\x01", Bytes[2]),
                _abi_encode(
                    listing_sig_domain_separator,
                    keccak256(_abi_encode(LISTING_TYPE_HASH, signed_listing.listing))
                )
            )
        ),
        signed_listing.owner_signature.v,
        signed_listing.owner_signature.r,
        signed_listing.owner_signature.s
    ) == owner


@internal
def _is_listing_signed_by_admin(signed_listing: SignedListing, signature_timestamp: uint256) -> bool:
    return ecrecover(
        keccak256(
            concat(
                convert("\x19\x00", Bytes[2]),
                convert(self, bytes20),
                keccak256(_abi_encode(signed_listing.owner_signature)),
                convert(signature_timestamp, bytes32)
            )
        ),
        signed_listing.admin_signature.v,
        signed_listing.admin_signature.r,
        signed_listing.admin_signature.s
    ) == self.protocol_admin