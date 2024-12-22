# DeFi Empire: A Simple DeFi Kingdom Clone on Avalanche

Welcome to the DeFi Empire project! This project focuses on creating a DeFi Kingdom clone using Avalanche's custom EVM Subnet. This README provides an overview of the project, a step-by-step guide, and the associated code for smart contracts, including an ERC20 token and a Vault contract.

---

## Project Overview

### Description
DeFi Empire is a blockchain-based gaming application where players can collect, build, and earn rewards through various activities such as battling, exploring, and trading. Built on Avalanche, this project leverages the EVM Subnet to deploy smart contracts on a custom chain with low fees and a custom token.

### Key Features
- **Custom Subnet:** Use Avalanche's EVM Subnet to deploy smart contracts.
- **In-Game Currency:** Create a native token for in-game transactions and rewards.
- **Smart Contracts:** Implement foundational contracts for game mechanics.
- **Player Interactions:** Enable players to trade, battle, and explore using tokens.

---

## Tools and Technologies
- **Unix-based System** (MacOS or Linux)
- **Solidity**
- **Remix IDE**
- **Metamask**
- **Web Browser**
- **Avalanche CLI**

---

## Step-by-Step Guide

### 1. Set Up the EVM Subnet
1. Use Avalanche CLI to deploy your custom EVM Subnet.
2. Define the native currency for the Subnet.

### 2. Connect to Metamask
1. Add your Subnet to Metamask following the guide provided in Avalanche documentation.
2. Select the Subnet as the active network in Metamask.

### 3. Deploy Smart Contracts
1. Open **Remix IDE** and connect it to your Metamask wallet using the Injected Provider.
2. Deploy the ERC20 and Vault contracts provided below.

### 4. Test the Application
1. Use Remix to interact with the deployed contracts.
2. Mint tokens, deposit, and withdraw funds using the Vault contract.

---

## Smart Contracts

### ERC20 Contract
The ERC20 token contract serves as the foundation for the in-game currency.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "DeFi Empire Token";
    string public symbol = "DFET";
    uint8 public decimals = 18;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint amount) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

### Vault Contract
The Vault contract allows users to deposit tokens and manage their shares in the pool.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    function transfer(address recipient, uint amount) external returns (bool);
    function balanceOf(address account) external view returns (uint);
}

contract Vault {
    IERC20 public immutable token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint _amount) external {
        uint shares = totalSupply == 0 ? _amount : (_amount * totalSupply) / token.balanceOf(address(this));
        totalSupply += shares;
        balanceOf[msg.sender] += shares;
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        totalSupply -= _shares;
        balanceOf[msg.sender] -= _shares;
        token.transfer(msg.sender, amount);
    }
}
```

---

## Submission Requirements
1. **GitHub Repository:**
   - Include the project in a public GitHub repository.
   - Add this README file to the root folder.

2. **Video Walkthrough:**
   - Record a video (5 minutes or less) demonstrating the project.
   - Include a code walkthrough and live demo.

3. **Test Environment:**
   - Demonstrate the functionality in a test environment.

---

## What We're Looking For
- **Functionality:** Ensure that the project meets all the requirements.
- **Clarity:** Provide a clear explanation of the code and its purpose.
- **Demo:** Include a working demo of the deployed contracts.

---

## Resources
- [Avalanche Documentation](https://docs.avax.network/)
- [Remix IDE](https://remix.ethereum.org/)
- [Metamask](https://metamask.io/)
