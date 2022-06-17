---
title: 패밀리 추출 및 Central 정보 연계DB 구성
date: 2022-06-03
author: ideook
layout: post
---

## 테이블 (목록)

### 필수

1. 패밀리 파일 정보
2. 패밀리 유형의 전체 속성
3. 패밀리 인스턴스의 관게정보(벽, 룸, 바닥)
4. 패밀리 추출 이력

### 옵션

- 사용 이력
- 요청 이력
- 카테고리 별 기준 정보
- 패밀리 매개변수 내용

## 테이블 정의

### 패밀리 파일

- FILE_FAMILY

| 명칭        | 설명                  |
| ----------- | --------------------- |
| SEQ         | SEQ                   |
| NM_FILE     | 파일명                |
| EXT_FILE    | 확장자                |
| PATH_NAS    | 파일 경로(NAS)        |
| VER_RVT     | Revit 버전            |
| SZ_FILE     | 용량                  |
| CHK_FILE    | Checksum              |
| VER_FILE    | 업데이트버전          |
| BY_EXT      | 추출여부              |
| ID_REL_PROJ | 추출 프로젝트(Rel ID) |
| IS_USE      | 사용여부              |
| DT_CREAT    | DATE                  |

### 패밀리

- FAMILY

| 명칭        | 설명     |
| ----------- | -------- |
| SEQ         | SEQ      |
| ID_REL_FILE | ID\_파일 |
| NM_CATG     | 카테고리 |
| NM_FML      | 이름     |

### 패밀리 심볼(유형)

- SYMBOL_FAMILY

| 명칭       | 설명        |
| ---------- | ----------- |
| SEQ        | SEQ         |
| ID_REL_FML | ID\_패밀리  |
| NM_SYM     | 이름        |
| IMG_THMN   | 썸네일\_IMG |
| JSON_GEOM  | Geometries  |
| JSON_PARAM | Parameters  |

### 관계정보 (인스턴스)

- INFO_REF

| 명칭       | 설명     |
| ---------- | -------- |
| SEQ        | SEQ      |
| ID_REL_SYM | ID\_심볼 |
| TYP_REL    | 정보유형 |
| VAL_REL    | 값       |
| DTL_REL    | 값\_상세 |
| IS_USE     | 사용여부 |

### 매개변수

- PARAMS_FAMILY

| 명칭   | 설명                 |
| ------ | -------------------- |
| SEQ    | SEQ                  |
| ID_REL | ID_REL(패밀리, 심볼) |
| NM_PG  | 매개변수 그룹        |
| KEY    | 매개변수 이름        |
| VALUE  | 매개변수 값          |
| IS_USE | 사용여부             |


```sql
CREATE TABLE EKP_BIM.dbo.LoadableFamily (
	SEQ int IDENTITY(1,1) NOT NULL,
	ID nvarchar(255) COLLATE Korean_Wansung_CI_AS NULL,
	ProjectTableID nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	CategoryID nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	CategoryName nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	Name nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	[Parameter] nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	MaterialIDs nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	ModifiedDate nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	CreatedDate nvarchar(MAX) COLLATE Korean_Wansung_CI_AS NULL,
	CONSTRAINT PK__Loadable__CA1938C09E17F887 PRIMARY KEY (SEQ)
);
```

CREATE TABLE TB_FAMILY_PARAMS (
    SEQ int IDENTITY(1,1) NOT NULL,
    ID_REL int NULL,
    BuiltInParameter nvarchar(255) NULL,
    Name nvarchar(255) NULL,
    ParameterGroup nvarchar(255) NULL,
    ParameterType nvarchar(255) NULL,
    Formula nvarchar(255) NULL,
    IsProject bit NULL,
    IsInstance bit NULL,
    IsReadOnly bit NULL,
    IsReporting  bit NULL,
    IsShared bit NULL,
    StorageType nvarchar(255) NULL,
    UserModifiable bit NULL,
    PRIMARY KEY (SEQ)
);


CREATE TABLE TB_FAMILY_FILE (
SEQ int IDENTITY(1,1) NOT NULL,
NM_FILE nvarchar(255) NULL,
EXT_FILE nvarchar(10) NULL,
PATH_FILE nvarchar(255) NULL,
VER_RVT nvarchar(255) NULL,
SZ_FILE nvarchar(255) NULL,
CHK_FILE nvarchar(255) NULL,
BY_EXT nvarchar(255) NULL,
ID_REL_PROJ int NULL,
IS_USE bit NULL,
DT_CREAT datetime NULL,
    PRIMARY KEY (SEQ)
);


CREATE TABLE TB_FAMILY (
SEQ int IDENTITY(1,1) NOT NULL,
ID_REL_FILE int NULL,
NM_CATG nvarchar(255) NULL,
NM_FML nvarchar(255) NULL,
    PRIMARY KEY (SEQ)
);
