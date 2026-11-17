---
project: playlist-to-mp3
type: project-overview
---

# PROJECT: playlist-to-mp3

> 이 문서는 프로젝트 목표와 기술 스택만 담음. 진행 상황(진행 중·완료·다음 할 일·결정 대기)은 적지 않음.
> - **진행 상황·남은 일**: [GitHub Issues](https://github.com/Yeonji-Gonji/playlist-to-mp3/issues)
> - **변경 이력·버전**: `git log`·태그, 현재 버전은 `pyproject.toml` 의 `version`
> - **개발 명령·구조·릴리스 규칙**: 루트 [README](../README.md) "개발·릴리스" 절

## 목표

YouTube 플레이리스트(또는 단일 영상) URL 을 넣으면 각 항목을 `가수 - 제목.mp3` 로 변환해 태그·앨범아트까지 임베드하는 크로스 OS(macOS·Windows·Linux) 데스크톱 GUI 앱. MIT, 1인 개발.

- **공개 소개**: YouTube 다운로드는 ToS 회색지대라 공개 소개는 GUI·패키징 역량 중심으로 씀

## 기술 스택

- **언어·UI**: Python ≥ 3.9 · PySide6(Qt) · yt-dlp · mutagen · (macOS) pyobjc
- **패키징**: hatchling. 실행 진입점 `playlist-to-mp3`(= `playlist_to_mp3.app:main`)
- **설치 경로**: main 브랜치 git 소스. `uv tool install git+https://github.com/Yeonji-Gonji/playlist-to-mp3`, 원클릭 설치 `install.sh`(macOS/Linux)·`install.ps1`(Windows). 선택 경로로 GitHub Releases 의 macOS `.app` 번들(PyInstaller, 미서명)
- **외부 바이너리**: ffmpeg·ffprobe. 해석 방식은 README "ffmpeg 처리" 절과 `playlist_to_mp3/ffmpeg_setup.py` 머리 주석
- **CI**: `.github/workflows/ci.yml`(사용자가 받는 설치 경로 그대로 휠 빌드·설치·헤드리스 import 검사). main 보호 룰셋 `main-pr-ci` 가 이 검사 통과를 머지 조건으로 요구함

## 참고 문서

- **구 현황판**: [`docs/archive/status-2026-07-09.md`](archive/status-2026-07-09.md)(작성 시점 값으로 고정)
- **과거 UI 설계 리포트**: `docs/archive/report-*.md`
