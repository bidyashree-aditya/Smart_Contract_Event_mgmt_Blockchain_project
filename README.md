# Smart_Contract_Blockchain_project
Blockchain_project

The project is based on smart-contracts in blockchain. Smart contracts are basically prorgams stored in blockchain that run when the given conditions are met.

In this insurance event management project smart-contacts is used to manage creation of events, buying of tickets (reserving spots) and transfer of tickets (transferring spots).


// SPDX-License-Identifier: MIT
pragma solidity >=0.5.0 <0.9.0;

contract EventContract {

    struct Event{

        address organizer;
        string name;
        uint date; // 0 1 2 
        uint price;
        uint ticketCount;  // 1 sec event date 0.5
        uint ticketRemain;
    }
    mapping(uint=>Event) public events;
    mapping(address=>mapping(uint=>uint)) public tickets;
    uint public nextId;
     
    function createEvent(string memory name, uint date, uint price  , uint ticketCount) external{

        require(date>block.timestamp, " You can oragnise event for future date");
        require(ticketCount>0, "You can organise event if you can create more than 0 tickets");

        events[nextId] = Event(msg.sender,name,date,price,ticketCount,ticketCount);
        nextId++;





    }
    function buyTicket(uint id, uint quantity) external payable  {
         require(events[id].date!=0, " Event does not exist");
         require(events[id].date>block.timestamp, " Event had already occured");
         Event storage _event = events[id];
         require(msg.value==(_event.price*quantity), " This is not enough");
         require(_event.ticketRemain>=quantity," Not enough tickets");
         _event.ticketRemain-= quantity;
         tickets[msg.sender][id]+=quantity;


         


    }
    function trasferTicket(uint id, uint quantity, address to) external{
         require(events[id].date!=0, " Event does not exist");
         require(events[id].date>block.timestamp, " Event had already occured");
         require(tickets[msg.sender][id]>= quantity, " You do not have enough tickets");
         tickets[msg.sender][id]-=quantity;
         tickets[to][id]+= quantity;


    }



}
