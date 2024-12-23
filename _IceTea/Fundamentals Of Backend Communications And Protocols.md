# Polling
- client gửi yêu cầu định kỳ đến server để kiểm tra xem có thông tin hoặc sự kiện mới nào xảy ra không. Polling thường được dùng khi server không chủ động thông báo (push) dữ liệu tới client.
## Short Polling
- Client gửi các yêu cầu HTTP liên tục (hoặc định kỳ) đến server để kiểm tra dữ liệu mới.
- Mỗi request hoạt động độc lập và server trả về phản hồi ngay lập tức, cho dù có dữ liệu mới hay không.
- Ex: kiểm tra tin nhắn mới mỗi 5s
- **Nhược điểm**: 
	- **Hiệu suất kém:** Gây lãng phí tài nguyên do phải gửi nhiều request ngay cả khi không có dữ liệu mới.
	- **Độ trễ cao:** Phụ thuộc vào khoảng thời gian polling, thông tin có thể bị chậm.

```
setInterval(fetchUpdates, 5000); (send request to backend every 5s)
```
## Long Polling
- Client gửi một request đến server, nhưng thay vì trả lời ngay lập tức, server giữ kết nối mở cho đến khi có dữ liệu mới hoặc hết thời gian chờ (timeout).
- Khi có dữ liệu mới, server trả lời và kết nối được đóng lại. Client sẽ gửi một request mới để lặp lại quy trình.
```js
const app = require("express")();

const jobs = {}
  
app.post("/submit", (req, res) => {
	const jobId = `job:${Date.now()}`
	jobs[jobId] = 0;
	updateJob(jobId,0);
	res.end("\n\n" + jobId + "\n\n");
})

app.get("/checkstatus", async (req, res) => {
	console.log(jobs[req.query.jobId])
	//long polling, don't respond until done
	while(await checkJobComplete(req.query.jobId) == false);
	res.end("\n\nJobStatus: Complete " + jobs[req.query.jobId] + "%\n\n")
})

app.listen(8080, () => console.log("listening on 8080"));

async function checkJobComplete(jobId) {
	return new Promise( (resolve, reject) => {
	if (jobs[jobId] < 100)
	this.setTimeout(()=> resolve(false), 1000);
	else
	resolve(true);
})
}

function updateJob(jobId, prg) {
	jobs[jobId] = prg;
	console.log(`updated ${jobId} to ${prg}`)
	if (prg == 100) return;
	this.setTimeout(()=> updateJob(jobId, prg + 10), 10000)
}
```