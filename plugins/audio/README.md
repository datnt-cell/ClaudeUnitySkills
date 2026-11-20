# Âm thanh – tạo, hậu kỳ, dựng video

| Skill | Dùng khi nào |
|---|---|
| `audio-generate` | Tạo/biên tập âm thanh qua Scenario MCP: nhạc, bài hát có vocal, SFX, foley, ambience, TTS, lồng tiếng, đổi giọng, tách stem, chấm nhạc cho video; kết nối Scenario MCP |
| `audio-post` | Biến âm thanh thô thành bộ hoàn chỉnh cho game: chọn take, loudness, loop, crossfade, layering/ducking, đặt tên, import Unity, AudioMixer |
| `video-assembly` | Dựng video từ clip: timeline, nối, chuyển cảnh, logo/tiêu đề, nhạc nền/voiceover, phụ đề, bản dọc/vuông |

## Bên trong `audio-generate`

```
audio-generate/
├── SKILL.md                     vòng lặp chung: tìm model → schema → chạy → chờ → nghe → lưu
└── references/
    ├── scenario.md, setup.md, shared-assets.md   kết nối Scenario MCP, auth, credit, job
    ├── elevenlabs.md            TTS, dubbing, đổi giọng, music, SFX
    ├── ace-step.md              bài hát, cover, repaint, stem
    ├── minimax-music.md         bài hát có cấu trúc, tự viết lyric
    └── sonilo.md                SFX/nhạc từ video
```

## Yêu cầu

- Đã cấu hình và cấp quyền **Scenario MCP**; đọc `audio-generate/references/setup.md` trước.
- Generation tốn credit: dùng `dry_run: true` trước khi chạy dài, `wait: false` cho job dài.

## Nguồn

`audio-generate` và `video-assembly` gộp từ các skill `scenario*` của `scenario-labs/skills` (MIT).
`audio-post` viết cho marketplace này.
