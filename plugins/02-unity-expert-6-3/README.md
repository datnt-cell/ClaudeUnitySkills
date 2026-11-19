# 02 · Unity 6.3 – Bộ chuyên gia

14 skill chuyên gia Unity 6.3 (6000.3.x) làm theo nguyên tắc **đo lường**:
mỗi quy trình đều kèm số liệu, test PlayMode/EditMode và ảnh chụp để chứng minh.
Nguồn: `scenario-labs/skills` (MIT), port từ `edemaistre/unity-expert-skills`.

## Danh sách skill

| Skill | Dùng khi nào |
|---|---|
| `scenario-unity-expert` | **Skill dẫn** – kênh batch mode / editor đang chạy, Unity CLI, Test Framework, build; nơi chứa toolkit `ut_*` và AgentKit dùng chung |
| `scenario-unity-2d` | Game 2D: import pixel/HD, PPU, atlas, Tilemap, Rule Tile, 2D light, platformer controller, camera, hit stop, screen shake |
| `scenario-unity-animation` | Mixamo/retargeting, Animator, blend tree, root motion, IK, Playables, Timeline, Cinemachine 3 |
| `scenario-unity-architecture` | Cấu trúc code: ScriptableObject vs runtime state, event, save/load, Input System, asmdef, domain reload, Git/Smart Merge/LFS |
| `scenario-unity-gameplay` | AI tuần tra/đuổi/đánh, NavMesh, state machine, projectile, layer & collision matrix, pooling, DOTS, Netcode, audio mixer |
| `scenario-unity-mobile` | Android/iOS: frame budget, texture ASTC/ETC2, touch control, safe area, AAB/keystore, Xcode, IAP, ads |
| `scenario-unity-performance` | Game chậm/giật: Profiler, Profile Analyzer, GC spike, draw call, SRP Batcher, LOD, shader warm-up, IL2CPP |
| `scenario-unity-pipeline-automation` | Import art hàng loạt, sinh prefab/scene bằng script, Addressables, build CI/GameCI |
| `scenario-unity-rendering-lighting` | Cấu hình URP, quality tier, lightmap/probe, tonemapping, TAA/STP/MSAA, GPU Resident Drawer |
| `scenario-unity-shaders` | HLSL và Shader Graph: nước, toon, dissolve, outline, Custom Function, renderer feature |
| `scenario-unity-ui` | Menu, settings, HUD, thanh máu, minimap, rebind phím |
| `scenario-unity-vfx` | Nổ, cầu lửa, đạn có muzzle/trail/impact, VFX Graph, overdraw trên mobile |
| `scenario-unity-web` | Build WebGL: Build Profiles, gzip/Brotli, hosting, giới hạn trình duyệt |
| `scenario-unity-world-building` | Terrain, đảo procedural, erosion, splat, cây cỏ |

## Ràng buộc quan trọng

Mỗi skill `scenario-unity-*` **import toolkit từ thư mục anh em**
`scenario-unity-expert/scripts/` (`ut_env`, `ut_run`, `ut_review`, `ut_stat`) và
AgentKit lõi. Đường dẫn được tính tương đối theo thư mục cha, nên:

- cài **cả nhóm**, không tách riêng một skill chuyên gia;
- giữ đủ 14 thư mục dưới cùng một `skills/`;
- xoá `scenario-unity-expert` sẽ làm hỏng toàn bộ các skill còn lại.

Tên thư mục (`scenario-unity-2d`, …) **không được đổi** vì code Python trỏ cứng
theo tên này.

## Yêu cầu

- Unity 6.3 LTS (đã kiểm trên 6000.3.21f1). Tài liệu gốc kiểm trên macOS Apple
  Silicon; code C#/Python không phụ thuộc nền tảng nhưng đường dẫn trong quy
  trình giả định Unity Hub.
- Python 3.9+ cho toolkit phía runner (chỉ dùng thư viện chuẩn; Pillow, numpy,
  Playwright là tuỳ chọn).

## Nguồn

Copy nguyên văn từ `scenario-labs/skills` → `skills/game-engines/unity`. Chỉ
README và plugin manifest này do marketplace viết thêm.
