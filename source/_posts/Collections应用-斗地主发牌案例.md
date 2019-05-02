title: Collections应用--斗地主发牌案例
author: 学亮
tags:
  - Collections
categories: []
date: 2019-04-29 17:27:00
---
> 具体规则：
1. 组装54张扑克牌
2. 将54张牌顺序打乱
3. 三个玩家参与游戏，三人交替摸牌，每人17张牌，最后三张留作底牌。
4. 查看三人各自手中的牌、底牌








<!-- more -->

````
import java.util.ArrayList;
import java.util.Collections;

/*
 * 模拟斗地主发牌 
 	买牌
 	洗牌
 	发牌
 */
public class CollectionsTest {
	public static void main(String[] args) {
		//买牌
		String[] arr = {"黑桃","红桃","方片","梅花"};
		String[] arr2 = {"A","2","3","4","5","6","7","8","9","10","J","Q","K"};
		
		ArrayList<String> box = new ArrayList<String>();
		//添加每张牌
		for (int i = 0; i < arr.length; i++) {
			//获取每一个花色
			for (int j = 0; j < arr2.length; j++) {
				//获取每一个数
				box.add(arr[i] + arr2[j]);
			}
			
		}
		box.add("大王");
		box.add("小王");
		//System.out.println(box.size());
		
	 	//洗牌
		Collections.shuffle(box);
		//System.out.println(box);
		
	 	//发牌
		ArrayList<String> 林志玲 = new ArrayList<String>();
		ArrayList<String> 林心如 = new ArrayList<String>();
		ArrayList<String> 舒淇 = new ArrayList<String>();
		
		//留三张底牌给地主
		for (int i = 0; i < box.size() - 3; i++) {
			/*
			 *  i = 0;i % 3 = 0;
			 *  i = 1;i % 3 = 1;
			 *  i = 2;i % 3 = 2;
			 *  i = 3;i % 3 = 0;
			 *  i = 4;i % 4 = 1;
			 *  i = 5;i % 5 = 2;
			 */
			
			if(i % 3 == 0) {
				林志玲.add(box.get(i));
			}
			else if(i % 3 == 1) {
				林心如.add(box.get(i));
			}
			else if(i % 3 == 2) {
				舒淇.add(box.get(i));
			}
		}
		
		System.out.println("林志玲：" + 林志玲);
		System.out.println("林心如：" + 林心如);
		System.out.println("舒淇：" + 舒淇);
	 
	
		System.out.println("底牌：");
	/*	System.out.println(box.get(box.size() - 1));
		System.out.println(box.get(box.size() - 2));
		System.out.println(box.get(box.size() - 3));*/
		
		for (int i = box.size() - 3; i < box.size(); i++) {
			System.out.println(box.get(i));
		}
	}
	
}

````