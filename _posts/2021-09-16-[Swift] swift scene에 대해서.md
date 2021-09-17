## SceneDelegate

## AppDelegate & SceneDelegate
* 그림 찾아서 넣어줄 것

AppDelegate
Process LifeCycle       UI LifeCycle
App Launched            Entered Foreground
App Terminate           Became Active
...                                 ...

->

* 그림 찾아서 넣어줄 것

AppDelegate                 SceneDelegate
Process LifeCycle           UI LifeCycle
                                        Entered Foreground
Scene LifeCycle             Became Active
Session Created              
Session Discarded               


iOS12까지는 하나의 앱은 하나의 window로 구성되어 있었다.
iOS13부터는 window의 개념이 scene으로 변경되었으며, 하나의 앱은 여러개의 scene을 가질수 있게 되었다.
다시 말해 하나의 앱에서 여러개의 scene을 보여줄수 있게 되었다.


## SceneDelegate
AppDelegate의 역할 중 UI의 상태를 알 수 있는 UILifeCycle에 대한 부분을 SceneDelegate가 하게 되었다.

## SceneDelegate의 생명주기
1. [SceneDelegate] scene(_:, willConnectTo:, options:)
```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
    // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
    // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
    guard let _ = (scene as? UIWindowScene) else { return }
}
```
scene이 앱에 추가될 때 호출. 단 여기서 ViewController와 같은 클래스 객체를 만들어 사용할 때, 아직 viewDidLoad()가 호출되지 않음

2. sceneDidDisconnect(_ :)
```swift
func sceneDidDisconnect(_ scene: UIScene) {
    // Called as the scene is being released by the system.
    // This occurs shortly after the scene enters the background, or when its session is discarded.
    // Release any resources associated with this scene that can be re-created the next time the scene connects.
    // The scene may re-connect later, as its session was not necessarily discarded (see `application:didDiscardSceneSessions` instead).
}
```
scene의 연결이 해제될 때 호출

3. sceneDidBecomeActive(_: )
```swift
func sceneDidBecomeActive(_ scene: UIScene) {
    // Called when the scene has moved from an inactive state to an active state.
    // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
}
```
app switcher에서 선택되는 등 scene과의 상호작용이 시작될 때 호출
* app switcher : 홈버튼을 두번누르거나 아이폰 하단에서 위로 스와이프 했을 때 현재 실행중인 앱들이 보이는 화면

4. sceneWillResignActive(_:)
```swift
func sceneWillResignActive(_ scene: UIScene) {
    // Called when the scene will move from an active state to an inactive state.
    // This may occur due to temporary interruptions (ex. an incoming phone call).
    print("[SceneDelegate] sceneWillResignActive(_ scene: UIScene)")
}
```
사용자가 scene과의 상호작용을 중지할 때 호출(다른 화면으로의 전환과 같은 경우)

5. sceneWillEnterForeground(_:)
```swift
func sceneWillEnterForeground(_ scene: UIScene) {
    // Called as the scene transitions from the background to the foreground.
    // Use this method to undo the changes made on entering the background.
    print("[SceneDelegate] sceneWillEnterForeground(_ scene: UIScene)")
}
```
scene이 foreground로 진입할 때 호출

6. sceneDidEnterBackground(_:)
``` swift
func sceneDidEnterBackground(_ scene: UIScene) {
    // Called as the scene transitions from the foreground to the background.
    // Use this method to save data, release shared resources, and store enough scene-specific state information
    // to restore the scene back to its current state.
    print("[SceneDelegate] sceneDidEnterBackground(_ scene: UIScene)")
}
```
scene이 background로 진입할 때 호출


## AppDelegate 생명주기




## AppDelegate - Scene LifeCycle
AppDelegate에는 Session LifeCycle에 대한 역할이 추가되었다.
SceneSession은 앱에서 생성한 모든 scene의 정보를 관리한다.

```swift
// MARK: UISceneSession Lifecycle
func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
    // Called when a new scene session is being created.
    // Use this method to select a configuration to create the new scene with.
    return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
}

func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
    // Called when the user discards a scene session.
    // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
    // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
}
```


## Scene?
UIKit은 UIWindowScene 객체를 사용하는 앱 UI의 각 인스턴스를 관리한다. Scene에는 UI의 하나의 인스턴스를 나타내는 windows와 view controllers가 들어있다. 또한, 각 scene에 해당하는 UIWindowSceneDelegate 객체를 가지고 있고, 이 객체는 UIKit과 앱 간의 상호작용을 조정하는데 사용된다.
Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행된다. 결과적으로 하나의 앱은 여러 Scene과 Scene delegate 객체를 동시에 활성화 할 수 있다.
(Scenes - Apple Development Document)


## Scene Session?
UISceneSession 객체는 Scene의 고유의 런타임 인스턴스를 관리한다. 사용자가 앱에 새로운 Scene을 추가하거나 요청하면, 시스템은 그 Scene을 추적하는 session 객체를 생성한다. 그 Session에는 고유한 식별자와 scene의 구성 세부사항(configuration details)가 들어있다.
UIKit은 session 정보를 그 Scene 자체의 생명주기동안 유지하고 app switcher에서 사용자가 그 Scene을 클로징하는 것에 대응하여 그 session을 파괴한다.
session 객체는 직접 생성하지 않고 UIKit이 앱의 사용자 인터페이스에 대응하여 생성한다. 
또한, 아래 함수를 통해서 UIKit에 새로운 Scene과 Session을 프로그래밍적 방식으로 생성할 수 있다.
(UISceneSession - Apple Development Document)


```swift
func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration 
func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>)
```

## iOS13부터 AppDelegate의 역할
1. 앱의 데이터 구조를 초기화 하는 것
2. 앱의 Scenes의 환경설정(Configuration)
3. 앱 밖에서발생한 알림(베터리 부족, 다운로드 완료 등)에 대응
4. 특정한 Scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것
5. 애플 푸쉬 알림 서브스와 같이 실행시 요구되는 모든 서비스를 등록
(UIApplicationDelegate - Apple Development Document)

## Deployment Target이 iOS 13 미만인 상황에서는?
iOS12이하는 단일 window 환경임으로, 아래와 같이 Scene처리를 제거할 수 있다.

1. SceneDelegate.swift 제거
2. AppDelegate에서 UISceneSession 관련 두개의 메소드 제거
3. SceneDelegate로 옮겨진 window 프로퍼티를 AppDelegate로 옮기기
```swift
var window: UIWindow?
```
4. info.plist에서 Scene과 관련된 Manifest인 Application Scene Manifest 삭제
