--- 
layout: single
title: IOS)기기별 Cell Size 변경해보기 
categories: CODING
tag: IOS)Ukit
--- 
#기기별로 cell의 Size를 변경해봅시다!

안녕하세요! collectionView로 UI를 구현하고있었는데요! 원하기기로 변경해가며 화면을 구현하다가 셀 사이즈가 기기마다 완전히 틀려지는걸 뒤늦게 알았습니다. ㅎㅎ 처음에는 어떻게 할까 고민을 많이 했는데요!
###### (이런고민은 저만하겠죠..ㅋㅋ)

자. 일단은 제가 봤던 문제는..

|iPhone 8|iPhone 12pro|
|--|--|
|![image desciption](https://user-images.githubusercontent.com/90044171/161029422-f5dbddaa-2d97-4a43-9ae9-914b878e80aa.png)|![image desciption](https://user-images.githubusercontent.com/90044171/161029490-8ac027ff-c514-4faf-b923-f996785e5064.png)


애초에 한 기기로만 만들기로 했다면 고정된 값을 줘서 상관없지만.. 이렇게 다른 기기를 사용하게되면 완~~전히 서로 멀어져있는걸 볼수있습니다.

방법은 아주 다양합니다. UIDevice.current로 현재 디바이스값을 인지하고 맞추는 법도있고 extension으로 확장해보는 법도있습니다. 저는 그보다 좀 더 간단한 방법을 사용해보았습니다! 

코드로 보겠습니다! 
```swift
            let DeviceWidth = UIScreen.main.bounds.size.width

            if DeviceWidth == 320.0 {
                print("iPhone 4, iPhone 5 iPhone SE(1세대)")
            } else if  DeviceWidth == 375.0 {
                print("iPhone 6,6s, iPhone 7,8, iPhone X, Xs,  iPhone 11 Pro, iPhone SE(2세대")
            } else if  DeviceWidth == 414.0 {
                print("iPhone 6+, 6s+, 7+, 8+, iPhone Xr, iPhone Xs Max, iPhone 11, iPhone 11 Pro Max")
            } else if  DeviceWidth == 390.0 {
                print("iPhone 12, 12 Pro")
            } else if  DeviceWidth == 360.0 {
                print("iPhone 12 mini")
            }
```
알록달록 아주이쁘네요 ㅎㅎ 
 > DeviceWidth = UIScreen.main.bounds.size.width
 
 코드를 보시면 DeviceWidth라는 상수에 휴대폰의 폭을 알수있는 UIScreen.main.bounds.size.width를 선언 해주었습니다! 
 
 ___

다들 아시겠지만 코드를 간단히 설명하자면 만약에 휴대폰의 폭이 == (숫자.0)이라면 { 이러한 값을 주겠다ㅏㅏ   }    
간단한 코드입니다. 네..  전 항상 if문을 좋아하는데요. 좋습니다!

저희는 이제 어떻게 해야하는지 방법을 알았습니다. 그럼 바로 써야겠죠?

collectionView의 자신의 셀사이즈를 알려주기위해선 `UICollectionViewDelegateFlowLayout`를 채택 해줘야겠죠  그리고 `sizeForItemAt`의 함수를 써줍니다~ 
```swift
extension IsCollectionView: UICollectionViewDelegateFlowLayout {
    func collectionView(
        _ collectionView: UICollectionView,
        layout collectionViewLayout: UICollectionViewLayout,
        sizeForItemAt indexPath: IndexPath) -> CGSize {

            let width: CGFloat = UIScreen.main.bounds.size.width
            let height: CGFloat = collectionView.frame.height

            return CGSize(width: width, height: height)
        }
``` 
린트를 썻더니 마구 나눠져있네요 ㅎㅎ

아무튼! 여기서 아까 위에서 본 if문을 응용해주면 완성됩니다! 

```swift
   func collectionView(
        _ collectionView: UICollectionView,
        layout collectionViewLayout: UICollectionViewLayout,
        sizeForItemAt indexPath: IndexPath) -> CGSize {
            
            var width: CGFloat = UIScreen.main.bounds.size.width
            let currentDeviceWidth = UIScreen.main.bounds.size.width
            
            if currentDeviceWidth == 375.0 { 
             // iPhone 6,6s, iPhone 7,8, iPhone X, Xs,  iPhone 11 Pro, iPhone SE(2세대)
            width = currentDeviceWidth - 70.0 * 3

            } else if currentDeviceWidth == 414.0 {
            // "iPhone 6+, 6s+, 7+, 8+, iPhone Xr, iPhone Xs Max, iPhone 11, iPhone 11 Pro Max"
            
            width = currentDeviceWidth - 80.0 * 3
            } else if currentDeviceWidth == 390.0 {
            // iPhone 12, 12 Pro

            width = currentDeviceWidth - 75.0 * 3
            }
            
            let height: CGFloat = collectionView.frame.height
            return CGSize(width: width, height: height)
        }
```

코드를 해석해보면 일단 `width`라는 변수를 `CGFloat`로 `UIScreen.main.bounds.size.width` 선언해서 셀에서의 휴대폰 폭을 알려줍니다. 
그리고 `currentDeviceWidth`라는 현재 디바이스의 폭을 알려주는 상수를 정해줍니다!


```swift
 if currentDeviceWidth == 375.0 { 
           "iPhone 6,6s, iPhone 7,8, iPhone X, Xs,  iPhone 11 Pro, iPhone SE(2세대)"
            width = currentDeviceWidth - 70.0 * 3
            }
```
만약에 `currentDeviceWidth`(현재디바이스 폭)이 375.0라면  {  width = 현재디바이스의 폭 - (원하는숫자) * (원하는숫자) 만큼 조정하겠다.
 }

 *(숫자가 375.0이면  iPhone 6,6s, iPhone 7,8, iPhone X, Xs,  iPhone 11 Pro, iPhone SE(2세대))* 요놈들 이겠죠? ㅎㅎ
  그리고 변경된 `width`이친구를 
  
   > return CGSize(width:`width`, height: height) 

여기에 넣어주면 이제부터는 디바이스가 바뀔때마다 if문을 따라 대응이 되겠죠? 

___

사실 제가 이번에 써본 방법이 전부가아니고 방법이 너무나도 많습니다. 저도 찾아보고 참고도 했지만 
참고한 코드를 저의 것으로 만들어 갈때마다 정말 발전하고 있다는 느낌이 들어서 뿌듯합니다. 

네! 오늘은 앱스토어에 있는 예쁜 앱을 아무거나 찾아서 따라서 만들어보던중 문제가 나와서 해결해 보았습니다! 
저처럼 이런문제로 헤매시는 분들도 간혹 계실텐데요! 정말 조금의 도움이라도 되었으면 행복하겠네요!! 😀😀

그리고.. markdown 문법을 조금 익혀가는 중이여서 이쁘고 멋진 틀이 잡혀가는 모습입니다.  
계속 발전해나가는 코린이가 되겠습니다! 참고해주셔서 감사합니다!