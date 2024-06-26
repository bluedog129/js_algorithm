# 풀이 방법
### 부르트포스
두 개의 배열 (participant와 completion)을 사용하여 2중 for문을 통해 동일한 이름을 찾아 participant 배열에서 제거하는 방식으로 문제를 해결할 수는 있습니다. 그러나 이 방법은 효율성 측면에서 문제가 있습니다. 이러한 접근 방식의 시간 복잡도는 O(n제곱)로, 각 참가자 이름을 완주자 배열에서 찾아야 하기 때문에 배열의 크기가 커지면 성능이 매우 떨어집니다. 특히 문제의 제한사항에서 최대 100,000명의 참가자를 언급하고 있으므로, 이 방법은 비효율적일 수 있습니다.

문제점
성능 저하: participant 배열의 각 요소를 completion 배열의 모든 요소와 비교해야 하기 때문에, 최악의 경우 100,000 x 99,999번의 비교가 발생할 수 있습니다.
배열 수정: 배열을 순회하면서 요소를 제거하는 것은 일반적으로 배열의 크기가 조정될 때마다 추가적인 비용이 발생하므로 비효율적입니다.

### 해시 테이블
participant 배열의 각 이름을 키로, 이름의 등장 횟수를 값으로 하는 해시 테이블을 만듭니다. 그 다음 completion 배열을 순회하면서 해시 테이블에서 해당 이름의 카운트를 감소시킵니다. 마지막에 카운트가 0이 아닌 이름이 완주하지 못한 선수의 이름입니다. 이 방법은 시간 복잡도 O(n)으로 훨씬 효율적입니다.

### 정렬 후 비교
두 배열을 정렬한 후, 순서대로 비교하여 처음으로 다른 요소가 나타난 위치의 participant 배열 요소가 완주하지 못한 선수입니다. 이 방법의 시간 복잡도는 정렬에 의해 O(nlogn)입니다.

따라서 가장 효율적인 방법인 해시테이블을 채택!

# 시간 복잡도
reduce를 이용한 해시 테이블 생성:
participant 배열의 각 요소에 대해 한 번씩만 접근하여, participantObject 해시 테이블을 생성합니다. 각 요소를 처리하는 시간 복잡도는
O(1)이며, 전체 배열을 처리하는 시간은 O(n)입니다.

forEach를 이용한 해시 테이블 카운트 감소:
completion 배열도 한 번 순회하며, 해시 테이블에서 해당 이름의 카운트를 감소시킵니다. 배열의 길이는 participant 배열의 길이보다 1 작으므로, 이 과정의 시간 복잡도도 O(n)입니다.

for...in을 이용한 완주하지 못한 선수 찾기:
participantObject 해시 테이블을 순회하여 카운트가 여전히 1 이상인 첫 번째 이름을 찾습니다. 최악의 경우, 모든 키를 검사해야 하지만 이 키의 수는 최대 participant 배열의 길이와 동일하므로 이 부분의 시간 복잡도도 O(n)입니다.

따라서, 각 단계는 모두 O(n)의 시간 복잡도를 가지며, 이를 합쳐도 전체 시간 복잡도는 O(n)으로 계산됩니다. 이는 매우 효율적인 알고리즘 구조로, 대량의 데이터를 처리하는 데 적합합니다.

# 알게된 지식
### forEach 메서드의 특징
비 반환성: forEach는 map이나 filter와 달리 배열을 변형시키지 않고 반환값도 없습니다. <br>
순회 기능: 배열의 모든 요소를 한 번씩 순차적으로 접근하여 주어진 작업을 수행합니다. <br>
사용 용이성: 간결하고 직관적인 문법을 제공하여 복잡하지 않은 배열 순회 작업에 적합합니다. <br>

```
// 완주한 선수들로 해시 테이블의 카운트 감소
completion.forEach(name => {
    if (participantObject[name]) {
        participantObject[name] -= 1;
    }
});
```
completion.forEach 호출: 이 구문은 completion 배열의 모든 요소에 대해 주어진 콜백 함수를 실행합니다. 배열의 각 요소 name은 완주한 선수의 이름을 나타냅니다.

콜백 함수: name 매개변수를 받는 화살표 함수((name => { ... }))가 forEach의 인자로 사용됩니다. 이 함수는 completion 배열의 각 원소에 대해 호출됩니다.

조건문 if (participantObject[name]): 이 조건문은 participantObject에서 해당 name을 키로 하는 값을 평가합니다. 만약 name 키에 해당하는 값이 존재하고, 그 값이 0이 아니면 (undefined나 0인 경우 false로 평가됩니다), 조건문 내의 코드가 실행됩니다.

값 감소 participantObject[name] -= 1: 조건문이 참인 경우, participantObject의 해당 이름의 값(카운트)을 1 감소시킵니다. 이는 해당 참가자가 완주했음을 나타내며, 중복 이름을 가진 참가자의 경우를 정확히 처리하기 위해 사용됩니다.