# 1장. 사용자 수에 따른 규모 확장성

## (9) 메시지 큐

> 메시지의 무손실(durability, 즉 메시지 큐에 일단 보관된 메시지는 소비자가 꺼낼 때까지 안전히 보관된다는 특성)을 보장하는, 비동기 통신(asynchronous communication)을 지원하는 컴포넌트다.

![1-12.mq.JPG](../images/chapter1/1-12.mq.JPG)

- 생산자 또는 발행자(producer/publisher)라고 불리는 입력 서비스가 메시지를 생성
- 메시지 큐에 발행(publish)한다.
- 큐에는 보통 소비자 혹은 구독자(consumer/subscriber)라 불리는 서비스 혹은 서버가 연걸되어있음
- 메시지를 받아 그에 맞는 동작을 수행하는 역할을 한다.