# Discord Quest Auto-Completer Bot

Một bot Discord viết bằng Python dùng để quản lý và tự động nhận, hoàn thành các nhiệm vụ (Quests) của Discord ở chế độ chạy ngầm bất đồng bộ. 

---

> [!CAUTION]
> ### ⚠️ TUYÊN BỐ MIỄN TRỪ TRÁCH NHIỆM & MỤC ĐÍCH HỌC TẬP (DISCLAIMER)
>
> 1. **Mục đích giáo dục và học tập**: 
>    Dự án này được phát triển hoàn toàn vì **mục đích học tập, nghiên cứu lập trình** (cụ thể là tìm hiểu về giao thức truyền tải HTTP API, lập trình bất đồng bộ `asyncio`/`aiohttp` trong Python, và cách phát triển ứng dụng Bot Discord). 
>
> 2. **Vi phạm Điều khoản Dịch vụ (Discord ToS)**: 
>    Việc sử dụng mã thông báo tài khoản cá nhân (User Token) để tự động hóa hành vi (self-botting) vi phạm nghiêm trọng Điều khoản dịch vụ của Discord. Hành động này có thể khiến tài khoản của người sử dụng **bị khóa vĩnh viễn (Terminated)**.
>
> 3. **Tuyên bố miễn trừ trách nhiệm hoàn toàn**: 
>    **Tác giả và những người đóng góp cho dự án này hoàn toàn MIỄN TRỪ MỌI TRÁCH NHIỆM pháp lý và dân sự.** 
>    - Chúng tôi không chịu trách nhiệm cho bất kỳ tổn thất, thiệt hại, hoặc việc tài khoản của bạn bị khóa/cấm khi sử dụng phần mềm này.
>    - Người dùng tự chịu hoàn toàn 100% rủi ro khi quyết định chạy thử nghiệm dự án này trên tài khoản cá nhân.
>    - Phần mềm này được cung cấp theo nguyên tắc **"AS IS"** (nguyên trạng), không đi kèm bất kỳ chế độ bảo hành hay cam kết nào.

---

## Tính Năng
- **Lệnh Slash (Slash Commands)**: Tương tác hiện đại bằng giao diện lệnh `/` của Discord.
- **Xác thực động và bảo mật**: Nhập token tài khoản cá nhân trực tiếp qua lệnh `/login` ẩn danh (ephemeral) chỉ gửi riêng đến bot.
- **Tự động chặn lộ token**: Bot kiểm tra và chặn ngay lập tức nếu người dùng cố chạy lệnh `/login` trong kênh công khai của máy chủ.
- **Hỗ trợ đa người dùng**: Lưu trữ cục bộ thông tin xác thực động qua `tokens.json`, cho phép nhiều tài khoản chạy song song cùng lúc.
- Hỗ trợ làm các nhiệm vụ: xem video (`WATCH_VIDEO`), chơi game trên máy tính (`PLAY_ON_DESKTOP`, `PLAY_ACTIVITY`), stream game (`STREAM_ON_DESKTOP`).
- Chạy ngầm hiệu quả mà không làm đơ bot nhờ cơ chế bất đồng bộ (`asyncio` + `aiohttp`).
- Gửi báo cáo tiến trình làm việc chi tiết về một kênh log được chỉ định.

---

## Hướng Dẫn Cài Đặt và Sử Dụng

### Bước 1: Cài đặt thư viện yêu cầu
Chạy lệnh cài đặt các gói cần thiết trong thư mục dự án:
```bash
pip install -r requirements.txt
```

### Bước 2: Cấu hình file `.env`
Mở file [.env](file:///d:/discordbot/.env) trong thư mục gốc và nhập thông tin cấu hình bot:
```env
BOT_TOKEN=nhập_token_bot_discord_của_bạn
LOG_CHANNEL_ID=nhập_id_kênh_chat_để_nhận_log
```
*(Lưu ý: Bạn không cần phải điền `USER_TOKEN` tại đây nữa, vì mỗi người dùng sẽ tự đăng nhập bằng token của họ ngay trên Discord!)*

**Cách lấy thông tin:**
1. **BOT_TOKEN**:
   - Truy cập trang [Discord Developer Portal](https://discord.com/developers/applications).
   - Tạo một Application mới -> Chọn tab **Bot** -> Nhấp **Reset Token** để lấy Token của Bot.
   - *Lưu ý: Bắt buộc cuộn xuống tab **Bot**, tìm mục **Privileged Gateway Intents** và kích hoạt tính năng **Message Content Intent** lên để bot đọc và đồng bộ hóa lệnh.*
2. **LOG_CHANNEL_ID**:
   - Vào Discord của bạn -> Cài đặt -> Nâng cao (Advanced) -> Bật **Developer Mode** (Chế độ nhà phát triển).
   - Chuột phải vào kênh văn bản bạn muốn nhận báo cáo làm quest từ bot -> Chọn **Sao chép ID Kênh (Copy Channel ID)** và dán vào file `.env`.

### Bước 3: Chạy Bot
Chạy lệnh khởi động bot:
```bash
python bot.py
```

Sau khi bot trực tuyến, nó sẽ tự động đồng bộ hóa cây lệnh Slash (Slash command tree) lên máy chủ Discord.

---

## Cách Hoạt Động Trên Discord

### 1. Đăng ký Token Tài Khoản Cá Nhân (Chỉ hoạt động trong DM)
- Tìm Bot của bạn trong danh sách thành viên máy chủ, nhấp chuột phải chọn **Message** để nhắn tin riêng.
- Gõ `/login` và điền tham số `token` bằng Token cá nhân của bạn:
  ```text
  /login token: <User_Token_Của_Bạn>
  ```
- Bot sẽ xác thực và liên kết tài khoản (ví dụ: `👤 Tài khoản liên kết: @username`).
- *Nếu bạn chạy lệnh này ở kênh công khai, bot sẽ chặn và báo lỗi dạng tin nhắn ẩn (chỉ bạn nhìn thấy) để ngăn token của bạn hiển thị với người khác.*

### 2. Các Lệnh Điều Khiển (Gõ `/` trên Discord)
Tất cả kết quả của các lệnh này đều trả về dưới dạng **tin nhắn ẩn danh (ephemeral)**, nghĩa là chỉ có duy nhất bạn có thể đọc được kết quả phản hồi của lệnh đó trong kênh.

- `/help` - Xem hướng dẫn sử dụng bot.
- `/status` - Xem danh sách quest cá nhân, trạng thái và tiến độ (`value/target`).
- `/start` - Bắt đầu vòng quét quest tự động chạy ngầm cho tài khoản của bạn. Tiến trình chạy sẽ gửi về kênh log chung của bot.
- `/stop` - Dừng quá trình quét quest tự động.
- `/check` - Thực hiện quét và làm quest lập tức một lần thủ công.
- `/logout` - Đăng xuất, hủy tiến trình chạy ngầm và xóa token của bạn khỏi hệ thống.
