원본: https://www.fusetools.com/docs/primitives/primitives

# 기본 요소들 (Primitives) #

fuse는 보다 복잡한 비주얼 요소들의 기본 빌딩 블록인 기본 요소들 세트(a set of primitive elements)를 제공합니다. 이러한 기본 요소(primitives)들은 [Text](https://www.fusetools.com/docs/fuse/controls/text) , [Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) , [Circle](https://www.fusetools.com/docs/fuse/controls/circle) , [Image](https://www.fusetools.com/docs/fuse/controls/image) 및 [Video](https://www.fusetools.com/docs/fuse/controls/video) 를 포함합니다.

## 모양 (Shapes) ##

앱에서 가장 많이 사용되는 두 가지 모양은 Rectangle 과 Circle 입니다. 이들은 fuse에서 각각 그들 자신의 타입을 가지고 있습니다.:

```
<Panel>
    <Circle Color="#bbfdff" />
    <Rectangle Color="#ff6eb4" />
</Panel>
```

[Rectangle](https://www.fusetools.com/docs/fuse/controls/rectangle) 과 [Circle](https://www.fusetools.com/docs/fuse/controls/circle) 에 대한 전체 설명서를 확인하십시오.

## 색 (Color) ##

Fuse 의 모든 비주얼 요소에는 Color 속성이 있습니다. 이 속성은 해당 요소의 주 색상을 직관적으로 간주되는 요소로 매핑합니다.

Rectangle 과 Circle 같은 모양(Shapes)의 경우, `Color` 속성이 해당 색상의 [SolidColor](https://www.fusetools.com/docs/fuse/drawing/solidcolor) 브러시로 변환되는 것을 예상할 수 있습니다. 이것은 다음 샘플을 의미합니다.:

```
<Rectangle Color="#f0f" />
```

실제로는 다음과 같이 변환됩니다.

```
<Rectangle Fill="#f0f"/>
```

`Fill` 속성은 [Brush](https://www.fusetools.com/docs/fuse/drawing/brush) 타입이기 때문에 `Fill="#f0f"` 부분이 실제로는 다음과 같이 더 확장됩니다.

```
<Rectangle>
    <SolidColor Color="#f0f"/>
</Rectangle>
```

[SolidColor](https://www.fusetools.com/docs/fuse/drawing/solidcolor) 가 Rectangle `Fill` 속성에 "자동 바인딩" 되는 곳입니다.

## 브러쉬 및 선 (Brushes and Strokes) ##

Fuse의 색상은 브러쉬(Brushes)로 표현됩니다. 일반적인 단색을 얻으려면 [SolidColor](https://www.fusetools.com/docs/fuse/drawing/solidcolor) 브러시를 사용할 수 있습니다. 모든 모양들(shapes)에는 브러시 타입의 `Fill` 속성이 있습니다. 모양(Shapes)들에는 단색을 설정하는 것이 일반적이기때문에 특별한 `Color` 속성이 존재습니다.

## 텍스트 (Text) ##

[Text](https://www.fusetools.com/docs/fuse/controls/text) 클래스는 정적 텍스트를 표시하는 기본적인 기본 요소(primitive) 입니다.

다음은 다양한 텍스트 관련 컨트롤들 간의 차이점에 대한 개요입니다.

- Text - 여러 줄을 감쌀 수 있는(wrap over) 상호 작용이 불가능한 텍스트를 표시합니다.
- TextInput - 사용자 상호작용이 가능한 한 줄 텍스트 컨트롤입니다.
    - 텍스트만 입력가능합니다. - 특별한 장식(decoration)이 없습니다.
    - DockLayout을 가지고 있습니다. 즉, 추가된 속성 `DockPanel.Dock` 을 사용하여 자식요소들을 배치할 수 있습니다.
    - bool IsPassword 를 가집니다.
    - TextInputActionHandler ActionTriggered 를 가집니다.
- TextBox - TextInput 과 비슷하지만, 기본 장식(decoration)을 사용하므로 비우기가 더 쉽습니다. (예: 프로토 타이핑 동안)
- TextView
    - 여러 줄 입력 가능한 TextInput 입니다.
    - 장식 되지 않았습니다. (Undecorated)
    - 비밀번호 입력이 불가합니다.
    - 특별한 액션이 불가합니다.
- Basic.TextInput - Fuse.BasicTheme 패키지의 장식(decoration)된 TextInput입니다.

## 이미지/비디오 (Image/Video) ##

[Image](https://www.fusetools.com/docs/fuse/controls/image) 및 [Video](https://www.fusetools.com/docs/fuse/controls/video) 요소들을 사용하여 Image 및 Video 를 쉽게 포함할 수 있습니다.

```
<Image File="someImage.jpg" />
<Video File="someVideo.mp4" />
```

[Image](https://www.fusetools.com/docs/fuse/controls/image) 와 [Video](https://www.fusetools.com/docs/fuse/controls/video) 모두 로컬 파일 대신 URL을 가져 오는 것을 지원합니다.

```
<Image Url="http://www.some.com/image.jpg" />
<Video Url="http://www.some.com/video.mp4" />
```

[Image](https://www.fusetools.com/docs/fuse/controls/image) 및 [Video](https://www.fusetools.com/docs/fuse/controls/video) 에서 자세한 문서를 확인하십시오.
