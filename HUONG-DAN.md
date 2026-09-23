# Hướng dẫn sử dụng (tiếng Việt)

Bộ công cụ này giúp bạn tìm việc, chấm điểm độ phù hợp của từng tin tuyển dụng, rồi tự viết CV và thư xin việc (cover letter) bằng LaTeX cho đúng tin đó.

Mọi thứ chạy trên máy bạn. Không có máy chủ nào, không tài khoản nào, không ai thu thập dữ liệu của bạn.

Bản hướng dẫn chi tiết bằng tiếng Anh nằm ở [SETUP.md](SETUP.md) và [README.md](README.md). File này là bản rút gọn để bắt đầu nhanh.

---

## 1. Cần cài gì trước

| Phần mềm | Dùng để làm gì | Cách cài |
|---|---|---|
| **Claude Code** | Bộ não của công cụ. Mọi lệnh `/...` đều chạy trong đây | `npm install -g @anthropic-ai/claude-code` |
| **Git** | Tải repo về | [git-scm.com](https://git-scm.com) |
| **LaTeX** | Biên dịch CV và cover letter ra PDF | Windows: [MiKTeX](https://miktex.org/download) · macOS: [MacTeX](https://tug.org/mactex/) · Linux: `sudo apt install texlive-full` |
| **Bun** | Chạy các công cụ quét trang tuyển dụng | `curl -fsSL https://bun.sh/install \| bash` |
| **Python 3.10+** | Công cụ tra cứu mức lương (không bắt buộc) | [python.org](https://python.org) |

Bạn cần tài khoản Claude Pro hoặc API key của Anthropic để dùng Claude Code.

---

## 2. Tải về và khởi động

```bash
git clone https://github.com/Nauq2001/ai-job-search.git
cd ai-job-search
claude
```

Lệnh `claude` mở Claude Code ngay trong thư mục vừa tải. Từ đây trở đi bạn chỉ gõ các lệnh gạch chéo bên dưới.

---

## 3. Bước đầu tiên: khai hồ sơ của bạn

```
/setup
```

Lệnh này sẽ hỏi bạn từng phần: tên, địa chỉ, ngôn ngữ, học vấn, kinh nghiệm, kỹ năng, khu vực muốn làm việc, và các điều kiện bắt buộc (ví dụ: phải có lương, không chuyển nhà).

Repo này được giao đi ở dạng **template trắng**, không chứa thông tin của ai cả. Sau khi chạy `/setup`, hồ sơ của bạn sẽ nằm trong `CLAUDE.md` và `.claude/skills/job-application-assistant/`.

Vài lưu ý khi khai:

- **Khai ngôn ngữ thật của bạn.** Công cụ dùng bảng này để loại những tin đòi một ngôn ngữ bạn không biết. Khai thừa thì sẽ nhận về những tin bạn không ứng tuyển nổi.
- **Khai trung thực kinh nghiệm.** Công cụ được thiết kế để không bịa. Nó chỉ viết được những gì bạn đã khai.

---

## 4. Quy trình dùng hằng ngày

Năm lệnh này là toàn bộ vòng đời một lần ứng tuyển:

```
/scrape      →  Quét các trang tuyển dụng, lưu tin mới, tự bỏ tin trùng
/rank        →  Chấm điểm và xếp hạng các tin đã quét
/apply       →  Viết CV + cover letter cho một tin cụ thể
/interview   →  Chuẩn bị câu hỏi và câu trả lời phỏng vấn
/outcome     →  Ghi lại kết quả: đã gửi, bị từ chối, được mời phỏng vấn...
```

### Các lệnh bổ trợ

| Lệnh | Tác dụng |
|---|---|
| `/add-portal` | Thêm trang tuyển dụng của thị trường bạn ở (VietnamWorks, ITviec, Indeed nước bạn...) |
| `/add-template` | Đăng ký mẫu CV hoặc cover letter riêng của bạn |
| `/upskill` | So hồ sơ với các tin đã lưu, chỉ ra kỹ năng còn thiếu và gợi ý lộ trình học |
| `/expand` | Bổ sung năng lực từ tài liệu bạn đưa vào (bằng cấp, thư giới thiệu, portfolio) |
| `/gmail-sync` | Đọc Gmail để tự cập nhật trạng thái các hồ sơ đã gửi |
| `/html-report` | Xuất bảng theo dõi ứng tuyển dạng trang web |
| `/reset` | Xoá sạch hồ sơ, quay về template trắng |

---

## 5. File sinh ra nằm ở đâu

| Thư mục | Nội dung |
|---|---|
| `cv/` | CV dạng LaTeX. Bản gốc đầy đủ là `main_example.tex`, bản riêng cho từng công ty là `main_<công-ty>.tex` |
| `cover_letters/` | Thư xin việc, kèm font Lato và Raleway đi sẵn |
| `job_scraper/seen_jobs.json` | Danh sách tin đã quét và trạng thái từng tin |
| `documents/` | Nơi bạn tự bỏ bằng cấp, thư giới thiệu vào |

**Biên dịch ra PDF:**

```bash
cd cv && lualatex -interaction=nonstopmode main_<tên-file>.tex
cd cover_letters && xelatex cover_<tên-file>.tex
```

CV dùng `lualatex`, cover letter dùng `xelatex` — không thay bằng `pdflatex` được, vì mẫu cover letter cần `fontspec`.

---

## 6. Về quyền riêng tư

File `.gitignore` đã chặn sẵn những thứ riêng tư: CV và cover letter đã điền thông tin, danh sách việc đã quét, bằng cấp trong `documents/`, dữ liệu lương.

Nhưng **hồ sơ trong `CLAUDE.md` và `.claude/skills/` thì git vẫn theo dõi**, vì đó vốn là file template. Nếu bạn định đẩy repo này lên GitHub công khai sau khi đã chạy `/setup`, hãy kiểm tra trước:

```bash
git status
git diff
```

Đừng commit những file đó nếu chúng đã chứa địa chỉ, số điện thoại hay email của bạn.

---

## 7. Gặp lỗi thì xem ở đâu

- Lỗi khi cài đặt hoặc biên dịch LaTeX: xem phần Troubleshooting trong [SETUP.md](SETUP.md).
- Muốn hiểu từng lệnh làm gì ở mức chi tiết: xem [README.md](README.md).
- Bản gốc của dự án: [MadsLorentzen/ai-job-search](https://github.com/MadsLorentzen/ai-job-search) — mọi công lao thuộc về tác giả gốc, repo này chỉ là một bản sao chia sẻ lại theo giấy phép MIT.
