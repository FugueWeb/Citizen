idrevo
======
Identity | Reputation | Voting
*Congress on the Blockchain*

Another header
--------------
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum. ::

	contract Crowdsale {

	    address public beneficiary;
	    uint public fundingGoal; uint public amountRaised; uint public deadline; uint public price;
	    token public tokenReward;
	    Funder[] public funders;
	    event FundTransfer(address backer, uint amount, bool isContribution);

	    /* data structure to hold information about campaign contributors */
	    struct Funder {
	        address addr;
	        uint amount;
	    }

	    /*  at initialization, setup the owner */
	    function Crowdsale(address _beneficiary, uint _fundingGoal, uint _duration, uint _price, token _reward) {
	        beneficiary = _beneficiary;
	        fundingGoal = _fundingGoal;
	        deadline = now + _duration * 1 minutes;
	        price = _price;
	        tokenReward = token(_reward);
	    }

	    /* The function without name is the default function that is called whenever anyone sends funds to a contract */
	    function () {
	        uint amount = msg.value;
	        funders[funders.length++] = Funder({addr: msg.sender, amount: amount});
	        amountRaised += amount;
	        tokenReward.sendCoin(msg.sender, amount / price);
	        FundTransfer(msg.sender, amount, true);
	    }

	    modifier afterDeadline() { if (now >= deadline) _ }

	    /* checks if the goal or time limit has been reached and ends the campaign */
	    function checkGoalReached() afterDeadline {
	        if (amountRaised >= fundingGoal){
	            beneficiary.send(amountRaised);
	            FundTransfer(beneficiary, amountRaised, false);
	        } else {
	            FundTransfer(0, 11, false);
	            for (uint i = 0; i < funders.length; ++i) {
	              funders[i].addr.send(funders[i].amount);
	              FundTransfer(funders[i].addr, funders[i].amount, false);
	            }
	        }
	        suicide(beneficiary);
	    }
	}


Useful links
------------

* `Ethereum <https://ethereum.org>`_

* `Browser-Based Compiler <https://chriseth.github.io/browser-solidity/>`_

* `Changelog <https://github.com/ethereum/wiki/wiki/Solidity-Changelog>`_

* `Story Backlog <https://www.pivotaltracker.com/n/projects/1189488>`_

* `Source Code <https://github.com/ethereum/solidity/>`_

* `Gitter Chat <https://gitter.im/ethereum/solidity/>`_

.. toctree::
   :maxdepth: 3
   :caption: Contents

   technical-proposal.rst
   cost-proposal.rst


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

