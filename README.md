# Unity Skills – Marketplace

Bộ skill cho phát triển game Unity và sản xuất âm thanh.
**Mỗi mảng là một skill**: chỉ cần biết mình đang làm mảng gì.

## Cấu trúc

```
plugins/
├── unity/    17 skill – mỗi skill một mảng Unity
└── audio/     3 skill – tạo âm thanh, hậu kỳ, dựng video
```

## unity (17)

| Skill | Mảng |
|---|---|
| `unity-core` | **Bắt đầu ở đây**: kết nối Unity (REST / batch / CLI), test, build, console, định tuyến |
| `unity-scene` | GameObject, Component, Prefab, Scene |
| `unity-code` | Kiến trúc code, ScriptableObject, event, save/load, Input, asmdef; checklist thiết kế; tạo script |
| `unity-ui` | Menu, HUD, settings, uGUI / UI Toolkit |
| `unity-rendering` | URP, ánh sáng, lightmap, post-processing, material |
| `unity-shaders` | HLSL, Shader Graph, renderer feature |
| `unity-vfx` | Particle, VFX Graph, overdraw |
| `unity-animation` | Animator, root motion, IK, Timeline, Cinemachine |
| `unity-2d` | Sprite, Tilemap, 2D light, platformer |
| `unity-gameplay` | AI, NavMesh, physics, pooling, DOTS, Netcode |
| `unity-world` | Terrain, ProBuilder, level, streaming |
| `unity-game-feel` | Cảm giác chơi, juice, hit stop, shake |
| `unity-assets-build` | Import, Addressables, YooAsset, HybridCLR, build CI |
| `unity-performance` | FPS, GC, draw call, Profiler, IL2CPP |
| `unity-mobile` | Android / iOS |
| `unity-web` | WebGL |
| `unity-tween-async` | DOTween, PrimeTween, UniTask |

Mọi skill có cùng khuôn:

```
<skill>/
├── SKILL.md       Hiểu → Làm → Kiểm chứng
├── references/    kiến thức, quy trình, checklist
├── actions/       thao tác trong Editor đang mở (REST server UnitySkills)
└── scripts/       chạy headless / batch mode
```

## audio (3)

| Skill | Dùng khi nào |
|---|---|
| `audio-generate` | Nhạc, bài hát, SFX, TTS, lồng tiếng, tách stem qua Scenario MCP (ElevenLabs, ACE-Step, MiniMax, Sonilo) |
| `audio-post` | Mix, loudness, loop, crossfade, đặt tên, import Unity, AudioMixer |
| `video-assembly` | Dựng video: timeline, nối clip, nhạc/voiceover, phụ đề |

## Cài đặt

```
/plugin marketplace add datnt-cell/ClaudeUnitySkills
/plugin install unity@claude-unity-skills
/plugin install audio@claude-unity-skills
```

## Yêu cầu

| Plugin | Yêu cầu |
|---|---|
| unity | `actions/`: gói `UnitySkills` trong project (REST `localhost:8090`–`8100`), Unity 2022.3+/6000.x. `scripts/`: Unity 6.3, Python 3.9+ |
| audio | Scenario MCP đã cấu hình và cấp quyền; có credit để generate |

Marketplace chỉ chứa tài liệu, script và routing — không kèm gói Unity hay server MCP.

## Quy ước

- Tên skill và thư mục skill giữ **ASCII tiếng Anh, không đổi** — script Python tham chiếu
  thư mục anh em theo tên (`unity-core/scripts/`).
- README, `displayName`, `description` trong `plugin.json` viết tiếng Việt; thân skill giữ tiếng
  Anh để ổn định khi so khớp trigger.
- Mỗi skill có frontmatter `name` + `description`; module REST nằm ở
  `skills/<skill>/actions/<module>/SKILL.md`.

## Nguồn

- Kiến thức chuyên gia + `scripts/` của `unity-*`, `audio-generate`, `video-assembly`:
  `scenario-labs/skills` (MIT), đã đổi tên và gộp lại.
- `actions/`, `rest/`, `unity-scene`, `unity-tween-async`, `unity-game-feel`, `audio-post`:
  viết cho marketplace này (tác giả `vantv`), MIT.

Xem `plugins/<plugin>/README.md` để biết chi tiết.
