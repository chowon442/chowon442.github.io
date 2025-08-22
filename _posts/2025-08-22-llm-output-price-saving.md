---
title: [Teamie] LLM Output 비용 절감하기
author: chowon
date: 2025-08-22
categories: [LLM]
tags: [llm]
---

LLM output 비용을 줄여보자.

### 기존 출력 형식

```json
{
    "lines": [
        {
            "line_number": "detailInfo_1",
            "type": 0,
            "original_content": "- 원본 줄 내용",
            "review_comment": null
        },
        {
            "line_number": "detailInfo_2",
            "type": 1,
            "original_content": "- 원본 줄 내용",
            "review_comment": "기업의 글로벌 MD 직무와 관련성이 낮은 일반적인 언어 학습 목적입니다. 글로벌 비즈니스 커뮤니케이션 역량 개발로 재구성하세요."
        }
    ]
}
```

### 개선 방식

original_content를 조회할 수 있는 별도의 조회 API를 구현하고 최종 출력 형태에서는 제거

-   LLM 출력 형태

    ```json
    {
        "lines": [
            {
                "line_number": "detailInfo_1",
                "type": 0,
                "review_comment": null
            },
            {
                "line_number": "detailInfo_2",
                "type": 1,
                "review_comment": "기업의 글로벌 MD 직무와 관련성이 낮은 일반적인 언어 학습 목적입니다. 글로벌 비즈니스 커뮤니케이션 역량 개발로 재구성하세요."
            }
        ]
    }
    ```

-   추가한 조회 API 출력 형태

    -   마스터 포트폴리오를 DB에서 가져와 줄 단위로 처리 후 반환

    ```json
    {
        "detailInfo": [
            {
                "type": "line",
                "number": 1,
                "text": "언어 학습의 한계를 넘어 실제적 표현과 문화 이해를 돕는 교류 환경 조성 목표"
            },
            {
                "type": "line",
                "number": 2,
                "text": "총 9명의 운영진(회장단 2, 기획국 3, 홍보국 2, 대외협력국 2)이 1년간 협업하여 운영"
            }
        ],
        "assignedTask": [
            { "type": "header", "text": "[동아리 운영 기획 총괄]" },
            {
                "type": "line",
                "number": 1,
                "text": "연간 활동계획 및 예산안 수립, 상반기 결산 보고 및 최종 운영보고 진행"
            },
            {
                "type": "line",
                "number": 2,
                "text": "정기모임(월 1회) 및 특별행사(분기 1회)의 전체 예산 관리 및 회계 처리 총괄"
            },
            { "type": "header", "text": "[조직 관리 및 협업]" },
            {
                "type": "line",
                "number": 3,
                "text": "기획국원 3인의 역할 분담(R&R) 및 업무 리듬 조율"
            }
        ],
        "keyAchievement": [
            {
                "type": "line",
                "number": 1,
                "text": "평균 행사 참여율 전년 대비 104.2% 달성"
            }
        ],
        "insight": [
            {
                "type": "line",
                "number": 1,
                "text": "예상치 못한 변수에 대응하기 위해 '플랜 B'를 사전에 준비하는 시스템을 구축하고, 인원 변동 등 리스크를 미리 시뮬레이션하며 위기관리 능력을 길렀다."
            }
        ]
    }
    ```

-   시스템 프롬프트 수정

    ```md
    ### 필수 준수 사항

    1. 모든 "-"로 시작하는 줄은 반드시 포함
       ...
    2. [소구분] 형태의 헤더는 첨삭 대상에서 제외. 첨삭 대상 각 줄의 원문은 내부적으로 "-"로 시작하지만, 출력 JSON에는 포함하지 않음
    ```

### 개선 결과

-   프로젝트 6개를 선택해서 AI 첨삭을 돌리면
    -   기존 방식: $0.6309
    -   개선 후: $0.5508
    -   약 13% 개선
