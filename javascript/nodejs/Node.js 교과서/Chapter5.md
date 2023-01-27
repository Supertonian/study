# 5. 패키지 매니저

npm init으로 패키지 시작 → package.json 생성

npx를 통해 글로벌하게 설치한 패키지를 실행(execute) 할 수 있음.

package-lock.json 은 왜 쓸까?

→ 버전을 정확하게 기록

버전 방식

major, minor, patch

^은 첫번 째 자리만 일치하는 지 확인 (major)

→ 보통 ^를 많이 씀. 그 이유는 두번째 버전 업데이트하는 것 자체가 기존 사람에게 영향을 안가는 업데이트이기 때문에. 두번째나 세번쨰를 체크하는 이유가 없음.

→ 단지 이 버전 올리는 방식을 사용하지 않는 사람 패키지의 경우 보장되지 않기 때문에 주의는 해야함.

npm version 명령어를 통해 package.json 버전을 올릴 수 있는데 이걸 하면 장점이 git commit과 git tag도 자동으로 된다.
