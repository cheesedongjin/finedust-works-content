# FineDust Works — 프로젝트 콘텐츠

finedustworks.com의 **Projects** 페이지와 홈의 **Lab** 목록은 이 폴더의 JSON을 실행 중에 불러와 그립니다.
파일을 고치고 커밋하면 사이트를 다시 배포하지 않아도 몇 분 안에 반영됩니다 (GitHub raw 캐시 약 5분).

```
site.json                      카테고리 6개와 공개할 프로젝트 순서
projects/<id>/project.json     프로젝트 하나의 모든 내용
projects/<id>/media/           그 프로젝트의 이미지·GIF·영상
```

## 프로젝트 추가하기

1. `projects/<id>/` 폴더를 만들고 `project.json`을 씁니다. `<id>`는 소문자·숫자·하이픈만 씁니다 (URL에 들어갑니다).
2. 미디어 파일은 `projects/<id>/media/`에 넣고 `"media/파일명"`처럼 상대 경로로 적습니다. 외부 URL도 됩니다.
3. `site.json`의 `projects` 배열에 `<id>`를 추가합니다. 배열 순서가 목록 순서입니다. 배열에서 빼면 비공개가 됩니다.
4. 상세 페이지 주소는 `https://finedustworks.com/projects/?id=<id>`입니다.

새로 만들 때는 `projects/fdpt/project.json`을 복사해서 시작하세요.

## 올리기 전에 미리 보기

사이트 저장소의 `content/` 폴더에 이 저장소를 클론해 두면(사이트 저장소에서는 git이 무시합니다), 로컬 서버에서 `?content=local`을 붙여 푸시 전의 내용을 볼 수 있습니다.

```
http://localhost:8417/projects/?id=fdpt&content=local
```

## project.json 필드

| 필드 | 필수 | 설명 |
|---|---|---|
| `id` | ✓ | 폴더 이름과 같게 |
| `name` | ✓ | 짧은 이름 (`FdPT`) |
| `fullName` |  | 긴 이름 (`FineDust Presentation Tool`) |
| `category` | ✓ | `robotics` · `devices` · `tools` · `agents` · `software` · `games` 중 하나 |
| `status` | ✓ | `idea` · `prototype` · `in-progress` · `beta` · `live` · `paused` · `archived` |
| `version`, `updated` |  | `0.1.0`, `YYYY-MM-DD` |
| `tagline` | ✓ | 한 줄 소개 |
| `summary` | ✓ | 두세 문장 설명 |
| `tags`, `stack` |  | 문자열 배열 |
| `cover` |  | `{ "src", "alt" }` 대표 이미지 |
| `links` |  | `[{ "label", "url", "primary"? }]` — `primary`는 하나만 |
| `github` |  | GitHub 저장소 URL. 입력한 프로젝트만 상세 페이지 상단에 GitHub 버튼 표시 |
| `overview` | ✓ | `concept`, `motivation`, `highlights[]` |
| `features` | ✓ | `[{ title, summary, media?, how, useCase, edge }]` |
| `architecture` | ✓ | `intro`, `stack[]`, `diagram?`, `flow[]`, `decisions[]`, `algorithms[]` |
| `development` | ✓ | `intro`, `timeline[]`, `challenges[]`, `abandoned[]`, `lessons[]` |
| `changelog` | ✓ | `releases[]`, `roadmap[]` |
| `extras` |  | 추가 탭. 아래 참고 |

비어 있는 항목은 화면에서 자동으로 숨겨집니다. 다섯 개 기본 탭(Overview · Features · Architecture · Development · Changelog)은 항상 같은 순서로 나옵니다.

### 미디어 (`features[].media`)

```json
{ "type": "image", "src": "media/a.webp", "alt": "...", "caption": "..." }
{ "type": "video", "src": "media/a.mp4",  "poster": "media/a.webp", "alt": "...", "caption": "..." }
{ "type": "embed", "src": "https://www.youtube.com/embed/...", "alt": "...", "caption": "..." }
```

GIF는 `image`로 넣습니다. 짧은 동작 영상은 GIF보다 `mp4`(음소거 자동 반복 재생)가 훨씬 가볍습니다.

### 아키텍처 다이어그램 (`architecture.diagram`)

`groups`는 왼쪽부터 오른쪽으로 놓이는 열, `nodes`는 그 안의 상자입니다. `edges`는 `[from, to, "라벨"?]`입니다.

### 추가 탭 (`extras`)

추가 탭의 제목(`extras[].title`)은 기본 탭과 일관되게 영어로 작성합니다. 예: `Playground`, `Gallery`, `Downloads`, `Runtime & Limitations`. 탭 안의 소제목과 본문은 기존 한국어 글쓰기 규칙을 따릅니다.

| type | 내용 | 필드 |
|---|---|---|
| `playground` | 직접 조작하는 데모 (iframe) | `title`, `intro`, `src`, `aspect`(`"16 / 10"`), `note` |
| `gallery` | 이미지 모음, 클릭하면 크게 보기 | `title`, `items[{ src, alt, caption }]` |
| `downloads` | 파일·소스·문서 | `title`, `items[{ label, url, note }]` |
| `notes` | 자유 서술 (Deep Dive, Retrospective 등) | `title`, `sections[{ heading, body }]` |

## 글쓰기 규칙

모든 프로젝트가 같은 목소리로 읽혀야 합니다.

- **문장은 합니다체.** 설명, 동기, 개발 과정처럼 문장으로 쓰는 칸은 모두 `~합니다`로 끝냅니다.
- **목록은 명사형.** `highlights`, 변경 기록, 사용 사례는 마침표 없이 명사로 끝냅니다. 예: `3D 모델 요소`, `저장소를 IndexedDB로 전환`
- **주어는 프로젝트.** "저는", "우리는" 대신 프로젝트나 기능을 주어로 씁니다.
- **수치는 구체적으로.** "빠르다" 대신 "약 1 MB", "10초에 30회"처럼 씁니다.
- **과장하지 않기.** "혁신적인", "최고의", "완벽한" 같은 말은 쓰지 않습니다.
- **기능 하나 = 세 칸.** `how`(실제 동작), `useCase`(언제 쓰는지), `edge`(무엇이 다른지)를 모두 채웁니다.
- **변경 기록 type**은 `added` · `changed` · `fixed` · `removed` 네 가지만 씁니다.
- **로드맵 status**는 `next` · `later` · `exploring` 세 가지만 씁니다.

### 텍스트 서식

문장 칸에서 아래 서식을 쓸 수 있습니다. 빈 줄(`\n\n`)은 문단을 나눕니다.

- `**굵게**`
- `` `코드` ``
- `[링크](https://...)`
