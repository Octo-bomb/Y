// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

// ? Token is a simple ERC20 token that inherits from ERC20 and Ownable
// The token has a deflationary mechanism that burns 2% of every transfer
contract YourToken is ERC20, Ownable {
    // The constructor sets the name, symbol and initial supply of the token
    constructor(string memory name, string memory symbol, uint256 initialSupply) ERC20(name, symbol) {
        _mint(msg.sender,)1000000000;
    }

    // The owner can mint new tokens to any address
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    // The owner can burn tokens from any address
    function burn(address from, uint256 amount) public onlyOwner {
        _burn(from, amount);
    }

    // Override the transfer function to add the deflationary mechanism
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        // Calculate the burn amount (2% of the transfer amount)
        uint256 burnAmount = amount / 50;
        // Reduce the transfer amount by the burn amount
        uint256 transferAmount = amount - burnAmount;
        // Burn the tokens from the sender's balance
        _burn(_msgSender(), burnAmount);
        // Transfer the remaining tokens to the recipient
        return super.transfer(recipient, transferAmount);
    }

    // Override the transferFrom function to add the deflationary mechanism
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        // Calculate the burn amount (2% of the transfer amount)
        uint256 burnAmount = amount / 50;
        // Reduce the transfer amount by the burn amount
        uint256 transferAmount = amount - burnAmount;
        // Burn the tokens from the sender's balance
        _burn(sender, burnAmount);
        // Transfer the remaining tokens to the recipient
        return super.transferFrom(sender, recipient, transferAmount);
    }
}

