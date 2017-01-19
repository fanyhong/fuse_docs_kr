원본: https://www.fusetools.com/docs/native-interop/native-ux-components

UX 마크 업용 사용자 정의 원시 제어 래퍼

이 튜토리얼에서는 Uno 및 외부 코드를 사용하여 기본 Android 또는 iOS 컨트롤에 대한 사용자 정의 래퍼를 작성하고이를 UX 태그에서 사용할 수 있도록 부트 스트랩하는 방법을 설명합니다. 예를 들어 슬라이더 컨트롤을 래핑하는 방법을 보여줍니다.

이 튜토리얼에서는 작업중인 플랫폼의 기본 언어, 즉 Android 용 Java 및 iOS 용 Objective-C에 익숙하다고 가정합니다.

LeafView 구현하기

우리가 구현할 수있는보기에는 두 가지 유형이 있습니다. ParentView를 사용하면 다른 뷰 (예 : 패널)가 포함 된보기를 래핑 할 수 있지만 LeafView는 다른보기 (예 : 단추, 스위치, 선택기)가없는보기를 래핑합니다. 이 예에서는 가장 일반적으로 필요한 것이기 때문에 LeafView에 초점을 맞 춥니 다.

우리는 슬라이더를 정의하는 클래스를 생성하여 속성을 처리하는 보일러 코드를 만듭니다. 또한 네이티브 클래스와 슬라이더 클래스 사이의 통신에 사용할 인터페이스 쌍을 선언합니다.

네임 스페이스 네이티브
{
    우노를 사용하여;
    Uno.UX 사용;

    Fuse.Controls를 사용하여;

    공용 인터페이스 ISliderHost
    {
        void OnValueChanged (float newValue);
    }

    공용 인터페이스 ISlider
    {
        float Value {set; }
    }

    공용 클래스 MySlider : Control, ISliderHost
    {
        부동 _value;
        [UXOriginSetter ( "SetValue")]
        public float Value
        {
            get {return _value; }
            set {SetValue (value, this); }
        }

        정적 읽기 전용 선택기 _valueName = "값";
        공공 무효 SetValue (float newValue, IPropertyListener 원점)
        {
            if (_value! = newValue)
            {
                _value = newValue;
                OnPropertyChanged (_valueName, origin);
            }
            if (origin! = null)
            {
                var ns = NativeSlider;
                if (ns! = null)
                    ns.Value = newValue;
            }
        }

        void ISliderHost.OnValueChanged (float newValue)
        {
            SetValue (newValue, null);
        }

        ISlider NativeSlider
        {
            get {NativeView를 ISlider로 반환합니다. }
        }
    }

}
LeafView 클래스를 확장하여 네이티브 뷰에 대한 래퍼를 만드는 것으로 넘어갑니다. iOS 및 Android 용으로 별도의 버전을 만들어야하며, 별도의 파일로 만들어 두는 것이 좋습니다. 하나의 플랫폼 만 지원하려면 괜찮습니다. 그러면 다른 플랫폼의 단계를 건너 뛸 수 있습니다.

Fuse.Controls.Native.iOS와 Fuse.Controls.Native.Android 네임 스페이스에는 각각 iOS와 Android 용 LeafView 클래스의 두 가지 버전이 있습니다.

외부 코드 (예 : 외부 속성)에 액세스하려면 Uno.Compiler.ExportTargetInterop을 사용하여 추가해야합니다.
iOS 용 LeafView 구현

우리는 Objective-C 코드를 사용하여 Uno에서 LeafView 클래스를 만듭니다.

네임 스페이스 Native.iOS
{
    우노를 사용하여;
    Uno.UX 사용;
    Uno.Compiler.ExportTargetInterop;

    Fuse.Controls.Native.iOS를 사용하여;

    [Require ( "Source.Include", "UIKit / UIKit.h")]
    extern (iOS) 공용 클래스 MySlider : LeafView, ISlider
    {
        [UXConstructor]
        public MySlider ([UXParameter ( "Host")] ISliderHost 호스트) : base (Create ()) {}

        [Foreign (Language.ObjC)]
        정적 ObjC.Object Create ()
        @ {
            :: UISlider * slider = [[:: UISlider alloc] init];
            [slider setMinimumValue : 0.0f];
            [slider setMaximumValue : 100.0f];
            반환 슬라이더;
        @}

        public float Value
        {
            get {리턴 값 GetValue (Handle); }
            set {SetValue (Handle, value); }
        }

        [Foreign (Language.ObjC)]
        정적 float GetValue (ObjC.Object 핸들)
        @ {
            :: UISlider * slider = (:: UISlider *) 핸들;
            return [슬라이더 값];
        @}

        [Foreign (Language.ObjC)]
        정적 void SetValue (ObjC.Object 핸들, 부동 값)
        @ {
            :: UISlider * slider = (:: UISlider *) 핸들;
            [슬라이더 setValue : 값 애니메이션 : false];
        @}

        void OnValueChanged ()
        {
            // TODO : 구현 된 값이 콜백으로 변경되었습니다.
        }
    }
}
위의 예제는 값 설정 만 구현하고 UIControlEventValueChanged에 연결하여 사용자가 컨트롤과 상호 작용할 때 이벤트를 다시 퓨즈로 공급하지 않습니다. 완전한 유용한 래퍼가 필요합니다.
LeafView 생성자가 ObjC.Object를 인수로 얻는 방법을 주목하십시오. 우리는 UISlider를 인스턴스화하고이를 기본 생성자에 대한 인수로 호출하는 정적 메서드를 만들어 피드를 제공합니다.

나중에 클래스 내에서 UISlider를 나타내는 ObjC.Object에 액세스하려면 위의 Value 속성을 구현하는 방법에서와 같이 Handle 속성을 사용할 수 있습니다.

MySlider 클래스에는 extern (iOS) 속성이 있으므로 iOS 용으로 빌드 할 때만 사용할 수 있습니다. UX 마크 업에서이 클래스를 사용할 수있게하려면 모든 플랫폼에서 유효해야합니다. 따라서 동일한 네임 스페이스 내에서 extern (! iOS)으로 표시된 공용 인터페이스 만 사용하여 더미 구현을 제공해야합니다.

네임 스페이스 Native.iOS
{
    extern (! iOS) public class MySlider
    {
        [UXConstructor]
        public MySlider ([UXParameter ( "Host")] ISliderHost 호스트) {}
    }
}
Android 용 LeafView 구현

그런 다음 동등한 Android 구현을 작성해 보겠습니다.

네임 스페이스 Native.Android
{
    우노를 사용하여;
    Uno.UX 사용;
    Uno.Compiler.ExportTargetInterop;

    Fuse.Controls.Native.Android를 사용하여;

    extern (Android) 공용 클래스 MySlider : LeafView, ISlider
    {
        ISliderHost _host;
        [UXConstructor]
        public MySlider ([UXParameter ( "Host")] ISliderHost 호스트) : base (Create ())
        {
            _host = 호스트; AddChangedCallback (핸들, OnValueChanged);
        }

        ISlider.Value 부동
        {
            set {SetValue (Handle, value); }
        }

        [Foreign (Language.Java)]
        정적 Java.Object Create ()
        @ {
            android.widget.SeekBar seekBar = new android.widget.SeekBar (@ (Activity.Package). @ (Activity.Name) .GetRootActivity ());
            seekBar.setMax (100);
            return seekBar;
        @}

        [Foreign (Language.Java)]
        void AddChangedCallback (Java.Object 핸들, 액션 <float> onValueChanged)
        @ {
            ((android.widget.SeekBar) 핸들) .setOnSeekBarChangeListener (새로운 android.widget.SeekBar.OnSeekBarChangeListener () {
                public void onProgressChanged (android.widget.SeekBar seekBar, int progress, boolean fromUser) {
                    onValueChanged.run ((float) progress);
                }
                public void onStartTrackingTouch (android.widget.SeekBar seekBar) {}
                public void onStopTrackingTouch (android.widget.SeekBar seekBar) {}
            });
        @}

        [Foreign (Language.Java)]
        void SetValue (Java.Object 핸들, float 값)
        @ {
            ((android.widget.SeekBar) 핸들) .setProgress ((int) value);
        @}

        void OnValueChanged (float newValue)
        {
            _host.OnValueChanged (newValue);
        }
    }
}
위의 구현은 또한 OnSeekBarChangeListener를 구현하여 Value proprotty를 업데이트하기 위해 안드로이드에서 이벤트를 다시 얻는 방법을 보여줍니다.

iOS의 경우, extern (! Android)에 더미 구현을 추가하여 UX 마크 업에서이 기능을 사용할 수 있도록해야합니다.

네임 스페이스 Native.Android
{
    extern (! Android) 공용 클래스 MySlider
    {
        [UXConstructor]
        public MySlider ([UXParameter ( "Host")] ISliderHost 호스트) {}
    }
}
UX 래퍼 컨트롤 만들기

LeafView 클래스는 Visual을 상속하지 않으므로 UX 마크 업 트리에서 직접 사용할 수 없습니다. 래퍼 역할을하는 중간 컨트롤 클래스를 만들어야합니다. 이것은 우리가 UX에 공개하는 공개 API와 각 플랫폼의 구현 세부 사항을 구분할 수있게 해주기 때문에 좋습니다. 또한 크로스 플랫폼에서 작동하는 통합 래퍼를 만들 수 있습니다.

Fuse.Controls.Control 클래스에는 깔끔한 마술이 있습니다. 다양한 시나리오에 대해 여러 템플릿을 지정할 수 있으며 상황에 따라 런타임에 올바른 템플릿을 인스턴스화합니다.

AndroidTemplate - NativeViewHost 내부에서 Android에서 사용됩니다.
iOSTemplate - iOS에서 NativeViewHost 내부에서 사용됩니다.
GraphicsTemplate - 그래픽 컨텍스트 (NativeViewHost 외부의 경우)에서 사용되며 데스크톱의 대체 기능으로 사용됩니다.
따라서 Slider가 모든 플랫폼에서 작동하도록하려면 각 시나리오에 대해 하나의 템플릿을 제공해야합니다. 이것을 호스트 attibuite에 전달하여 Native.MySlider와 네이티브 상대를 연결합니다.

<Native.MySlider ux : Class = "NativeSlider">
    <Native.Android.MySlider ux : Template = "AndroidAppearance"Host = "this"/>
    <Native.iOS.MySlider ux : Template = "iOSAppearance"Host = "this"/>
    <Text ux : Template = "GraphicsAppearance">
        MySlider는이 컨텍스트에서는 사용할 수 없습니다.
    </ 텍스트>
</Native.MySlider>
이렇게하면 모든 것이 연결되므로 UX 마크 업에서 <NativeSlider />를 사용하면 NativeViewHost 내부에있는 경우 각 플랫폼에서 올바른 구현을 사용하게됩니다. 다른 시나리오에서 오류 텍스트 메시지를 표시합니다.

원한다면 오류 메시지를 표시하는 대신 GraphicsTemplate으로 슬라이더를 올바르게 구현할 수 있습니다.

끝났어.

행복한 코딩. 이 자습서는 진행중인 작업입니다. 문제가 발생하면 의견을 보내주십시오.
