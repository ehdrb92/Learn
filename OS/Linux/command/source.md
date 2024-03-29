Linux의 `source` 명령은 현재 셸 환경에서 지정된 파일에서 명령을 읽고 실행하는 데 사용됩니다. 새 셸 프로세스를 생성하는 스크립트를 직접 실행하는 것과 달리 `source`는 기존 셸 내에서 명령을 실행합니다. 즉, 스크립트에서 변경한 환경 변수나 현재 디렉터리에 대한 모든 변경 사항은 스크립트가 완료된 후에도 유지됩니다. 이 명령은 새 셸을 시작할 필요 없이 스크립트에서 환경 변수를 설정하는 등 환경을 구성하는 데 특히 유용합니다.

### 구문

`source` 명령의 기본 구문은 다음과 같습니다:

```bash
source file [arguments]
```

또는 약식 표기법을 사용하는 것입니다:

```bash
. file [arguments]
```

### 요점

- **현재 셸 실행:** `source`는 서브셸이 아닌 현재 셸 컨텍스트에서 스크립트를 실행합니다. 즉, 스크립트의 모든 변수나 변경 사항은 현재 셸 세션에 영향을 미칩니다.
- **환경 구성:** 일반적으로 `~/.bashrc`, `~/.profile` 또는 환경 변수를 설정하는 기타 사용자 지정 스크립트 등의 구성 설정을 새로 고치거나 적용하는 데 사용됩니다.
- **스크립트 인수:** 스크립트를 소싱할 때 스크립트에 인수를 전달하면 스크립트가 직접 실행되는 것처럼 처리할 수 있습니다.

### 예제

1. **`.bashrc` 파일 소싱하기**
   셸 세션을 구성하는 스크립트인 `~/.bashrc` 파일을 변경한 경우 이를 실행하여 즉시 적용할 수 있습니다:

   ```bash
   source ~/.bashrc
   ```

   이 명령은 `.bashrc`의 내용을 다시 실행하여 정의한 새 별칭, 함수 또는 환경 변수를 적용합니다.

2. **사용자 정의 스크립트와 함께 `source` 사용하기**
   프로젝트에 여러 환경 변수를 설정하는 스크립트 `envsetup.sh`가 있다고 가정해 보겠습니다. 이러한 변수를 현재 세션에 적용하려면 다음과 같이 실행합니다:

   ```bash
   source envsetup.sh
   ```

   이렇게 하면 `envsetup.sh`에서 내보내거나 변경한 내용을 현재 셸에서 즉시 사용할 수 있습니다.

### 자주 사용하는 옵션

`source` 명령에는 다른 명령과 같은 옵션이 없습니다. 동작은 간단합니다. 현재 셸 컨텍스트 내에서 인자로 지정된 파일의 내용을 실행합니다.

셸 구성을 효율적으로 관리 및 적용하고 현재 셸 환경을 변경해야 하는 스크립팅 작업을 수행하려면 `source`의 작동 방식을 이해하는 것이 중요합니다. 특히 셸 스크립트 및 환경 구성 작업을 자주 하는 Linux 사용자에게는 기본 명령어입니다.
