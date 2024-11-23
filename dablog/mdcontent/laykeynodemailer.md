# Cách Lấy App Key Google để Cấu Hình NodeMailer Gửi Email  

Gửi email từ ứng dụng Node.js có thể là một thử thách, đặc biệt khi bạn muốn sử dụng dịch vụ Gmail. Nếu bạn đã thử và gặp phải các vấn đề như "Less secure app access" bị tắt hoặc tài khoản bị khóa do đăng nhập lạ, thì bạn không hề cô đơn! Giải pháp cho vấn đề này là sử dụng **OAuth2** với **NodeMailer** — một công cụ mạnh mẽ và an toàn. Hôm nay, chúng ta sẽ cùng nhau khám phá cách lấy App Key Google và cấu hình NodeMailer để gửi email một cách chuyên nghiệp.

## Tại Sao Phải Dùng OAuth2?  

### **1. Bảo Mật Cao Hơn**  
OAuth2 không yêu cầu bạn cung cấp trực tiếp mật khẩu tài khoản Gmail, giúp tránh nguy cơ bị lộ thông tin. Thay vào đó, bạn sẽ sử dụng các thông tin xác thực như **Client ID**, **Client Secret** và **Refresh Token**.  

### **2. Tránh Tài Khoản Bị Khóa**  
Google rất nghiêm ngặt trong việc phát hiện các đăng nhập không bình thường. Sử dụng OAuth2 giúp ứng dụng của bạn được nhận diện chính thống, tránh tình trạng bị khóa tài khoản.

### **3. Giải Pháp Cho Các Ứng Dụng Lớn**  
Nếu bạn đang phát triển một ứng dụng gửi hàng trăm hoặc hàng nghìn email mỗi ngày, OAuth2 là lựa chọn duy nhất để đảm bảo tính ổn định và bảo mật lâu dài.

---

## Hướng Dẫn Từng Bước Lấy App Key Google  

### **Bước 1: Truy Cập Google Cloud Console**  
Truy cập vào [Google Cloud Console](https://console.cloud.google.com/) — đây là nơi bạn quản lý tất cả các dự án và API của Google. Nếu bạn chưa có tài khoản Google Cloud, hãy tạo ngay một tài khoản miễn phí!

![Google Cloud Console](https://raw.githubusercontent.com/Gawasna/Multimedia-archive/refs/heads/main/dablog/thumbs/hhawud.png)

---

### **Bước 2: Tạo Dự Án Mới**  
Mỗi API trên Google Cloud đều cần nằm trong một dự án cụ thể.  
1. Trong giao diện chính, nhấn vào nút **"Create Project"**.  
2. Đặt tên cho dự án, ví dụ: **"NodeMailer Gmail Integration"**.  
3. Nhấn **"Create"** và đợi Google tạo xong dự án.  

> 💡 **Mẹo:** Đặt tên dự án rõ ràng để sau này dễ quản lý hơn.

![Tạo Dự Án Mới](https://raw.githubusercontent.com/Gawasna/Multimedia-archive/refs/heads/main/dablog/thumbs/uuwssn.png)

---

### **Bước 3: Kích Hoạt Gmail API**  
Để NodeMailer có thể gửi email, bạn cần kích hoạt **Gmail API**:  
1. Vào mục **"APIs & Services"** > **"Library"**.  
2. Tìm kiếm **Gmail API** trong thanh tìm kiếm.  
3. Nhấn **"Enable"** để kích hoạt API.  

> 🔑 **Lưu ý:** Nếu bạn không kích hoạt API này, NodeMailer sẽ không thể gửi email thông qua Gmail.

![Kích Hoạt Gmail API](https://raw.githubusercontent.com/Gawasna/Multimedia-archive/refs/heads/main/dablog/thumbs/Screenshot%202024-11-23%20191544.jpg)

---

### **Bước 4: Tạo OAuth 2.0 Credentials**  
Giờ là lúc bạn tạo thông tin xác thực OAuth!  
1. Vào **"APIs & Services"** > **"Credentials"**.  
2. Nhấn **"Create Credentials"** và chọn **"OAuth client ID"**.  
3. Nếu bạn chưa cấu hình **OAuth consent screen**, Google sẽ yêu cầu bạn làm bước này trước. Hãy chọn:  
   - **User Type:** Chọn **External** nếu ứng dụng dành cho người ngoài.  
   - Điền các thông tin cần thiết như **Tên ứng dụng**, **Email hỗ trợ**, và **Logo (nếu có)**.  

4. Quay lại phần **Credentials** và tạo **OAuth client ID**:  
   - **Application type:** Chọn **Web application**.  
   - Điền các trường cần thiết và nhấn **"Create"**.  

Bạn sẽ nhận được **Client ID** và **Client Secret** — hãy lưu lại cẩn thận!

![Tạo OAuth client ID](https://kb.pavietnam.vn/wp-content/uploads/2021/09/oauth-client-id-google-4.png.webp)

---

### **Bước 5: Lấy Refresh Token**  
Refresh Token là chìa khóa để ứng dụng của bạn duy trì kết nối lâu dài. Bạn có thể lấy nó thông qua **OAuth2 Playground**:  
1. Truy cập [OAuth2 Playground](https://developers.google.com/oauthplayground/).  
2. Nhập **Client ID** và **Client Secret** vừa tạo.  
3. Chọn quyền **Gmail API** và xác thực tài khoản.  
4. Sao chép **Refresh Token** được tạo ra.  

> 🚀 **Mẹo:** Refresh Token giúp bạn không cần đăng nhập lại mỗi lần gửi email.

![OAuth2 Playground](https://developers.google.com/static/google-ads/api/images/playground-authcode.png?hl=vi)

---

### **Bước 6: Cấu Hình NodeMailer Trong Dự Án**  
Bây giờ, hãy nhúng các thông tin trên vào code Node.js của bạn:

```javascript
const nodemailer = require('nodemailer');

const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    type: 'OAuth2',
    user: 'your-email@gmail.com',
    clientId: 'YOUR_CLIENT_ID',
    clientSecret: 'YOUR_CLIENT_SECRET',
    refreshToken: 'YOUR_REFRESH_TOKEN',
  },
});

const mailOptions = {
  from: 'your-email@gmail.com',
  to: 'recipient@example.com',
  subject: 'Test Email with NodeMailer',
  text: 'Chúc mừng! Bạn đã cấu hình thành công NodeMailer với Gmail!',
};

transporter.sendMail(mailOptions, (err, info) => {
  if (err) {
    console.error('Lỗi gửi email:', err);
  } else {
    console.log('Email gửi thành công:', info.response);
  }
});
