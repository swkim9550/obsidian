- LIFO(Last-In-First-Out)
- 가장 마지막에 추가된 요소가 가장 먼저 제거 됨
- 활용 예시
	- 함수 호출 및 관리
	- 웹 브라우저의 뒤로 가기 기능: 웹 브라우저는 사용자가 방문한 페이지의 기록을 스택에 저장하여, 뒤로 가기 기능을 구현 함
	- 괄호 검사 : 프로그래밍에서 괄호의 짝이 맞는지 검사할 때 사용 열린 괄호를 만나면 스택에 push하고, 닫힌 괄호를 만나면 스택에 pop하여 짝이 맞는지 확인한다.
- 주요 연산
	- Push : 스택에 새 요소 추가
	- Pop: 스택에서 가장 최근에 추가된 요소를 제거하고 반환
	- Peek/Top: 최상단 요소를 반환하지만 제거하지 않음
	- IsEmpty
	- Size


//큐 두개를 사용하여 스택 만들기
```js
class Stack<T> {
    private primaryQueue = new Queue<T>();
    private secondaryQueue = new Queue<T>();

    push(element: T) {
        this.primaryQueue.enqueue(element);
    }

    pop(): T | undefined {
        while (this.primaryQueue.size() > 1) {
            this.secondaryQueue.enqueue(this.primaryQueue.dequeue()!);
        }

        const poppedElement = this.primaryQueue.dequeue();

        // Swap the roles of primaryQueue and secondaryQueue
        [this.primaryQueue, this.secondaryQueue] = [this.secondaryQueue, this.primaryQueue];

        return poppedElement;
    }

    isEmpty(): boolean {
        return this.primaryQueue.isEmpty();
    }

    size(): number {
        return this.primaryQueue.size();
    }

    peek(): T | undefined {
        while (this.primaryQueue.size() > 1) {
            this.secondaryQueue.enqueue(this.primaryQueue.dequeue()!);
        }

        const topElement = this.primaryQueue.peek();

        while (!this.secondaryQueue.isEmpty()) {
            this.primaryQueue.enqueue(this.secondaryQueue.dequeue()!);
        }

        return topElement;
    }
}

```

//일반 스택 구현 
```js
class Stack<T> {
    private elements: T[] = [];

    push(element: T) {
        this.elements.push(element);
    }

    pop(): T | undefined {
        return this.elements.pop();
    }

    peek(): T | undefined {
        return this.elements[this.elements.length - 1];
    }

    isEmpty(): boolean {
        return this.elements.length === 0;
    }

    size(): number {
        return this.elements.length;
    }
}

// 스택 사용 예제
const stack = new Stack<number>();
stack.push(1);
stack.push(2);
stack.push(3);

console.log(stack.pop()); // 3
console.log(stack.peek()); // 2
console.log(stack.isEmpty()); // false
console.log(stack.size()); // 2

```