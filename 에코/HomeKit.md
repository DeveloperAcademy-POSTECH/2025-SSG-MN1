>[!question]
>GQ1. HomeKit은 무엇인가.
>GQ2. HomeKit의 특징은 무엇인가.
>GQ3. HomeKit의 사용 방법은 무엇인가?


## HomeKit: 안전하고 통합된 스마트홈 프레임워크

**HomeKit**은 Apple이 개발한 스마트홈 플랫폼으로, 사용자가 iPhone, iPad, Mac, Apple Watch 등 Apple 기기 및 Siri를 통해 여러 제조사의 스마트홈 액세서리(조명, 잠금장치, 온도 조절기, 카메라 등)를 **하나의 통합된 환경에서 안전하게 제어하고 관리**할 수 있도록 하는 소프트웨어 프레임워크입니다.

Apple의 공식 개발자 문서에 따르면, HomeKit은 특정 앱이나 제품이 아닌, 다양한 스마트홈 기기들이 서로 원활하게 통신하고 협력하도록 만드는 기반 기술입니다. 핵심 목표는 복잡한 스마트홈 환경을 사용자 중심으로 단순화하고, 강력한 보안과 개인정보 보호를 보장하는 것입니다.

### HomeKit의 주요 특징

공식 문서에서 강조하는 HomeKit의 핵심적인 특징은 다음과 같습니다.

- **통합된 제어:** 사용자는 Apple의 기본 앱인 **'홈(Home)' 앱** 하나로 여러 브랜드의 모든 HomeKit 지원 기기를 추가하고 관리할 수 있습니다. 각기 다른 제조사의 앱을 일일이 실행할 필요 없이, '홈' 앱에서 모든 기기의 상태를 확인하고 제어할 수 있습니다.
    
- **Siri 음성 제어:** "Siri야, 나 집에 왔어" 또는 "Siri야, 안방 불 꺼줘"와 같은 간단한 음성 명령으로 등록된 기기들을 제어할 수 있습니다. 여러 동작을 묶어 '장면(Scene)'으로 설정하면, "잘 시간이야" 한마디로 모든 조명을 끄고, 문을 잠그고, 온도를 조절하는 등의 복합적인 작업을 수행할 수 있습니다.
    
- **강력한 보안 및 개인정보 보호:** HomeKit의 가장 큰 특징 중 하나는 보안입니다. 모든 통신은 종단간 암호화(end-to-end encryption)를 통해 보호됩니다. 이는 Apple을 포함한 그 누구도 사용자의 집에서 오가는 데이터를 들여다볼 수 없음을 의미합니다. 특히 **'HomeKit 보안 비디오(HomeKit Secure Video)'** 기능은 보안 카메라의 영상을 사용자의 iCloud에 암호화하여 저장하기 전에, HomePod이나 Apple TV 같은 홈 허브에서 먼저 분석하여 개인정보를 최대한 보호합니다.
    
- **자동화:** 사용자의 위치, 시간, 특정 센서의 감지 등 다양한 조건에 따라 기기들이 자동으로 작동하도록 설정할 수 있습니다. 예를 들어, 사용자가 집을 떠나면 자동으로 모든 조명이 꺼지고 문이 잠기도록 설정하거나, 매일 아침 7시에 창문 블라인드가 열리도록 자동화할 수 있습니다.
    
- **홈 허브(Home Hub):** HomePod, HomePod mini 또는 Apple TV를 홈 허브로 설정하면, 사용자가 집 밖에 있을 때도 원격으로 액세서리를 제어하거나 자동화 기능을 실행할 수 있습니다. 홈 허브는 집 안에서 HomeKit 액세서리들과 안전하게 통신하는 중심점 역할을 합니다.
    

### 개발자와 제조사를 위한 생태계

- **HomeKit 프레임워크:** 앱 개발자들은 HomeKit 프레임워크를 사용하여 HomeKit과 연동되는 앱을 만들 수 있습니다. 이를 통해 사용자는 기기를 설정하고, 제어하며, 커스텀 기능을 제공받을 수 있습니다.
    
- **MFi 프로그램:** HomeKit 액세서리를 상업적으로 제조하고 판매하려는 회사는 Apple의 **MFi(Made for iPhone/iPad/etc.) 인증 프로그램**에 참여해야 합니다. 이는 해당 제품이 Apple의 성능 및 보안 기준을 충족함을 보장합니다.
    
![[스크린샷 2025-07-15 오전 7.32.30.png]]
![[스크린샷 2025-07-15 오전 7.33.16.png]]

```swift
import HomeKit

// HomeKit의 핵심 기능을 관리하는 컨트롤러
class HomeKitController: NSObject, HMHomeManagerDelegate {

    // 1. HomeKit 설정 및 권한 관리
    private let homeManager = HMHomeManager()

    override init() {
        super.init()
        // HomeKit 데이터 변경을 감지하기 위해 delegate를 self로 설정합니다.
        homeManager.delegate = self
    }

    // HomeKit 데이터 로딩이 완료되면 이 함수가 호출됩니다.
    func homeManagerDidUpdateHomes(_ manager: HMHomeManager) {
        print("🏠 HomeKit 데이터 준비 완료!")
        // 예시: 데이터가 준비되면 자동으로 조명 상태를 확인합니다.
        checkLightStatus()
    }

    // --- 헬퍼 함수: 특정 장비와 기능(특성) 찾기 ---

    /// "거실 조명"을 찾아주는 내부 함수
    private func findLivingRoomLight() -> HMAccessory? {
        // 2. 기본 집(Primary Home) 접근
        guard let primaryHome = homeManager.primaryHome else {
            print("오류: 기본 집을 찾을 수 없습니다.")
            return nil
        }
        
        // 3. 장비(Accessory) 검색
        guard let lightAccessory = primaryHome.accessories.first(where: { $0.name == "거실 조명" }) else {
            print("오류: '거실 조명'을 찾을 수 없습니다.")
            return nil
        }
        
        // 장비가 현재 연결 가능한지 확인합니다.
        guard lightAccessory.isReachable else {
            print("오류: '거실 조명'에 연결할 수 없습니다.")
            return nil
        }
        
        return lightAccessory
    }

    /// 조명의 전원 기능(특성)을 찾아주는 내부 함수
    private func findPowerCharacteristic() -> HMCharacteristic? {
        guard let light = findLivingRoomLight() else { return nil }

        // 4. 장비 제어 (특성 찾기)
        // 조명 서비스(Lightbulb)에서 전원 상태(PowerState) 특성을 찾습니다.
        return light.services
            .first(where: { $0.serviceType == HMServiceTypeLightbulb })?
            .characteristics.first(where: { $0.characteristicType == HMCharacteristicTypePowerState })
    }
    
    // --- 실제 사용 함수 ---

    /// 조명의 현재 전원 상태를 확인하는 함수
    func checkLightStatus() {
        guard let powerState = findPowerCharacteristic() else { return }
        
        // 특성 값 읽기(Read)
        powerState.readValue { error in
            if let error = error {
                print("상태 읽기 오류: \(error.localizedDescription)")
                return
            }
            
            // 읽어온 값(Bool)을 확인합니다.
            if let isOn = powerState.value as? Bool {
                print("💡 현재 조명 상태: \(isOn ? "켜짐" : "꺼짐")")
            }
        }
    }

    /// 조명 전원을 켜는 함수
    func turnOnLight() {
        guard let powerState = findPowerCharacteristic() else { return }
        
        // 특성 값 쓰기(Write)
        powerState.writeValue(true) { error in
            if let error = error {
                print("조명 켜기 오류: \(error.localizedDescription)")
            } else {
                print("✅ 조명을 켰습니다.")
            }
        }
    }

    /// 조명 전원을 끄는 함수
    func turnOffLight() {
        guard let powerState = findPowerCharacteristic() else { return }
        
        // 특성 값 쓰기(Write)
        powerState.writeValue(false) { error in
            if let error = error {
                print("조명 끄기 오류: \(error.localizedDescription)")
            } else {
                print("✅ 조명을 껐습니다.")
            }
        }
    }
}
```

## References
- Apple의 공식 문서 & ChatGPT