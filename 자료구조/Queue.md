- FIFO(first in first out) 선입선출
- 주요 연산
	- enqueue: 큐의 끝에 새로운 요소 추가
	- dequeue: 큐의 시작 부분에서 요소를 제거하고 반환
	- peek: 큐의 시작 부분에 있는 요소를 반환하지만 제거하지 않음
	- isEmpty: 비어 있는지 확인
	- Size: 큐의 저장된 요소의 수 반환
- 사용 예시
	- 실시간 처리 시스템 -> 요청을 순차적으로 처리하는데 큐를 사용
	- 작업 스케줄링: 운영체제에서 프로세스 관리에 큐를 사용하여, 어떤 프로세스가 다음에 CPU 시간을 받을지 결정

```js
class Queue<T> {
    private elements: T[] = [];

    enqueue(element: T) {
        this.elements.push(element);
    }

    dequeue(): T | undefined {
        return this.elements.shift();
    }

    peek(): T | undefined {
        return this.elements[0];
    }

    isEmpty(): boolean {
        return this.elements.length === 0;
    }

    size(): number {
        return this.elements.length;
    }
}

// 큐 사용 예제
const queue = new Queue<number>();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);

console.log(queue.dequeue()); // 1
console.log(queue.peek());    // 2
console.log(queue.isEmpty()); // false
console.log(queue.size());    // 2

```