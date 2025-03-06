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
npx nx g @nx/nest:app <folder-name> (apps/be-demo)

npx nx g @nx/react:app <folder-name> (apps/demo)

nx g @nx/js:lib libs/prisma --unitTestRunner=none --bundler=none --simple-name --minimal (config prisma)
```
>[!info]
>**`--unitTestRunner=none`**  → Không thiết lập bất kỳ framework kiểm thử nào (bình thường Nx có thể thiết lập Jest hoặc Vitest).
> **`--bundler=none`**:  Không thiết lập bất kỳ trình bundler nào (bình thường Nx hỗ trợ `esbuild`, `rollup`, v.v.).
>**`--simple-name`**: Đặt tên thư viện đơn giản mà không có tiền tố namespace.
>**`--minimal`**: Tạo thư viện **tối giản nhất có thể**, chỉ tạo những file cần thiết.

## NestJS Prisma Clients
https://github.com/nrwl/nx-recipes/tree/main/nestjs-prisma#nx--nestjs--prisma
https://www.prisma.io/docs/guides/use-prisma-in-pnpm-workspaces
```
nx g @nx/nest:lib my-prisma-client
```
# Prisma
- **install prisma**: pnpm install -D prisma
- pnpm prisma init --db
## Config path to schema.prisma
https://www.prisma.io/docs/orm/prisma-schema/overview/location
**package.json**: 
```javascript
"prisma": {
  "schema": "db/schema.prisma"
}
```
- `npx prisma studio`: view db on web
## Prisma generator
- update file node_module/@prisma/client  => giúp truy vấn db type-safe.
![[prisma_generator.png]]
## Prisma pull
![[prisma_pull.png]]
![[prisma_pull_db-change.png]]
## Prisma Migrate

![[prisma-migrate.png]]

- querying (with [Prisma Client](https://www.prisma.io/docs/orm/prisma-client))
- data modeling (in the [Prisma schema](https://www.prisma.io/docs/orm/prisma-schema))
- migrations (with [Prisma Migrate](https://www.prisma.io/docs/orm/prisma-migrate))
- prototyping (via [`prisma db push`](https://www.prisma.io/docs/orm/reference/prisma-cli-reference#db-push))
- seeding (via [`prisma db seed`](https://www.prisma.io/docs/orm/reference/prisma-cli-reference#db-seed))
- visual viewing and editing (with [Prisma Studio](https://www.prisma.io/studio))



=> 
- `prisma db pull`: db -> schema.prisma
- `prisma db push`: schema.prisma -> db => create migration file
- `prisma migrate dev`: sync with db and create migration file

## Prisma migrate deploy
- áp dụng các file migrate chưa được áp dụng
- `npx prisma migrate deploy`


## Seeding
- Set data default when inital
