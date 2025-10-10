# PowerPoint Translator with Azure OpenAI

Azure OpenAI를 활용한 PowerPoint 프레젠테이션 자동 번역 도구입니다. GPT 모델을 사용하여 고품질의 자연스러운 번역을 제공합니다.

## 주요 기능

- **다국어 지원**: 한국어, 영어, 일본어, 중국어(간체/번체), 스페인어, 프랑스어, 독일어, 포르투갈어
- **배치 번역**: 여러 텍스트를 한 번에 효율적으로 번역
- **선택적 번역**: 특정 슬라이드만 선택하여 번역 가능
- **서식 보존**: 원본 PPT의 폰트 크기, 굵기, 색상 등 서식 유지
- **자동 폰트 적용**: 목표 언어에 최적화된 폰트 자동 설정
- **자연스러운 표현**: 비즈니스/기술 용어를 고려한 자연스러운 번역

## 필수 요구사항

### Python 패키지
```bash
pip install python-pptx openai python-dotenv
```

### Azure OpenAI 설정
- Azure OpenAI 서비스 구독
- GPT-4 또는 GPT-3.5-turbo 모델 배포
- API 키 및 엔드포인트

## 설치 방법

1. **저장소 클론 또는 파일 다운로드**
```bash
git clone <repository-url>
cd powerpoint-translator
```

2. **필수 패키지 설치**
```bash
pip install python-pptx openai python-dotenv
```

3. **환경 변수 설정**

`.env` 파일을 생성하고 다음 내용을 추가합니다:
```env
AZURE_OPENAI_API_KEY=your_api_key_here
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com/
```

## 사용 방법

### 기본 사용법

```python
from ppt_translator import AzureOpenAITranslator, PowerPointTranslator

# 번역기 초기화
translator = AzureOpenAITranslator(
    deployment_name="gpt-4.1",  # 배포된 모델 이름
    temperature=0.1,
    max_tokens=4000
)

ppt_translator = PowerPointTranslator(translator)

# 전체 슬라이드 번역
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_ko.pptx",
    target_language="ko",
    enable_polishing=True
)
```

### 특정 슬라이드만 번역

```python
# 1, 3, 5, 7번 슬라이드만 번역
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_selected_ko.pptx",
    target_language="ko",
    slide_numbers=[1, 3, 5, 7],
    enable_polishing=True
)
```

### 슬라이드 정보 확인

```python
# 프레젠테이션의 슬라이드 정보 출력
ppt_translator.get_slide_info("presentation.pptx")
```

### 다양한 언어로 번역

```python
# 영어로 번역
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_en.pptx",
    target_language="en"
)

# 일본어로 번역
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_ja.pptx",
    target_language="ja"
)

# 중국어(간체)로 번역
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_zh.pptx",
    target_language="zh"
)
```

## 클래스 및 메서드

### AzureOpenAITranslator

Azure OpenAI를 사용한 번역 엔진 클래스입니다.

#### 초기화 매개변수
- `api_key` (str): Azure OpenAI API 키 (기본값: 환경변수에서 로드)
- `api_version` (str): API 버전 (기본값: "2025-01-01-preview")
- `azure_endpoint` (str): Azure OpenAI 엔드포인트 URL (기본값: 환경변수에서 로드)
- `deployment_name` (str): 배포된 모델 이름 (기본값: "gpt-4.1")
- `temperature` (float): 생성 온도 0.0~1.0 (기본값: 0.1)
- `max_tokens` (int): 최대 토큰 수 (기본값: 4000)

#### 주요 메서드
- `translate_batch(texts, target_language, enable_polishing)`: 텍스트 배치 번역

### PowerPointTranslator

PowerPoint 프레젠테이션 번역 핸들러 클래스입니다.

#### 초기화 매개변수
- `translator` (AzureOpenAITranslator): AzureOpenAITranslator 인스턴스

#### 주요 메서드

**translate_presentation(input_path, output_path, target_language, slide_numbers, batch_size, enable_polishing)**
- `input_path` (str): 입력 PPTX 파일 경로
- `output_path` (str): 출력 PPTX 파일 경로
- `target_language` (str): 목표 언어 코드 (기본값: "ko")
- `slide_numbers` (List[int], optional): 번역할 슬라이드 번호 리스트 (None이면 전체)
- `batch_size` (int): 배치 번역 크기 (기본값: 20)
- `enable_polishing` (bool): 자연스러운 표현 개선 여부 (기본값: True)

**get_slide_info(input_path)**
- `input_path` (str): PPTX 파일 경로
- 프레젠테이션의 슬라이드 정보를 출력합니다

## 지원 언어

| 언어 코드 | 언어 | 기본 폰트 |
|---------|------|----------|
| ko | 한국어 | 맑은 고딕 |
| en | 영어 | Arial |
| ja | 일본어 | Yu Gothic UI |
| zh | 중국어(간체) | Microsoft YaHei |
| zh-tw | 중국어(번체) | Microsoft JhengHei |
| es | 스페인어 | Arial |
| fr | 프랑스어 | Arial |
| de | 독일어 | Arial |
| pt | 포르투갈어 | Arial |

## 설정 옵션

### Temperature (생성 온도)
- **낮은 값 (0.0 ~ 0.3)**: 더 일관되고 예측 가능한 번역 (권장)
- **중간 값 (0.4 ~ 0.7)**: 균형잡힌 번역
- **높은 값 (0.8 ~ 1.0)**: 더 창의적이지만 일관성 낮음

### Batch Size (배치 크기)
- **작은 배치 (10 ~ 20)**: 안정적이고 정확한 번역 (권장)
- **큰 배치 (30 ~ 50)**: 빠른 처리 속도, 단 API 제한에 주의

### Enable Polishing (표현 개선)
- **True**: 자연스러운 표현으로 개선 (권장)
- **False**: 직역에 가까운 번역

## 주의사항

1. **API 사용량**: Azure OpenAI API는 사용량에 따라 비용이 발생합니다
2. **토큰 제한**: 슬라이드당 텍스트가 너무 많으면 배치 크기를 줄이세요
3. **폰트 호환성**: 목표 시스템에 설정된 폰트가 설치되어 있어야 합니다
4. **백업**: 원본 파일은 항상 백업해두는 것을 권장합니다
5. **이미지/도형**: 이미지나 도형 내 텍스트는 번역되지 않습니다

## 문제 해결

### API 인증 오류
```python
# .env 파일의 API 키와 엔드포인트를 확인하세요
AZURE_OPENAI_API_KEY=your_api_key_here
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com/
```

### 번역 품질 개선
```python
# temperature를 낮추고 enable_polishing을 True로 설정
translator = AzureOpenAITranslator(
    deployment_name="gpt-4",  # GPT-4 사용 권장
    temperature=0.1,
    max_tokens=4000
)
```

### 처리 속도 개선
```python
# batch_size를 증가 (API 제한 범위 내에서)
ppt_translator.translate_presentation(
    input_path="presentation.pptx",
    output_path="presentation_ko.pptx",
    target_language="ko",
    batch_size=30  # 기본값 20에서 증가
)
```

## 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다.

