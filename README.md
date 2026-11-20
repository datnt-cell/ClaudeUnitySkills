# Unity Skills – Marketplace

Bộ skill cho phát triển game Unity: tự động hoá Editor, kiến thức chuyên gia
Unity 6.3, thiết kế cảm giác chơi và sản xuất âm thanh.
Mỗi nhóm là một plugin độc lập — thêm marketplace một lần, cài nhóm nào cần.

## Cấu trúc

```
plugins/
├── 01-unity-editor-automation/   12 skill – gọi Unity Editor qua REST
├── 02-unity-expert-6-3/          14 skill – chuyên gia Unity 6.3 (đo lường)
├── 03-game-feel/                  1 skill – cảm giác chơi & juice
└── 04-audio-production/           8 skill – tạo/cắt/ghép/trộn âm thanh
```

Mỗi nhóm có `README.md` tiếng Việt mô tả chi tiết từng skill. Tổng: **35 skill**.

## 01 · Tự động hoá Editor (12)

Gọi Unity Editor từ chat qua REST server của gói `UnitySkills`
(`localhost:8090`–`8100`, Unity 2022.3+/6000.x).

| Skill | Làm được gì |
|---|---|
| `unity-core` | Sổ tay giao thức: discovery, chế độ, dryRun, batch, mã lỗi |
| `unity-scene-authoring` | GameObject, Component, Prefab, Scene |
| `unity-rendering` | Material, Light, Shader Graph, Volume, URP, Decal |
| `unity-ui` | UGUI, UI Toolkit |
| `unity-scripting` | Tạo/sửa Script, ScriptableObject |
| `unity-assets-pipeline` | Import, dọn dẹp, tối ưu, package, validation |
| `unity-animation-timeline` | Animator, Timeline, Cinemachine, camera |
| `unity-physics-navigation` | Physics, NavMesh, Terrain, ProBuilder |
| `unity-addressables-yooasset` | Addressables, YooAsset, HybridCLR |
| `unity-tweening` | DOTween, PrimeTween, UniTask |
| `unity-dev-workflow` | Batch job, debug/console, profiler, history, Test Runner |
| `unity-editor-tools` | Sửa YAML tay, Unity CLI, behavior graph |

## 02 · Chuyên gia Unity 6.3 (14)

Mỗi quy trình kèm số liệu, test và ảnh chụp. Có toolkit Python `ut_*` + AgentKit C#.

| Skill | Dùng khi nào |
|---|---|
| `scenario-unity-expert` | Skill dẫn: batch mode, editor đang chạy, Unity CLI, Test Framework |
| `scenario-unity-2d` | Sprite/PPU, atlas, Tilemap, 2D light, platformer, camera, shake |
| `scenario-unity-animation` | Mixamo, Animator, root motion, IK, Timeline, Cinemachine 3 |
| `scenario-unity-architecture` | SO vs runtime state, event, save/load, asmdef, Git + checklist thiết kế (pattern, ADR, blueprint, script review) |
| `scenario-unity-gameplay` | AI, NavMesh, state machine, projectile, pooling, DOTS, Netcode |
| `scenario-unity-mobile` | Android/iOS: budget, texture, safe area, AAB, IAP, ads |
| `scenario-unity-performance` | FPS, spike, GC, draw call, Profiler, LOD, shader warm-up |
| `scenario-unity-pipeline-automation` | Import rule, sinh prefab, Addressables, CI |
| `scenario-unity-rendering-lighting` | URP, lightmap, probe, tonemapping, TAA/STP |
| `scenario-unity-shaders` | HLSL, Shader Graph: nước, toon, dissolve, outline |
| `scenario-unity-ui` | Menu, settings, HUD, minimap, rebind phím |
| `scenario-unity-vfx` | Nổ, đạn, muzzle/trail/impact, VFX Graph, overdraw |
| `scenario-unity-web` | WebGL: Build Profiles, nén, hosting |
| `scenario-unity-world-building` | Terrain, đảo procedural, erosion, foliage |

> **Bắt buộc giữ nguyên cụm:** các skill chuyên gia import `scripts/` của
> `scenario-unity-expert` theo thư mục anh em. Không cài lẻ, không đổi tên thư mục.

## 01 hay 02? (phân vai)

Hai nhóm phủ cùng chủ đề nhưng khác vai trò:

- **02 quyết định, chẩn đoán, chứng minh**: chọn cách làm, sửa lỗi "trông sai / chạy sai", đo số liệu.
- **01 thao tác ngay trong Editor đang mở** (qua REST server UnitySkills): thêm light, đổi
  material, tạo Canvas, ghi script…

| Chủ đề | 02 (biết) | 01 (làm) |
|---|---|---|
| UI | `scenario-unity-ui` | `unity-ui` |
| Ánh sáng, URP, post | `scenario-unity-rendering-lighting` | `unity-rendering` |
| Shader | `scenario-unity-shaders` | `unity-rendering` |
| Animation, camera | `scenario-unity-animation` | `unity-animation-timeline` |
| Physics, NavMesh, AI | `scenario-unity-gameplay` | `unity-physics-navigation` |
| Terrain, level | `scenario-unity-world-building` | `unity-physics-navigation` |
| Import, Addressables, build | `scenario-unity-pipeline-automation` | `unity-assets-pipeline`, `unity-addressables-yooasset` |
| Hiệu năng | `scenario-unity-performance` | `unity-dev-workflow` |
| Kiến trúc code | `scenario-unity-architecture` | `unity-scripting` |

Description của mỗi cặp đều ghi rõ "không dùng khi… → dùng skill kia" để Claude chọn đúng.

## 03 · Game Feel & Juice (1)

| Skill | Dùng khi nào |
|---|---|
| `unity-game-feel` | Nhảy lơ lửng, đánh không lực, bấm không ăn, camera mất nhân vật; thêm hit stop, shake, kick, squash & stretch; rubric chấm cảm giác |

Kèm 2 module: `feel-recipes` (code C#) và `feel-audit` (rubric A–G).

## 04 · Sản xuất âm thanh (8)

Tạo, biến đổi, cắt, tách, ghép, trộn — qua Scenario MCP + playbook local.

| Skill | Dùng khi nào |
|---|---|
| `scenario` | Kết nối MCP và vòng lặp generate cơ bản |
| `scenario-audio` | Router âm thanh + tiện ích cắt/tách/xuất/compose |
| `scenario-elevenlabs` | TTS, lồng tiếng, đổi giọng, tách giọng, music/SFX |
| `scenario-sonilo` | Text→SFX/music, video→SFX/music, chấm nhạc cho clip |
| `scenario-ace-step` | Bài hát, cover, repaint, thêm layer, tách stem |
| `scenario-minimax-music` | Bài hát có cấu trúc, tự viết lyric, instrumental |
| `scenario-video-assembly` | Timeline, nối clip, lớp nhạc/voiceover, phụ đề |
| `audio-post-production` | Mix, loudness, loop, crossfade, đặt tên, import Unity, AudioMixer |

## Cài đặt

```
/plugin marketplace add vantv/ClaudeUnitySkills
/plugin install 01-unity-editor-automation@claude-unity-skills
/plugin install 02-unity-expert-6-3@claude-unity-skills
/plugin install 03-game-feel@claude-unity-skills
/plugin install 04-audio-production@claude-unity-skills
```

(Thay `vantv/ClaudeUnitySkills` bằng remote thật, hoặc dùng đường dẫn local.)

## Yêu cầu

| Nhóm | Yêu cầu |
|---|---|
| 01 | Gói `UnitySkills` trong project Unity, REST server `localhost:8090`–`8100` |
| 02 | Unity 6.3 (6000.3.x), Python 3.9+ cho toolkit |
| 03 | Không cần gì; đọc kèm 02 để lấy code cảm giác đã test |
| 04 | Scenario MCP đã cấu hình và cấp quyền; có credit để generate |

Marketplace này chỉ chứa tài liệu và routing — không kèm gói Unity hay server MCP.

## Quy ước

- Tên thư mục plugin có số thứ tự (`01-…` → `04-…`) để luôn sắp đúng thứ tự sử dụng.
- Tên skill và tên thư mục skill giữ **ASCII, không đổi** — code Python và
  frontmatter tham chiếu theo tên này.
- Mô tả (README, `displayName`, `description` trong `plugin.json`) viết bằng
  tiếng Việt; phần thân skill giữ tiếng Anh để ổn định khi so khớp trigger.
- Mỗi skill có frontmatter `name` + `description`; module nằm ở
  `skills/<skill>/modules/<module>/SKILL.md`.

## Nguồn

- 02: `scenario-labs/skills` → `skills/game-engines/unity` (MIT), port từ `edemaistre/unity-expert-skills`.
- 04: `scenario-labs/skills` các skill `scenario*` (MIT).
- 01: viết cho marketplace này.
- 03 và skill `audio-post-production`: viết cho marketplace này, dựa trên số liệu của 02.

Xem `plugins/<nhóm>/README.md` để biết chi tiết từng skill.
