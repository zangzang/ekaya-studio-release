# DevVault Releases

DevVault 데스크톱 앱 다운로드 전용 레포입니다. 소스코드는 이 레포에 없으며, 별도 private 레포에서
빌드되어 여기 [Releases](../../releases)에만 게시됩니다.

## 제품

- **DevVault** — 통합 앱 (마크다운, Git 익스플로러, 칸반, 메신저, DB 클라이언트 등)
- **DevVault Design** — 이미지/아이콘 스튜디오
- **DevVault Diff** — 파일/폴더 비교 도구
- **DevVault Modeler** — ERD/데이터 모델링 도구

각 제품은 Releases 탭에서 `{product}-v{version}` 태그로 이력이 남고, 실제 배포판(설치 파일)은
해당 릴리스의 Assets에서 플랫폼에 맞는 파일을 받으면 됩니다.

- Windows: `.msi` 또는 `.exe`
- macOS: `.dmg`
- Linux: `.deb` 또는 `.AppImage`

설치된 앱은 `{product}-latest` 릴리스를 통해 자동 업데이트를 확인합니다.
