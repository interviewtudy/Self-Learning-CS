# 0과 1로 숫자를 표현하는 방법

## 정보 단위

- 비트: 0과 1을 나타내는 가장 작은 정보 단위
- n비트는 2^n 가지 정보를 표현할 수 있음

### 다양한 단위

- 바이트: 1byte == 8bit
- 메가바이트: 1MB == 1000byte
- 킬로바이트: 1kB == 1000MB
- 기가바이트: 1GB == 1000kB
- 테라바이트: 1TB == 1000GB
- 1kB 를 1024byte 로 표기하는건 잘못된 관습. 1024로 묶어 표현하는것은 KiB, MiB, GiB, TiB

### 워드

CPU 가 한 번에 처리할 수 있는 데이터 크기

- 절반을 하프워드, 1배를 풀 워드, 2배를 더블 워드라고 함
- CPU 마다 다르지만 현대 컴퓨터는 주로 32비트, 64비트의 단위를 씀

<br/><br/>

## 이진법

0과 1만으로 모든 숫자를 표현하는 방법

- 그냥 수만 사용하면 어떤 진법으로 표현했는지 알 수 없기 때문에 아래첨자로 (2)를 쓰거나 앞에 0b를 붙임

### 이진수의 음수 표현

주로 2의 보수를 구해 음수로 간주함

- 2의 보수: 어떤 수를 그보다 큰 2^n 에서 뺀 값을 의미함
- ex. 11 의 2의 보수는 100(11보다 큰 2^n) - 11 = 01
- 0과 1을 뒤집고 1을 더하면 2의 보수를 쉽게 구할 수 있음
- 이진수만 보면 음수와 양수를 구분하기 어렵기 때문에 실제로는 **플래그**를 사용함
- 0의 경우 보수를 취하면 자리 올림 비트가 생겨서 해당 비트를 버려야 함
- 2^n 형태의 이진수는 보수를 취했을 때 자신과 똑같다는 문제가 발생함

<br/><br/>

## 십육진법

0~9, A~F 를 통해 15를 넘어가는 시점에 자리 올림을 하는 숫자 표현 방식

- 이진법을 썼을 때 숫자의 길이가 너무 길어진다는 단점이 있어 사용함
- 아래첨자 (16) 또는 0x를 붙여 구분
- 아래첨자는 수학적 표현 방식, 0x 는 코드상 표기 방식임
- 십육진수 1자리 == 이진수 4자리 == 4비트 이므로 상호 변환이 쉬움

<br/><br/><br/><br/>

# 0과 1로 문자를 표현하는 방법

## 문자 집합과 인코딩

- 문자 집합: 컴퓨터가 인식하고 표현할 수 있는 문자의 모음
- 문자 인코딩: 문자를 0과 1로 변환하는 과정
- 문자 디코딩: 0과 1을 문자로 변환하는 과정

## 아스키 코드

아스키는 초창기 문자 집합 중 하나로 영어 알파벳, 아라비아 숫자, 일부 특수 문자를 포함함

- 아스키 문자 집합에 속한 문자들은 각각 7비트로 표현되고 2^7, 128 개의 문자를 표현할 수 있음
- 아스키 코드에는 백스페이스, 스페이스 같은 제어 문자도 포함되어 있음
- 코드 포인트(code point): 글자에 부여된 고유한 값(ex. 아스키 문자 A의 코드포인트는 65)
- 한글 표현 불가능
- 8비트의 **확장 아스키**가 등장하기도 했음

## EUC-KR

KS X 1001, KS X 1003 이라는 문자 집합을 기반으로 하는 완성형 인코딩 방식

- 영어권 외의 나라들이 자신들의 언어를 0과 1로 표현할 수 있게 하기 위해 등장한 인코딩 방식
- 한글 한 글자에 2바이트 코드가 부여됨
- 완성형이기 때문에 문자 집합에 정의되지 않은 한글은 표현이 불가능 했고 이를 해결하기 위해 등장한게 **CP949** 였지만 이도 충분하진 않았음

### 한글 인코딩

- 완성형 인코딩: 초성, 중성, 종성의 조합으로 이루어진 완성된 하나의 글자에 고유한 코드를 부여하는 인코딩 방식
- 조합형 인코딩: 초성, 중성, 종성을 위한 비트열을 할당해 조합으로 하나의 글자코드를 완성하는 인코딩 방식

## 유니코드와 UTF-8

모든 언어를 아우르는 문자 집합과 통일된 표준 인코딩 방식을 통해 언어별로 인코딩을하는 수고로움을 덜기 위해 등장

### 유니코드

- 현대 문자를 표현할 때 가장 많이 사용되는 표준 문자 집합
- 대부분 나라의 문자, 특수 문자, 화살표, 이모티콘까지도 표현 가능

### UTF-8

유니코드 문자에 부여된 값을 인코딩하는 방식

- 1바이트부터 4바이트의 인코딩 결과를 만들어냄
- 유니코드 문자에 부여된 값의 범위에 따라 결정됨
