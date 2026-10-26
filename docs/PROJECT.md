---
project: playlist-to-mp3
type: project-status
status: 운영
updated: 2026-10-26
---

# PROJECT: playlist-to-mp3 온보딩 시트

> **이 문서는 리포 온보딩·아키텍처·고정 함정 시트다.**
> - **배포 버전 기준**: `pyproject.toml` 의 `version` + `git log`(설치는 main 브랜치 git 소스를 직접 받는다)
> - **남은 일·결정 필요**: GitHub Issues ("허브 기준 점검 2026-10-26" 이슈가 보강 체크리스트)
> - 2026-07-09 이전 현황판은 [archive/status-2026-07-09.md](archive/status-2026-07-09.md)(작성 시점 값으로 고정)

## 요약

YouTube 플레이리스트(또는 단일 영상) URL 을 넣으면 각 항목을 `가수 - 제목.mp3` 로 변환해 태그·앨범아트까지 임베드하는
크로스 OS(macOS·Windows·Linux) 데스크톱 GUI 앱. v1.0.1, MIT. 1인 개발.

## 핵심 사양

- **스택**: Python ≥ 3.9 · PySide6(Qt) · yt-dlp · mutagen · (macOS) pyobjc
- **패키징**: hatchling, `uv tool install git+https://github.com/Yeonji-Gonji/playlist-to-mp3`. 원클릭 설치 = `install.sh`(macOS/Linux)·`install.ps1`(Windows)
- **실행 진입점**: `playlist-to-mp3` (= `playlist_to_mp3.app:main`)
- **외부 바이너리**: ffmpeg·ffprobe. `playlist_to_mp3/ffmpeg_setup.py` 가 PATH → 번들(`vendor/`) → brew 순으로 해석
- **CI**: `.github/workflows/ci.yml` = ruff → 휠 빌드·폰트 포함 확인 → 새 가상환경 설치 → 헤드리스 import·실행 파일 확인. 사용자가 받는 설치 경로 그대로다.
  main 보호 룰셋 `main-pr-ci`(PR 필수 + `verify` 통과)로 CI 를 통과하지 않은 커밋은 main(= 설치 소스)에 올라가지 않는다. GUI 동작은 수동 확인

## 배포 · 환경 구조 (고정 규칙 및 함정)

- **배포 방식**: 별도 릴리스 없이 main 브랜치가 곧 설치 소스다. `uv tool upgrade playlist-to-mp3` 가 main 최신을 받으므로
  **main 에 깨진 커밋이 올라가면 모든 사용자의 다음 업그레이드가 깨진다**. 동작 확인 없이 main 에 올리지 않는다.
- **버전**: `pyproject.toml` 의 `version` 과 `playlist_to_mp3/__init__.py` 의 `__version__` 을 함께 올린다. 머지 후 그 커밋에 `v<버전>` 태그를 단다(설치 기준은 main 유지, 태그는 기록용, 2026-10-26 결정).
- **운영·검증 함정**
  - macOS `.app`(PyInstaller 번들)은 미서명이라 첫 실행 시 우클릭 → 열기 가 필요하다. 기본 배포 경로는 uv 이고 번들은 선택이다. 유료 서명(Apple Developer)은 하지 않는다(2026-10-26).
  - 폰트(`playlist_to_mp3/fonts/*.ttf`)는 패키지 폴더 안에 있어 휠에 자동 포함된다. `force-include` 로 다시 넣으면 최신 hatchling 이 중복 경로로 빌드를 거부한다(2026-10-26 설치 장애, v1.0.1 에서 수정). CI 가 휠에 폰트가 있는지 검사한다.
  - YouTube 다운로드는 ToS 회색지대다. 공개 소개는 GUI·패키징 역량 중심으로 쓴다(2026-10-26 확정).
  - 변경 후 최소 확인: `uv run playlist-to-mp3` 로 창이 뜨고, 짧은 공개 영상 1개를 변환해 태그·앨범아트가 박히는지 본다.

## 아키텍처 · 기능 모듈 지도

| 영역 | 경로 / 모듈 | 역할 및 고정 규칙 |
| --- | --- | --- |
| GUI·변환 파이프라인 | `playlist_to_mp3/app.py` | Qt 창, URL 입력 → yt-dlp 다운로드 → mutagen 태그 임베드. 다크·라이트, frameless glass UI |
| ffmpeg 해석 | `playlist_to_mp3/ffmpeg_setup.py` | ffmpeg/ffprobe 위치 탐색·안내 |
| 패키징 | `pyproject.toml`, `install.sh`, `install.ps1`, `Playlist to MP3.spec` | uv 설치 스크립트, 선택적 PyInstaller 번들 |
| 아이콘 | `icon.iconset/`, `icon.icns`, `icon_1024.png`, `make_icon.py` | macOS 아이콘 생성 |

## 참고 / 외부 링크

- 이슈 추적: GitHub Issues
- 과거 UI 설계 리포트: `docs/archive/report-*.md`
