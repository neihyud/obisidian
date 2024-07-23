- Gno.land: layer 1 blockchain, cho phép thực thi smart contracts, sử dụng 'interprete version'  dựa trên Go
- [[BlockChain]]
- Intergrate Gno: specialized GnoVM
- [[Mnemonic]]
- GNOTs: loại tiền tệ hoặc token sử dụng trong Gno.land

|                       | Blockchain                                                                                                                                                | Gno.land |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Language              | Ethereum                                                                                                                                                  | Gno.la   |
| Virtual Machine       | EVM                                                                                                                                                       | Gn       |
| Consensus             | PoW                                                                                                                                                       | Tender   |
| Governance (Quản trị) |                                                                                                                                                           |          |
| Status                | Statusful: có thể lưu trữ và thay đổi trạng thái trên blockchain<br>Statusless: không lưu trạng thái trên blockchain mà sử dụng cơ chế khác để lưu  less: |          |
|                       |                                                                                                                                                           |          |
|                       |                                                                                                                                                           |          |
- Để tương tác với Realm cần account: gnokey và published on Gno.land
	- running on node: using gloland (the underlying chain is powered by tendermint2)
- **Gnokey**:
	- A key pair is what is used to sign transactions that are broadcast to the gno.land blockchain
	- Used for account & key management and general interactions with the Gnoland blockchain.
- Private user key: tương tác với gno.land blockchains
- **Gnodev**:

| **Tính Năng**   | gnodev                                    | gnoland start                                   |
| --------------- | ----------------------------------------- | ----------------------------------------------- |
| **Mục Đích**    | Phát triển và quản lý ứng dụng blockchain | Khởi động và vận hành một nút (node) blockchain |
| Chức năng chính | Thêm tài khoản, triển khai ứng dụng       | Khởi động hoặc dừng nút, kết nối mạng           |
| Sử dụng         | Trong quá trình phát triển và kiểm tra    | Trong môi trường thực tế hoặc thử nghiệm        |
| Thực thi        | Quản lý ứng dụng, ví, và cấu hình         | Khởi động nút, kết nối với mạng                 |


#  Concepts
- **BlockChain**:
	- chia sẻ thông tin minh bạch trong một mạng lưới kinh doanh
	- dữ liệu có sự nhất quán: không thể thay đổi mà không có sự đồng ý của mạng lưới
	- tạo một hệ thống chống làm giả, phi tập trung để ghi lại giao dịch
	- đặc điểm: [blockchain](https://aws.amazon.com/vi/what-is/blockchain/?aws-products-all.sort-by=item.additionalFields.productNameLowercase&aws-products-all.sort-order=asc)
		- phi tập trung: 
			- từ một thực thể tập trung sang một mạng lưới phân tán.
			- tính minh bạch
		- bất biến:
		- đồng thuận: 
	- Các thành phần chung:
		- Sổ cái phân tán: 
			- CSDL dùng chung trong mạng lưới, tương tự một file dùng chung cho mọi người trong nhóm có thể chỉnh sửa
			- không thể xóa mục nhập sau khi hoạt động đã được ghi lại
		- **Smart Contract**: [[Smart Contract]]
		- Mật mã hóa khóa công khai:
			- tính năng bảo mật, xác định những người tham gia trong mạng lưới
- [[Smart Contract]]: 
	- digital contracts stored on a blockchain, automatically executed when predetermined terms and conditions are met (when called by a user on the blockchain)
- **Realms**: (smart constract)
	- refers to a specific instance of a smart contract, written in Gno
	- Packages vs Realms:
		- Pure Package
			- can view source on-chain
			- contains functionalities and utilities, used in realms
			- stateless => chỉ xem source code của họ trên on-chain
			- default import path: `gno.land/p/~~~`
			- can not import realms but can be imported to realms or packages
		- Realms:
			- Smart Contracts in Gno
			- stateful: xem source code, biểu diễn trạng thái nội bộ của họ ( Render ())
			- can own assets (tokens)
			- default import path: `gno.land/r/~~~`
			![[Pasted image 20240715095923.png]]
	- **GnoVM**:
		- a virtual machine that interprets Gno
		- work with **Tendermint2**
- **Function**:
	- init():
		- cornerstone, automatically triggered when new realm added onchain
		- purposes:
			- initial state, specifically, setting global variables
			- communicates with another realm, ex: register itself in a registry
- **Proof of Contribute**
	- gno.land chain utilizes a reputation-based consensus mechanism instead of **proof-of-stake**
	- Component:
		- **gno.land**:
			- powered by the TM2 engine
			-  offers permissionless smart contracts with the `GnoVM` and can self-configure from contracts using the `GnoSDK`.
		- **worxDAO**:
			- The governance entity consisting of contributors, responsible for governing the `r/sys` realms, including `validators` and `config`.
- **Onchain**:
	- hoạt động, giao dịch, dữ liệu được ghi trực tiếp trên blockchain => 
		- minh bạch: đều công khai và có thể kiểm tra được
		- bảo mật: bảo vệ bởi cơ chế đồng thuận
		- độ tin cậy cao: do tính không thay đổi được
	- chi phí cao, chậm
- **Offchain**: 
	- các hoạt động, giao dịch, dữ liệu xử lý bên ngoài block chain
		- chi phí thấp, nhanh, linh hoạt
		- minh bạch, bảo mật thấp: do không công khai và thiếu cơ chế chấp nhận đồng thuận
#### How to deploy a Realm/Package
- Điều kiện:
	- có quyền truy cập vào `gnoland` node
	- tạo một keypair
```go
gnokey maketx addpkg \
--pkgpath "gno.land/r/demo/counter" \
--pkgdir "./r/counter" \
--gas-fee 10000000ugnot \
--gas-wanted 800000 \
--broadcast \
--chainid dev \
--remote localhost:26657 \
MyKey
```
Let's analyze all of the flags in detail:
- `--pkgpath` - path where the package/realm will be placed on-chain
- `--pkgdir` - local path where the package/realm is located
- `--gas-wanted` - the upper limit for units of gas for the execution of the transaction - similar to Solidity's gas limit
- `--gas-fee` - similar to Solidity's gas-price
- `--broadcast` - broadcast the transaction on-chain
- `--chain-id` - id of the chain to connect to - local or remote
- `--remote` - `gnoland` node endpoint - local or remote
- `MyKey` - the keypair to use for the transaction
#### Component
- **Gno.land**: 
	- được vận hành bời TM2 engine, cung cấp smart contract không cần cấp phép hỏi GnoVM, tự config bằng GnoSDK
	- **TM2 Engine**: cung cấp consensus mechanism, đảm bảo network được bảo mật
	- **Permissionless Smart Contract**: bất kỳ ai có thể tạo và deploy smart contract không cần phê duyệt
	- **GnoVM**: thực thi smart contract trên gnoland
	- **GnoSDK**: cho phép tạo và configuage smart contract có thể tương tác liền mạch với blockchain
- **workDAO**:

- **Governor package**: package cung cấp build a **DAO**, tạo điều kiện thuận lợi cho việc quản lý và propose -> vote -> execution
	- build DAOs like eval_dao and contrib_dao
		- **eval_dao**: evaluation on contributions, members in eval_dao is given equally 1:1 voting power(or any value you define), which is initially set during the DAO creation by  As soon as an evaluation is done, proper amount of tokens will be transferred to the contributor from pool.setting weight of a member.
		- **Contributors** are members of **contrib_dao**, formed in a tierred membership system, that voting power of each member is identified by it's weight, instead of a fixed value, it is calculated by tokens one hold, by a customizable helper to get proper tier one should reside, also through a snapshot mechanism to avoid flash loan issues.
	- The Governor Package (gno.land/p/demo/gor): A pure package (module) for building a DAO
	- eval_dao: The DAO responsible for evaluating contributions (1 vote per account)
	- contrib_dao: The DAO responsible for governing the gno.land chain (1 vote per token)
```go
p/demo/gor/
ㅣ
ㅣ─ checkpoints/
ㅣ     ㄴ  checkpoints.gno : Creates checkpoints based on block heights (Used for balance snapshots)
ㅣ
ㅣ─ counters/
ㅣ     ㄴ  counters.gno : Counts the Nonce of addresses (A security measure to prevent replay attacks)
ㅣ
ㅣ─ governor/
ㅣ     ㄴ dao_votes.gno : Implements the voting feature. Offers 4 options > Against, For, Abstain, NWV
ㅣ     ㄴ deposit.gno : Implements the deposit feature. The burn function exists as a placeholder as of now.
ㅣ     ㄴ governor.gno : Implements features for creating and specifying the specs of a DAO.
ㅣ     ㄴ governor_settings.gno : Implements features for managing the configs of a DAO such as voting period, quorum, etc.
ㅣ     ㄴ membership.gno : Implements features for DAO membership management such as inviting, removing members, etc.
ㅣ     ㄴ propose.gno : Implements the status, structure, and types of proposals that can be submitted.
ㅣ
ㅣ─ snapshot_token/
ㅣ     ㄴ iss_token.gno : The interface of the snapshot contract.
ㅣ     ㄴ snapshot_token.gno : Implements features to check, mint, burn, transfer, and delegate voting power of/from an account.
```
