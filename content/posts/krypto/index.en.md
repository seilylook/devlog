---
title: "Krypto"
date: 2023-10-18T16:13:56+09:00
featuredImage: "/images/featured-image/ethereum.jpeg"
tags: ["Development", "Project", "Front-end", "Smart Contract", "Blockchain"]
categories: ["Project"]
draft: true
---

# Introduction

`React`와 `Solidity`를 사용해 `Ethereum` 거래 서비스 구현.

## Tech stack

- Front-end

  - React
  - Tailwindcss
  - ethers (version 5)
  - vite

- Smart Contract

  - hardhat
  - chai
  - ethers (version 5)
  - ethereum-waffle(version 4)

## Directory structure

```shell
client/
├── images
├── public/
├── src/
│   ├── assets/
|   |── components/
|   |── context/
|   |── utils/
|   |── App.jsx
├── package.json/
├── tailwind.config.js/
├── vite.config.js/
```

```shell
smart_contract/
├── artifacts/
├── cache/
├── contract/
│   ├── Transactions.sol/
├── scripts/
│   ├── deploy.js/
├── package.json/
```
