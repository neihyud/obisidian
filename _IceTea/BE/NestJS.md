---
title: NestJS
---

# NestJS 
- **Serialization**:  is a process that happens before objects are returned in a network response (vd: k return passwork )
# Lifecycle
![[life_circle.png]]
![[Pasted image 20240817181113.png]]
![[Pasted image 20240729181917.png | 700]]
![[Pasted image 20240817162134.png]]
- **providers**: Các provider sẽ được khởi tạo bởi bộ injector của Nest và có thể được chia sẻ ít nhất trong module này.
- **controllers**: Tập hợp các controller được định nghĩa trong module này và cần được khởi tạo.
- **imports**: Danh sách các module được import, mà các module này xuất ra các provider cần thiết cho module hiện tại.
- **exports**: Tập hợp các provider được cung cấp bởi module này và nên có sẵn để sử dụng trong các module khác khi import module này. Bạn có thể sử dụng chính provider hoặc chỉ token của nó (giá trị `provide`).
![[Pasted image 20240817195736.png]]
# DI
- DI (Dependency Injection) kỹ thuật lập trình giảm sự phục thuộc giữa các đối tượng, các phụ thuộc đối tượng không được tạo bên trong đối tượng mà được cung cấp từ bên ngoài
    - thường được thực hiện qua 3 cách:
        - Constructor Injection 
        - Setter Injection  
        - Interface Injection.
```java=
=> ex: constructor injection

public interface Outfit {
    void wear();
}    

public class Bikini implements Outfit {
    public void wear() {
        System.out.println("");
    }
}

public class Girl {
    private Outfit outfit;

    public Girl(Outfit outfit) {
        this.outfit = outfit;
    }

    public void dressUp() {
        outfit.wear();
    }
}
```

- IOC (Inversion of Control): nguyên tắc lập trình
# Providers
- can injected as a dependency => create various relationship with each other
- **injected**: có thể sử dụng **private** => declare and initialize

# Dependencies
![[Pasted image 20240729181846.png]]
![[Pasted image 20240729181905.png]]


# File name convention

![[Pasted image 20240729181944.png]]
# CLI 
![[Pasted image 20240729182001.png]]

![[Pasted image 20240729182010.png | 500]]

![[Pasted image 20240729182101.png]]

**Provider**: things that can be used as dependencies for others classes


![[Pasted image 20240729182047.png]]
![[Pasted image 20240729182116.png]]

Notice the use of the **private** syntax. This shorthand allows us to both declare and initialize the **catsService** member immediately in the same location.


# Migration
- change structure table of database
- TypeOrm: synchronize-true only use for dev-environment
- if true, its will auto update when change structure table 

  app.useGlobalPipes(new ValidationPipe({ whitelist: true })): loại bỏ các tham số không cần thiết trong dto
  
```javascript=
class A {
    repo: Repo
    contructor(repo: Repo)
}
```


typeOrm: nếu gọi save() với plain text thì hook ko được thực thi (AfterUpdate)

![[Pasted image 20240729182140.png]]
save và remove hook được thực thi


![[Pasted image 20240729182149.png]]
![[Pasted image 20240729182206.png]]


**Intercept incoming**

![[Pasted image 20240729182217.png]]
# Guard
- **Guards** are executed after all middleware, but before any interceptor or pipe.
- But middleware, by its nature, is dumb. It doesn't know which handler will be executed after calling the next() function. On the other hand, Guards have access to the ExecutionContext instance, and thus know exactly what's going to be executed next

1# Serialization
- Serialization is a process that happens before objects are returned in a network response.