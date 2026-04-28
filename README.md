# Transparent-Proxy-Admin
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract TransparentProxyAdmin {
    address public proxyAdmin;
    address public implementation;

    error NotAdmin();
    error ZeroAddress();

    event Upgraded(address indexed newImplementation);

    constructor() {
        proxyAdmin = msg.sender;
    }

    modifier onlyAdmin() {
        if (msg.sender != proxyAdmin) revert NotAdmin();
        _;
    }

    function upgradeTo(address newImplementation) public onlyAdmin {
        if (newImplementation == address(0)) revert ZeroAddress();
        implementation = newImplementation;
        emit Upgraded(newImplementation);
    }
}
