---
title: Varmeta

---

# var-meta


:::warning
- Api Learning
    - Edit redeem: 
        - /api/upload/signs3 => {type: image/png}
            - preUrl => điền vào posman
            - banner -> vào body upload 1 image
        bannerKey: id (name-...)
        bannerURl: url image
:::

:::success
learn and earn
- /admin/: sso-admin
- no / : token bình thường
:::

:::info
**UAT** : 
- **Admin**: https://admin-uat.kldx.com/
- **Investor**: https://account-uat.kldx.com/

**Staging**:
- **Admin**: https://admin-staging.kldx.com/
- **Investor**: https://account-staging.kldx.com/

**Production**: 
- **Admin**: https://admin.kldx.com/
- **Investor**: https://kldx.com/

**Jira:**
corporate.var@gmail.com
Xinchao!123 
:::

## Guide

:::success
**Trình tự thực hiện sẽ thế này:** 
Dù ghi là bug production nhưng em test trên UAT trước nha.
- Nếu UAT cũng tồn tại lỗi tương tự -> sửa trên UAT trước, branch: staging
- Nếu UAT không có lỗi này -> sửa trên môi trường staging: branch: staging-release/v1.1.2-AHF

=> Sau khi sửa xong thì mình sẽ đẩy lên các môi trường UAT trước, sau đó đến Staging. Oke thì mới đẩy production.

**Về cách thực hiện:** 
**Đầu tiên lấy mã lỗi của bug này:** RHT-1846, RHT-1830, tạo một issue trên Space (Space là trang để bên varmeta mình quản lí task), và thêm thông tin vào đấy: kiểu RHT-1846: [PROD] As admin, I am unable to edit and click 'Save as Draft' during the cap table when the project is oversubscribed.
Fix bug như ở trên, xong thì đánh Local Done
Push code lên và ghi các commit ở trong phần comment của task Space. Ghi thêm branch e thực hiện fix bug vào trong mục branch của task. Vì thế nên **sửa task trong 1-2 commits** thôi nhá.
=> A build lại server và bên tester update status tiếp
:::


:::success
**Run backend**

- Bên trong kldx -> yarn install 
- cd vào **src/utils/common** -> yarn install -> yarn build -> yarn link
- cd vảo src/utils/server

Vào từng thư mục aws, crypt, db, db-orm, notify, sftp, web3-connection và thực hiện yarn install -> yarn build trong từng thư mục. Nếu build lỗi mà có mấy cái digicap-... gì đó thì yarn link cái đó. Ví dụ yarn link @digicap/utils-common, xong yarn build lại


cd lại thư mục src/utils/server -> yarn install -> yarn build. Build xong thì vào src/utils/server/dist/types/src/core.d.ts, sửa dòng /db-orm -> ../../db-orm.

cd vào src/services/admin-api và yarn install -> yarn build. 
cd vào src/services/api và yarn install -> yarn build
File env của src/services/admin-api
Attachment file type: unknown
env
4.69 KB
File env của src/services/api
Attachment file type: unknown
env
3.12 KB
:::


## Task
:::info

- kldx: nodev16
    - ui/admin: nodev16
    - service/admin: v16
- **UAT**
    trangdang.var+Commercial@gmail.com
    trangdang.var+HeadCommercial@gmail.com
    trangdang.var+Operator@gmail.com
    trangdang.var+HeadPML@gmail.com / Trang@123

- **Staging**
nguyenngoctu.qc2512+stagingHeadPML@gmail.com/Password!99
nguyenngoctu.qc2512+stagingCommercial@gmail.com/Password!99
nguyenngoctu.qc2512+stagingHOC@gmail.com/Password!99
nguyenngoctu.qc2512+stagingHOO@gmail.com/Password!99
nguyenngoctu.qc2512+stagingOperator@gmail.com/Password!99
HOC là headCommercial còn HOO là head operator nhá

-> phải điền đẩy đủ các trường -> gửi cho head comerital
-> để add them invester trong cap-table -> p có thời gian open
-> để check xem có save không thì cần allocation ( table issuarance (project_offering_date, project_closing_date))  (cần vào account operation)

---
operator: edit được các table
commercal: edit project detail

- status (approved -> open): headPML để publish lên blockchain
- Commercial -> HeadCommercial -> HeadPML (Aprroved -> Pushing) -> Public
:::

:::danger
- min token raise <= current <= max token raise: không có nút edit và save as draft
:::

```javascript=
UAT: done
Staging: staging-release/v1.1.2-AHF -> done
```

## KLDX
### Status Project
![image](https://hackmd.io/_uploads/ryElDxagA.png)
