---
layout: post
title: "5. 실전 정렬"
date: 2025-02-24
---

<div style="text-align: center;">
	<img src="/사진들/알고리즘/실전정렬.png" alt="alt text" />
</div>

  
> 알고리즘 문제를 풀다보면 Map(딕셔너리)자료형이나 객체 배열들을 정렬해서 출력하도록 하는 경우가 많습니다. 단순 배열이나 리스트는 Java라면 "Arrays.sort()", 파이썬이라면 "sort()"나 "sorted()"로 쉽게 풀리지만 Map(딕셔너리)나 객체 배열들은 특정 변수에 접근해야는 로직을 거쳐야하므로 난감할 때가 많았습니다. 그러므로 이번 기회를 통해 제대로 정리해보려고 합니다.

# Map(딕셔너리) 정렬

## Java

자바에서는 TreeMap을 활용하면 키값을 기준으로 자동으로 오름차순으로 정렬해줍니다.
내림차순으로 하고 싶다면 TreeMap 인스턴스 생성할 때, 인자로 Comparator.reverseOrder()를 넣어 주면 됩니다.

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        // TreeMap 생성 (자연 순서로 키가 정렬됨)
        Map<String, Integer> map = new TreeMap<>();
                                 //new TreeMap<>(Comparator.reverseOrder());
                                 // -> 내림차순
        map.put("apple", 5);
        map.put("banana", 2);
        map.put("cherry", 7);

        // 출력 (키 기준으로 자동 정렬)
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " " + entry.getValue());
        }
    }
}

/*

apple 5
banana 2
cherry 7

*/

```

## Python

파이썬에서는 sort()나 sorted() 함수의 첫번째 인자에는 정렬하고자하는 딕셔너리 변수.items()로 튜플의 리스트를 만들어주고, 2번째 인자에는 key=lambda "인자" : 기준값 형식으로정렬 할 수 있습니다. 내림차순을 하고 싶다면 "reverse=True"도 함께 지정해주면 됩니다.

```python
my_dict = {'apple': 5, 'banana': 2, 'cherry': 7} 
sorted_items = sorted(my_dict.items(), key=lambda x: x[0]) #x[1]이면 벨류값 기준
                                        #reverse=True 해주면 내림차순
print(sorted_items)

"""
[('apple', 5), ('banana', 2), ('cherry', 7)]
"""
```

---

# 객체 배열 정렬

## Java 

자바에서 객체 배열의 멤버 변수를 기준으로 정렬하려면 Arrays.sort()의 첫번째  인자에는 정렬하고자하는 배열, 2번째 인자에는 Compator 인터페이스를 구현한 익명객체를 사용해야합니다.

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;

class Person {
	private String name;
	private int height;
	private double weight;

	public Person(String name, int height, double weight) {
		this.name = name;
		this.height = height;
		this.weight = weight;
	}

	public double getWeight() {
		return weight;
	}

	public String getName() {
		return name;
	}

	public void print() {
		System.out.println(name + " " + height + " " + weight);
	}

 public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		Person[] person = new Person[5];

		for (int i = 0; i < person.length; i++) {
			String name = sc.next();
			int height = sc.nextInt();
			double weight = sc.nextDouble();

			person[i] = new Person(name, height, weight);
		}
		sc.close();

		for (int i = 0; i < person.length; i++) {
			person[i].print();
		}

		System.out.println("name");
		Arrays.sort(person, new Comparator<Person>() {
			@Override
			public int compare(Person o1, Person o2) {
				// TODO Auto-generated method stub
				                //오름차순
				return o1.getName().compareTo(o2.getName());
			}
		});
		for (int i = 0; i < person.length; i++) {
			person[i].print();
		}

		System.out.println("weight");
		Arrays.sort(person, new Comparator<Person>() {
			@Override
			public int compare(Person o1, Person o2) {
				// TODO Auto-generated method stub
				             //내림차순
				return (int) (o2.getWeight() - o1.getWeight());
			}
		});
		for (int i = 0; i < person.length; i++) {
			person[i].print();
		}
	}
}

}

/*
name
Jung 160 41.2
Kim 155 28.9
Lee 150 35.6
Park 165 38.7
Sin 148 32.7

weight 
Jung 160 41.2 
Park 165 38.7 
Lee 150 35.6 
Sin 148 32.7 
Kim 155 28.9
*/
```

## Python

파이썬에서도 딕셔너리를 정렬할 때와 마찬가지로 sort()나 sorted()함수에 인자로 key= lambda "인자" : 기준값 으로 정렬 할 수 있습니다.

```python
# Person 클래스
class Person:
    def __init__(self, name, height, weight):
        self.name = name
        self.height = height
        self.weight = weight

    def get_name(self):
        return self.name

# Person 객체 리스트
person = [
    Person("John", 180, 75),
    Person("Alice", 165, 60),
    Person("Bob", 175, 68)
]

# person.sort()의 key 매개변수로 람다식 사용
person.sort(key=lambda p: p.get_name())  # 여기서 p는 person 리스트의 각 요소
                          # reverse=True 하면 내림차순

# 정렬된 결과 출력
for p in person:
    print(p.get_name())

```