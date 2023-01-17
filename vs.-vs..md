# 블로킹 Vs. 논블로킹, 동기 Vs. 비동기

### 1. 시작하기에 앞서 <a href="#1" id="1"></a>

Block과 non-block, sync와 async 개념을 이해하기 위해서는 다음의 두 용어를 짚고 넘어가야 한다.

* **제어권**
  * 제어권은 자신(함수)의 코드를 실행할 권리 같은 것이다. 제어권을 가진 함수는 자신의 코드를 끝까지 실행한 후, 자신을 호출한 함수에게 돌려준다.
* **결과값을 기다린다는 것**
  * A 함수에서 B 함수를 호출했을 때, A 함수가 B 함수의 결과값을 기다리느냐의 여부를 의미한다.

### 2. Blocking(블로킹)과 Non-blocking(논블로킹) <a href="#2-blocking-non-blocking" id="2-blocking-non-blocking"></a>

블로킹과 논블로킹은 A 함수가 B 함수를 호출했을 때, **제어권을 어떻게 처리하느냐에 따라 달라진다.**

#### 1) 블로킹 <a href="#1" id="1"></a>

**블로킹**은 A 함수가 B 함수를 호출하면, **제어권을 A가 호출한 B 함수에 넘겨준다.**

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2F8cdc0a02-d469-47d5-96c8-f6aeef204eb7%2Fimage.png)

1. A함수가 B함수를 호출하면 B에게 제어권을 넘긴다.
2. 제어권을 넘겨받은 B는 열심히 함수를 실행한다. A는 B에게 제어권을 넘겨주었기 때문에 함수 실행을 잠시 멈춘다.
3. B함수는 실행이 끝나면 자신을 호출한 A에게 제어권을 돌려준다.

#### 2) 논블로킹 <a href="#2" id="2"></a>

**논블로킹**은 A함수가 B함수를 호출해도 **제어권은 그대로 자신이 가지고 있는다**.

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2Fc839fc04-1788-4063-ab38-b0d4a312dbf4%2Fimage.png)

1. A함수가 B함수를 호출하면, B 함수는 실행되지만, **제어권은 A 함수가 그대로 가지고 있는다.**
2. A함수는 계속 제어권을 가지고 있기 때문에 B함수를 호출한 이후에도 자신의 코드를 계속 실행한다.

### 3. Synchronous(동기)와 Asynchronous(비동기) <a href="#3-synchronous-asynchronous" id="3-synchronous-asynchronous"></a>

동기와 비동기의 차이는 **호출되는 함수의 작업 완료 여부를 신경쓰는지의 여부**의 차이이다.

#### 1) 동기 <a href="#1" id="1"></a>

함수 A가 함수 B를 호출한 뒤, **함수 B의 리턴값을 계속 확인하면서 신경쓰는 것**이 동기이다.

#### 2) 비동기 <a href="#2" id="2"></a>

함수 A가 함수 B를 호출할 때 **콜백 함수를 함께 전달**해서, 함수 B의 작업이 완료되면 함께 보낸 콜백 함수를 실행한다.

함수 A는 함수 B를 호출한 후로 **함수 B의 작업 완료 여부에는 신경쓰지 않는다.**

### 4. 블로킹과 논블로킹, 동기와 비동기 비교 <a href="#4" id="4"></a>

#### 1) Sync-Blocking <a href="#1-sync-blocking" id="1-sync-blocking"></a>

동기를 블로킹처럼 실행하는 것은 이해하기 쉽다.

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2Ff6212fee-ee42-4023-ae02-d2dc15eec46a%2Fimage.png)

함수 A는 함수 B의 리턴값을 필요로 한다(**동기**). 그래서 제어권을 함수 B에게 넘겨주고, 함수 B가 실행을 완료하여 리턴값과 제어권을 돌려줄때까지 기다린다(**블로킹**).

#### 2) Sync-Nonblocking <a href="#2-sync-nonblocking" id="2-sync-nonblocking"></a>

그런데, 동기를 논블로킹처럼 작동시킬 수 있다. 다음의 그림을 보자.

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2Ffe5d1231-4c3c-4caf-bdd8-2287926e38ca%2Fimage.png)

A 함수는 B 함수를 호출한다. 이 때 **A 함수는 B 함수에게 제어권을 주지 않고**, 자신의 코드를 계속 실행한다(**논블로킹**).

그런데 **A 함수는 B 함수의 리턴값이 필요하기 때문**에, 중간중간 B 함수에게 함수 실행을 완료했는지 물어본다(**동기**).

즉, 논블로킹인 동시에 동기인 것이다.

#### 3) Async-Nonblocking <a href="#3-async-nonblocking" id="3-async-nonblocking"></a>

비동기 논블로킹은 이해하기 쉽다. A 함수는 B 함수를 호출한다.

이 때 제어권을 B 함수에 주지 않고, 자신이 계속 가지고 있는다(**논블로킹**). 따라서 B 함수를 호출한 이후에도 멈추지 않고 자신의 코드를 계속 실행한다.

그리고 B 함수를 호출할 때 **콜백함수**를 함께 준다. B 함수는 자신의 작업이 끝나면 A 함수가 준 콜백 함수를 실행한다(**비동기**).

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2Fb9566928-9a6b-4111-9cad-528daa45475d%2Fimage.png)

#### 4) Async-blocking <a href="#4-async-blocking" id="4-async-blocking"></a>

Async-blocking의 경우는 사실 잘 마주하기 쉽지 않다.

A 함수는 B 함수의 리턴값에 신경쓰지 않고, 콜백함수를 보낸다(**비동기**).

그런데, B 함수의 작업에 관심없음에도 불구하고, A 함수는 B 함수에게 제어권을 넘긴다(**블로킹**).

따라서, A 함수는 자신과 관련 없는 B 함수의 작업이 끝날 때까지 기다려야 한다.

![](https://velog.velcdn.com/images%2Fnittre%2Fpost%2F9b6754f0-8721-4308-8a62-d884c7315d15%2Fimage.png)

Async-blocking의 경우 sync-blocking과 성능의 차이가 또이또이하기 때문에 사용하는 경우는 거의 없다.



### 출처

{% embed url="https://velog.io/@nittre/%EB%B8%94%EB%A1%9C%ED%82%B9-Vs.-%EB%85%BC%EB%B8%94%EB%A1%9C%ED%82%B9-%EB%8F%99%EA%B8%B0-Vs.-%EB%B9%84%EB%8F%99%EA%B8%B0" %}
