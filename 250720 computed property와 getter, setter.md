## 🏠 프로퍼티(property)란?

먼저, **"프로퍼티"**는 우리가 **사람이나 사물의 정보를 저장할 때 사용하는 값**이에요.
예를 들어, 사람이 있다면 이런 정보가 있겠죠?

| 이름(name) | 나이(age) | 휴가일수(vacation) |
| -------- | ------- | -------------- |
| 철수       | 10살     | 14일            |
이런 정보 하나하나가 **"프로퍼티"**에요.

---
## 🧺 저장 프로퍼티(stored property)

"저장 프로퍼티"는 그냥 어떤 값을 **직접 저장**해두는 거예요.
예를 들면, 철수에게 휴가 14일을 줬다고 할게요.

```swift
struct Person {
	var name: String
	var vacationDays: Int
}
```

철수는 14일 휴가가 있으니까 이렇게 저장해요:

```swift
var chulsoo = Person(name: "철수", vacationDays: 14)
```

근데 만약 철수가 5일을 썼다고 하면, 이렇게 빼줘야 해요:

```swift
chulsoo.vacationDays -= 5  // 14 - 5 = 9일 남음
```

**문제는 뭐냐면?**  
이렇게 하면 **원래 몇 일 있었는지** 기억하기가 힘들어요. 😟

---
## 🧮 계산 프로퍼티(computed property)

그래서 나온 게 바로 **계산 프로퍼티**예요!
계산 프로퍼티는 **직접 값을 저장하지 않고, 그때그때 계산해서 알려줘요.**

> 🍰 예를 들어볼게요!

철수는 휴가 14일을 받았고, 지금까지 5일을 썼어요.

**남은 휴가 = 받은 휴가 - 쓴 휴가**

이걸 매번 계산해서 알려주게 만들 수 있어요:

```swift
struct Person {
	var vacationAllocated = 14   // 받은 휴가
	var vacationTaken = 0        // 쓴 휴가
	var vacationRemaining: Int { 
		return vacationAllocated - vacationTaken
	}
}
```

이제 남은 휴가를 쓸 필요 없이 그냥 `vacationRemaining`을 **읽기만 하면 자동 계산**돼요!

```swift
var chulsoo = Person() 
chulsoo.vacationTaken = 5 
print(chulsoo.vacationRemaining) // 14 - 5 = 9일 남음
```

---

## 🧐 근데 왜 **읽기만 되고**, 바꾸려고 하면 안 될까?

계산 프로퍼티는 계산만 해주는 거라서 기본적으로는 **읽기 전용(getter)**이에요.

```swift
chulsoo.vacationRemaining = 5 // ❌ 에러! 값을 넣을 수 없음
```

왜냐하면...

> “남은 휴가를 5일로 바꾼다”면  
> 어떤 걸 바꿔야 할까요?

- 휴가 **쓴 거**를 바꿔야 할까요?    
- 받은 휴가 **전체 양**을 바꿔야 할까요?

**Swift는 혼란스러워요!** 그래서 우리가 직접 알려줘야 해요. 그게 바로 **setter**예요.

---
## ✍️ setter를 직접 만들면 어떻게 될까?

예를 들어, **남은 휴가를 5일로 바꾸면**,  
쓴 휴가를 기준으로 받은 휴가를 다시 계산해주도록 만들 수 있어요:

```swift
struct Person {
	var vacationAllocated = 14
	var vacationTaken = 0
	
	var vacationRemaining: Int {
	get { 
		return vacationAllocated - vacationTaken
	}
	set {
		vacationAllocated = vacationTaken + newValue
		}
	}
}
```

이제는 이렇게 쓸 수 있어요:

```swift
var chulsoo = Person()
chulsoo.vacationTaken = 4
chulsoo.vacationRemaining = 5
print(chulsoo.vacationAllocated) // 4 + 5 = 9일로 바뀜!
```

---
## 🧠 정리하면!

| 개념      | 뜻              | 예시                                  |
| ------- | -------------- | ----------------------------------- |
| 저장 프로퍼티 | 값을 그대로 저장하는 것  | `var age = 10`                      |
| 계산 프로퍼티 | 값을 계산해서 보여주는 것 | `vacationAllocated - vacationTaken` |
| getter  | 값을 읽을 때 실행     | `print(vacationRemaining)`          |
| setter  | 값을 넣을 때 실행     | `vacationRemaining = 5`             |

---
## 🎁 비유로 쉽게 설명해볼게요!

**🧮 저장 프로퍼티는 "노트에 써놓은 숫자"**  
**🧠 계산 프로퍼티는 "머릿속으로 계산해서 말해주는 숫자"**

예)
- “너 휴가 며칠 있어?”
    - 저장 프로퍼티: "노트 보니까 9일이야!"
    - 계산 프로퍼티: "14일 받았고, 5일 썼으니까... 9일 남았어!"