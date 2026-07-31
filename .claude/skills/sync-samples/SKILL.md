---
name: sync-samples
description: dev-vault(private)의 앱 샘플 파일을 dev-vault-releases(public)의 apps/<app>/samples/ 로 옮기고 README를 갱신한다. "샘플 동기화", "샘플 업데이트", "새 샘플 올려줘"에 반응한다. 공개 레포로 파일을 내보내므로 push 전 반드시 사용자 승인을 받는다.
---

# 샘플 동기화 (private → public)

원본은 항상 `../dev-vault/docs/apps/<app>/samples/` 이고, 이 레포는 **사본만** 둔다.
반대 방향으로 편집하지 않는다 — 여기서 고친 샘플은 다음 동기화에서 덮여 사라진다.

## 대응표

| 원본 (dev-vault) | 사본 (이 레포) |
|---|---|
| `docs/apps/wbs/samples/` | `apps/wbs/samples/` |
| `docs/apps/data-modeler/samples/` | `apps/data-modeler/samples/` |

앱이 늘면 이 표에 한 줄 추가하고, `apps/<app>/README.md`와 `apps/README.md` 표에도 넣는다.

## 1. 차이를 먼저 본다

```bash
for app in wbs data-modeler; do
  echo "=== $app"
  diff -rq "../dev-vault/docs/apps/$app/samples" "apps/$app/samples"
done
```

`Only in ../dev-vault/...`(신규) · `differ`(변경) · `Only in apps/...`(원본에서 삭제됨)를
**분류해서 사용자에게 먼저 보여준다.** 삭제는 특히 확인받는다.

## 2. 공개해도 되는지 본다 (건너뛰지 않는다)

공개 레포로 나가는 파일이다. 새로 올리거나 바뀐 샘플은 **내용을 직접 열어** 확인한다.

- 실명·사내 조직명·실제 고객사명·이메일·사번·URL·자격증명이 들어 있지 않은가
- 미완성 스크래치 파일이 아닌가 — 엔티티가 1개뿐이거나 용어/단어 수에 비해
  모델이 비어 있으면 작업 중 파일일 가능성이 높다. 사용자에게 물어보고 뺀다.
- `_history`(변경 이력)는 **그 자체가 보여줄 기능이므로 지우지 않는다.** 파일이 커지는
  것은 감수한다. 크기를 줄이려고 키를 임의로 빼지 않는다.

걸리는 게 있으면 복사하지 말고 멈춰서 묻는다.

## 3. 복사한다

```bash
cp ../dev-vault/docs/apps/<app>/samples/<파일> apps/<app>/samples/
```

원본에서 사라진 파일은 사용자 확인 후 `git rm` 한다.

## 4. README를 맞춘다

파일만 바꾸고 README를 두면 표와 실제가 어긋난다. 반드시 같은 커밋에서 고친다.

- `apps/<app>/README.md` — 샘플 표(파일명·규모·무엇을 보여주는가). 규모 숫자는 세서 쓴다:
  ```bash
  python3 -c "
  import json,sys
  d=json.load(open(sys.argv[1]))
  print({k: len(v) for k,v in d.items() if isinstance(v,list)})
  " apps/<app>/samples/<파일>
  ```
  설명은 파일의 `meta.description`이 있으면 그것을 근거로 쓰고, 없으면 내용을 보고 쓴다.
  **추측하지 않는다.**
- `apps/README.md` — 앱별 샘플 개수
- 앱이 새로 늘었으면 루트 `README.md`의 샘플 목록에도 추가

## 5. 커밋 · push

```bash
git add apps && git status --short
git commit -m "chore(samples): <앱> 샘플 동기화"
```

**push는 사용자 승인을 받고 한다.** public 레포이므로 push 즉시 공개되고,
잘못 올린 파일은 커밋을 지워도 GitHub 캐시에 남을 수 있다.
