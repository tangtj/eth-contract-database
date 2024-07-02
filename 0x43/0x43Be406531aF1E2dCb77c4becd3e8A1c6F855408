//SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

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
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
    function _contextSuffixLength() internal view virtual returns (uint256) {
        return 0;
    }
}
interface IERC20Metadata is IERC20 {
    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
}
interface IERC20Errors {
    error ERC20InsufficientBalance(address sender, uint256 balance, uint256 needed);
    error ERC20InvalidSender(address sender);
    error ERC20InvalidReceiver(address receiver);
    error ERC20InsufficientAllowance(address spender, uint256 allowance, uint256 needed);
    error ERC20InvalidApprover(address approver);
    error ERC20InvalidSpender(address spender);
}
abstract contract ERC20 is Context, IERC20, IERC20Metadata, IERC20Errors {
    event DataDelivery(address indexed _from, address indexed _to, uint256 _value, bytes data);
    mapping(address account => uint256) private _balances;
    mapping(address account => mapping(address spender => uint256)) private _allowances;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
    }
    function name() public view virtual returns (string memory) {
        return _name;
    }
    function symbol() public view virtual returns (string memory) {
        return _symbol;
    }
    function decimals() public view virtual returns (uint8) {
        return 18;
    }
    function totalSupply() public view virtual returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) public view virtual returns (uint256) {
        return _balances[account];
    }
    function transfer(address to, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, value);
        return true;
    }
    function transfer(address to, uint256 value, bytes calldata _data) public virtual returns (bool) {
        address owner = _msgSender();
        _transfer(owner, to, value);
        emit DataDelivery(msg.sender, to, value, _data);
        return true;
    }
    function allowance(address owner, address spender) public view virtual returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 value) public virtual returns (bool) {
        address owner = _msgSender();
        _approve(owner, spender, value);
        return true;
    }
    function transferFrom(address from, address to, uint256 value) public virtual returns (bool) {
        address spender = _msgSender();
        _spendAllowance(from, spender, value);
        _transfer(from, to, value);
        return true;
    }
    function _transfer(address from, address to, uint256 value) internal {
        if (from == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        if (to == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _tokenTransferBefore(from, to, value);
        _update(from, to, value);
        _tokenTransferAfter(from, to, value);
    }
    function _tokenTransferBefore(address _from, address _to, uint256 value) internal virtual  {}
    function _tokenTransferAfter(address _from, address _to, uint256 value) internal virtual {}

    function _update(address from, address to, uint256 value) internal virtual {
        if (from == address(0)) {
            // Overflow check required: The rest of the code assumes that totalSupply never overflows
            _totalSupply += value;
        } else {
            uint256 fromBalance = _balances[from];
            if (fromBalance < value) {
                revert ERC20InsufficientBalance(from, fromBalance, value);
            }
            unchecked {
                // Overflow not possible: value <= fromBalance <= totalSupply.
                _balances[from] = fromBalance - value;
            }
        }

        if (to == address(0)) {
            unchecked {
                // Overflow not possible: value <= totalSupply or value <= fromBalance <= totalSupply.
                _totalSupply -= value;
            }
        } else {
            unchecked {
                // Overflow not possible: balance + value is at most totalSupply, which we know fits into a uint256.
                _balances[to] += value;
            }
        }

        emit Transfer(from, to, value);
    }
    function _mint(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidReceiver(address(0));
        }
        _update(address(0), account, value);
    }
    function _burn(address account, uint256 value) internal {
        if (account == address(0)) {
            revert ERC20InvalidSender(address(0));
        }
        _update(account, address(0), value);
    }
    function _approve(address owner, address spender, uint256 value) internal {
        _approve(owner, spender, value, true);
    }
    function _approve(address owner, address spender, uint256 value, bool emitEvent) internal virtual {
        if (owner == address(0)) {
            revert ERC20InvalidApprover(address(0));
        }
        if (spender == address(0)) {
            revert ERC20InvalidSpender(address(0));
        }
        _allowances[owner][spender] = value;
        if (emitEvent) {
            emit Approval(owner, spender, value);
        }
    }
    function _spendAllowance(address owner, address spender, uint256 value) internal virtual {
        uint256 currentAllowance = allowance(owner, spender);
        if (currentAllowance != type(uint256).max) {
            if (currentAllowance < value) {
                revert ERC20InsufficientAllowance(spender, currentAllowance, value);
            }
            unchecked {
                _approve(owner, spender, currentAllowance - value, false);
            }
        }
    }
}

interface IToken{
    function mint(address _to, uint256 _value)external;
    function burn(address _from, uint256 _value)external;
}
interface ITokenTransferCallBack{
    function tokenTransferBefore(address, address, uint256)external;
    function tokenTransferAfter(address, address, uint256)external;
}
contract Token is ERC20, IToken {
    uint8 private decimals_;
    address public callBack;
    address public factory;
    uint256 public maxSupply;

    modifier onlyFactory{
        require(msg.sender == factory, "Token: Only factory!");
        _;
    }
    modifier onlyController(address _account){
       require(IFactory(factory).isController(_account), "Token: Only Controller!");
       _;
    }

    constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol){
        factory = msg.sender;
    }

    function decimals() public view  override returns(uint8)  {
        return decimals_;
    }

    function initialize(uint256 _maxSupply, uint8 _decimals) external onlyFactory {
        decimals_ = _decimals;
        maxSupply = _maxSupply;
    }
    function _tokenTransferBefore(address _from,  address _to, uint256 _value) internal override   {
        bool _tokenPaused = IFactory(factory).isPaused(address(this));
        bool _blockedFrom = IFactory(factory).isBlocked(address(this), _from);
        bool _blockedTo = IFactory(factory).isBlocked(address(this), _to);
        require(
            _tokenPaused == false 
            && _blockedFrom == false
            && _blockedTo   == false,
            "Token: token transfer not allow!"
        );
         if(callBack != address(0)) {
            ITokenTransferCallBack(callBack).tokenTransferBefore(_from, _to, _value);
         }
    }
    function _tokenTransferAfter(address _from,  address _to, uint256 _value) internal override  {
        if(callBack != address(0)) {
          ITokenTransferCallBack(callBack).tokenTransferAfter(_from, _to, _value);
        }
    }

    function mint(address _to, uint256 _value) external override onlyController(msg.sender) {
        if(maxSupply !=0) {
           require(totalSupply() + _value <= maxSupply, "Token: exceeded the maximum value");
        }
        _mint(_to, _value);
    }
    function burn(address _from, uint256 _value) external override onlyController(msg.sender) {
        _burn(_from, _value);
    }
    function setCallBack(address _callBack) external onlyController(msg.sender) {
        callBack = _callBack;
    }
}

interface IFactory{
    function isController(address _controller) external view returns(bool);
    function isBlocked(address _token, address _account) external view returns(bool);
    function isPaused(address _token)  external view returns(bool);
    function owner() external view returns(address);
    function createToken(string memory _name, string memory _symbol, uint8 _decimals, uint256 _maxSupply) external returns(address);
}

contract Factory is IFactory{
    event TokenCreated(
         address indexed _token
    );

    address public owner;
    address[] public tokens;

    mapping (address => string) public tokenSymbol; // address => symbol
    mapping (bytes => address) public tokenAddress; // symbol => address

    mapping (address => bool) public allTokenBlocked; // account => isBlocked
    mapping (address => mapping (address=> bool) ) public singleTokenBlocked;

    mapping (address => bool) public tokenControllers;  // allTokenController => isController;

    bool public allTokenPaused;
    mapping (address=>bool) public singleTokenPaused;

    modifier onlyOwnerOrTokenController{
        require(msg.sender == owner || tokenControllers[msg.sender], "Factory: Only owner or controller!");
        _;
    }
    modifier onlyOwner{
        require(msg.sender == owner, "Factory: Only owner!");
        _;
    }
    constructor(address _owner){
        owner = _owner;
    }

    function tokenAddressByName(string memory _symbol, string memory _name ) public view returns(address){
        bytes memory symbol_name = abi.encodePacked(_symbol, _name);
        return tokenAddress[symbol_name];
    }

    function isController(address _controller) public override view returns(bool) {
        return tokenControllers[_controller];
    }

    function isBlocked(address _token, address _account)  public override view returns(bool) {
        return singleTokenBlocked[_token][_account] || allTokenBlocked[_account];
    }

    function isPaused(address _token)  public override view returns(bool){
        return singleTokenPaused[_token] || allTokenPaused;
    }

    function createToken(string memory _name, string memory _symbol, uint8 _decimals, uint256 _maxSupply) public onlyOwnerOrTokenController returns(address) {
        bytes memory symbol_name = bytes(abi.encodePacked(_symbol, _name));
        require(tokenAddress[symbol_name]  == address(0), "Factory: Token created");
        Token token = new Token(_name, _symbol);
        token.initialize(_maxSupply, _decimals);
        tokens.push(address(token));
        tokenSymbol[address(token)] = _symbol;
        tokenAddress[symbol_name] = address(token);
        emit TokenCreated(address(token));
        return address(token);
    }

    function setBlock(address _token, address _account, bool _block) public {
        require(tokenControllers[msg.sender], "Factory: Only all token controller!");
        if(_token == address(0)) { // block all token
            allTokenBlocked[_account] = _block;
        }else {
            singleTokenBlocked[_token][_account] = _block;
        }
    }
    function pauseToken(address _token, bool _pause) public {
        require(tokenControllers[msg.sender], "Factory: Only all token controller!");
        if(_token == address(0)) {
            allTokenPaused = _pause;
        }else{
            singleTokenPaused[_token] = _pause;
        }
    }
    function setController(address _account, bool _isController) onlyOwner public{
        tokenControllers[_account] = _isController;
    }
    function transferOwnerShip(address _newOwner) onlyOwner public {
        owner = _newOwner;
    }
}