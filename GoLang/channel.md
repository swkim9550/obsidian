고루틴(Goroutine)은 Go 언어에서 병행성을 구현하기 위한 경량 스레드로, 동시에 여러 작업을 실행하는 데 사용됩니다. 채널(Channel)은 고루틴 간 통신과 동기화를 위한 중요한 도구로, 데이터를 안전하게 전달하고 공유할 수 있게 해줍니다.

**채널(Channel)의 기본 동작:**

- 채널은 `make` 함수로 생성하며, 데이터를 보내고 받을 때 사용됩니다.
- 채널은 일반적으로 두 가지 종류가 있습니다: 버퍼가 있는 채널과 버퍼가 없는 채널.
- 버퍼가 없는 채널은 데이터를 보낼 때 다른 고루틴에서 받을 때까지 블록됩니다.
- 버퍼가 있는 채널은 데이터를 보낼 때 채널이 가득 차면 블록되며, 받을 때 채널이 비어 있으면 블록됩니다.



```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // 정수 값을 보내고 받을 수 있는 정수 채널 생성
    ch := make(chan int)

    // 고루틴에서 데이터를 보내는 함수
    go func() {
        for i := 1; i <= 5; i++ {
            fmt.Printf("보내는 중: %d\n", i)
            ch <- i // 채널에 데이터 전송
            time.Sleep(time.Second)
        }
        close(ch) // 채널 닫기
    }()

    // 메인 고루틴에서 데이터를 받는 함수
    for num := range ch { // 채널에서 데이터 수신
        fmt.Printf("받은 데이터: %d\n", num)
    }
}

```