# 한국어 토크나이저 비교 실험

국방 도메인 한국어 텍스트를 활용한 BPE, WordPiece, 형태소 분석기 비교 평가 프로젝트입니다.

## 배경

전 직장(국방 데이터 분석 연구원) 경험을 바탕으로, 공공데이터에서 제공하는 국방일보 PDF를 수집해 한국어 NLP 토크나이저 성능을 비교합니다.

## 데이터

- **출처**: 국방일보 (2024.05 ~ 2025.04, 월별 PDF 12개)
- **수집 방법**: PyMuPDF로 PDF 단어 단위 추출 (공백 복원)
- **총 용량**: 약 29.7 MB

## 실험 항목

| 토크나이저 | 유형 | 학습 여부 | vocab_size |
|-----------|------|---------|-----------|
| BPE | 서브워드 | ✅ | 7,000 |
| WordPiece | 서브워드 | ✅ | 12,000 |
| Mecab | 형태소 분석기 | ❌ (비교만) | - |
| Okt | 형태소 분석기 | ❌ (비교만) | - |
| Kkma | 형태소 분석기 | ❌ (비교만) | - |

> BPE와 WordPiece의 vocab_size가 다른 이유: WordPiece는 연속 서브워드에 `##` 접두사를 붙여 별개 토큰으로 저장하므로 동일 코퍼스에서 약 2배의 vocab이 필요합니다.

**평가 지표**: 어휘 크기 / OOV 비율 / Fertility / 토크나이징 속도

**평가 조건**: 학습 데이터 10% / 50% / 100% 크기별 성능 비교

## 주요 결과

| 모델 | 어휘 크기 | OOV (%) | Fertility | 속도 (k tok/s) |
|------|---------|---------|-----------|--------------|
| BPE (10%) | 7,000 | 0.0 | 7.966 | 1,117.8 |
| BPE (50%) | 7,000 | 0.0 | 8.866 | 1,333.5 |
| BPE (100%) | 7,000 | 0.0 | 10.235 | 811.9 |
| WordPiece (10%) | 12,000 | 0.0 | 7.739 | 1,108.0 |
| WordPiece (50%) | 12,000 | 0.0 | 8.765 | 873.4 |
| WordPiece (100%) | 12,000 | 0.0 | 10.639 | 388.7 |
| Mecab | - | - | 8.212 | 4.7 |
| Okt | - | - | 6.991 | 0.7 |
| Kkma | - | - | 9.180 | 0.4 |

자세한 분석은 [report.md](report.md)를 참고하세요.

## 프로젝트 구조

```
nlp_leedonghoon/
├── main.ipynb                  # 실험 노트북
├── report.md                   # 실험 결과 리포트
├── README.md
├── data/
│   ├── kookbang/               # 원본 PDF 파일 (12개)
│   ├── kookbang_corpus.txt     # 추출된 전체 텍스트 (~29.7MB)
│   ├── eval_set.txt            # 평가셋
│   └── splits/
│       ├── train_10%.txt
│       ├── train_50%.txt
│       └── train_100%.txt
├── models/
│   ├── bpe_10%.json
│   ├── bpe_50%.json
│   ├── bpe_100%.json
│   ├── wordpiece_10%.json
│   ├── wordpiece_50%.json
│   └── wordpiece_100%.json
└── tokenizer_comparison.png
```

## 실행 방법

```bash
pip install pymupdf tokenizers konlpy python-mecab-ko matplotlib pandas
```

`main.ipynb`를 Jupyter에서 처음부터 순서대로 실행합니다.

**참고**: Kkma는 Java가 필요합니다 (`brew install --cask temurin`)
