# NFT-rental-time-limited-lending-solidity
NFT rental (time‑limited lending) solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

contract NFTRental {
    IERC721 public nft;

    struct Rent {
        address owner;
        address user;
        uint256 endTime;
    }

    mapping(uint256 => Rent) public rents;

    constructor(address _nft) {
        nft = IERC721(_nft);
    }

    function lend(uint256 id, address user, uint256 duration) public {
        require(nft.ownerOf(id) == msg.sender, "Not owner");
        rents[id] = Rent(msg.sender, user, block.timestamp + duration);
    }

    function currentUser(uint256 id) public view returns (address) {
        if (block.timestamp > rents[id].endTime) return nft.ownerOf(id);
        return rents[id].user;
    }
}
