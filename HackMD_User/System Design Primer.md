---
title: How to approach a system design interview question
tags: [Design System]

---

###### tags `Design System`

# How to approach a system design interview question

 - Step1: Gather requirements and scope the problem. Ask questions to clarify use cases and constraints. Discuss assumptions.
	- Who is going to use it?
	- How are they going to use it? ^3fda03
	- How many users are there?
	- What does the system do?
	- What are the inputs and outputs of the system?
	- How much data do we expect to handle?
	- How many requests per second do we expect?
	- What is the expected read to write ratio?
- Step2: Create high level design
- Step3: Design core components
	- **Number Everyone Should Know**:
		- L1 cache reference 0.5 ns
		- Branch mispredict 5 ns
		- L2 cache reference 7 ns
		- Mutex lock/unlock 100 ns
		- Main memory reference 100 ns
		- Compress 1K bytes with Zippy 10,000 ns
		- Send 2K bytes over 1 Gbps network 20,000 ns
		- Read 1 MB sequentially from memory 250,000 ns
		- Round trip within same datacenter 500,000 ns
		- Disk seek 10,000,000 ns
		- Read 1 MB sequentially from network 10,000,000 ns
		- Read 1 MB sequentially from disk 30,000,000 ns
		- Send packet CA->Netherlands->CA 150,000,000 ns
		- [[#^3fda03]]