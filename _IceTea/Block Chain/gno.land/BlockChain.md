![[blockchain-concept.png|500]]
![[concept-block.png|400]]
- Data => chứa transaction data
- A blockchain is distributed: multiple copies are saved on many machines, and all match for it to be valid 
- **Core**: [Blockchain Architecture](https://mlsdev.com/blog/156-how-to-build-your-own-blockchain-architecture)
	- **Node**: has an independent copy of the whole blockchain ledger
	- **Transaction**: smallest building block of blockchain system( records, information, ...) that serves as the purpose of blockchain
	- **Block**: a data structure used to for keeping a set of transactions which is distributed to all nodes in the network
	- **Chain**: a sequence of blocks in a sepecific order
	- **Miners**: specific nodes which perform the block verification process before adding anything to the blockchain structure
	- **Consensus (consensus protocol)** - a set of rules and arrangements to carry out blockchain operations
	=> Any new record or transaction within the blockchain implies the building of a new block. Each record is then proven and digitally signed to ensure its genuineness. Before this block is added to the network, it should be verified by the majority of nodes in the system.
- Khi một block được tạo, nó sẽ gửi đến tất cả các nodes bên trong hệ thống blockchain => verify block là chính xác => block được add vào local blockchain của mỗi node
	=> tất cả nodes trong blockchain tạo moojt **consensus protocol**
	**Consensus System**: set of network rules, 
	
- là một CSDL phân tán lưu trữ thông tin theo cách blocks và liên kết với nhau tạo thành chain
	- block:
		- đơn vị lưu trữ thông tin
		- chứa dữ liệu giao dịch, dấu thời gian và một liên kết đến khối trước nó
		- một block có thể chứa nhiều transaction
	- chuỗi:
		- kết nối với nhau tạo thành blockchain
	- phi tập trung (decentralized): blockchain không có điểm kiểm soát, duy trì bởi một mạng lưới các node độc lập
	- node: một máy tính tham gia vào blockchain
	- consensus (đồng thuận): quá trình các node trong blockchain đồng ý về trạng thái hiện tại của blockchain
	- sổ phân tán (distributed ledger): blockchain là một dạng sổ cái phân tán, nơi thông tin được lưu trữ và chia sẻ trên toàn mạng
- **Genesis block**:
	- khối đầu tiên trong một mạng lưới blockchain
- **Secure**: 3 technique
	- Proof of Work
	- Merkle Tree Hash Methodology
	- P2P Network
- **Node**: là một bản sao của ledger
- **Blockchain structures fall into 3 categories**:
	- **Public blockchain**: data, access to the system is avaiable to anyone who wiling to participate
	- **Private blockchain**: only by users from a specific organization or authorized users who have an invitation for participation.
	- Consortium blockchain
	![[type_category_blockchain.png]]
## BlockChain Work

![[blockchain_work0.png|500]]
- b1. Ghi lại giao dịch
	Ai tham gia, xảy ra ở đâu, khi nào, vì sao, giá trị trao đổi, điều kiện tiên quyết
- b2. Đạt được đồng thuận
- b3. Liên kết các khối
	- giao dịch trên blockchain sẽ được ghi vào block (~ ghi trên giấy trong sổ cái), một hàm băm cũng được thêm vào block - vai trò liên kết khối, khi thay đổi -> băm thay đổi -> phát hiện dữ liệu giả
	- 
## BlockChain Layer
- **ref**: https://goonus.io/layer-0-layer-1-layer-2-layer-3-la-gi-tim-hieu-ve-cac-blockchain-layer/
- Cấp độ cở sở hạ tầng hoạt động cùng nhau để vận hành hệ thống. Mỗi layer được xây dựng dựa trên layer trước đó và tận dụng cở sở hạ tầng của các layer trước đó
	- **Network Layer**: 
		- mạng vật lý
		- các node giao tiếp với nhau => mạng blockchain
		- phân phối dữ liệu trên mạng
	- **Consesus Layer**:
		- node đều đồng ý về tính hợp lệ của giao dịch
		- dựa trên cơ chế đồng thuận: ex Proof of Work (Pow), Proof of Stake (PoS) để xác thực giao dịch và thêm vào block chain
	- **Data layer**:
		- lưu trữ dữ liệu giao dịch
		- bao gồm: [[######sổ cái giao dịch]] - chứa tất cả giao dịch trong blockchain và CSDL trạng thái, trạng thái hiện tại của blockchain
	- **Application Layer**: gồm smart constract, dApp
	- **Hardware Layer**: phần cứng
## Layer
- **Layer 0**: 
	- nền tảng layer 1 xây dựng trên nó
	- vai trò: CSHT, mạng, app
	- gồm: internet, phần cứng, các kết nối
- **Layer 1:** 
	- network layer (Mạng lưới Ethereum)
	- chịu trách nghiệm về đồng thuận, ngôn ngữ lập trình, time block??, 
## Resource
###### Sổ cái giao dịch

###### Proof of Work
[PoW - Example - JS](https://simplyexplained.com/blog/Implementing-proof-of-work-javascript-blockchain/)
- verify the accuracy of new transaction added to blockchain
- làm chậm tiến trình tạo block
- ngăn chặn spam tạo khối: 
	- trong bitcoin: 
		- PoW yêu cầu hash một block bằng một số lượng cụ thể các số 0
		- Quá trình tính toán để tìm kiếm một hash hợp lệ được gọi là "đào"
	-  
[Decentralized Autonomous Organization](https://www.investopedia.com/tech/what-dao/) (DAO)


- Chỉ là lưu trữ các transaction, giữa các node, các node kết nối với nhau, khi một transaction được tạo ra từ một node, nó sẽ gửi đến các node khác để tính toán lại
- smart contract: phải gọi lên mới thực hiện được

![[stable-coin.png| 500]]