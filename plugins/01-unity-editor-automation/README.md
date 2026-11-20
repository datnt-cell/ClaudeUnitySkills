# 01 · Unity – Tự động hoá Editor

Nhóm công cụ gọi Unity Editor qua REST server của gói **UnitySkills**
(`localhost:8090`–`8100`, Unity 2022.3+/6000.x). Đây là nhóm dùng hằng ngày:
tạo scene, prefab, material, UI, script, xử lý asset, build, profiler.

| Skill | Làm được gì |
|---|---|
| `unity-core` | Sổ tay giao thức đầy đủ: discovery, operating mode, dryRun, batch, mã lỗi, guide mode. **Đọc trước** — các skill khác chỉ giữ 3 luật cứng và trỏ về đây |
| `unity-scene-authoring` | GameObject, Component, Prefab, Scene, Editor control |
| `unity-rendering` | Material, Light, Shader, Shader Graph, Volume, post-processing, URP, Decal |
| `unity-ui` | UGUI và UI Toolkit (layout, theme, binding) |
| `unity-scripting` | Tạo/sửa script, ScriptableObject |
| `unity-assets-pipeline` | Import, dọn asset, tối ưu, package, validation |
| `unity-animation-timeline` | Animator, Timeline, Cinemachine, camera Scene View |
| `unity-physics-navigation` | Physics, NavMesh, Terrain, ProBuilder |
| `unity-addressables-yooasset` | Addressables, YooAsset, HybridCLR (nóng cập nhật code) |
| `unity-tweening` | DOTween Pro, PrimeTween, UniTask (design + sinh code tween) |
| `unity-dev-workflow` | Batch/async job, snapshot, debug/console, profiler, history, bookmark, Test Runner, project settings |
| `unity-editor-tools` | Sửa YAML tay (fallback), Unity CLI thử nghiệm, behavior graph |

## Ghi chú

- Giao thức REST đầy đủ chỉ nằm ở `unity-core`. Mỗi skill còn lại chỉ giữ 3 luật
  cứng (dryRun trước, batch khi ≥2 đối tượng, xử lý `MODE_RESTRICTED`/`SURFACE_EXCLUDED`).
- Nhóm này chỉ **thao tác**. Tư vấn thiết kế (pattern, asmdef, async, ADR, blueprint,
  script review, scene contract, Inspector UX) đã chuyển sang plugin 02:
  `scenario-unity-architecture/references/design/`; checklist hiệu năng code sang
  `scenario-unity-performance/references/code-red-flags.md`.
- Trước khi gọi skill có tham số chưa chắc chắn: `POST /skill/<name>?mode=dryRun`
  cho tới khi `valid:true`, rồi gọi thật.
- Đọc `unity-core` trước nếu đây là phiên đầu tiên.

## Nguồn

Viết riêng cho marketplace này (tác giả `vantv`), MIT.
