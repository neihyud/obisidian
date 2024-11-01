# Note
- <font color="#ff0000">It is not possible to check for NULL values with comparison operators, such as =, <, or <>.</font>
- If you want to check that there is some value, you should use the operator IS NOT NULL.
# MVCC (Multi-Version Concurrency Control)
- A technique used by PostgreSQL to allow multiple transactions to access the same data concurrently without conflicts or delays. It ensures that each transaction has a consistent snapshot of the database and can operate on its own version of the data.
- Trong MVCC, mỗi giao dịch khi bắt đầu sẽ "thấy" một bản sao cụ thể của dữ liệu, gọi là _phiên bản_ (version) của dữ liệu tại thời điểm đó. Khi một giao dịch sửa đổi dữ liệu, nó không ghi đè trực tiếp lên bản gốc, mà tạo ra một phiên bản mới của dữ liệu với các thay đổi. Điều này giúp các giao dịch khác vẫn có thể đọc phiên bản trước đó của dữ liệu cho đến khi giao dịch sửa đổi được hoàn tất.
# Query Processing
## Parse
-  queries are checked for syntactic validity.
-  converts plain text SQL statements into a parse tree. The parse tree can then be processed by the other subsystems of the backend.
## Analyzer
- examines the query to ensure all referenced objects exist and are accessible, and it also infers the data types of all expressions.
## Rewriter
-  any views referred to in the query are expanded and any rule-based substitutions are applied.
## Planner
-  The planner evaluates various strategies for executing the query based on the available indexes and the estimated cost of different approaches.
## Executor
- the executor carries out the chosen plan, actually retrieving or modifying data.
# Domain
 - Kiểu dữ liệu do người dùng tự định nghĩa
