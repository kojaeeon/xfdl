# Nexacro 14 XFDL 분석기

`index.html`을 [다운로드](https://raw.githubusercontent.com/kojaeeon/xfdl/main/index.html)해 Chromium 기반 Edge 또는 Chrome에서 열고 `.xfdl` 파일을 선택합니다. 설치와 서버 연결 없이 동작합니다. 저장소의 [예제 XFDL](examples/check-review.xfdl)로 먼저 확인할 수 있습니다.

## 결과표

| 표 | 표시 항목 |
|---|---|
| 요소 · 이벤트 | 요소 경로, 유형, 이벤트, 핸들러, 핸들러 본문의 직접 호출 함수, 원본 파일 행 번호 |
| 데이터셋 | 데이터셋 ID, 컬럼 ID, 타입, 길이, 연결된 컴포넌트 속성 또는 Grid 셀의 바인딩 |

각 표를 따로 검색할 수 있습니다. CSV 버튼은 현재 표에서 검색에 걸린 행을 UTF-8 BOM 형식으로 저장합니다. 인코딩 자동 감지는 UTF-8, XML에 선언된 EUC-KR/CP949, BOM이 있거나 시그니처를 확인할 수 있는 UTF-16을 처리합니다. 필요한 경우 수동으로 인코딩을 바꿀 수 있습니다.

### 분석 범위

- XML 선언과 스크립트의 `addEventHandler`, `setEventHandler`, `addEventHandlerLookup`, `setEventHandlerLookup` 호출을 읽습니다. 스크립트 등록의 대상 경로와 이벤트 및 핸들러를 정적으로 확인할 수 있어야 합니다.
- Nexacro 14 스크립트의 `function(obj:Button, e:nexacro.ClickEventInfo)` 타입 표기를 처리합니다. 함수 안의 직접 호출을 소스에 나타나는 순서대로 표시하며 같은 이름은 한 번만 표시합니다. 중첩 함수 내부는 해당 함수의 별도 본문으로 처리합니다.
- 분석 대상은 선택한 XFDL입니다. `include`로 참조한 외부 스크립트의 정의와 실행 시 계산되는 이벤트 이름이나 대상은 분석 범위 밖입니다. 정의를 찾지 못하는 핸들러에는 `정의 미확인`을 표시합니다.
- `Dataset/ColumnInfo`의 일반 컬럼과 상수 컬럼을 읽습니다. `BindItem`과 Grid의 `binddataset` 및 `bind:컬럼ID` 속성에서 UI 연결을 찾습니다.
- 파일은 브라우저에서 읽으며 XFDL의 스크립트는 구문 분석만 합니다. JavaScript 구문 분석기 Acorn 8.15.0과 MIT 라이선스가 HTML에 포함되어 있습니다.

형식 참고 문서는 [Nexacro 14 개발자 가이드](https://docs.tobesoft.com/developer_guide_nexacro_14_en_kr/9bdeab5af5b269f4)와 [Dataset XML 형식](https://docs.tobesoft.com/admin_guide_nexacro_14_en_kr/ea2d0a940547e4a5)입니다.
