# 어금니 팀의 프로젝트
## Project 진행에 앞서... 
- <구성원 소개>
  - 이유진

  - 이아진

  - 이현준

  - 임수진

  - 엄세영
- <보실 프로젝트 방향성>
  - 아이디어 과정
  - 진행 방향
  - 역할 분담
  - 회의
## 특징
- 간단한 런처로 가장 인기 있는 대규모 언어 모델 제공
- 여러 GPU에서 더 빠른 추론을 위한 Tensor Parallelism
- SSE(Server-Sent Events)를 사용한 토큰 스트리밍
- 총 처리량 증가를 위한 수신 요청의 동적 일괄 처리
- 비트샌드바이트를 사용한 양자화
- 금고 중량 적재
- 대형 언어 모델용 워터마크를 사용한 워터마크
- Logits 워퍼(온도 스케일링, topk, 반복 페널티 ...)
- 중지 시퀀스
- 로그 확률
- 프로덕션 준비(Open Telemetry, Prometheus 메트릭을 사용한 분산 추적)

## 공식적으로 지원되는 아키텍처
- BLOOM
- BLOOMZ
- MT0-XXL
- Galactica
- SantaCoder
- GPT-Neox 20B
- FLAN-T5-XXL
- FLAN-UL2

다른 아키텍처는 다음을 사용하여 최선을 다해 지원됩니다.

```
AutoModelForCausalLM.from_pretrained(<model>, device_map="auto")

or

AutoModelForSeq2SeqLM.from_pretrained(<model>, device_map="auto")
```

## 시작

### Docker
시작하는 가장 쉬운 방법은 공식 Docker 컨테이너를 사용하는 것입니다.

```
model=bigscience/bloom-560m
num_shard=2
volume=$PWD/data # share a volume with the Docker container to avoid downloading weights every run

docker run --gpus all --shm-size 1g -p 8080:80 -v $volume:/data ghcr.io/huggingface/text-generation-inference:latest --model-id $model --num-shard $num_shard
```

그런 다음 /generate 또는 /generate_stream 경로를 사용하여 모델을 쿼리할 수 있습니다.

```
curl 127.0.0.1:8080/generate \
    -X POST \
    -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":17}}' \
    -H 'Content-Type: application/json'
```

```
curl 127.0.0.1:8080/generate_stream \
    -X POST \
    -d '{"inputs":"What is Deep Learning?","parameters":{"max_new_tokens":17}}' \
    -H 'Content-Type: application/json'
```

또는 파이썬에서:

`pip install text-generation`
```
from text_generation import Client

client = Client("http://127.0.0.1:8080")
print(client.generate("What is Deep Learning?", max_new_tokens=17).generated_text)

text = ""
for response in client.generate_stream("What is Deep Learning?", max_new_tokens=17):
    if not response.token.special:
        text += response.token.text
print(text)
```

## 마치며...

- 개발
  - 이유진

  - 이아진

  - 이현준

  - 임수진

  - 엄세영

- 테스트

  - 이아진

  - 이현준




![대체 텍스트](https://post-phinf.pstatic.net/MjAyMDExMTNfMTE1/MDAxNjA1MjI3Njc0MjE2.eKvPw2gXT-jKOxeQBIvr5BcqxGxY1Vq6tBTGpRnpn-Mg.xLodY3QaTQIfKZGE-WbDQC8PpRSwFIRHwE1eFTROzjEg.JPEG/%EB%B3%80%ED%99%98%EB%8F%84%EB%A7%9D%EA%B0%80%EB%8A%94_%EC%96%B4%EA%B8%88%EB%8B%88.jpg?type=w1200)
