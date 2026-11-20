# Unity – bộ skill theo mảng

**Mỗi mảng là một skill.** Bạn chỉ cần tự hỏi "mình đang làm mảng gì?" rồi chọn đúng skill đó.
Claude cũng tự chọn skill dựa trên `description`.

| Skill | Mảng | Có thao tác REST |
|---|---|:---:|
| `unity-core` | **Bắt đầu ở đây.** Cách nói chuyện với Unity: REST server (Editor đang mở), batch mode, Unity CLI, live editor; test, build, console, lỗi compile, undo/snapshot, sửa YAML khi bí; định tuyến sang các mảng | ✓ |
| `unity-scene` | GameObject, Component, Prefab, Scene, điều khiển Editor | ✓ |
| `unity-code` | Kiến trúc code, ScriptableObject vs runtime state, event, save/load, Input System, asmdef, Git; checklist thiết kế (pattern, ADR, blueprint, script review); tạo/sửa script | ✓ |
| `unity-ui` | Menu, HUD, settings, uGUI / UI Toolkit, scale theo màn hình, font | ✓ |
| `unity-rendering` | URP, quality tier, ánh sáng, lightmap/probe, post-processing, material, decal | ✓ |
| `unity-shaders` | HLSL, Shader Graph, renderer feature, variant, shader cost | ✓ |
| `unity-vfx` | Particle System, VFX Graph, nổ, đạn, trail, overdraw | |
| `unity-animation` | Mixamo/retarget, Animator, root motion, IK, Timeline, Cinemachine | ✓ |
| `unity-2d` | Sprite/PPU, atlas, Tilemap, 2D light, platformer controller, camera 2D | |
| `unity-gameplay` | AI, NavMesh, physics, state machine, projectile, pooling, DOTS, Netcode, Behavior | ✓ |
| `unity-world` | Terrain, đảo procedural, ProBuilder, level, streaming | ✓ |
| `unity-game-feel` | Cảm giác chơi: input buffer, coyote, hit stop, shake, juice, rubric chấm | |
| `unity-assets-build` | Import rule, dọn asset, package, Addressables, YooAsset, HybridCLR, build CI | ✓ |
| `unity-performance` | FPS, spike, GC, draw call, Profiler, LOD, IL2CPP | ✓ |
| `unity-mobile` | Android/iOS: budget, texture, safe area, AAB/Xcode, IAP, ads | |
| `unity-web` | WebGL: Build Profiles, nén, hosting, giới hạn trình duyệt | |
| `unity-tween-async` | DOTween, PrimeTween, UniTask (luật API theo đúng phiên bản) | ✓ |

## Bên trong mỗi skill: cùng một khuôn

```
unity-ui/
├── SKILL.md       Hiểu (stance, số liệu, lỗi hay gặp) → Làm (workflow + bảng thao tác REST) → Kiểm chứng (quality gates)
├── references/    kiến thức chuyên gia, quy trình chi tiết, checklist
├── actions/       thao tác trong Editor đang mở qua REST server UnitySkills
└── scripts/       chạy headless / batch mode, code mẫu đã test
```

`unity-core` có thêm `rest/` — giao thức REST đầy đủ (`PROTOCOL.md`), guide mode (`GUIDE.md`),
hướng dẫn thao tác tay (`manual/`).

## Hai cách thực thi

| Tình huống | Cách |
|---|---|
| Editor đang mở và có gói **UnitySkills** (REST `localhost:8090`–`8100`) | `actions/` của skill mảng đó — luôn dryRun trước |
| Không có REST server / chạy tự động | `scripts/` (toolkit Python `ut_*` + AgentKit C#, batch mode) |

## Yêu cầu

- Thao tác REST: gói `UnitySkills` trong project, Unity 2022.3+ / 6000.x.
- Scripts headless: Unity 6.3 (đã kiểm trên 6000.3.21f1), Python 3.9+ (chỉ thư viện chuẩn;
  Pillow, numpy, Playwright tuỳ chọn).
- **Giữ nguyên cả cụm `skills/`**: script của các skill import toolkit từ
  `unity-core/scripts/` theo thư mục anh em. Không đổi tên thư mục skill.

## Nguồn

- Kiến thức chuyên gia và `scripts/`: `scenario-labs/skills` → `skills/game-engines/unity` (MIT),
  port từ `edemaistre/unity-expert-skills`. Đã đổi tên skill (`scenario-unity-*` → `unity-*`),
  gộp với phần REST và chỉnh sửa cục bộ — không còn đồng bộ thẳng được với upstream.
- `actions/`, `rest/`, `unity-scene`, `unity-tween-async`, `unity-game-feel`,
  `unity-code/references/design/`: viết cho marketplace này (tác giả `vantv`), MIT.
