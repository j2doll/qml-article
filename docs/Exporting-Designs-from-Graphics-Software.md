
# 그래픽 소프트웨어에서 디자인 내보내기 (Exporting Designs from Graphics Software)

- http://doc.qt.io/qtcreator/quick-export-to-qml.html

Adobe Photoshop 및 GIMP와 같은 그래픽 소프트웨어에서 QML 파일로 디자인을 내보낼 수 있습니다.

## 어도비 포토샵(Adobe Photoshop)에서 QML로 내보내기

QML 에셋(asset) 내보내기를 사용하여 Adobe Photoshop에서 .ui.qml 파일로 디자인을 내보낼 수 있습니다. QML 에셋 내보내기 도구는 Photoshop 용 PNG EXPRESS 플러그인을 기반으로하는 템플릿을 제공합니다. 이를 통해 PSD 파일을 QML 형식으로 내보내 각 PSD 파일을 .ui.qml 파일로 변환할 수 있습니다.
[http://www.pngexpress.com/](http://www.pngexpress.com/)

생성된 문서의 파일 이름은 PSD 파일의 이름을 기반으로 합니다. 태그가 지정된 이미지 및 텍스트 객체는 절대 위치 지정과 함께 내보내집니다. 텍스트 개체는 글꼴 및 정렬 정보와 함께 내보내집니다.

자세한 내용은 QML 에셋 내보내기(QML Asset Exporter) 도구의 문서를 참조하십시오.
[https://github.com/Pelagicore/QmlAssetExporter](https://github.com/Pelagicore/QmlAssetExporter)

## 김프(Gimp)에서 QML로 내보내기

각 장면은 각 레이어에 대한 이미지 또는 텍스트 아이템이있는 단일 QML 파일로 변환되어 개발 PC에 저장됩니다. 각 레이어는 아이템으로 내보내집니다.

편집을 위해 Qt Creator에서 QML 파일을 열 수 있습니다. 기본적으로 내보내기 스크립트는 Qt Quick 1 파일을 생성합니다. 디자인 모드에서 파일을 편집하려면 내보내기 스크립트에서 import 문을 변경하여 Qt Quick 2를 가져옵니다. 또는 파일을 생성 한 후 각 파일에서 import 문을 변경할 수 있습니다.

## 변환 규칙

다음 규칙이 전환에 적용됩니다.

- 레이어(layer) 이름은 아이템 이름으로 사용됩니다. 공백과 해시 기호(#)는 밑줄 문자로 대체되어 아이템에 유효한 ID를 만듭니다.
- 그림자(shadow)와 같은 레이어 스타일은 이미지로 변환됩니다.
- 오프셋(offset), 크기(size), 순서(ordering) 및 불투명도(opacity)가 보존됩니다.
- 텍스트 아이템은 이미지(Image) 아이템으로 변환되도록 지정하지 않는 한 텍스트(Text) 아이템으로 변환됩니다.
- 숨겨진 레이어를 내보낼 수 있으며 표시 여부가 숨김으로 설정됩니다.
- PNG 이미지가 이미지 하위 디렉토리에 복사됩니다.

## 변환할 파일 준비

사용하기 쉬운 QML 파일을 만들려면 다음과 같이 내보내기 용 김프 디자인을 준비하십시오.

- 아이템 수를 최소화하려면 각 레이어를 텍스트 또는 이미지 아이템으로 내보내므로 레이어 수를 최소화하십시오.
- 일부 레이어가 내보내지지 않았는지 확인하려면 해당 레이어를 숨기고 내보내는 동안 숨겨진 내보내기 확인란의 선택을 취소하십시오.
- 레이어를 내보낸 후 레이어를 쉽게 찾으려면 설명이 포함된 이름을 사용하십시오.
- 내보내는 동안 이미지 크기가 유지되는지 확인하려면 배경 레이어와 같이 완전히 숨겨져있는 레이어를 하나 이상 만드십시오. 내보내기 스크립트가 완전히 채워진 레이어를 찾지 못하면 모든 이미지를 캔바스의 크기로 조정합니다.
- 내보내기 중에 오류를 방지하려면 레이어가 잠겨 있지 않은지 확인하십시오. 잠긴 레이어(locked layer)는 내보낼 수 없습니다.
- 예상치 못한 결과를 피하려면 블렌딩 모드(Blending Mode) 효과를 사용하지 마십시오. 그들은 내보내지지 않습니다.

## 내보내기 스크립트 실행하기

이 스크립트는 김프 2에서 작동하도록 테스트되었습니다. 김프 2에서 김프 2를 다운로드 할 수 있습니다.

1. Qt 코드 검토 에서 내보내기 스크립트 인 qmlexporter.py 가 들어있는 저장소를 복제하십시오.
[https://codereview.qt-project.org/#/admin/projects/qt-labs/gimp-qmlexporter](https://codereview.qt-project.org/#/admin/projects/qt-labs/gimp-qmlexporter)
참고: 스크립트에 대한 최신 정보는 저장소의 INSTALL.txt를 읽으십시오.

2. 내보내기 스크립트를 김프 설치 디렉토리의 플러그인 디렉토리에 복사하십시오.

3. 파일의 속성을 검사하여 실행 파일인지 확인하십시오.
   Linux에서 다음 명령을 실행하십시오. chmod u+rx

4. 디자인 모드에서 편집할 수 있는 QML 파일을 생성하려면 qmlexporter.py 에서 import 문을 편집하십시오.
 예:
```python
  f.write( 'import QtQuick 2.5 \n' )
```  

5. 김프를 다시 시작하여 내보내기 명령을 파일 메뉴에 추가하십시오.

6. 파일 > QML 로 내보내기를 선택하여 디자인을 QML 파일로 내 보냅니다.

7. 레이어를 QML 문서로 내보내기 대화 상자에서 QML 파일의 이름과 위치를 입력하고 내보내기를 클릭 합니다 .

QML 파일은 지정한 위치에 저장됩니다. Qt Creator에서 파일 > 파일 열기 또는 프로젝트를 선택하여 QML 파일을 엽니다.

참고: 기존 파일은 경고없이 대체됩니다.




