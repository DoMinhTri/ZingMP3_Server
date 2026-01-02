# ZingMp3 API

## Giới thiệu
Dự án cung cấp API Zing MP3 với server Express và UI HTML để test nhanh.

## Cài đặt và chạy Server

1. **Cài đặt phần mềm**
   ```bash
   Tải phần mềm chạy nodeJS cho Window
   ```

2. **Vào thư mục dự án** (nếu chưa vào)
   ```bash
   Mở nodeJS command prompt
   Dùng lệnh CD đường dẫn thu mục để duy chuyển vào thư mục
   cd ZingMP3_Server
   ```

3. **Chạy server**
   - Dùng lệnh `node server.js` để chạy server  
   - Tìm 1 bài hát: `node server_1.js`
   - Tìm tất cả bài hát cùng tên: `node server_total.js`
   

## UI kiểm thử
Mở trình duyệt tới: `http://localhost:5555/`
- Tìm kiếm theo từ khóa: nhập tên bài hát/ca sĩ → “Tìm kiếm”. Kết quả trả về rút gọn, tự động điền Song ID bài đầu.
- Tìm kiếm theo Song ID (nhập thủ công): nhập Song ID → dùng các nút:
  - Phát nhạc: stream 128kbps qua `/api/song/stream?id=...`
  - Link Stream: trả nguyên JSON từ API getSong (giữ nguyên `data.128`)
  - Thông tin: trả JSON chi tiết bài hát (getInfoSong)
  - Lời bài hát: tải file `.lrc`, hiển thị link file ở dòng đầu và nội dung lyric đã bỏ timestamp ở dưới (mỗi câu một dòng)

Ghi chú:
- Audio, link, dòng ghi chú chỉ xuất hiện sau khi bấm “Phát nhạc”.
- Chất lượng mặc định 128kbps; trường 320 có thể là “VIP”.

## Danh sách endpoint server (demo)
- Kiểm tra: `GET /health`
- Tìm kiếm (v2): `GET /api/search?q=<keyword>`
- Lấy link stream: `GET /api/song?id=<songId>` → trả JSON nguồn (giữ nguyên `data.128`)
- Stream phát nhạc: `GET /api/song/stream?id=<songId>` → 302 redirect tới URL mp3 128kbps
- Thông tin bài hát: `GET /api/info-song?id=<songId>`
- Lời bài hát: `GET /api/lyric?id=<songId>`

Các route tham khảo thêm (nếu cần bật lại trong UI):
- Top 100: `GET /api/top100`
- Home: `GET /api/home`
- Chart Home: `GET /api/chart-home`
- New Release Chart: `GET /api/newrelease-chart`
- Playlist chi tiết: `GET /api/detail-playlist?id=<playlistId>`
- Nghệ sĩ: `GET /api/artist?name=<alias>`
- Danh sách bài của nghệ sĩ: `GET /api/artist-songs?id=<artistId>&page=1&count=15`
- MV (thể loại): `GET /api/list-mv?id=<genreId>&page=1&count=15`
- Thể loại MV: `GET /api/category-mv?id=<genreId>`
- Video: `GET /api/video?id=<videoId>`

## Ví dụ nhanh
- Lấy link stream (JSON gốc):
```bash
curl "http://localhost:5555/api/song?id=ZOACFBBU"
```
- Phát trực tiếp (trình duyệt):
```
http://localhost:5555/api/song/stream?id=ZOACFBBU
```
- Tìm kiếm:
```bash
curl "http://localhost:5555/api/search?q=l%E1%BA%A1c%20tr%C3%B4i"
```

## Lưu ý
- ZingMP3 có thể thay đổi `version`, `apiKey`, logic ký `sig` và yêu cầu cookie hợp lệ. Module đã xử lý tự động các phần này ở phía server.
- Truy cập trực tiếp các link upstream (trên zingmp3.vn) từ trình duyệt có thể lỗi do thiếu cookie hoặc chữ ký hết hạn. Hãy dùng các route local trong server của dự án.

## Bản quyền
- Dựa trên module gốc: [ZingMp3API]
- Dự án hiện tại dùng cho mục đích học tập/khảo sát API.
