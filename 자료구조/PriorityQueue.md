- 각 요소가 우선순위와 함께 저장되는 데이터 구조
- 높은 우선순위를 가진 요소가 낮은 우선순위를 가진 요소보다 먼저 처리된다
- 우선순위 큐는 배열, 힙 또는 다른 데이터 구조를 사용하여 구현 가능
- 이진 힙을 사용

```js
class PriorityQueue<T> {
    private heap: T[] = [];
    private comparator: (a: T, b: T) => number;

    constructor(comparator: (a: T, b: T) => number) {
        this.comparator = comparator;
    }

    private getLeftChildIndex(parentIndex: number): number {
        return 2 * parentIndex + 1;
    }

    private getRightChildIndex(parentIndex: number): number {
        return 2 * parentIndex + 2;
    }

    private getParentIndex(childIndex: number): number {
        return Math.floor((childIndex - 1) / 2);
    }

    private swap(indexOne: number, indexTwo: number) {
        [this.heap[indexOne], this.heap[indexTwo]] = [this.heap[indexTwo], this.heap[indexOne]];
    }

    enqueue(item: T) {
        this.heap.push(item);
        let currentIndex = this.heap.length - 1;
        let parentIndex = this.getParentIndex(currentIndex);

        while (currentIndex > 0 && this.comparator(this.heap[currentIndex], this.heap[parentIndex]) < 0) {
            this.swap(currentIndex, parentIndex);
            currentIndex = parentIndex;
            parentIndex = this.getParentIndex(currentIndex);
        }
    }

    dequeue(): T | undefined {
        const item = this.heap.shift();
        if (this.heap.length > 0) {
            this.heap.unshift(this.heap.pop()!);
            let currentIndex = 0;

            while (this.getLeftChildIndex(currentIndex) < this.heap.length) {
                let smallerChildIndex = this.getLeftChildIndex(currentIndex);
                if (this.getRightChildIndex(currentIndex) < this.heap.length &&
                    this.comparator(this.heap[this.getRightChildIndex(currentIndex)], this.heap[smallerChildIndex]) < 0) {
                    smallerChildIndex = this.getRightChildIndex(currentIndex);
                }

                if (this.comparator(this.heap[smallerChildIndex], this.heap[currentIndex]) < 0) {
                    this.swap(smallerChildIndex, currentIndex);
                    currentIndex = smallerChildIndex;
                } else {
                    break;
                }
            }
        }
        return item;
    }

    isEmpty(): boolean {
        return this.heap.length === 0;
    }

    peek(): T | undefined {
        return this.heap[0];
    }
}

// 사용 예제
const priorityQueue = new PriorityQueue<number>((a, b) => a - b);
priorityQueue.enqueue(5);
priorityQueue.enqueue(3);
priorityQueue.enqueue(10);

console.log(priorityQueue.dequeue()); // 3
console.log(priorityQueue.peek());    // 5

```