# 문제 요약

이 문제는 카카오톡 선물하기 기능을 통해 다음 달에 친구들이 선물을 얼마나 받을지 예측하는 시나리오입니다.<br> 친구들의 선물 주고받은 기록과 특정 규칙을 기반으로 다음 달에 각 친구가 받을 선물의 수를 예측해야 합니다.

규칙은 다음과 같습니다:

두 친구 사이에 선물 교환 기록이 있을 경우, 더 많은 선물을 준 친구가 다음 달에 상대방에게 선물을 하나 받습니다.<br>
선물 교환 기록이 없거나 교환 횟수가 같을 경우, 선물 지수(이번 달까지 준 선물 수에서 받은 선물 수를 뺀 값)가 더 큰 친구가 다음 달에 선물을 하나 받습니다.<br>
두 친구의 선물 지수가 같을 경우, 다음 달에 서로 선물을 주고받지 않습니다.<br>
문제의 입력(인자)으로는 친구들의 이름을 담은 배열 friends와 이번 달까지 선물을 주고받은 기록을 나타내는 배열 gifts가 주어지며, 출력으로는 다음 달에 가장 많은 선물을 받는 친구가 받을 선물의 수를 반환해야 합니다.

# 코드 구조

변수 초기화:

len: 친구들의 수 (friends.length).</br>
nameMap: 친구들의 이름과 해당 인덱스를 매핑하는 Map 객체.</br>
giftTable: 친구들 간 선물 주고받은 횟수를 기록하는 2차원 배열.</br>
rankInfo: 각 친구의 선물 지수를 저장하는 배열.</br>
nextMonth: 다음 달에 각 친구가 받을 선물의 수를 기록하는 배열.</br>

친구 이름 매핑:
friends 배열을 순회하면서 각 친구의 이름과 인덱스를 nameMap에 저장합니다.</br>

선물 주고받은 내역 기록:
gifts 배열을 순회하면서 선물을 준 친구와 받은 친구의 이름을 분리하고, 해당하는 giftTable의 위치에 선물 횟수를 증가시킵니다.</br>

선물 지수 계산:
각 친구에 대해 준 선물의 총 수를 계산하고, 받은 선물의 수를 빼서 rankInfo에 저장합니다.</br>

다음 달 받을 선물 계산:</br>
모든 친구 쌍에 대해 두 친구 간의 선물 교환 기록을 비교합니다.</br>
한 친구가 다른 친구보다 더 많이 선물을 줬다면, 더 많이 준 친구가 다음 달에 선물을 하나 받습니다.</br>
선물을 동일하게 주고받았다면, 선물 지수가 더 큰 친구가 다음 달에 선물을 하나 받습니다.</br>

최대 선물 수 반환:
nextMonth 배열에서 최대값을 찾아 반환합니다. 이 값은 다음 달에 가장 많은 선물을 받는 친구가 받을 선물의 수를 나타냅니다.

# 알게된 지식

### new Map(); 객체 생성

Map 객체는 키-값 쌍을 저장하며, 키에 대해 반복 작업을 빠르게 수행할 수 있도록 최적화된 구조입니다. 이 코드에서는 각 친구의 이름을 키로 하고 해당 이름에 대응하는 배열 인덱스를 값으로 저장하는 데 Map을 사용합니다. Map을 사용하는 이유는 이름으로 빠르게 인덱스를 검색할 수 있기 때문입니다. </br>

```
const nameMap = new Map();
friends.forEach((name, idx) => {
    nameMap.set(name, idx);
});
```

### 콜백 함수

콜백 함수는 다른 함수에 인자로 넘겨주는 함수입니다. forEach 메소드에서 사용되며, 각 배열 요소에 대해 실행됩니다. 여기서는 friends 배열의 각 원소(친구의 이름)와 그 인덱스를 받아 nameMap에 저장하는 용도로 콜백 함수가 사용됩니다.

### .fill(0)

fill 메소드는 배열의 모든 요소를 정적인 값으로 채웁니다. 이 코드에서는 2차원 배열 giftTable을 생성할 때 각 요소를 0으로 초기화하기 위해 사용됩니다.

```
const giftTable = new Array(len).fill(0).map(_ => new Array(len).fill(0));
```

### .reduce()

reduce 메소드는 배열의 각 요소에 대해 리듀서 함수를 실행하고, 하나의 결과값을 반환합니다. 이 코드에서는 giftTable의 각 행에서 준 선물의 총 수를 계산할 때 사용됩니다.

```
rankInfo[i] = giftTable[i].reduce((acc, cur) => acc += cur, 0);
```

### Math.max(...nextMonth)

Math.max 함수는 주어진 숫자 중 가장 큰 값을 반환합니다. 스프레드 연산자 ...는 nextMonth 배열을 개별 인자로 확장하여 Math.max 함수에 전달합니다. 이를 통해 nextMonth 배열 내에서 가장 큰 값을 찾아 반환합니다.

이 코드는 선물 데이터를 분석하여 다음 달에 각 친구가 받을 선물의 수를 예측하고, 가장 많은 선물을 받게 될 친구의 선물 수를 찾아내는 과정을 자세하게 구현합니다.

### forEach, set 의 차이점

목적: </br>set은 Map에서 키와 값을 저장하거나 업데이트하는 데 사용됩니다. </br>forEach는 배열이나 다른 컬렉션의 각 요소에 대해 주어진 함수를 실행하는 데 사용됩니다. </br>

반환 값: </br>set은 Map 객체 자체를 반환합니다(메소드 체이닝 가능). </br>forEach는 아무 값도 반환하지 않습니다(undefined). </br>

사용 컨텍스트: <br>set은 Map 객체에 한정되어 사용됩니다. </br>forEach는 배열, Map, Set 등 다양한 컬렉션에서 사용될 수 있습니다. </br>

### 메소드 체이닝

메소드 체이닝(Method Chaining)은 객체 지향 프로그래밍에서 일련의 메소드 호출을 연쇄적으로 수행하는 프로그래밍 패턴입니다. 이 패턴의 핵심은 각 메소드가 실행 후에 해당 객체의 참조(reference)를 반환하여, 다음 메소드 호출이 즉시 그 객체에 대해 수행될 수 있도록 하는 것입니다.

메소드 체이닝의 구조
메소드 체이닝을 구현할 때 주의할 점은 각 메소드가 체이닝을 계속 이어나갈 수 있도록, 적절한 값을 반환해야 한다는 것입니다. 예를들어

```
class Car {
    constructor(model) {
        this.model = model;
        this.speed = 0;
    }

    setSpeed(speed) {
        this.speed = speed;
        return this;  // 객체 자신을 반환
    }

    applyBrake(decrement) {
        this.speed -= decrement;
        return this;  // 객체 자신을 반환
    }

    showSpeed() {
        console.log(this.speed);
        return this;  // 객체 자신을 반환
    }
}
myCar.setSpeed(100).applyBrake(20).showSpeed();
```

의 경우

setSpeed(100): 이 메소드는 자동차의 속도를 설정하고, 메소드 체이닝을 위해 this를 반환합니다. this는 myCar 객체 자신을 참조합니다.<br>
applyBrake(20): 이전 메소드에서 반환된 this (즉, myCar 객체)를 통해 이 메소드가 호출됩니다. 속도를 감속시키고, 다시 this를 반환하여 추가적인 메소드 호출이 가능하게 합니다.<br>
showSpeed(): 이전 메소드에서 반환된 this를 통해 호출되며, 현재 속도를 출력합니다. 이 메소드 역시 this를 반환하므로, 필요하다면 더 많은 메소드를 연속적으로 호출할 수 있습니다.<br>
