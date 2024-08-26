# MyToken Smart Contract

## Overview

The `OJODELE` contract is an ERC20 token with additional features like minting and burning. It is owned by an address that has exclusive rights to mint, burn tokens, and transfer ownership. The token is named `ojodele` with the symbol `ojodele`.

## Features

- **Minting**: The owner can mint new tokens to any address.
- **Burning**: The owner can burn tokens from any address.
- **Transfer**: Standard ERC20 token transfers between addresses.
- **Approval and Allowance**: Standard ERC20 approval and allowance mechanisms.
- **Ownership Transfer**: The contract owner can transfer ownership to a new address.

## Contract Details

### Constructor

constructor(address initialOwner)

This full `README.md` file provides an organized and detailed description of the `OJODELE` contract.


```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, ERC20Burnable, Ownable {
    constructor(address initialOwner)
        ERC20("ojoDele", "OJODELE")
        Ownable(initialOwner)
    {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    function burn(address account, uint256 amount) public onlyOwner {
        _burn(account, amount);
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(
            sender,
            _msgSender(),
            allowance(sender, _msgSender()) - amount
        );
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return super.allowance(owner, spender);
    }

    function changeOwner(address newOwner) public onlyOwner {
        require(newOwner != address(0), "New owner is the zero address");
        transferOwnership(newOwner);
    }
}


