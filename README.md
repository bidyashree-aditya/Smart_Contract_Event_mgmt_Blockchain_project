# Smart_Contract_Event_mgmt_Blockchain_project
Blockchain_project

The project is based on smart-contracts in blockchain. Smart contracts are basically prorgams stored in blockchain that run when the given conditions are met.

In this event management project smart-contacts is used to manage creation of events, buying of tickets and transfer of tickets.


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
