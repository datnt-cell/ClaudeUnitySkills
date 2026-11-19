# 04 · Sản xuất âm thanh

Tạo – biến đổi – cắt – ghép – trộn âm thanh. Gồm 7 skill chạy qua **Scenario
MCP** (nguồn `scenario-labs/skills`, MIT) và 1 skill playbook local.

## Danh sách skill

| Skill | Dùng khi nào |
|---|---|
| `scenario` | **Kết nối gốc**: cấu hình MCP, `recommend` / `search` / `model_schema_get` / `model_run`, `jobs_wait`, `asset_display`, `asset_download`, phạm vi team/project |
| `scenario-audio` | Router chung: nhạc nền, bài hát có vocal, SFX, foley, ambience, clip loop, voiceover, transcription, **tiện ích cắt/tách/xuất** (`audio-cut`, `audio-split`, `audio-extract`, `compose-video`), tách stem |
| `scenario-elevenlabs` | TTS kèm tag cảm xúc, music v2, sound-effects v2, lồng tiếng sang ngôn ngữ khác, đổi giọng, tách giọng khỏi nhiễu |
| `scenario-sonilo` | Text → SFX/music, video → SFX/music (chấm nhạc cho clip câm), giữ giọng và thay nhạc, điều khiển theo từng đoạn |
| `scenario-ace-step` | Bài hát từ lyric, track instrumental, cover/restyle, repaint một đoạn, thêm layer, hoàn thiện bản thu dở, tách stem |
| `scenario-minimax-music` | Bài hát đầy đủ với `[Verse]`/`[Chorus]`, tự viết lyric từ brief, instrumental từ style prompt, cover giữ melody |
| `scenario-video-assembly` | Dựng shot list, timeline, nối clip + transition, overlay, **lớp nhạc nền và voiceover**, burn phụ đề, xuất nhiều tỉ lệ |
| `audio-post-production` | **Playbook local**: chọn take, gain staging, crossfade, loop liền mạch, đặt tên file, loudness, import settings, AudioMixer, ducking, QA checklist |

## Chọn nhanh theo nhu cầu

| Bạn muốn | Bắt đầu ở |
|---|---|
| Một tiếng động trong game | `scenario-audio` → `recommend` với `capability="txt2audio"` |
| Nhạc nền/ambience lặp | `scenario-audio` (bật loop trong schema) → kiểm tra mối nối bằng `audio-post-production` |
| Bài hát có vocal | `scenario-ace-step` / `scenario-minimax-music` / `scenario-elevenlabs` |
| Hội thoại 2 người, có cảm xúc | `scenario-elevenlabs` (cú pháp tag khác nhau từng model — đọc schema) |
| Lồng tiếng/đổi giọng bản thu cũ | `scenario-elevenlabs` |
| Chấm nhạc cho clip câm | `scenario-sonilo` |
| Cắt một đoạn, tách track, lấy audio từ video | tiện ích của `scenario-audio` |
| Tách vocal/stem | `scenario-audio` hoặc ACE-Step stem extract |
| Ghép nhạc + voiceover lên bản dựng | `scenario-video-assembly` |
| Chuẩn bị đưa vào Unity | `audio-post-production` |

## Yêu cầu

- Đã cấu hình và cấp quyền **Scenario MCP**; đọc `skills/scenario/SKILL.md` và
  `skills/scenario/references/setup.md` trước.
- Generation tốn credit: dùng `dry_run: true` trước khi chạy dài, `wait: false`
  cho job bài hát/video.
- 7 skill Scenario là Markdown độc lập (không import script dùng chung), nên có
  thể cài lẻ; nhưng nên giữ `scenario` cùng để có phần kết nối.

## Nguồn

Copy nguyên văn 7 skill từ `scenario-labs/skills` (MIT). README, manifest và
`audio-post-production` do marketplace này viết thêm.
