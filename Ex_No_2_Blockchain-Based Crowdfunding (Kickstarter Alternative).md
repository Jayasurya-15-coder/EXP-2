# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
A project owner starts a campaign with a funding goal and deadline.


Contributors can send ETH to the campaign.


If the goal is met before the deadline, funds are released to the project owner.


If the goal is not met, contributors can withdraw their funds.


## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Expected Output:
Users can contribute ETH to the campaign.

<img width="1918" height="1038" alt="Screenshot 2026-05-17 200534" src="https://github.com/user-attachments/assets/36bc0492-ac69-49ec-99b0-c853b2c9b6a5" />

<img width="1918" height="1038" alt="Screenshot 2026-05-17 200534" src="https://github.com/user-attachments/assets/47f556b9-6efe-4f0c-bf29-6d51e62dc61b" />


If the goal is met, the creator can withdraw funds.

<img width="1915" height="1056" alt="Screenshot 2026-05-17 200813" src="https://github.com/user-attachments/assets/fab7eaea-6fea-48d6-ba4c-198d841ad30e" />

If the goal is not met, contributors can claim a refund.

<img width="1918" height="1108" alt="Screenshot 2026-05-17 200828" src="https://github.com/user-attachments/assets/5701f2b7-e306-4c8c-a084-0c88664c5851" />

# High-Level Overview:
Teaches decentralized fundraising.


Avoids fraud by ensuring funds are only transferred if the goal is met.

# RESULT: 
Thus, a decentralized crowdfunding platform has been created and successfully executed.
