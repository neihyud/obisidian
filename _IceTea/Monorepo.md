# Nx
```react= 
npx create-nx-workspace --pm pnpm
```
- [install](https://nx.dev/getting-started/installation)
- [command](https://nx.dev/nx-api/nx/executors/run-commands)
- **Run task**
```
npx nx <target> <project> <...options>
```
You can also run multiple targets:
```
npx nx run-many -t <target1> <target2>
```

-` nx g @nx/nest:app my-nest-lib`
- All libraries that we generate automatically have aliases created in the root-level `tsconfig.base.json`.
## Adding Another Application
```npm
npx nx list @nx/react
```

```shell
npx nx g @nx/nest:app 
```
## Sharing Code with Local Libraries
### Creating Local Libraries
```shell
npx nx g @nx/react:library shared-ui --directory=libs/shared/ui --unitTestRunner=vitest --bundler=none
```

```shell
nx g @nx/js:lib my-prisma-schema --unitTestRunner=none --bundler=none --simple-name --minimal
```

# Prisma
- `npx prisma studio`: view db on web


---

1. **Khởi tạo Migrations (Initial Migrations):** Khi bạn bắt đầu một dự án mới hoặc thêm Prisma vào một dự án hiện có, bạn cần khởi tạo một migration ban đầu để thiết lập cơ sở dữ liệu của bạn. Điều này được thực hiện bằng lệnh
    
    `npx prisma migrate dev --name init`
    
    Lệnh này sẽ tạo ra một migration mới dựa trên schema hiện tại của bạn và áp dụng nó vào cơ sở dữ liệu.
    
2. **Tạo Migration mới (Creating New Migrations):** Khi bạn thay đổi schema của mình (ví dụ: thêm, sửa hoặc xóa models hoặc fields), bạn cần tạo một migration mới để phản ánh các thay đổi này. Bạn có thể làm điều này bằng lệnh:
    
    bash
    
    Copy code
    
    `npx prisma migrate dev --name <tên_migration>`
    
    Lệnh này sẽ tạo ra các tệp migration mới mô tả các thay đổi và áp dụng chúng vào cơ sở dữ liệu.
    
3. **Áp dụng Migrations (Applying Migrations):** Nếu bạn có sẵn các migration chưa được áp dụng, bạn có thể áp dụng chúng vào cơ sở dữ liệu bằng lệnh:
    
    bash
    
    Copy code
    
    `npx prisma migrate deploy`
    
    Lệnh này sẽ chạy tất cả các migration chưa được áp dụng, đảm bảo rằng cơ sở dữ liệu của bạn luôn ở trạng thái mới nhất.
    
4. **Xem lại lịch sử Migration (Reviewing Migration History):** Prisma lưu trữ thông tin về các migration đã được áp dụng trong một bảng đặc biệt trong cơ sở dữ liệu của bạn. Bạn có thể kiểm tra lịch sử migration để xem các thay đổi đã được thực hiện như thế nào.
    
5. **Undo Migrations (Rollback Migrations):** Prisma không hỗ trợ trực tiếp việc rollback migration thông qua lệnh CLI như các công cụ khác. Tuy nhiên, bạn có thể tạo các migration ngược lại để hoàn tác các thay đổi trước đó.
    
6. **Migration Locking:** Khi làm việc trong môi trường nhiều người dùng, Prisma Migrate cung cấp cơ chế khóa để ngăn chặn xung đột khi nhiều người dùng cùng tạo và áp dụng migrations.
    

### Quy trình chung để làm việc với Prisma Migrate:

1. **Sửa đổi schema.prisma:** Thay đổi hoặc thêm mới các models và fields trong tệp `schema.prisma`.
2. **Tạo Migration mới:** Chạy lệnh `npx prisma migrate dev --name <tên_migration>` để tạo và áp dụng migration mới.
3. **Kiểm tra và chạy ứng dụng:** Kiểm tra ứng dụng của bạn để đảm bảo rằng các thay đổi đã được áp dụng đúng cách và không gây ra lỗi.