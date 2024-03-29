Linux의 `chmod` (change mode) 명령어는 파일과 디렉터리의 접근 권한을 변경하는 데 사용됩니다. 권한은 사용자가 파일과 디렉터리에서 수행할 수 있는 작업을 제어합니다. 세 가지 유형의 권한이 있습니다:

- **읽기** (r): 파일의 내용을 읽거나 디렉터리를 나열할 수 있습니다.
- **쓰기** (w): 파일에 쓰거나 디렉터리에서 파일을 추가/제거할 수 있습니다.
- **실행** (x): 파일을 프로그램으로 실행하거나 디렉터리에 들어가서 그 안에서 다른 작업을 실행할 수 있습니다.

세 가지 범주의 사용자에 대해 권한이 지정됩니다:

- **소유자** (u): 파일의 소유자입니다.
- **그룹** (g): 파일 그룹의 구성원입니다.
- **기타** (o): 소유자도 그룹의 구성원도 아닌 사용자입니다.

### 기본 사용법

권한은 기호 모드(문자 `r`, `w`, `x`, `u`, `g`, `o` 및 연산자 `+`, `-`, `=` 사용) 또는 절대 모드(8진수 사용)를 사용하여 변경할 수 있습니다.

- **기호 모드**: 권한을 명시적으로 추가, 제거, 설정합니다.

  - **권한 추가**: `chmod u+w filename`은 소유자에게 쓰기 권한을 추가합니다.
  - **권한 제거**: `chmod o-r filename`은 기타 사용자의 읽기 권한을 제거합니다.
  - **권한 설정**: `chmod g=rx filename`은 그룹 권한을 읽기 및 실행만으로 설정합니다.

- **절대 (8진수) 모드**: 각 권한은 숫자로 표현됩니다: 읽기(4), 쓰기(2), 실행(1). 각 범주(소유자, 그룹, 기타)에 대해 권한은 합산되며, 세 합은 세 자리 8진수를 형성하기 위해 연결됩니다.
  - `chmod 755 filename`은 소유자의 권한을 읽기, 쓰기, 실행(7), 그룹의 권한을 읽기 및 실행(5), 기타의 권한을 읽기 및 실행(5)으로 설정합니다.

### 유용한 옵션들

- **`-R` 또는 `--recursive`**: 디렉터리와 그 내용물의 권한을 재귀적으로 변경합니다. 예를 들어, `chmod -R 755 directory`는 디렉터리와 그 안의 모든 파일의 권한을 `755`로 변경합니다.

- **`--verbose`**: 처리되는 파일을 보여주며 자세한 출력을 표시합니다. 디버깅이나 변경 사항 확인에 유용합니다.

- **`--reference=RFILE`**: 다른 파일(`RFILE`)과 동일한 권한을 설정합니다. 예를 들어, `chmod --reference=file1 file2`는 `file2`의 권한을 `file1`과 동일하게 설정합니다.

### 예시

- **소유자에게 스크립트 실행 권한 부여**: `chmod u+x script.sh`
- **소유자와 그룹에게 읽기 및 쓰기 권한 설정, 기타 사용자에게는 권한 없음**: `chmod 660 file`
- **소유자에게 모든 권한 부여 및 기타 사용자에게 읽기 및 실행 권한 부여**: `chmod 755 directory`

### 고려 사항

- 권한을 수정하려면 파일의 소유자이거나 루트 권한이 있어야 합니다.
- 권한을 설정할 때 특히 `chmod -R`을 사용할 때 주의해야 합니다. 잘못된 권한 설정은 보안 위험이나 운영 문제로 이어질 수 있습니다.
- 스크립트와 실행 파일의 경우, `x` 권한을 설정하기 전에 누가 그것들을 실행해야 하는지 고려하여 보안 문제를 피하세요.

`chmod`는 Linux에서 파일 시스템 보안 및 조직 관리를 위한 필수 명령어로, 사용자가 파일 및 디렉터리에 대한 접근을 효과적으로 제어할 수 있게 해줍니다.
