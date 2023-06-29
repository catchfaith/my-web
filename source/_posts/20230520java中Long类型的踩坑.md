---
title: java long类型的踩坑
date: 2023-05-20 19:07:05
categories: Java
---
## 事件场景
在做一次递归的中 需要对比两个Longid 使用了 ==，发现id大于110多左右的时候可以正常插入但是到了200+的时候 == 判断走到if中卡了一天，最开始以为是list.stream()或者是递归写的有问题，后面在多次调试对比发现是Long类型的问题

- 直接声明的Long或者是long [-128~127]之间使用 == 或者 equlas都是true

> 因为java中Long包对常用的做了缓存，如果在这个区间内的话回直接从缓存中取，所以==地址值也是一样的 为true 假如超出了范围 进行 == 就是false
``` java
    Long x = 200L;
    Long l = 200L;
    System.out.println(x == l);//输出false
    System.out.println(x.equals(l)); //输出true
    

```

- new 出来的必须使用equals 虽然做了缓存 但是new出来的是新分配的内存，地址不一样 使用 == 数值即便是一样的也是false
``` java
    Long c = new Long(100L);
    Long d = new Long(100L);
    System.out.println(c==d);
    // 输出：false
```

- 使用 == 不管是直接声明的，还是new出来的 ，只要一边有基本数据类型 都可以正常对比。会自动拆箱
``` java
    Long e = new Long(200L); //会将Long类型拆箱
    int g = 200;
    System.out.println(e == g);
    //输出 true
```

## 总结
在对于两个Long类型对比都使用equals() 