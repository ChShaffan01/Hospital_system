    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.17;

    contract hospital{
    address manager = msg.sender;
    struct peteint_info{
        string Name;
        uint Phone;
        uint Age;
        uint Bill;
        uint Charge_Date;
        uint DisCharged_Date;

        }
    mapping(address => peteint_info) public peteints;

    struct peteint_bill_info{
        uint treatment;
        uint service;
        uint medical;
        uint other_charges;
    }
    mapping (address => peteint_bill_info) public  peteints_bill;
    modifier onlymanager(){
    require(manager == msg.sender, "only manager can be call this");
    _;
    }

    function Registration(string memory _name,uint _number,uint _age ) public  {

        peteints[msg.sender].Name = _name;
        peteints[msg.sender].Phone = _number;
        peteints[msg.sender].Age = _age;
        peteints[msg.sender].Charge_Date = block.timestamp;
        peteints[msg.sender].DisCharged_Date = 0;
    }
    function Add_Charges(address _addr,uint _treatment,uint _service,uint _medical,uint _other_charges) public onlymanager {

    peteints_bill[_addr].treatment += _treatment;
    peteints_bill[_addr].service += _service;
    peteints_bill[_addr].medical += _medical;
    peteints_bill[_addr].other_charges += _other_charges;

    uint temp = _treatment + _medical + _service + _other_charges;
    peteints[_addr].Bill += temp; 
    }

    function Discharge() public payable {

    require(msg.value >= peteints[msg.sender].Bill,"Insufficient balance");
    
    payable (manager).transfer(msg.value);
        peteints[msg.sender].Name = "";
        peteints[msg.sender].Phone = 0;
        peteints[msg.sender].Age = 0;
        peteints[msg.sender].Bill = 0;
        peteints[msg.sender].Charge_Date = 0;
        peteints[msg.sender].DisCharged_Date = block.timestamp;

// treatment data
    peteints_bill[msg.sender].treatment = 0;
    peteints_bill[msg.sender].service = 0;
    peteints_bill[msg.sender].medical = 0;
    peteints_bill[msg.sender].other_charges = 0;

    }
    function getinfo() external  view  returns (string memory Name_ , uint Phone_ , uint Age_ , uint Bill_ , uint Charge_Date_ , uint DisCharged_Date)
    {
    return (peteints[msg.sender].Name,peteints[msg.sender].Phone,peteints[msg.sender].Age,peteints[msg.sender].Bill,peteints[msg.sender].Charge_Date,peteints[msg.sender].DisCharged_Date);
    }
    }

