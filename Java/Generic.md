# Generic
자바 제너릭은 클래스, 인터페이스, 메소드를 정의할 때 타입을 파라미터로 사용할 수 있게 해주는 기능.

제네릭 특징

- 타입 안정성 보장: 컴파일 시점에 타입 체크를 수행하여 런타임 오류를 방지합니다.
- 코드 재사용성 향상: 다양한 타입에 대해 동일한 코드를 사용할 수 있습니다.
- 타입 캐스팅 제거: 명시적인 타입 캐스팅이 필요 없어 코드가 간결해집니다.

일반적으로 우리가 쓰던 방식인 클래스, 인터페이스, 메서드 내에서 타입을 지정하는게 아닌, 외부에서 타입을 지정하는 방식이다.

→ 타입의 경계를 지정하고, 컴파일 때 해당 타입으로 캐스팅하여 매개변수화 된 유형을 삭제하는 것

---

## 제네릭 타입 소거

제네릭은 자바 1.5버전부터 도입된 문법이기 때문에 이전 자바버전과의 호환성을 위해 컴파일시 제네릭타입은 사라진다. (=클래스파일에는 제네릭 타입에 대한 정보x)

컴파일 타임에만 타입 제약 조건을 정의하고, 런타임시에는 타입을 제거하기 때문에 개발자가 잘못된 방향으로 제네릭을 사용한다면, 힙 오염에 빠지게 되는 위험성을 가지고 있다.

### Reifiable Type(실체화 타입) / Non-Refiable Type(비실체화 타입)

### 실체화 타입

컴파일 이후에도 타입 정보가 소거되지 않고 런타임에 유지되는 타입. 

⇒ 런타임에도 정확한 타입 정보를 알 수 있는 타입

1. primitive 타입 : 제네릭이 아니므로 항상 실체화
2. Number, Integer, String 등 클래스, 인터페이스 타입
3. Raw 타입 : 제네릭 파라미터를 지정하지 않은 타입 
4. 비한정 와일드카드 List<?> : 와일드카드는 소거되긴 하지만, ?는 컴파일때 Object로 변환되기 때문에, 실체화 타입이라고 볼 수 있다.

### 비실체화 타입

컴파일시 소거되는 타입

1. 제네릭 클래스 / 인터페이스 : List<String> 같은 구체적인 제네릭 타입은 런타임시 List로만 인식됨
2. List<? extends Number>같은 한정 와일드카드 : 위와 같이 Raw타입인 List로 인식됨

```java
List<Integer> list = new ArrayList<>();

List list = new ArrayList();
```

## 소거 과정

- bounded type( <?>, <T>) 는 Object로 변환된다.
    
    
    ```java
    public <T> List<T> getListOf(T t1, T t2) {
      ...
    }
    ```
    
    ```java
    public List getListOf(Object t1, Object t2) {
      ...
    }
    ```
    
- unbounded type(<T extends Comparable> 등)은 Comparable로 변환됨.
    
    
    ```java
    public <T> List<T extends Comparable> getListOf(T t1, T t2) {
      ...
    }
    ```
    
    ```java
    public List getListOf(Comparable t1, Comparable t2) {
      ...
    }
    ```
    
- 타입 소거 후 타입이 일치하지 않으면 타입캐스팅을 한다.
    
    
    ```java
    class Box<T> {
        List list = new ArrayList(); // Object
    
        void add(Number item) {
            list.add(item);
        }
    
        Number getValue(int i) {
            return list.get(i); // 형변환 불일치 (Number != Object)
        }
    }
    ```
    
    ```java
    class Box {
        List list = new ArrayList(); // Object
    
        void add(Number item) {
            list.add(item);
        }
    
        Number getValue(int i) {
            return (Number) list.get(i); // 캐스팅 연산자 추가
        }
    }
    ```
    
    ```java
    /* 치환 전 */
    class Box {
        List list = new ArrayList(); // Object
    
        void add(Number item) {
            list.add(item);
        }
    
        Number getValue(int i) {
            return list.get(i); // 형변환 불일치 (Number != Object)
        }
    }
    ```
    
    ```java
    /* 치환 후 */
    class Box {
        List list = new ArrayList(); // Object
    
        void add(Number item) {
            list.add(item);
        }
    
        Number getValue(int i) {
            return (Number) list.get(i); // 캐스팅 연산자 추가
        }
    }
    ```
    

---

## 2. 제너릭 사용 방법

![image](https://github.com/user-attachments/assets/d6a84616-87fa-40a3-895d-d8625ec30666)


타입은 꼭 한글자일 필요는 없고, 설명과 대응되지 않아도 된다. 암묵적인 규칙을 따르는게 편함.

또한, 타입 파라미터로 명시할 수 있는 것은 참조 타입밖에 없다. primitive type은 올 수 없다.

## 2. 제너릭 클래스

타입 파라미터를 가지는 클래스

```java
public class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }
}
```

사용 방법

``` java
Box<Integer> intBox = new Box<>();
intBox.set(10);
int value = intBox.get();
```

## 3. 제너릭 메서드

자체적으로 타입 파라미터를 가지는 메서드

``` java
public static <E> void printArray(E[] array) {
    for (E element : array) {
        System.out.print(element + " ");
    }
    System.out.println();
}
```

## 4. 와일드카드

와일드카드(?)는 알 수 없는 타입을 나타낸다.

- 상한 경계 와일드카드: `<? extends T>`
- 하한 경계 와일드카드: `<? super T>`
- 무제한 와일드카드: `<?>`

```java
void test(List<? extends Number> list) {
	list.add(1); // 컴파일 에러
}

List<? extends Number> list = new ArrayList<>();

void abc() {
    list.add(1); // 컴파일 에러
}
```

### 상한 경계 와일드카드

```java
class MyGrandParent {

}

class MyParent extends MyGrandParent {

}

class MyChild extends MyParent {

}
```

- produce 상황

```java
void printCollection(Collection<? extends MyParent> c) {
    // 컴파일 에러
    for (MyChild e : List<MyParent> list) {
        System.out.println(e);
    }

    for (MyParent e : c) {
        System.out.println(e);
    }

    for (MyGrandParent e : c) {
        System.out.println(e);
    }

    for (Object e : c) {
        System.out.println(e);
    }
}
```

c의 원소 e는 MyChild일 수 있지만, 아닐 수도 있다.

만약 c가 `List<MyParent>`인 경우, `MyParent`를 `MyChild`로 다운캐스팅이 불가능하기 때문에 컴파일 에러가 난다.

### 하한경계 와일드카드

consume 상황

```java
void addElement(Collection<? super MyParent> c) {
    c.add(new MyChild());
    c.add(new MyParent());
    c.add(new MyGrandParent());  // 불가능(컴파일 에러)
    c.add(new Object());         // 불가능(컴파일 에러)
}
```

produce 상황

```java
void printCollection(Collection<? super MyParent> c) {
    // 불가능(컴파일 에러)
    for (MyChild e : c) {
        System.out.println(e);
    }

    // 불가능(컴파일 에러)
    for (MyParent e : c) {
        System.out.println(e);
    }

    // 불가능(컴파일 에러)
    for (MyGrandParent e : c) {
        System.out.println(e);
    }

    for (Object e : c) {
        System.out.println(e);
    }
}
```

**PECS(Producer-Extends, Consumer-Super) 공식**

### 주의사항

```java
class Sample<T> {
    public void someMethod() {
        // 제네릭 타입 자체로 타입을 지정하여 객체를 생성하는 것은 불가능
        T t = new T();
    }
}
```

```java
class Sample<T> {
    public static T someMethod() {

    }
}
```
