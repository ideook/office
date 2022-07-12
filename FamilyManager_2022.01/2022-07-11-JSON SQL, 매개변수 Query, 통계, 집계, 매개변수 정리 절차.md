---
title: JSON SQL, 매개변수 Query, 통계, 집계, 매개변수 정리 절차
date: 2022-07-11
author: ideook
layout: post
---

### JSON QUERY

```sql
SELECT
	A.SEQ,
	BuiltInParameter,
	Name,
	ParameterGroup,
	ParameterType,
	Formula,
	IsProject,
    IsInstance,
    IsReadOnly,
    IsReporting,
    IsShared,
    StorageType,
    UserModifiable
FROM
	TB_FAMILY_PARAM A
CROSS APPLY OPENJSON(A.JSON_PARAM, '$')
WITH (
    BuiltInParameter NVARCHAR(100) '$.BuiltInParameter',
    Name NVARCHAR(100) '$.Name',
    ParameterGroup NVARCHAR(100) '$.ParameterGroup',
    ParameterType NVARCHAR(100) '$.ParameterType',
    Formula NVARCHAR(500) '$.Formula',
    IsProject bit '$.IsProject',
    IsInstance bit '$.IsInstance',
    IsReadOnly bit '$.IsReadOnly',
    IsReporting bit '$.IsReporting',
    IsShared bit '$.IsShared',
    StorageType NVARCHAR(30) '$.StorageType',
    UserModifiable bit '$.UserModifiable'
) As PARAM
```

## 매개변수 관리

### 매개변수 기준

- 작성 규칙 확인
- 기본 매개변수 확인 및 입력

### 매개변수 관리

- 추출할때 기록하고 추출
- 내려 보낼때 기록하고 전송
- 읽기전용 False 삭제
- 인스턴스, 프로젝트 삭제
- 기준 정보를 DB로 구축 (김승배)
- 추출 또는 내려보낼때 기준정보가 있는지 검토하거나 추가해주자

## 통계, 집계

매개변수를 조회하여 Insight를 뽑아보자.

- 많이 사용되는 매개변수
- 기본 매개변수는 어떤것이 있는지
- 프로젝트 속성으로 어떤것이 많이 입력되는지
- 인스턴스 속성으로 어떤것이 많이 입력되는지
- 중복되는 매개변수는 뭐가 있는지