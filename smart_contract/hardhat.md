# Add smart contracts to project

### Create smart_contract folder at same level as client

```bash
> client
> smart_contract
```

### go to smart contract

```bash
cd ..
cd smart_contract
```

### Add `hardhat` and `ts_node`

```bash
% npm install --save-dev ts-node

% npx hardhat


# ✔ What do you want to do? · Create a TypeScript project
# ✔ Hardhat project root: · /Users/lance/Dropbox/projects-git-win/practice/large projects/send_eth_dapp/smart_contract
# ✔ Do you want to add a .gitignore? (Y/n) · y
# ✔ Do you want to install this sample project's dependencies with npm (hardhat @nomicfoundation/hardhat-toolbox)? (Y/n) · y

# npm install --save-dev "hardhat@^2.22.3" "@nomicfoundation/hardhat-toolbox@^5.0.0"

# added 554 packages, and audited 575 packages in 42s

# 94 packages are looking for funding
#   run `npm fund` for details

# found 0 vulnerabilities

# ✨ Project created ✨

# See the README.md file for some example tasks you can run

# Give Hardhat a star on Github if you're enjoying it! ⭐️✨

#      https://github.com/NomicFoundation/hardhat


# DEPRECATION WARNING

#  Initializing a project with npx hardhat is deprecated and will be removed in the future.
#  Please use npx hardhat init instead.


% npx hardhat test

# WARNING: You are currently using Node.js v21.6.0, which is not supported by Hardhat. This can lead to unexpected behavior. See https://hardhat.org/nodejs-versions


# Downloading compiler 0.8.24
# Compiled 1 Solidity file successfully (evm target: paris).


#   Lock
#     Deployment
#       ✔ Should set the right unlockTime (2699ms)
#       ✔ Should set the right owner
#       ✔ Should receive and store the funds to lock
#       ✔ Should fail if the unlockTime is not in the future (88ms)
#     Withdrawals
#       Validations
#         ✔ Should revert with the right error if called too soon
#         ✔ Should revert with the right error if called from another account
#         ✔ Shouldn't fail if the unlockTime has arrived and the owner calls it
#       Events
#         ✔ Should emit an event on withdrawals (51ms)
#       Transfers
#         ✔ Should transfer the funds to the owner (62ms)


#   9 passing (3s)


```

This installs smart contract framework and dependencies
