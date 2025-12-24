# TIP20TOKEN.sol
TIP20TOKEN.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./ITIP20.sol";

contract TIP20Token is ITIP20 {
    string public override name;
    string public override symbol;
    uint256 public override totalSupply;
    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowances;

    constructor(string memory _name, string memory _symbol, uint256 _initialSupply, address admin) {
        name = _name;
        symbol = _symbol;
        totalSupply = _initialSupply;
        balances[admin] = _initialSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        return true;
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        allowances[msg.sender][spender] = amount;
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        require(balances[sender] >= amount, "Insufficient balance");
        require(allowances[sender][msg.sender] >= amount, "Allowance exceeded");
        balances[sender] -= amount;
        balances[recipient] += amount;
        allowances[sender][msg.sender] -= amount;
        return true;
    }
}
