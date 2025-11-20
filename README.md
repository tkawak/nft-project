# MyStandardSBT

このプロジェクトは、ERC5192標準に準拠したERC1155ベースのSoulbound Token (SBT) スマートコントラクトです。

## 概要

`MyStandardSBT` は、譲渡不可能なトークン（Soulbound Token）を管理するためのコントラクトです。OpenZeppelinのERC1155標準を拡張し、ERC5192 ("Minimal Soulbound NFTs") インターフェースを実装しています。

## 特徴

- **SBT (Soulbound Token):** 一度ミントされると、他のアドレスに転送することはできません。
- **ERC5192準拠:** `locked` 関数と `Locked` イベントを実装しており、標準的なSBTとして認識されます。
- **管理者機能:** コントラクトの所有者（Owner）のみが新しいトークンをミントできます。
- **Burnable:** トークンの所有者は自身のトークンを焼却（Burn）することができます。
- **URI更新可能:** メタデータのURIは所有者によって更新可能です。

## 技術仕様

- **Solidity Version:** ^0.8.20
- **継承:**
  - `ERC1155` (OpenZeppelin)
  - `Ownable` (OpenZeppelin)
  - `ERC1155Burnable` (OpenZeppelin)
- **インターフェース:** `IERC5192`

## 主要な関数

### 状態確認・設定
- `locked(uint256 tokenId)`: 指定されたトークンIDがロックされているかを返します。このコントラクトでは常に `true` を返します。
- `setURI(string memory newuri)`: メタデータのURIを設定します（所有者のみ実行可能）。
- `supportsInterface(bytes4 interfaceId)`: インターフェースのサポート状況を返します（ERC1155およびERC5192に対応）。

### ミント（発行）
- `mint(address account, uint256 id, uint256 amount, bytes memory data)`: 単一のトークンをミントし、`Locked` イベントを発行します（所有者のみ実行可能）。
- `mintBatch(address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data)`: 複数のトークンを一括ミントし、各IDに対して `Locked` イベントを発行します（所有者のみ実行可能）。

### 転送制限ロジック
- `_update`: トークンの状態更新時に呼び出されます。
  - **転送（Transfer）:** `from` と `to` が共に非ゼロアドレスの場合、エラー（revert）となり転送を阻止します。
  - **発行（Mint）:** `from` がゼロアドレスの場合は許可されます。
  - **焼却（Burn）:** `to` がゼロアドレスの場合は許可されます。

## ライセンス

MIT

