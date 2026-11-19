# 03 · Game Feel & Juice (cảm giác chơi)

Skill hướng dẫn **cảm giác chơi**: làm sao để game không chỉ "chạy đúng" mà
còn "đã tay". Viết riêng cho marketplace này, dựa trên số liệu đã kiểm trong
nhóm `02-unity-expert-6-3`.

## Nội dung

| Tệp | Nội dung |
|---|---|
| `skills/unity-game-feel/SKILL.md` | 5 lớp cảm giác (input → movement → camera → impact → presentation), bảng số liệu, workflow, lỗi thường gặp, quality gate |
| `skills/unity-game-feel/modules/feel-recipes/SKILL.md` | Code C# cụ thể: hit stop, shake + kick, Cinemachine 3 impulse, damping bất đối xứng, coyote/buffer, squash & stretch, bảng xếp lớp impact |
| `skills/unity-game-feel/modules/feel-audit/SKILL.md` | Rubric chấm điểm A–G kèm mẫu báo cáo, dùng trước/sau playtest |

## Khi nào dùng

- "Nhảy bị lơ lửng", "đánh không có lực", "bấm không ăn", "camera mất nhân vật".
- Cần thêm juice: hit stop, rung màn hình, kick, particle, punch UI.
- Chuẩn bị playtest: cần checklist và thang điểm để so sánh.

## Số liệu cốt lõi (đã có nguồn trong skill)

| Giá trị | Ý nghĩa |
|---|---|
| 0.02–0.07 s | thời lượng hit stop mỗi cú đánh |
| 0.15 s / 0.2 s | cửa sổ coyote time / jump buffer |
| 0.6 s → 0.1 s, ngưỡng −15 u/s | damping camera dọc bất đối xứng |
| Multiplier = 0 | tắt shake/hit stop cho accessibility |

## Liên kết

- Code cảm giác đã test: `../02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/`
- Tween UI: `../01-unity-editor-automation/skills/unity-tweening/`
- VFX va chạm: `04-audio-production` (âm thanh) + `02-unity-expert-6-3/skills/scenario-unity-vfx`
