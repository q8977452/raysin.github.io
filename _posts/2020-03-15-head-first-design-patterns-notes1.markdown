---
layout: post
title:  "Head First Design Patterns Notes"
date:   2020-03-18 22:32:30 +0800
categories: Design_Patterns
tags : 設計守則 SOLID 設計模式 LoD CARP SRP OCP LSP ISP DIP
mathjax: true
---
# 設計守則(Design Principle)

## 淺白敘述

* 找出程式中可能需要更動之處，把它們獨立出來，不要和那些不需要更動的程式碼混在一起。(把會變動的部分取出並封裝起來，以便可以以後可以輕易地擴充此部分，而不影響不需要更動的其他部分)

* 寫程式是針對介面而寫，而不是針對實踐方法而寫。(寫程式是針對超型態(supertype)而寫)

* 多用合成，少用繼承

* 設計時，盡量讓需要的互動的物件之間關係鬆綁

* 類別應該開放，以便擴充;應該關閉，禁止修改

* 依賴抽象類別，不要依賴具象類別

* 認識極少化守則 : 只和你的密友談話

* 別呼叫我們，我們會呼叫你(好萊塢守則)

* 一個類別應該只具有一個改變的理由

## 實際定義

1. 單一職責原則 (SRP：Single Responsibility Principle)
	
	一個類別，應該只有一個引起它變化的原因

2. 開放、封閉原則 (OCP：Open Closed Principle)
	
	對於擴展是開放的 (open for extension)
	對於修改是封閉的 (closed for modification)

3. 里氏(Liskov)代換原則 (LSP：Liskov Substitution Principle)
	
	子類別必須能替換父類別。

4. 介面隔離原則 (ISP：Interface Segregation Principle)

5. 依賴倒轉原則 (DIP：Dependency Inversion Principle)
	
	抽象不應該依賴細節，細節應該依賴抽像。因為抽像相對較穩定。
	高層模組不應該依賴低層模組，兩個都應該依賴抽像。
	針對接口編寫程式，不要對具體實現的東西編寫程式。

6. 迪米特法則 (LoD：Law of Demeter)
	
	最少知識原則 Principle of Least Knowledge
	只和自己眼前的朋友交談 Only talk to your immediate friends
	低耦合

	[例如]
	郵差送來掛號信，須要蓋收件人印章。
	一般人不會叫郵差自己進屋找印章，既浪費時間也不安全。
	正常都是自己進屋拿，或是請其他家人幫忙拿。
	因為不應該給郵差進屋找東西的權限、郵差也不須要知道印章放在屋內何處。

7. 合成/聚合重覆使用原則 (CARP)(Composite/Aggregate Reuse Principle)
	
	多用合成/聚合，少用繼承。
	在兩個物件有 has-a (has-parts、is-part-of)關係時 => 合成/聚合 (A has a B)
	當兩個物件有 is-a (is-a-kind-of)關係時 => 繼承 (Superman is a kind of Person)
	合成 (Composite)：A、B兩物件有合成關係時，表示其中一個物件消失(ex:書本)，另一個物件也會消失(ex:章節)。
	聚合 (Aggregate)：A、B兩物件有聚合關係時，表示其中一個物件消失(ex:球隊)，另一個物件不會消失(ex:球員)。
	
## 列表

|  縮寫   | 英文全名   | 中譯  |
|  ----  | ----  | ----  |
| SRP  | Single Responsibility Principle | 單一職責原則  |
| OCP  | Open Closed Principle | 開放、封閉原則  |
| LSP  | Liskov Substitution Principle | 里氏(Liskov)代換原則 |
| ISP  | Interface Segregation Principle | 介面隔離原則  |
| DIP  | Dependency Inversion Principle | 依賴倒轉原則  |
| LoD  | Law of Demeter | 迪米特法則  |
| CARP  | Composite/Aggregate Reuse Principle | 合成/聚合重覆使用原則  |


# Reference

* https://www.tenlong.com.tw/products/9789867794529
* https://xyz.cinc.biz/2013/08/blog-post_16.html

