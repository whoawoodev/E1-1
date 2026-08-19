# E1-1 · CLI 개발 환경 구축과 Docker 컨테이너 워크플로우

CLI 환경에서 터미널과 Docker의 핵심 기능을 익히고, Git/GitHub과 연동하여 코드 관리부터 컨테이너 실행까지의 전 과정을 실습한다.

- **목표** — 로컬 환경에 의존하지 않고 누구나 같은 방식으로 실행·검증할 수 있는 개발 환경을 손으로 구성한다.
- **수행 흐름** — 터미널로 작업 디렉토리·권한 정리 → Docker 점검 및 컨테이너 운영 → Dockerfile로 커스텀 이미지 제작 → 포트 매핑으로 접속 확인 → 바인드 마운트·볼륨으로 변경 반영과 데이터 영속성 검증 → Git 설정 및 GitHub 연동

<br>

## I. 디렉토리 구조

```
E1-1/
├── README.md                  # 수행 항목 요약과 문서 링크
├── docs/                      # 항목별 수행 로그 (명령어 + 실행 결과)
│   ├── 01-terminal.md
│   ├── 02-permission.md
│   ├── 03-docker.md
│   ├── 04-dockerfile.md
│   ├── 05-port.md
│   ├── 06-bind-mount.md
│   ├── 07-volume.md
│   └── 08-git-github.md
├── images/                    # 문서에서 참조하는 캡처 이미지
└── troubleshooting/           # 실습 중 발생한 문제와 해결 과정
```

수행 로그를 항목별 파일로 분리했다. 실행 결과를 잘라내지 않고 그대로 남기기 위해서다. `docker info`처럼 출력이 100줄에 가까운 명령도 있어, 한 문서에 모으면 어느 항목을 수행했는지 확인하기 어려워진다. 이 README는 목차 역할만 하고, 실제 증거는 각 문서에 있다.

<br>

## II. 실행 환경

| 항목 | 값 |
| --- | --- |
| OS | macOS 15.7.4 |
| Shell | zsh |
| 컨테이너 런타임 | OrbStack |
| Docker | 28.5.2 |
| Git | 2.53.0 |

> 학습장 iMac은 `sudo` 권한이 제한되어 Docker Desktop을 직접 설치할 수 없다. OrbStack은 관리자 권한 없이 컨테이너를 실행·관리할 수 있고, 실행 시 Docker 엔진이 함께 구동되어 터미널에서 `docker` 명령을 그대로 사용할 수 있다.

<br>

## III. 수행 항목

| 번호 | 상태 | 항목 | 문서 |
| --- | --- | --- | --- |
| 01 | ✅ | 터미널 기본 조작 (생성·복사·이동·삭제, 파일 내용 확인) | [docs/01-terminal.md](docs/01-terminal.md) |
| 02 | ✅ | 파일·디렉토리 권한 확인 및 변경 | [docs/02-permission.md](docs/02-permission.md) |
| 03 | ✅ | Docker 점검, 컨테이너 운영, 이미지·컨테이너 정리 | [docs/03-docker.md](docs/03-docker.md) |
| 04 | ✅ | Dockerfile로 커스텀 이미지 제작 | [docs/04-dockerfile.md](docs/04-dockerfile.md) |
| 05 | ✅ | 포트 매핑 및 브라우저 접속 확인 | [docs/05-port.md](docs/05-port.md) |
| 06 | ✅ | 바인드 마운트로 호스트 변경 반영 | [docs/06-bind-mount.md](docs/06-bind-mount.md) |
| 07 | ✅ | 볼륨 생성 및 데이터 영속성 검증 | [docs/07-volume.md](docs/07-volume.md) |
| 08 | ⬜ | Git 사용자 정보·기본 브랜치 설정, GitHub 연동 | [docs/08-git-github.md](docs/08-git-github.md) |

<br>

## IV. 동작 구조 설계

| 질문 | 답변 |
| --- | --- |
| 프로젝트 디렉토리 구조를 어떤 기준으로 구성했는가? | |
| 포트/볼륨 설정을 어떤 방식으로 재현 가능하게 정리했는가? | |

<br>

## V. 핵심 기술 원리

| 질문 | 관련 문서 | 답변 |
| --- | --- | --- |
| 이미지와 컨테이너의 차이를 빌드/실행/변경 관점에서 구분하면? | [03](docs/03-docker.md), [04](docs/04-dockerfile.md) | |
| 컨테이너 내부 포트로 직접 접속할 수 없는 이유와, 매핑이 필요한 이유는? | [05](docs/05-port.md) | |
| 절대 경로와 상대 경로는 어떤 상황에서 각각 선택하는가? | [06](docs/06-bind-mount.md) | |
| 파일 권한 숫자 표기(755, 644)는 어떤 규칙으로 결정되는가? | [02](docs/02-permission.md) | |

<br>

## VI. 심층 인터뷰

| 질문 | 관련 문서 | 답변 |
| --- | --- | --- |
| 호스트 포트가 이미 사용 중이라 포트 매핑이 실패한다면, 어떤 순서로 원인을 진단하는가? | [05](docs/05-port.md) | |
| 컨테이너 삭제 후 데이터가 사라지는 것을 방지하려면 어떤 대안이 있는가? | [07](docs/07-volume.md) | |
| 이 미션에서 가장 어려웠던 지점과 해결 과정(가설 → 확인 → 조치)은? | [트러블슈팅](troubleshooting/) | |

<br>

## VII. 트러블슈팅

<!-- 실습 중 발생한 문제를 troubleshooting/ 아래 문서로 작성하고 여기에 링크 -->
