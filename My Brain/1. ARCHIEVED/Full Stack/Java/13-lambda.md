---
title: 
date: 2024-01-09T14:14:00
---
Related:[[JavaOOP]]
Tag: #java #lambda
___
**lambda là gì?** nó có từ SE8
nhiệm vụ của #lambda là thay thế cho kỹ thuật anonymous ở interface và chỉ dùng đc ở interface ko dùng đc ở abstract class
ưu điểm: nhanh, code ít
syntax: (argument-list) -> (body)
nếu body có một lệnh thì ko cần return
```java
package runtime;
/*
    Trong 1 file java có thể có nhiều class
        nhưng chỉ có 1 class đc trùng tên file và phải public
*/
public class Program {

    public static void main(String[] args) {
        Human h1 = () -> {//viết code cho thằng show
            System.out.println("Ahihi");
        };//viết zậy gọi là #lamda kỹ thuật này thay thế cho anonymous và đây cũng là lý do interface nên có 1 method
        h1.show();
        
        //demo
        Math m1 = (int a, int b)->{
            return a + b;
        };
        System.out.println(m1.add(5, 7));
        
        //short hand
        Math m2 = (a, b)->(a + b);
        System.out.println(m2.add(5, 7));
    }
}

//inner class 
@FunctionalInterface//nó sẽ quy ước cho interface này ko đc có 2 cái method
interface Human{
    public void show();
}

interface Math{
    public int add(int a, int b);
}
```
> [!hint] 
> **tại sao interface chuẩn thì chỉ có 1 method?** tại vì để dùng lambda, vì khi code lambda ko viết tên method và lambda sẽ tự bổ sung code cho 1 cái method, vậy khi interface có 2 method thì lambda ko biết bổ sung code cho method nào 

[Lớp lồng nhau trong java (Java #innerclass)](https://gpcoder.com/2416-lop-long-nhau-trong-java-java-inner-class/)