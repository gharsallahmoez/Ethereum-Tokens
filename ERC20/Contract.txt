pragma solidity ^0.4.25;
import "browser/IERC20Token.sol";
contract FirstToken is ERC20Interface{
    
    //INITIALISE PARAMETERS
    
    string public name = "FirstToken ";
    string public symbol = "FTN";
    uint8  public decimals = 18;
    uint256 public totalSupply = 1000000*(10**18);
    address owner=0;
    uint256 price = 0 ;
    //ARRAYS
    mapping (address => uint256) internal balances;
    mapping  (address => mapping (address => uint256)) internal allowed;
    
    //CONSTRUCTOR
    
    constructor (address _owner) public {
        owner=_owner;
        balances[owner]=totalSupply;
    }
    //TRANSFER FUNCTION 
    
        function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balances[msg.sender] >= _value);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }
    
    // APPROVE FUNCTION 
        /*Set allowance for other address
        Allows `_spender` to spend no more than `_value` tokens in your behalf */
        
        function approve(address _spender, uint256 _value) public returns (bool success) {
    //el msg.sender bech ya3ti lel spender coins
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }
    
    //TRANSFER FROM FUNCTION 
    
     function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
         //el _from 9adeh we5o mel msg.sender coins ? n7otouh fi variable allowance
        uint256 allowance = allowed[_from][msg.sender]; 
        require(balances[_from] >= _value && allowance >= _value); 
        balances[_to] += _value;
        balances[_from] -= _value;
        //ena7iw el coins li we5ouhom el _from mel msg.sender
        allowed[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }
    
    //BALANCE OF ADDRESS
    
        function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }
    
    //RETURN THE ALLOWANCE
    
        function allowance(address _owner, address _spender) public view returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
    
    //RETURN THE TOTAL totalSupply
        function totalSupply() public constant returns (uint256 totalSupplyValue) {        
            return totalSupply;
    }
    

/****************************  FEATURES  *************************************************/

    //SET TOKEN PRICE
    
     function setPrice(uint256 _price) public {
         require (msg.sender==owner);
            price = _price*(10**18);
     }
     
     //GET TOKEN PRICE
     
       function getPrice() public constant returns (uint256 _price) {        
            return price;
    }
    
    //buy tokens
    
         	function() public payable
	{
			uint256 _amount = ((msg.value * price)/(10**18));
			
			if ((balanceOf(this) < _amount) && _amount > 0) revert();
			
		   	balances[msg.sender] += _amount;
		   	
		    balances[this] -= _amount;

		   	emit Transfer(this, msg.sender, _amount);
		}
		

	    
		
    //ETHEREUM balance of owner ethereum
    
     function BalanceEther() public constant returns (uint256 _BalanceEther) {        
        return address(this).balance;
    }
    
    //SEND ETHEREUM TO OWNER
    
    	function sendEtherToOwner(uint256 _amount) public{
    	     require(msg.sender==owner);
             owner.transfer(_amount);    
}

}