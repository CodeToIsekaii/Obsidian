---
title: 
date: 2024-01-08T16:46:00
---
Related:[[JavaOOP]]
Tag: #java #inheritance  #abstract
___

```java
package runtime;

import data.Shape;
import data.Disk;
import data.Rectangle;
import data.Square;

public class Program {
    public static void main(String[] args) {
        //tạo ra 1 cái mảng có thể lưu trữ đc rectangle, square , disk
        //tạo trực tiếp
        Shape ds[] = new Shape[4];
        ds[0] = new Disk("tía", "Pink", 2.0);
        ds[1] = new Rectangle("hường", "hồng", 2, 3);
        ds[2] = new Square("lục", "green", 5);
        //tạo gián tiếp 
	   //khuôn//biến con trỏ//phễu  
        Disk d = new Disk("mợ", "đỏ", 5);//mỗi lần new là tạo vùng nhớ bự chà bá và lưu vào heap 
        ds[3] = d;    //và d là biến pointer nằm ở stack và trỏ vào heap để điều khiển vùng nhớ bự chà bá đó
        
        //in ra
        for (Shape item : ds) {
            item.paint();
        }
        #anonymous
        //vừa đúc vừa vá
        Shape xxx = new Shape("xxx", "Da") {//tổng là kỹ thuật anonymouse
            @Override
            public double getPerimeter() {
                return 12;
            }
            
            @Override
            public double getArea() {
                return 200;
            }
            
            @Override
            public void paint() {
                String str = String.format("XXX    |%-10s|%-10s|%5.2f|%5.2f|",
                            owner, color, getPerimeter(), getArea());
                System.out.println(str);
            }
        };//class anonymous
        xxx.paint();
        
        //Drift: con trỏ của object #pointer
        Rectangle r1 = new Rectangle("R1", "Red", 2, 5);//1 hình chữ nhật đc tạo 1 cái khuôn rectangle đc r1 điều khiển
        Square s1 = new Square("S1", "Yellow", 4 );
        Rectangle r2 = new Square("R2", "Green", 8);
        r2.paint();// in ra square bth vì nó bị override nằm ở trên nên thấy 
        //r2. thì sẽ ko có drawTitle            //cha nhỏ hơn con nên chỉ trỏ dc trong vùng của nó
        //thực ra trong vùng nhớ con đã có vùng nhớ cha
        //nhưng r2 là 1 con trỏ dành cho Rectangle, nên nó chỉ 
        //sờ vào đc khu vực của Rectangle
        //mà drawTitle là 1 method riêng của Square
        //khai cha new con
        //=> xài đc override method, nhưng ko xài đc hàm riêng của con
        //Drift => ép kiểu
        Square tmp = (Square)r2; //r2 là rectangle nhưng đc tạo là new square nên nội thất là hình vuông nên tao nói nó là hình vuông
        tmp.drawTitle();//tmp con trỏ mới dài hơn nên chấm đc
        ((Square)r2).drawTitle();//viết ngắn hơn
    }      
}
```
![[Pasted image 20240111092736.png|250]]
**muốn tạo object cần j?**
    cái khuôn - class
    cái phễu - cóntructor
    biến con trỏ = new ???;
    Dog chipu = new Dog(?,?,?);
        
**object đc tạo từ class con thì sao?**
    y chang luôn cũng cần khuôn , phễu, biến con trỏ
    Square sq = new Square(?,?);
        
Tuy nhiên nếu ta phân tích kỹ vùng nhớ của object con thì thấy vùng new con có vùng new cha
- Mục đích:
	+ giúp thằng con có tất cả những gì cha có
	+ bản thân của thằng con và thằng cha có thể sống độc lập
	+ nhưng để nhận cha con, thì thằng con cho cha những điểm chung và kế thừa là lúc nó lấy lại những thứ đó
	=> ko cần phải làm lại những điều người khác đã làm tốt
- **Phân tích sâu vào object con**
	Square s1 = new Square();
	 new Rectangle() + code riêng của con
	thằng con chính là: new cha() + code riêng của con
	    super(vùng nhớ thằng cha nằm trong thằng con)
        di truyền   +   extends(vùng biến dị so với cha)
        inherit
- **Nếu cha là abstract class thì sao?**
    object con sẽ như nào: new cha + code của riêng con + code cho abs method của cha,                                        bản thân abs class ko tạo đc object
    nó cần class trung gian nhưng đôi khi mình lười, mình ko thích đi qua trung gian mà cứ đòi dùng cái khuôn thủng  để đúc ra object
     => kỹ thuật anonymous / mượn gió bẻ măng
     #anonymous
> [!attention] 
> **khi nào dùng anonymous**: khi muốn tạo object từ abs class mà ko thông qua class trung gian em đúc ra object từ cái khuôn bị thủng
**Ưu điểm**: ko cần tạo class trung gian(ko cần đặt tên) tạo object trực tiếp từ khuôn thủng
**Nhược điểm**: mỗi lần đúc là mỗi lần implement và chỉ làm đc 1 lần ko thể làm lại đc 

> [!warning] 
> trong 1 hệ thống đang vận hành bình thường có 1 thằng rất giống những thằng trong hệ thống nhưng ko biết tên -> anonymouse 

#abstract 
0. tạo class con
1. khai báo đặc tính riêng của con
2. cho con nhận cha = "extends"
3. tạo phễu: constructor
4. làm phần việc mà cha giao cho em là bổ sung cho code cho những abs method đã kế thừa nếu thằng con ko bổ sung thì nó sẽ là abs class và con của nó sẽ làm 
5. nếu ko có abs method thì sao? xem cái nào cần độ thì độ
```java
package data;

public class Rectangle extends Shape{ //implement : thực thi theo code làm rõ cái chưa rõ (vá)
    protected double width ;
    protected double height; 
    //tạo phễu
                    //của cha đặt đầu ngon hơn
    public Rectangle(String owner, String color, double width, double height) {
        super(owner, color);//nó phải ở đầu vì là cha, tạo thằng cha ròi thêm thắt mới ra con
        this.width = width;
        this.height = height;
    }
    //super? new Cha => new Shape
    //super: phải là câu lệnh đầu tiên trong phễu
    //getter 

    public double getWidth() {
        return width;
    }
    public double getHeight() {
        return height;
    }
    @Override
    public double getPerimeter() {
        return (width + height)* 2;
    }
    @Override
    public double getArea() {
        return width * height;
    }
    @Override
    public void paint() {
        String str = String.format("Rectangle|%-10s|%-10s|%5.2f|%5.2f|%5.2f|%5.2f|",
                owner, color, width, height, getPerimeter(), getArea());
        System.out.println(str);
    }
    
}
```
```java
package data;

public class Square extends Rectangle{
    //tạo phễu
    public Square(String owner, String color, double edge) {
        super(owner, color, edge, edge);
    }
    //super? new Cha => new Rectangle
    @Override
    public void paint() {
        String str = String.format("Square   |%-10s|%-10s|%5.2f|%5.2f|%5.2f|%5.2f|",
                owner, color, width, height, getPerimeter(), getArea());
        System.out.println(str);
    }
    //thêm 1 cái hàm nữa
    public void drawTitle(){
        System.out.println("I luv u");
    }
}  
```
Shape là class cha của mọi hình: cha của tam giác, cha của hình tròn, cha của hình vuông,...
*nếu nó là cha thì nó phải làm sao?*
cha chỉ chứa những điểm chung của các con, cha là người gom tụ những điểm chung của các con
```java
package data;

public abstract class Shape {//cái khuôn bị thủng
    protected String owner;
    protected String color; 
    //tạo phễu
    public Shape(String owner, String color) {
        this.owner = owner;
        this.color = color;
    }
    //getter
    public String getOwner() {
        return owner;
    }

    public String getColor() {
        return color;
    }
    public abstract double getPerimeter();//để trống để cho thằng con biết có tồn tại công thức và phải làm,
    public abstract double getArea();//          còn ko có thì có nghĩa là nó ko cần phải làm
    public abstract void paint();
    //tại sao ko viết công thức cho diện tích và chu vi ở đây?
    //ko có công thức tính chung cho tất cả các hình
    //vậy nếu mình bỏ 1 công thức nào đó vào đấy thì các thằng con
    //sẽ kế thừa công thức đó, và công thức đó ko đúng với nó
    
    //tại sao class Shape lại là abstract class?
    //vì nó có chứa abstract method
    
    //tại sao lại có abstract method?
    //
}
```

> [!caution] 
> Lưu ý về #abstract class
abstract class đc ví như 1 cái khuôn bị thủng mà nếu thủng thì ko đúc đc
abstract class thì ko tạo đc object ( nếu tạo đc thì object
sẽ có những method ko dùng đc) 

 *vậy abstract cần làm gì ?*  sẽ cần class khác kế thừa
*class khác kế thừa Shape để làm gì?* để và lỗ hổng mà người đi trước để lại, vá lỗ thủng và Shape chưa định nghĩa (getPerimeter, gatArea, Paint)

*vậy nếu class kế thừa đó ko vá đc hết thì sao?*  thì nó lại cần class khác kế thừa và vá tiếp những gì chưa xong

<mark style="background: #FFB8EBA6;">abstract class ko tạo đc object
nhưng mà ta có thể cố chấp dùng cái khuôn thủng để đúc object</mark>
=> kỹ thuật #anonymous 
```java
*ví dụ :
abs class Shape(){
    abs method getPerimeter();
    abs method getArea();
    abs method paint();
}
class Rectangle(){
     method getPerimeter();(w+h)*2
     method getArea();   w*h
     method paint(); dsgere
}
class Square(){
     method getPerimeter();(w+h)*2
     method getArea();   w*h
     method paint(); dsgere
}
```

```java
package data;

public class Disk extends Shape{
    private double radius;
    public static final double  PI = 3.14;

    public Disk(String owner, String color, double radius ) {
        super(owner, color);
        this.radius = radius;
    }
    //getter
    public double getRadius() {
        return radius;
    }
    @Override
    public double getPerimeter() {
        return PI * 2 * radius;
    }
    @Override
    public double getArea() {
        return PI * radius * radius;
    }
    @Override
    public void paint() {
       String str = String.format("Disk     |%-10s|%-10s|%5.2f|%5.2f|%5.2f|",
                owner, color, radius, getPerimeter(), getArea());
        System.out.println(str);
    }
   
}
```