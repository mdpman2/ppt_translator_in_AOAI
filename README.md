# PowerPoint Translator with Azure OpenAI (Optimized Edition)

Azure OpenAI를 활용한 **고성능 비동기 PowerPoint 프레젠테이션 자동 번역 도구**입니다.
GPT-4o 모델과 비동기 병렬 처리를 사용하여 빠르고 정확하며 자연스러운 번역을 제공합니다.

## 🚀 주요 기능 (Optimized)

- **⚡ 초고속 비동기 처리**: `asyncio` 기반 병렬 처리로 번역 속도 획기적 개선 (기본 5개 슬라이드 동시 처리)
- **📊 표 & 그룹 지원**: 기존에 누락되던 **표(Table)** 내부 텍스트와 **그룹화된 도형(Group)**의 텍스트까지 완벽 추출 및 번역
- **🔄 자동 재시도 (Retry)**: API 호출 실패 시 지수 백오프(Exponential Backoff)로 자동 재시도하여 안정성 확보
- **🎨 서식 완벽 보존**: 원본 PPT의 폰트 크기, 굵기, 색상 등 서식 유지 + **동아시아 폰트(한글/중국어) 자동 보정**
- **🌍 다국어 지원**: 한국어, 영어, 일본어, 중국어(간체/번체), 스페인어 등

## 📦 필수 요구사항

### Python 패키지
```bash
pip install python-pptx openai python-dotenv nest_asyncio
```

### Azure OpenAI 설정
- Azure OpenAI 서비스 구독
- **GPT-4o** 또는 **GPT-4 Turbo** 모델 배포 (권장)
- API 키 및 엔드포인트

## 🛠️ 설치 및 설정

1. **저장소 클론**
```bash
git clone <repository-url>
cd powerpoint-translator
```

2. **패키지 설치**
```bash
pip install python-pptx openai python-dotenv nest_asyncio
```

3. **환경 변수 설정 (.env)**
```env
AZURE_OPENAI_API_KEY=your_api_key_here
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com/
```

## 💻 사용 방법

**Jupyter Notebook** 환경에서 실행하는 것을 권장합니다.

### 기본 실행 코드

```python
import asyncio
from ppt_translator import AzureOpenAITranslator, PowerPointTranslator

async def main():
    # 1. 번역기 초기화
    translator = AzureOpenAITranslator(
        deployment_name="gpt-4o",  # 고성능 모델 권장
        temperature=0.1
    )

    ppt_translator = PowerPointTranslator(translator)

    # 2. 번역 실행
    await ppt_translator.translate_presentation(
        input_path="input.pptx",
        output_path="output_ko.pptx",
        target_language="ko",
        concurrency=5  # 동시 처리 슬라이드 수 (속도 조절)
    )

# Jupyter Notebook에서는 await main() 사용
# 일반 Python 스크립트에서는 asyncio.run(main()) 사용
if __name__ == "__main__":
    await main()
```

## 🔍 클래스 및 옵션

### AzureOpenAITranslator
비동기 Azure OpenAI 클라이언트를 래핑한 번역 엔진입니다.

- `deployment_name`: 배포된 모델명 (예: "gpt-4o")
- `max_tokens`: 응답 최대 토큰 수 (기본 4000)

### PowerPointTranslator
PPT 파일 처리를 담당하는 핸들러입니다.

#### `translate_presentation` 메서드 옵션
- `input_path`: 입력 파일 경로
- `output_path`: 저장할 파일 경로
- `target_language`: 목표 언어 코드 (`ko`, `en`, `ja`, `zh`, `es` 등)
- `concurrency`: 동시에 처리할 슬라이드 개수 (기본값: 5). 높을수록 빠르지만 API Rate Limit에 주의하세요.

## 📝 지원 언어 및 폰트 매핑

| 언어 코드 | 언어 | 자동 적용 폰트 |
|:---:|:---:|:---:|
| `ko` | 한국어 | 맑은 고딕 (Malgun Gothic) |
| `en` | 영어 | Arial |
| `ja` | 일본어 | Yu Gothic UI |
| `zh` | 중국어(간체) | Microsoft YaHei |

## ⚠️ 주의사항

1. **API Rate Limit**: `concurrency`를 너무 높게 설정하면 Azure OpenAI의 분당 토큰 제한(TPM)에 걸릴 수 있습니다. 오류 발생 시 `concurrency`를 줄이세요 (예: 5 -> 3).
2. **이미지 텍스트**: 이미지 내에 포함된 텍스트는 번역되지 않습니다. (OCR 미지원)
3. **백업 필수**: 원본 파일은 반드시 백업해두세요.

## 📜 라이선스
MIT License
