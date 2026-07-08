# PROJECT: playlist-to-mp3 현황판

> 갱신일: 2026-07-09
> 히스토리·의사결정 정본: obsidian vault `projects/playlist-to-mp3.md` (private)

## 현재 상태

- YouTube 플레이리스트/영상 → `가수 - 제목.mp3` 변환 크로스OS(macOS·Windows·Linux) 데스크톱 GUI 앱. v1.0.0, MIT.
- PySide6(Qt) GUI + yt-dlp + mutagen. uv 원클릭 설치(install.sh/ps1). 1인 단독 개발.

## 최근 완료

- 2026-07-04 크로스OS(uv/pip) 배포로 재패키징 + ffmpeg 자동 해석
- 2026-07-02 UI 폴리시: Apple-style / frameless glass / 다크·라이트 (설계 리포트 docs/archive/)

## 알려진 이슈 / 남은 일

- 미서명 macOS .app (첫 실행 시 우클릭→열기 필요)
- 포폴 등재 시 YouTube 다운로드 ToS 회색지대 고려 → GUI/패키징 역량 중심 프레이밍 (vault 노트 참조)

## 링크

- GitHub: Yeonji-Gonji/playlist-to-mp3
