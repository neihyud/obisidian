![[blockchain-concept.png|500]]
![[concept-block.png|400]]- A blockchain is distributed: multiple copies are saved on many machines, and all match for it to be valid 
- là một CSDL phân tán lưu trữ thông tin theo cách blocks và liên kết với nhau tạo thành chain
	- block:
		- đơn vị lưu trữ thông tin
		- chứa dữ liệu giao dịch, dấu thời gian và một liên kết đến khối trước nó
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
## BlockChain Work
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
- verify the accuracy of new transaction added to blockchain
[Decentralized Autonomous Organization](https://www.investopedia.com/tech/what-dao/) (DAO)