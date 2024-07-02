---
title: NestJS

---

# NestJS 
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


# 

![image](https://hackmd.io/_uploads/rJ8nA8H-0.png)

![image](https://hackmd.io/_uploads/HJCe7DHWC.png)

![image](https://hackmd.io/_uploads/B1YEmvBbA.png)

# File name convention

![image](https://hackmd.io/_uploads/ryVayRr-C.png)

# CLI 
![image](https://hackmd.io/_uploads/ByQRr0r-0.png)

![image](https://hackmd.io/_uploads/B1RFFXDbR.png)

![image](https://hackmd.io/_uploads/HytpP-hXA.png)

Provider: things that can be used as dependencies for others classes

![image](https://hackmd.io/_uploads/S1v7t-2mC.png)

![Screenshot from 2024-05-23 23-21-02](https://hackmd.io/_uploads/BJ4Ua1TmC.png)

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

![image](https://hackmd.io/_uploads/H18bL4GER.png)
save và remove hook được thực thi

![image](https://hackmd.io/_uploads/H1uOUVfVR.png)



![Screenshot from 2024-05-28 06-29-15](https://hackmd.io/_uploads/HyFFv9G4A.png)
intercept incoming



![image](https://hackmd.io/_uploads/B1ERmKQVC.png)

# Guard
- **Guards** are executed after all middleware, but before any interceptor or pipe.
- But middleware, by its nature, is dumb. It doesn't know which handler will be executed after calling the next() function. On the other hand, Guards have access to the ExecutionContext instance, and thus know exactly what's going to be executed next

# Serialization
- Serialization is a process that happens before objects are returned in a network response.