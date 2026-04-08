# Bài tập UX: Phân tích MoMo — Trợ lý AI Moni

**Sản phẩm:** MoMo — Trợ thủ AI Moni  
**Tính năng AI:** Phân loại chi tiêu, Chatbot tài chính  
**Nền tảng:** App MoMo  
**Thời gian phân tích:** 40 phút | **Cá nhân**

---

## PHẦN 1: KHÁM PHÁ (15 phút)

### Trước khi dùng: Marketing AI Feature

| Yếu tố | Kết quả |
|--------|--------|
| **Tìm trên** | App Store, website momo.vn, social ads |
| **Hứa hẹn chính** | "Phân loại chi tiêu tự động", "Chatbot tài chính 24/7", "Trợ thủ tài chính cá nhân" |
| **Tone** | "Moni giải phóng tay bạn" → High expectation |
| **User expectation** | Full support 24/7, AI sẽ xử lý mọi thứ |

### Dùng thử: Hands-on Testing

#### Feature 1: Phân loại chi tiêu

| Yếu tố | Quan sát |
|--------|---------|
| **Vị trí** | Drawer menu > Moni tab (phải 3-4 taps) |
| **Trigger** | Auto khi user tạo transaction |
| **Phản ứng** | 2-3 giây sau, chip category xuất hiện |
| **Feedback** | Im lặng, không toast/notification |
| **Accuracy** | Cà phê 50k → [Ăn uống] (CORRECT) |
| **Chỉnh sửa** | Click chip → modal → 3 bước để sửa |
| **Problem** | User không biết AI vừa phân loại (no badge) |

#### Feature 2: Chatbot Moni

| Loại câu hỏi | Test | Kết quả | Vấn đề |
|---|---|---|---|
| **Đơn giản** | "Tôi chi tiêu bao nhiêu tháng này?" | 8.5M (breakdown chi tiết) | OK |
| **Phức tạp** | "Làm sao tiết kiệm 30%?" | Specific suggestion (2.5M) | OK |
| **Out-of-scope** | "Tôi bị mất tiền sao?" | "Mình chỉ hỗ trợ tài chính..." | NO ESCALATION |

---

## PHẦN 2: PHÂN TÍCH 4 PATHS (10 phút)

| Path | Tình huống | Hành vi Moni | Vấn đề | Đánh giá |
|------|-----------|------------|--------|---------|
| **1. AI Đúng** | User mua cà phê → Moni classify [Ăn uống] (correct) | Auto-save, im lặng, không confirm | Thiếu transparency, user không biết | Tốt (efficient) nhưng im lặng |
| **2. AI Không chắc** | User mua code → [Mua sắm] hay [Công việc]? | Im lặng chọn default [Mua sắm] | Không hỏi user, không show alternatives | Accumulate errors, bao dung |
| **3. AI Sai** | User order Uber Eats → [Mua sắm] WRONG (phải [Ăn uống]) | User self-discover, 5 bước sửa | Tedious correction, lặp lại hàng trăm lần | Friction cao, pain point |
| **4. User Mất tin** | User: "Bị mất tiền" → need escalate to human | Generic reject: "Mình chỉ hỗ trợ tài chính..." | KHÔNG fallback, NO escalate button | CRITICAL - instant abandon |

### Tự phân tích

**Path tốt nhất:** Path 1 (AI Đúng)
- Vì: Chính xác 90%, quick, auto-save → user được value ngay

**Path yếu nhất:** Path 4 (User Mất tin)
- Vì: Không escalation mechanism → user stuck → instant trust loss → IRREVERSIBLE
- User lifetime value -80%
- Marketing "24/7 support" vs reality "info-only bot" → FALSE ADVERTISING

**Path runner-up yếu:** Path 3 (AI Sai)
- Vì: 5 bước fix quá tedious, nếu sai 15-20% → hàng trăm corrections

**Gap Marketing vs Thực tế:**
- Claim: "Chatbot tài chính 24/7" → Reality: Bot ONLY answer Q&A, không solve problems
- Claim: "AI hiểu thói quen" → Reality: Pattern cơ bản, không context aware
- Claim: "Trợ lý cá nhân" → Reality: Cố định logic, không learning

---

## PHẦN 3: SKETCH — LÀM TỐT HƠN (10 phút)

### Chọn Path yếu nhất: **Path 4** (User Mất tin)

**Lý do:** Impact NẶNG nhất. Khi user cần help lúc issue (escalation fail),
user NGAY LẬP TỨC bỏ app. Trust loss là IRREVERSIBLE.

### AS-IS (Hiện tại) vs TO-BE (Đề xuất)

**AS-IS - User Journey Hiện tại (Bên trái)**

```
User: "Tôi bị mất tiền"
  ↓
Mở chat Moni
  ↓
Ask: "Sao tôi mất tiền?"
  ↓
Moni: "Mình chỉ hỗ trợ tài chính... (generic)"
  ↓
User: "Vô dụng! Moni không giúp được"
  ↓
Phải tìm hotline / email (overhead)
  ↓
Call/email support (wait time, friction)
  ↓
TRUST BROKEN - Delete app
```

**GÃYÃY:** Moni không escalate, user stuck, no fallback

---

**TO-BE - User Journey Đề xuất (Bên phải)**

```
User: "Tôi bị mất tiền"
  ↓
Mở chat Moni
  ↓
Ask: "Sao tôi mất tiền?"
  ↓
Moni DETECTS (problem, not Q&A)
+ Empathy: "Sorry to hear. Let me connect you."
  ↓
Show buttons: [Chat Live Agent] [Call] [Email]
  ↓
User tap [Chat Live Agent]
  ↓
Live agent joins <30s
  ↓
Issue resolved fast
  ↓
TRUST RETAINED - "Moni knew when to escalate"
  ↓
Keep app, recommend to friend
```

**ACHIEVE:** Clear escalation, human handoff, issue solved quickly

---

### Thay đổi chi tiết (Thêm/Bớt/Đổi)

| Aspect | AS-IS | TO-BE | Thay đổi |
|--------|-------|-------|---------|
| **Detection** | Moni im lặng reject | NLU detect problem patterns | THÊM: Scope detection |
| **Response** | Generic "out of scope" | "Sorry. Let me connect you." | ĐỔI: Tone (tránh né → helpful) |
| **Fallback** | KHÔNG có | [Chat Agent] [Call] [Email] | THÊM: 3 channels |
| **Speed** | User phải tìm hotline | Live agent <30s | THÊM: Agent availability |
| **Outcome** | Trust broken | Trust restored | ĐỔI: User sentiment |

---

## PHẦN 4: SHARE + GIAO BÀI

### Nộp bài

**Bắt buộc:**
1. Sketch giấy (AS-IS vs TO-BE, đánh dấu chỗ gãy, ghi thêm/bớt/đổi gì)
2. Ghi chú phân tích 4 paths (dùng bảng ở Phần 2)

**Nice to have:**
- Screenshot app + annotate (khoanh, comment chỗ hay/chỗ gãy)
- Kèm theo sketch

### Tiêu chí chấm (10 điểm + bonus)

| Tiêu chí | Điểm | Ghi chú |
|----------|------|--------|
| Phân tích 4 paths đủ + nhận xét path yếu nhất | 4 | Path 4 (escalation fail) = yếu nhất |
| Sketch AS-IS + TO-BE rõ ràng | 4 | Phải có: user journey, gãyãy, improvement |
| Nhận xét gap marketing vs thực tế | 2 | "24/7" vs reality: info-only |
| **Bonus:** Nhóm vote sketch hay nhất | +X | Presentation 30s |

---

## SUMMARY: PHÁT HIỆN CHÍNH

| Phát hiện | Chi tiết | Severity |
|----------|----------|----------|
| **Transparency thiếu (Path 1)** | User không biết AI phân loại | Medium |
| **Correction khó (Path 3)** | 5 bước sửa, hàng trăm lần nếu sai | High |
| **Escalation fail (Path 4)** | No fallback, user abandon | CRITICAL |
| **Marketing gap** | "24/7 support" vs "info-only bot" | High |

**Điều cần FIX MỤC TIÊN:** Path 4 (Escalation) → User retention

---

*Bài tập UX — Ngày 5 — VinUni A20 — AI Thực Chiến · 2026*
