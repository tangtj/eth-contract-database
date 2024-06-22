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

pragma solidity ^0.8.0;

interface IVoteWeightSource {

    // Function to get the index and vote weight based on a timestamp
    function GetIndexAndVoteWeight(
        address user,
        uint40 inputTimestamp
    ) external view returns (bool isValid, uint64 index, uint256 voteWeight);

    // Low gas alternative to GetIndexAndVoteWeight for validating and retrieving vote weight
    function GetAndValidateVoteWeight(
        address user,
        uint64 indexUint64,
        uint40 inputTimestampUint40
    ) external view returns (bool isValid, uint96 voteWeight);
}

// votedPower is 0 if < 1M votes, else 1 if < 3M votes, else 2 if less than 10M votes, 
//   else 3 if < 30M, else 4 if < 100M, else 5 if < 300M, else 6.  Thus voted power is sort of like a decibels metric.
// Hopefully this enables efficient spam filtration and also polls that start with high votePower have higher
// pollNumber. (top 3 bits of 40-bit number are the initial votedPower upon creation, 
// if creator both creates the poll and votes on it in one transaction).  There is a gas optimization
// for voters that vote on polls and they voted on recent polls of the same votePower.  Therefore it
// can cost less to vote on polls that start off above the spam filter (e.g. at least above 0 in power).
// Which helps facilitate more voting by reducing gas costs of those votes.  This is because
// the cheapest way to vote will be to vote all votePower in a single vote, these vote records will 
// be stored in-order in an array indexed by the votePower.  The voteRecord is only the 4-bits of which
// which option was chosen (add 1 to it, so it is non-zero for true vote).  So a 256-bit word can 
// hold 64 votes.  

// votedOption has 1 of 16 values:
// 0 means unvoted
// 1 means voted through the partial-vote mechanism
// 2 means Reserved/(Previosly used for Invalid Proposal) (Option 0)
// 3 means Active Abstain (Option 1)
// 4 means Poll Creator-defined Option 0 (Option 2)
// 5 means Poll Creator-defined option 1 (Option 3)
// 6 means Poll Creator-defined option 2 (Option 4)
// 7 means Poll Creator-defined option 3 (Option 5)
// 8 means Poll Creator-defined option 4 (Option 6)
// 9 means Poll Creator-defined option 5 (Option 7)
// 10 means Poll Creator-defined option 6 (Option 8)
// 11 means Poll Creator-defined option 7 (Option 9)
// 12 means Poll Creator-defined option 8 (Option 10)
// 13 means Poll Creator-defined option 9 (Option 11)
// 14 means Poll Creator-defined option 10 (Option 12)
// 15 means Poll Creator-defined option 11 (Option 13)

// We will not actually let people vote Invalid Proposal explicitly, since we have changed the 
// design to use a flag so any vote can also vote invalid proposal.  But we keep Invalid Proposal
// reserved here in case we need a flag late in design and don't want to have to change so much code.

// The vote weight that is voted can be read from the voteWeight contract so
// the amount voted does not need to be stored when full vote weight is being voted.

// The sum gas for votes at votePowers the voter has previously voted on, where the voter votes
// frequently on votes of that vote power is 21k (base transaction) + ~5200 (Get and Verify voteWeight)
// plus read and modify non-zero storage (5100) (check if already voted) plus read and modify non-zero storage
// (5100) (update vote total for selected options), plus some extra calldata (~200), plus emitting an event (~700).
// Thus, gas is trending for frequent voters on polls with non-spammed votePowers, of 37,300.  So less than 2x the minimum (< 42k)

contract Poll {

    //DEBUG_TEST:
    // DO THIS!!!!! : SET ALL block.timestamp to debug_time in this file.
    //uint256 public debug_time;
    //address public owner;
    function debug_time() external view returns( uint256 ){
        return block.timestamp;
    }

    mapping(address => address) public delegationOffers;
    mapping(address => address) public delegationAcceptances;

    //Vocabulary: Delegator is the person letting another account vote with their vote power.
    // Delegator is also called the offeror.  The delegator makes the offer of delegation.
    //Delegatee, also called delegate or acceptor, is the person who receives the ability to vote
    // with the vote power of a different account.  The delegate accepts the o
    // Both parties can cancel a delegation which deletes the offer and, if the offer was accepted
    // it deletes the acceptance.

    //Here's how delegation works.  It requires two transactions, an offer and acceptance
    // During offer, the delegationOffers[offeror] is set to the delegate's address
    // During accpetance, delegationAcceptances[delegate] is set to offeror's address.
    // Offering to a different delegate when another delegate already accepted
    // cancels the offer (and acceptance if it was accepted), so that the original
    // delegator's delegation is removed.
    // Accepting an offer requires the acceptor to not already have an accepted delegation in place
    // (otherwise the acceptance transaction will fail).  The acceptor (delegate) can remove the
    // previous delegation by calling cancelDelegationAcceptance.  This also deletes the offer
    // that they accepted (so they cannot re-accept it later unless it is re-offered).

    function offerDelegation(address offerDelegationTo) external {
        cancelDelegationOffer();
        delegationOffers[msg.sender] = offerDelegationTo;
    }

    // Succeeds even if there is no delegation to remove.
    function cancelDelegationOffer() public {
        address previousOffer = delegationOffers[msg.sender];
        if(previousOffer != address(0)) {
            delegationOffers[msg.sender] = address(0);
            if( delegationAcceptances[previousOffer] == msg.sender ) {
                delegationAcceptances[previousOffer] = address(0);
            }
        }
    }

    // Requires you haven't already accepted. 
    function accept(address acceptDelegationFrom) external {
        address offer = delegationOffers[acceptDelegationFrom];
        require( offer == msg.sender, "No offer to accept" ); 
        address previousAcceptance = delegationAcceptances[msg.sender];
        require( previousAcceptance == address(0), "Delegation exists. Cancel first");
        delegationAcceptances[msg.sender] = acceptDelegationFrom;
    }

    function cancelDelegationAcceptance() external {
        address previousAcceptance = delegationAcceptances[msg.sender];
        require(previousAcceptance != address(0), "No delegation to cancel");
        // for extra safety we put a check here although the state machine makes it so this shouldn't be required.
        if(delegationOffers[previousAcceptance] == msg.sender) {
            delegationOffers[previousAcceptance] = address(0);
        }
        delegationAcceptances[msg.sender] = address(0);
    }

    
    // We include explcit dissalowance of address(0) for noobs to make things extra clear
    // Exercise for reader: why is "owner != address(0)" not needed?
    //DEBUG ONLY BEGIN
        /*modifier onlyOwner() {
        require((msg.sender == owner) && (owner != address(0)), "Not owner.");
        _;
    }
    function removeOwnerForever() external onlyOwner {
        owner = address(0);
    }
    function SetDebugTime(uint256 newTime) external onlyOwner() {
        debug_time = newTime;
    }
    function AddDebugTime(uint256 newTime) external onlyOwner() {
        debug_time += newTime;
    }*/
    // DEBUG ONLY END

    //PRODUCT_TEST:
    //PRODUCTION_LAUNCH:
    // DO THIS!!!!! : SET ALL debug_time to block.timestamp in this file.

    // Debug:
    //IVoteWeightSource public immutable voteWeightSource;
    // Production
    address public constant _voteWeightSourceAddress = 0x3871F0d0396Dbad8E970C274A7Ed8A2Ffb5B6EC1;
    IVoteWeightSource public constant voteWeightSource = IVoteWeightSource(_voteWeightSourceAddress);

    constructor(){
        //Debug, pass address parameter as input to constructor
        //address  _voteWeightSourceAddress) {
        //voteWeightSource = IVoteWeightSource(_voteWeightSourceAddress);
        
        //DEBUG_TEST:
        //AUTOMATED_TEST:
        // owner = msg.sender;
        //DEBUG_TEST:
        // debug_time = block.timestamp;
        //AUTOMATED _TEST
        //debug_time = MintOneStart - 1000;
        //PRODUCT_TEST:
        //PRODUCTION_LAUNCH:
        // Do nothing
    
    }

    mapping(uint256 => uint256) public numPolls;  //Poll indices start at votedPower * 2**37 for each powerIndex;  Valid powerIndex is at most 6.
                                                    // We use a mapping to avoid bounds checking inefficiency.

    // SimpleVoteRecords.  Indexed like this: simpleVoteRecords[user address][rawPollIndex/64];
    // We can index by rawPollIndex to keep almost all the code using rawPollIndices (with high bits 37-39 being the powerIndex); 
    // Only numPolls, and UI will use pollIndex that does not include high bits with powerIndex (UI will use prettyPollIndex)
    // A user's first vote on a poll of a given votePower will have higher gas (15k higher, to non-zero storage).
    // But then all votes in the same word (same votePower, and same value of pollIndex%64 of another vote that
    // user made) will have reduced gas.
    mapping(address => mapping(uint256 => uint256)) public simpleVoteRecords;

    uint256 public numPrettyPollIndexes;
    mapping(uint256 => uint256) public prettyPollIndexToRawPollIndex;

    // cumulativeVotes should only be updated for the first complex vote.  Other indices will have zero for this value.
    // This could have been done to let people vote slightly more efficiently by packing each vote into 128-bits, but
    // there would still be a requirement for the sum total of new words to be written to storage to be two, and most of the use-case for
    // complex votes is to propose a poll with some number, perhaps abstaining votes, to increase the votedPower of the poll, but the
    // creator can still reserve some of their voting power to use later after public discussion has taken place.  Therefore the real purpose
    // of ComplexVoteRecord is to enable a user to do two partial votes, the total of which is all their vote weight.  So no further optimization
    // is warranted (e.g. to optimize for a user to vote 3 times).  The user can still vote 3+ times but this contract will cost 15k more gas than the max optimized
    // implementation (packed 128-bit ComplexVoteRecords) would be for that.

    struct ComplexVoteRecord {
        // First two here are overall records for this user and poll.  We put them in this struct to save gas.
        // This design is pretty efficient for people who will vote one vote less than their max, or two votes of any kind.
        // At 3+ votes it gets a little gas inefficient but that is niche use-case and its only ~15k more gas for such users for votes 3+.
        uint56 timesThisUserVotedThisPoll; // Only used in first index (0), i.e. complexVoteRecords[user address][pollIndex][0]
        uint96 cumulativeVotes;            // Only used in first index (0), i.e. complexVoteRecords[user address][pollIndex][0]
        uint96 voteWeightThisRecord;
        uint8 pollChoice; // Same values 0-13 as described above.  reserved, abstain, and poll creator options 0-11;
    }

    // complexVoteRecords[user address][pollIndex][voteNumber]
    // high 
    // To put any vote record into complexVoteRecords requires setting the corresponding
    // simpleVoteRecords[user address][pollIndex] to 1, which indicates complex mode is being
    // used for this user for this pollIndex.
    mapping(address => mapping(uint256 => mapping(uint256 => ComplexVoteRecord))) public complexVoteRecords;


    // Each vote option is a separate word, and stores in high bits the number of vote options,
    // the voteWeightTimestamp, voteStartTimestamp, and voteEndTimestamp.
    // This forces the poll creator to non-zero all the vote options (pay gas for that), and 
    // voters can read whichever option they want to cast their vote for, before writing, and
    // check to see if their option that they are voting is valid.
    
    // An event noting the power increased, can be emitted from a dedicated transaction and 
    // will also be emitted whenever the total votes of an option being voted by a particular
    // user increases from the previous vote power to a higher vote power.
    // Note that this will duplicate the event emission so that it could be rebroadcast
    // for each option (as voters do not check the total number of votes, but rather the
    // total number of votes for their option, and only that option gets updated with
    // the new vote power).  This seems reasonable as at most 14x repeated events of power
    // escalation will occur, and usually it will just be one or two leading options that increase
    // votePower of a poll, so duplication is minimized.
    // The dedicated transaction for votePower will avoid duplicates and emit a different event.
    // In this way, a poll that gets a lot of votes can naturally become discernable from spam.

    function GetPower(uint256 numVotes) private pure returns (uint8) {
            numVotes /= 1000000000000000000000000; // 1M with 18 decimals (27 zeros)
            if( numVotes == 0 ) {
                return 0;
            }
            if( numVotes < 3 ) {
                return 1;
            }
            if( numVotes < 10 ) {
                return 2;
            }
            if( numVotes < 30 ) {
                return 3;
            }
            if( numVotes < 100 ) {
                return 4;
            }
            if( numVotes < 300 ) {
                return 5;
            }
            return 6;
    }

    // HERE IS THE FORMAT OF SPLIT INDICES AND HOW THEY
    // ARE USED WITH THE POLL STRING TO SEGMENT THE QUESTION AND OPTIONS:
    // The indices where splits should occur in the corresponding pollString to separate
    // the poll question from the first option, the first option from the second option, and so on.
    // 16-bit chunks, up to 12 (separating poll question plus 12 options ) where least significant 16-bits
    // is first, separating poll question from first option, next least-signficant 16-bits separates
    // first option from second option, and so on.  Leave unused splits as 0.  Must have at least one split.
    // Splits must be greater than 0, in increasing order, and less than the length of the string
    // As an example, pollString "My pollOption 1Option 2Option Numero Tres" would be encoded as:
    // (7 << (16*0)) | (15 << (16*1)) | (23 << (16*2)).

    // Validate increasing order, at least one valid split, and non-empty strings.
    function validateSplitIndices(uint256 stringLength, uint256 splitIndexes) private pure returns (uint8 numCreatorOptions) {
        uint256 prev = 0;
        numCreatorOptions = 0;
        for( uint256 i = 0; i < (12*16); i += 16 ) {
            uint256 current = (splitIndexes>>i) & 0xFFFF;
            if( current == 0 ) {
                require(i > 0, "No splits");
                return numCreatorOptions;
            } else {
                require(current>prev,"Decreasing order");
                require(current < stringLength, "Out of bounds index");
                // Every valid split is a creatorOption because total strings is
                // total splits + 1, and first string is poll, not creatorOption.
                ++numCreatorOptions;
                prev = current;
            }
        }
    }

    mapping(uint256 => uint256) public rawPollIndexToPrettyPollIndex;

    // DEBUG_TEST:
    // PRODUCTION_TEST:
    // uint256 constant MintOneStart = 1707001200;

    // PRODUCTION_LAUNCH
    uint256 constant MintTwoEnd = 1712527200;

    // pollOptionStates[pollIndex][optionIndex (0-13)] // although 0 is reserved and not allowed.
    struct PollOptionState {
        
        uint96 voteTotal;  
        uint8 totalVoteOptions; // This is 2+ total creator  options (0 is reserved, 1 is Active Abstain)
        uint40 voteWeightTimestamp;
        uint40 pollEndTimestamp;
        uint72 invalidProposalVoteInGwei; // When user votes, we divide their vote by gwei (1 billion) to tally this number.
                                                        // since we don't have quite enough bits to hold 18 decimals, we just hold 9.
                                                        // We could hold 3 more decimals but we prefer Shannon to Lovelace ;-)
    }

    mapping(uint256 => mapping(uint256 => PollOptionState)) public pollOptionStates;

    struct PollCreationOptionsStruct{
        // uint8 pollChoice; // We force poll choice to be abstain in this case since it is intended as a spam filter and it is
                                // a stronger indicator of signal/noise if the creator is willing to be unbiased in their vote
                                // they perform simultaneously with the poll creation.
        uint96 voteWeight;
        uint64 voteWeightIndex;
        uint40 voteWeightTimestamp; //must be greater than or equal to MintTwoEnd and ~less than block.timestamp - 15 minutes.
        uint40 duration; // Used to be pollEndTimestamp but instead it has been made a duration since it could
                         // take a long time for a transaction to get added to a block and we don't want the end
                         // timestamp to be invalid due to it taking a while for the transaction to be added to  block.
                        // Minimum duration for 
        bool voteAsDelegate;
    }

    // Index creator to allow enable-listing (reliable) black-listing (not reliable, users can change wallets) etc by UI.
    // Index votedPower to enable spam filtering.  Polls initiated by a vote with large voteWeight by the creator are 
    //   less likely to be spam.
    // Index rawPollIndex to allow users who want more info on a poll to quicly get it, including poll question and options.
    //   This can enable an ipfs web page that takes one query string parameter, its rawPollIndex, to quickly get all the
    //   info about a poll.  (Votes will also be indexed by rawPollIndex, so they will be able to see all the votes as well)

    // votedPowerIncrease events can be emitted if a user wants to promote a poll to take advantage of the anti-spam power of 
    //   the votedPower represented by the total votes that have been cast on it so far.  This will also be indexed by
    //   rawPollIndex for users to easily see the highest power yet promoted for the poll.

    event NewPollCreated(address indexed creator, uint8 indexed votedPower, uint40 indexed rawPollIndex, uint40 prettyPollIndex, 
        string pollString, uint192 pollStringSplits, uint40 voteWeightTimestamp, uint40 pollStartTimestamp, uint40 pollEndTimestamp);

    // Choice is chosen option 0 is reserved, not votable, 1 is Active Abstain, 2+ are poll creator options.
    event Voted(uint40 indexed rawPollIndex, address indexed voter, uint8 choice, uint96 voteWeight, bool invalid);

    function GetSimpleVoteStateAndSetIfNotZero(uint256 rawPollIndex, uint256 newSimpelVoteStateIfPrevZero, bool voteAsDelegate) private returns(uint256 prevState)
    {
        // See above definition of simpleVoteRecords for why it's done this way, to reduce gas of single votes of max weight,
        // with some resilience to spam/DOS of this optimization.
        uint256 rawPollIndexDiv64 = rawPollIndex/64;
        uint256 withinWordIndex = (rawPollIndex%64)*4;
        uint256 simpleVoteRecord;
        if( voteAsDelegate ) {
            require(delegationAcceptances[msg.sender] != address(0), "No delegation accepted");
            simpleVoteRecord = simpleVoteRecords[delegationAcceptances[msg.sender]][rawPollIndexDiv64];
        } else {
            simpleVoteRecord = simpleVoteRecords[msg.sender][rawPollIndexDiv64];
        }
        prevState = (simpleVoteRecord>>withinWordIndex)& 0xF; // Get 4-bits at 256-bit word indexed 0-63.
        if( prevState == 0) {
            simpleVoteRecord = (simpleVoteRecord & (~(0xF << withinWordIndex))) | 
                ((newSimpelVoteStateIfPrevZero&0xF)<<withinWordIndex);
                // small gas inefficiency anding newSimpelVoteStateIfPrevZero with 0xF, but makes clear and safe.
            if( voteAsDelegate ){
                simpleVoteRecords[delegationAcceptances[msg.sender]][rawPollIndexDiv64] = simpleVoteRecord;
            } else {
                simpleVoteRecords[msg.sender][rawPollIndexDiv64] = simpleVoteRecord;
            }
        }
    }

    // complexVoteRecords[user address][pollIndex][voteNumber]

    function ComplexVote(uint256 rawPollIndex, uint256 validatedWeight, uint256 voteWeight, uint256 pollChoice, 
                            bool voteAsDelegate) private {
        ComplexVoteRecord storage baseComplexVoteRecordStorage;
        if(voteAsDelegate) {
            require(delegationAcceptances[msg.sender] != address(0), "No delegation available." );
            baseComplexVoteRecordStorage = complexVoteRecords[delegationAcceptances[msg.sender]][rawPollIndex][0];
        } else {
            baseComplexVoteRecordStorage = complexVoteRecords[msg.sender][rawPollIndex][0];
        }
        ComplexVoteRecord memory baseComplexVoteRecord = baseComplexVoteRecordStorage;
        uint256 newTotal = uint256(baseComplexVoteRecord.cumulativeVotes) + uint256(voteWeight);
        require( newTotal <= validatedWeight, "Exceeds user vote limit");
        baseComplexVoteRecordStorage.cumulativeVotes = uint96(newTotal);
        uint256 timesThisUserVotedThisPoll = baseComplexVoteRecord.timesThisUserVotedThisPoll;
        require(timesThisUserVotedThisPoll < (0xFFFFFFFF), "Too many votes"); //We are capping you at ~4B separate vote casts per poll lol.
        baseComplexVoteRecordStorage.timesThisUserVotedThisPoll = uint56(timesThisUserVotedThisPoll + 1);
        if( timesThisUserVotedThisPoll == 0 ) {
            baseComplexVoteRecordStorage.pollChoice = uint8(pollChoice);
            baseComplexVoteRecordStorage.voteWeightThisRecord = uint96(voteWeight);
        } else {
            ComplexVoteRecord storage targetComplexVoteRecordStorage;
            if(voteAsDelegate) {
                targetComplexVoteRecordStorage = 
                complexVoteRecords[delegationAcceptances[msg.sender]][rawPollIndex][timesThisUserVotedThisPoll];
            } else {
                targetComplexVoteRecordStorage = 
                complexVoteRecords[msg.sender][rawPollIndex][timesThisUserVotedThisPoll];
            }
                targetComplexVoteRecordStorage.pollChoice = uint8(pollChoice);
                targetComplexVoteRecordStorage.voteWeightThisRecord = uint96(voteWeight);
            
        }
    }

    // Although we could save a hair of gas by having a separate "vote all" function (11 bytes of calldata, 176 gas), it seems
    // much cleaner to have a single vote function.

    // Reverts if poll choice is out of bounds, or if block.timestamp >= voteEnd
    //function CompleteVote(uint256 rawPollIndex, uint256 pollChoice, uint256 voteWeight, )

    function Vote(uint40 rawPollIndex, uint8 pollChoice, uint96 voteWeight, uint64 voteWeightIndex, bool invalid, bool voteAsDelegate) public {
        require(voteWeight > 0, "Can't vote zero");
        require(pollChoice != 0, "Zero option reserved");

        PollOptionState storage myPollOptionStateStorage = pollOptionStates[rawPollIndex][pollChoice];
        PollOptionState memory myPollOptionState = myPollOptionStateStorage;
        require(pollChoice < myPollOptionState.totalVoteOptions, "Option out of bounds");

        // Debug:
        //require(debug_time < uint256(myPollOptionState.pollEndTimestamp), "Too late to vote");
        // Production:
        require(block.timestamp < uint256(myPollOptionState.pollEndTimestamp), "Too late to vote");

        bool isValid;
        uint96 validatedWeight;
        if( voteAsDelegate ) {
            require(delegationAcceptances[msg.sender] != address(0), "No delegation accepted.");
            ( isValid,  validatedWeight ) = voteWeightSource.GetAndValidateVoteWeight(delegationAcceptances[msg.sender], voteWeightIndex, myPollOptionState.voteWeightTimestamp);
        } else {
            ( isValid,  validatedWeight ) = voteWeightSource.GetAndValidateVoteWeight(msg.sender, voteWeightIndex, myPollOptionState.voteWeightTimestamp);
        }
        
        require(isValid, "Invalid voteWeightIndex");
        if( validatedWeight == voteWeight ) { // Candidate for simpleVote
            // Attempt to simple vote, i.e. one single vote of all available weight.  Gas-optimized

            // pollChoice+2 because here, 0 and 1 are not options but represent nonvoted and complex vote
            uint256 prevSimpleVoteState = GetSimpleVoteStateAndSetIfNotZero(rawPollIndex, pollChoice+2, voteAsDelegate);
            require( prevSimpleVoteState == 0, "Can't vote max, already voted");
        } else {
            // Attempt to complex vote, i.e. possible to vote portions on same poll.  
            // Gas-optimized for single-vote, and good for 2 votes.  Gas is not optimized for 3+ votes, 15k more gas 
            //   than optimal design for that case.  This efficiently lets poll creators use some initial vote to 
            //   avoid spam filter, and decide their later vote after community discourse (2 votes).  It also lets
            //    voters with very large vote weight efficiently vote only a small portion of their total and
            //   vote the rest as an abstain (something that could be interpreted as a gesture) or just not vote the rest.

            uint256 prevSimpleVoteState = GetSimpleVoteStateAndSetIfNotZero(rawPollIndex, 1, voteAsDelegate);
            require(prevSimpleVoteState < 2, "Already voted max");
            ComplexVote(rawPollIndex, validatedWeight, voteWeight, pollChoice, voteAsDelegate); // only succeeds if there was enough previously unvoted vote weight.
        }

        myPollOptionStateStorage.voteTotal = myPollOptionState.voteTotal + voteWeight;
        if( invalid ) {
            myPollOptionStateStorage.invalidProposalVoteInGwei = uint72(uint256(myPollOptionState.invalidProposalVoteInGwei) + 
                (uint256(voteWeight)/1_000_000_000));
        }
        if( voteAsDelegate) {
            emit Voted(rawPollIndex, delegationAcceptances[msg.sender], pollChoice, voteWeight, invalid);
        } else {
            emit Voted(rawPollIndex, msg.sender, pollChoice, voteWeight, invalid);
        }
        

    }


    function AddPollOptionsAndVote(uint256 rawPollIndex, uint256 numCreatorOptions, 
        PollCreationOptionsStruct memory myPollCreationOptionsStruct) private {

        
        mapping(uint256 => PollOptionState) storage myOptionStates = pollOptionStates[rawPollIndex];
        // Don't forget 2 default options 0: Invalid Proposal and 1: Active Abstain
        uint256 timeStampToAnalze = myPollCreationOptionsStruct.voteWeightTimestamp;

        // Debug:
        // require(timeStampToAnalze >= MintOneStart, "Vote weight too early");
        // Production:
        require(timeStampToAnalze >= MintTwoEnd, "Vote weight too early");

        // DEBUG_TEST:
        require(timeStampToAnalze <= (block.timestamp - 15 minutes), "Vote weight too late");
        // PRODUCTION_TEST:
        // PRODUCTION_LAUNCH:
        // require(timeStampToAnalze < (block.timestamp - 1 days), "Vote weight too late");
        
        // Debug:
        //timeStampToAnalze = debug_time + myPollCreationOptionsStruct.duration;
        // Production:
        timeStampToAnalze = block.timestamp + myPollCreationOptionsStruct.duration;
        
        // Debug:
        //require(timeStampToAnalze >= (debug_time + 7 days), "Poll ends too early"); // Minimum 7 days duration
        //require(timeStampToAnalze <= (debug_time + 28 days), "Poll ends too late"); // Max 28-day duration
        // Production
        require(timeStampToAnalze >= (block.timestamp + 7 days), "Poll ends too early"); // Minimum 7 days duration
        require(timeStampToAnalze <= (block.timestamp + 28 days), "Poll ends too late"); // Max 28-day duration

        // Skip 0 which is reserved
        // 1 is the Abstain Option index
        // 2 inclusive through 2 + numCreatorOptions, exclusive (so 2 through 1+numCreatorOptions inclusive) are the creatorOptions
        for( uint256 i = 1; i < numCreatorOptions+2; ++i) {
            // We continue to reserve 0 option, option 1 is abstain, 2+ are creator options
            myOptionStates[i].totalVoteOptions = uint8(numCreatorOptions+2);
            myOptionStates[i].voteWeightTimestamp = myPollCreationOptionsStruct.voteWeightTimestamp;

            // Debug:
            //myOptionStates[i].pollEndTimestamp = uint40(debug_time + myPollCreationOptionsStruct.duration);
            // Production:
            myOptionStates[i].pollEndTimestamp = uint40(block.timestamp + myPollCreationOptionsStruct.duration);
        }
        
        if( myPollCreationOptionsStruct.voteWeight != 0) {
            Vote(uint40(rawPollIndex), 1, myPollCreationOptionsStruct.voteWeight, 
                    myPollCreationOptionsStruct.voteWeightIndex, false, myPollCreationOptionsStruct.voteAsDelegate);
        }
    }

    // Stores abiEncodePacked of poll string and splitIndices (splitIndices as 192-bit).
    mapping(uint256 => bytes32) public pollHashes;

    struct StackWorkaround {
        uint8 votedPower;
        uint40 rawPollIndex;
        uint40 prettyPollIndex;
        uint8 numCreatorOptions;
    }

    // splitIndices is up to 12 16-bit values (the first is the lowest 16-bits, last is highest 16-bits)
    // , which are the split-indices in inputString
    // If fewer than 13 substrings should be created, then use higher order 16-bit values of 0
    // to indicate emptiness.  The indices are validated for correctness - they must be strictly
    // increasing, greater than 0, and the max one must be less than the length of the inputString
    // We were going to use inscription mechanics to store the polls but we want contracts to be able to
    // create polls which they can then use the results of, and contract calls do not create inscriptions.
    // After the last valid splitIndex, insert 0 for the higher 16-bit entries in splitIndices.
    function CreatePollAndVote(PollCreationOptionsStruct memory myPollCreationOptionsStruct,
        string calldata inputString, uint192 splitIndexes
    ) external returns(uint40 rawPollIndexUint40) {
        // Here we assume the voteWeight is correct and only check it later at the end of the transaction.
        // If it is wrong, the failed transaction gas will be higher with this design, but
        // this reduces temporary variables and this function runs quickly into stackToDeep issues.
        // We use this all-in-one function so it is easier to manage creation of polls.


        StackWorkaround memory myStack;

        myStack.numCreatorOptions = validateSplitIndices(bytes(inputString).length, splitIndexes);

        // Function has stack issues so we dual purpose this var which is votedPower and then rawPollIndex
        myStack.votedPower = GetPower(myPollCreationOptionsStruct.voteWeight);
        myStack.rawPollIndex = uint40(numPolls[myStack.votedPower]);
        require(myStack.rawPollIndex < (1<<37), "Deploy another poll contract.");
        
        numPolls[myStack.votedPower] = myStack.rawPollIndex+1;
        myStack.rawPollIndex |= uint40(uint256(myStack.votedPower) << 37); //These rawPollIndexes are hard to read but contain the votedPower 
                                            //  inherently (in the high bits).
                                            // If you want an easier to read pollIndex, use prettyPollIndex;

        myStack.prettyPollIndex = uint40(numPrettyPollIndexes);
        numPrettyPollIndexes = myStack.prettyPollIndex + 1;
        prettyPollIndexToRawPollIndex[myStack.prettyPollIndex] = myStack.rawPollIndex;
        rawPollIndexToPrettyPollIndex[myStack.rawPollIndex] = myStack.prettyPollIndex;

        if(myPollCreationOptionsStruct.voteAsDelegate) {
            require(delegationAcceptances[msg.sender] != address(0), "No delegation accepted");
            emit NewPollCreated(delegationAcceptances[msg.sender], myStack.votedPower, myStack.rawPollIndex, myStack.prettyPollIndex, inputString, splitIndexes, 
            // Debug:
            //myPollCreationOptionsStruct.voteWeightTimestamp, uint40(debug_time), uint40(debug_time + myPollCreationOptionsStruct.duration));
            // Production:
            myPollCreationOptionsStruct.voteWeightTimestamp, uint40(block.timestamp), uint40(block.timestamp + myPollCreationOptionsStruct.duration));

        } else {
            emit NewPollCreated(msg.sender, myStack.votedPower, myStack.rawPollIndex, myStack.prettyPollIndex, inputString, splitIndexes, 

            // Debug:
            //myPollCreationOptionsStruct.voteWeightTimestamp, uint40(debug_time), uint40(debug_time + myPollCreationOptionsStruct.duration));
            // Production:
            myPollCreationOptionsStruct.voteWeightTimestamp, uint40(block.timestamp), uint40(block.timestamp + myPollCreationOptionsStruct.duration));
        }
        
        AddPollOptionsAndVote(myStack.rawPollIndex,  myStack.numCreatorOptions, myPollCreationOptionsStruct);

        // We will store hash on-chain and emit event with poll string & split info so it doesn't need to be stored on-chain
        // We emit the string in an event so we know it is available off-chain.  If someone wants to analyze
        // the text and options on-chain they can pass it in as an input to a different contract, as well as the hash
        // and rawPollIndex, to verify the passed string and splits are correct.
        pollHashes[myStack.rawPollIndex] = keccak256(abi.encodePacked(inputString, splitIndexes,block.timestamp)); // Include start of poll in the timestamp
                                                                                                                    // because otherwise there is no way for
                                                                                                                    // contracts to validate a poll submitted
                                                                                                                    // by a different party conforms to some requirement
                                                                                                                    // like which options are available, and that the
                                                                                                                    // duration is sufficiently long.  In theory contracts
                                                                                                                    // could want to verify the initial vote amount and the
                                                                                                                    // address of the creator, but it's more likely that
                                                                                                                    // those heavier use-cases will have the contract that
                                                                                                                    // wants to observe a particular poll actually create
                                                                                                                    // the poll or query a trusted contract that created the
                                                                                                                    // poll.

        return myStack.rawPollIndex;
    }

    function GetPollStatus(uint40 rawPollIndex) external view returns( uint40 remainingTime, uint96 invalidTotal, uint96[14] memory voteSums ) {
        require( numPolls[rawPollIndex>>37] > rawPollIndex&((1<<37)-1), "bad rawPollIndex" );
        uint40 endTime = pollOptionStates[rawPollIndex][1].pollEndTimestamp; // it's not yet remaining time, it's end time, we dual-purpose to avoid stack issues.

        // Debug: 
        // if( endTime <= debug_time) {
        // Production:
        if( endTime <= block.timestamp) {
            remainingTime = 0;
        } else {
            // Debug:
            //remainingTime = uint40(endTime - debug_time);
            // Production:
            remainingTime = uint40(endTime - block.timestamp);
        }
        uint256 invalidTotal256 = 0;
        for( uint256 i = 0; i < uint256(pollOptionStates[rawPollIndex][1].totalVoteOptions); ++i) {
            voteSums[i] = pollOptionStates[rawPollIndex][i].voteTotal;
            invalidTotal256 += uint256(pollOptionStates[rawPollIndex][i].invalidProposalVoteInGwei)*1_000_000_000;
        }
        invalidTotal = uint96(invalidTotal256);
    }

    mapping(uint256 => uint256) public promotionLevel;
    event PollPromotion(uint8 indexed votedPower, uint40 rawPollIndex);
    // If a poll has gotten a lot of votes and you want to help users filter it above the spam
    // you can emit an event with this function which UI can listen to to see polls with many votes.
    function PromotePoll(uint8 newVotedPower, uint40 rawPollIndex) external {
        uint256 prevPower = promotionLevel[rawPollIndex];
        if( prevPower >= newVotedPower ) {
            return;
        }
        require( numPolls[rawPollIndex>>37] > rawPollIndex&((1<<37)-1), "bad rawPollIndex" );
        // Debug:
        //require( pollOptionStates[rawPollIndex][1].pollEndTimestamp > debug_time, "poll over");
        // Production:
        require( pollOptionStates[rawPollIndex][1].pollEndTimestamp > block.timestamp, "poll over");
        
        uint256 sum = 0;
        for( uint256 i = 0; i < uint256(pollOptionStates[rawPollIndex][1].totalVoteOptions); ++i) {
            sum += pollOptionStates[rawPollIndex][i].voteTotal;
        }
        uint256 votedPower = GetPower(sum);
        if( votedPower > uint256(rawPollIndex>>37) ) {
            if(votedPower > prevPower) {
                if(votedPower >= newVotedPower) {
                    promotionLevel[rawPollIndex] = votedPower;
                    emit PollPromotion(uint8(votedPower), rawPollIndex);
                }
            }
        }
    }
}