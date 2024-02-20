`git stash` 명령은 다른 작업을 할 수 있도록 작업 디렉터리에 변경한 내용을 일시적으로 보관(또는 저장)하는 Git의 강력한 도구입니다. 복잡한 그림을 작업하다가 갑자기 다른 종이에 급한 내용을 적어야 할 때 `git stash`를 사용하면 미완성된 작업을 Git 저장소에 커밋할 필요 없이 코드 변경 내용을 바로 저장할 수 있습니다.

### 기본 사용법

- **변경사항 저장**: 변경 내용을 저장하려면 간단히 실행하면 됩니다:

  ```sh
  git stash
  ```

  이 명령은 작업 디렉터리와 스테이징 영역의 수정 내용을 가져와서 저장한 후 되돌리기 때문에 깨끗한 작업 디렉터리가 남게 됩니다. 마치 현재 작업 중인 도면을 안전하고 쉽게 접근할 수 있는 서랍에 넣어두고 깨끗한 작업 공간으로 돌아가는 것과 비슷합니다.

- **Stash 목록 만들기**: 저장된 변경 내용을 모두 나열할 수 있습니다:

  ```sh
  git 숨김 목록
  ```

  그러면 모든 스태시 목록이 표시되어 서랍에 있는 파일의 태그를 넘기는 것처럼 일시적으로 따로 보관한 작업을 확인할 수 있습니다.

- **Stash 적용하기**: 보관한 변경내용을 다시 가져오려면
  ```sh
  git stash apply
  ```
  이 명령은 가장 최근에 저장한 변경 내용을 작업 디렉터리에 다시 적용한다. 최신이 아닌 다른 저장소를 적용하려면 `git stash apply stash@{1}`와 같이 저장소 ID로 지정할 수 있습니다(예: `git stash list`를 통해 확인).

### 추가 옵션

- **메시지가 있는 Stash 생성하기**: 더 나은 정리를 위해 메시지로 보관함 이름을 지정할 수 있습니다:

  ```sh
  git stash save "설명 메시지"
  ```

  이렇게 하면 나중에 쉽게 식별할 수 있도록 서랍에 파일에 라벨을 붙이는 것처럼 스태쉬를 식별하는 데 도움이 됩니다.

- **추적되지 않은 파일 숨김**: 기본적으로 `git stash`는 추적된 파일에 대한 변경내용만 숨김 처리합니다. 추적되지 않은 파일(새 파일)을 stash에 포함하려면, 추적되지 않은 파일에 대해

  ```sh
  git stash -u
  ```

  또는

  ```sh
  git stash --include-untracked
  ```

  이것은 현재 도면과 새 스케치를 모두 서랍에 넣는 것과 비슷합니다.

- **보관함 열기**: 가장 최근의 숨김을 적용한 다음 숨김 목록에서 즉시 제거하려면 다음을 사용하면 됩니다:

  ```sh
  git stash pop
  ```

  이것은 서랍에서 도면을 꺼낸 다음 보관함이라고 표시된 태그를 버리는 것과 같습니다.

- **보관함 삭제하기**: 보관함이 더 이상 필요하지 않다고 판단되면 다음과 같이 삭제할 수 있습니다:
  ```sh
  git stash drop stash@{n}
  ```
  여기서 `n`은 `git stash list`에 있는 저장소의 인덱스입니다. 이렇게 하면 저장된 변경 내용을 효과적으로 버릴 수 있으므로 계속하지 않기로 결정한 스케치를 폐기하는 것과 같습니다.

`git stash`는 현재 작업을 커밋할 준비가 되지 않았지만 다른 브랜치에서 버그를 수정하는 등 컨텍스트를 전환해야 하는 상황에서 특히 유용합니다. 현재 작업의 진행 상황을 잃지 않고 작업 디렉토리를 깨끗하게 유지하는 데 도움이 됩니다.