---
title: MongoDB & Mongoose
tags: [Database]

---

###### tags: `Database`
# MongoDB & Mongoose
https://mongoosejs.com/docs/



# Tag
$in: []: tìm match array
$pull: {array: id} => xóa một phần tử trong mảng
$push: {array: id} => thêm một phần tử trong mảng
$nin: {array: [id]} => không match array
populate: {
    path: this.field trong Schema gọi populate (A.populate => field trong A)
}

- plugin: công cụ sử dụng login trong nhiều lược đồ
- query ís not promise
-  Query middleware executes when you call exec() or then() on a Query object, or await on a Query object. In query middleware functions, this refers to the query.
-  Lưu trữ khoảng 16Mb, feature in schema 10, array: n*100 (n < 5)

# Aggregation

![](https://hackmd.io/_uploads/HkecKvHEh.png)

- $lookup: left join (array) thường đi với $unwin (mảng => giá trị riêng lẻ)

# Schema: 
- khi tạo một intance Schema có thể gắn
- `method:`: gán method cho schema
- `virtual`: giả sử schema có first name và last name, muốn lưu first name + last name (k lưu vào mongodb) thì ta có thể sử dụng `virtual` 
- `aliases`: gán định danh cho một thuộc tính của Schema để gọi ra ngoài, e.g 'name' có aliase `n` thì khi gọi `obj.name` ta có thể gọi `obj.n`
- `query`: trợ giúp truy vấn

option: plugInTags: có thể add cho tất cả Schema, hoặc add cho một schema
# Schema Types

- chứa các option thuộc tính: e.g type Number có options: min,max,...
- ObjectId(): return về một id


# Refercence

- skip(params): chỉ lấy từ params trở về sau, thường đi với `limit` để phân trang hoặc lấy giới hạn trả về
- lean() (trong mongoose): để trả về js thuần không trả về schema nên không thể sử dụng save(), delete(), update() ...

# Options
- upsert: true. update => không tìm thấy documents, chèn một document ms
- $in: match với bất kỳ giá trị nào trong mảng
- $unset: remove field cụ thể, nếu field k tồn tại thì k ảnh hưởng gì đến hoạt động 

# Syntax
- updateOne([filter], [update], [option])
    - option.upsert: k có thì tạo mới
    - .n: số doc match
    - .nModified: số documents modified
- findOne([conditions],'chọn field trả về')
- $in: giống where ... and ...
- .populate(param1, param2): giống với left join like trong sql
    - ref: cho biết nó liên kết với model nào
    - param1: ref model của chính nó để tham chiếu tới bảng khác
    - param2: select field in model ref khác (nếu k có thì nó trả về toàn bộ)
    - (require model vào file chứa populate)
    ```javascript=
    params:
        - `path`: (String|Obj|Array): path trong model hiện tại để ref 
        tới model khác (attr nào)
        - `select`: chọn các tham số từ model hiện tại tham chiếu tới model
        khác
        - `model`: tham chiếu đến model nào đó, nếu k có tham số này thì 
        nó mặc định lấy ở tên ở attr 'ref'
        - `match`: điều kiện tìm kiếm
    ```

```javascript=
UserModel.find()                       // find all users
         .skip(100)                    // skip the first 100 items
         .limit(10)                    // limit to 10 items
         .sort({firstName: 1}          // sort ascending by firstName
         .select({firstName: true}     // select firstName only
         .exec()                       // execute the query
         .then(docs => {
            console.log(docs)
          })
         .catch(err => {
            console.error(err)
          })

```

# Nếu không tìm thấy giá trị
- .find(): []
- .findOne(): null




# MongoDb
- index in mongodb
https://www.digitalocean.com/community/tutorials/how-to-use-indexes-in-mongodb#step-5-creating-a-compound-field-index

https://www.digitalocean.com/community/tutorials/how-to-use-indexes-in-mongodb#step-4-creating-an-index-on-an-embedded-field

# Index trong mongodb
- hỗ trợ thực thi truy vấn, nếu k có p quét toàn bộ collection
- index: mongo duy trì một danh sách riêng, chứa những con trỏ chỉ đến sản phẩm
- một query k có index gọi là table scan
- _id: index mặc định duy nhất, k cho hai tài liệu trùng giá trị, k thể bỏ _id
    - tên chỉ mục:  
    - sự kết hợp giữa các keys được lập index và mỗi key của index (1: tăng dần hoặc -1: giảm dần)
- sau khi tạo không thể thay đổi index
- nếu index đã có trong truy vấn thì mongodb sẽ sử dụng nó trước để thu hẹp query r mới duyệt qua tất cả collection (trong compound index)
- khi find, thì nó sẽ lấy mọi document từ collection và so sánh xem cái nào khớp thì nó sẽ thêm vào một danh sách được trả lại
## Các loại index
- Single Field: ngoài index default còn có chỉ mục tăng dần giảm dần, các key thì không quan trọng, chỉ đánh index cho một field
- Tạo index trong field trường nhúng: sử dụng dấu .
- Compound Index (index tổng hợp): người dùng định nghĩa trên nhiều field
- Multikey Index(): isMultiKey(giá trị mặc định) trong single field để tạo mutilkey index trong db => dành cho field array (khi find khi chưa có index thì nó quét bắng vs số lượng có index)
### Index text
- chỉ định ngôn ngữ cho index text: defalt_language
- mongodb tự động bỏ qua từ dùng (vd a, an, the, this)=> triển khai tìm kiếm gốc hậu tố
- loại bỏ ed, es
- value: text mặc định ngôn ngữ là english
- chỉ định tên cho index, sử dụng key: name
- tìm kiếm văn bản chỉ định cho mỗi tài liệu có chứa cụm từ tìm kiếm trong trường được lập index
- feature: weight chỉ định tầm quan trọng tương đói của field được lập index
- vd: cà phê ngọt, sử dụng index text để tìm kiếm các text chỉ chứa ít nhất một từ trong đó, thiết kế riêng cho full text search dựa trên các trường này
### Full text search
- là một hình thức nâng cao trong việc tìm kiếm dữ liệu tong database
- không sử dụng field như tong index text mà sử dụng toán tử $text => cho biết truy vấn sử dụng index text
- { $text: { $search: "ice cream" } } // tìm kiếm môt trong hai từ ice cream
- để tìm kiêm đầy đủ  { $text: { $search: "\"ice cream\" } }
- để loại trừ sử dụng - trước từ muốn loại trừ
- cho điểm kết quả và sắp xếp theo điểm
```javascript=
db.recipes.find(
    { $text: { $search: "spiced espresso" } },
    { score: { $meta: "textScore" } }
)
```



## Relationship in mongodb
- relationship: 1-1, 1-N, N-N, được biểu thị qua hai mô hình: nhúng, tham chiếu
- nhúng: nhóm các thông tin cần thiết và giữ nó trong một document duy nhất
    - giới hạn kịch 16kb tại thời điểm viết, nesting limit 100 level khi viết
- tham chiếu:



- event loop lấy code từ call stack để thực hiện, khi gặp bất đồng bộ nó sẽ chuyển sang cho pool of threads dể thực hiện, sau đó các threads sẽ xử lý song song bất đồng bộ sau đó return về một event cho event loop sau khi hoàn tất
 => bản chất của event loop k hoạt động tốt với CPU intensive operations vì các hoạt động dựa trên CPU được thực thi bởi chính event loop (k được gửi cho thread pool)
 =>  event loop vẫn chiếm dụng và program bị chặn không cho thực thi thêm
- process: khi chạy sẽ tạo ra một bộ nhớ riêng
- thread: k có bộ nhớ riêng, nằm trong bộ nhớ của process

# Transaction

- withTransaction: tự động thực hiện commit và tự động khôi phục khi xảy ra lỗi trong transaction


```javascript=
session.startTransaction();        // for starting a new transaction
await session.commitTransaction(); // for committing all operations
await session.abortTransaction();  // for rollback the operations


const conn = require("../models/connection"); 
const User = require("../models/user.model");
const ShippingAddress = require("../models/address.model");
 
const register = async () => {

    const session = await conn.startSession();
    try {
        session.startTransaction();                    
        const user = await User.create([
            { 
                name: 'Van Helsing' 
            }
        ], { session });

        await ShippingAddress.create([
            {
                address: 'Transylvania',
                user_id: user.id
            }
        ], { session });
        await session.commitTransaction();
        
        console.log('success');
    } catch (error) {
        console.log('error');
        await session.abortTransaction();
    }
    session.endSession();
}
```


```javascript=
const conn = require("../models/connection"); 

const example = async () => {
    const session = await conn.startSession();

    try {
        session.startTransaction();  

        await Model.create([{ /* payload */ }], { session });

        await Model.deleteOne({ /* conditions */ }, { session });

        await Model.updateOne({ /* conditions */ }, { /* payload */ }, { session } );

        await Model.findByIdAndUpdate(_id, { /* payload */  }, { session });

        const user = new Model( /* payload */);
        await user.save({ session });
        
        await session.commitTransaction();
         
    } catch (error) { 
        await session.abortTransaction();
    }
    session.endSession();
}    
```