# UNLOCKME
## Solution
### Challenge overview
Challenge cung cấp 1 file exe tên là unlockme.exe. Trước tiên ta sẽ phân tích file này bằng công cụ IDA Free
### Phân tích hàm main
Trước tiên ta phân tích logic hoạt động của hàm main
#### B1: Nhận input
<pre lang="markdown"> 
  std::vector<std::string>::vector<char **,void>(argv + 1, &argv[argc], &v17);
    if ( std::vector<std::string>::size(v16) == 2 )
      {
        v7 = std::vector<std::string>::operator[](0);
        std::string::operator=(v15, v7);
        v8 = std::vector<std::string>::operator[](1);
        std::string::operator=(v14, v8);
      }
      else
      {
        v9 = (_DWORD *)std::getline<char,std::char_traits<char>,std::allocator<char>>(&std::cin, v15);
        if ( (unsigned __int8)std::ios::operator!((char *)v9 + *(_DWORD *)(*v9 - 12))
          || (v10 = (_DWORD *)std::getline<char,std::char_traits<char>,std::allocator<char>>(&std::cin, v14),
              (unsigned __int8)std::ios::operator!((char *)v10 + *(_DWORD *)(*v10 - 12))) )
        {
          std::operator<<<std::char_traits<char>>(&std::cerr, "No input provided\n");
          v6 = 1;
</pre>
Ta thấy rằng, chương trình yêu cầu người dùng nhập vào 2 input
#### B2: Xử lý input
<pre lang="markdown"> 
  `anonymous namespace'::fG9((int)v13, (int)v15, (int)v14);
</pre>
Hàm fG9 nhận input1 (v15) và input2 (v14) thực hiện biến đổi và ghi kết quả vào v13
#### B3: So sánh kết quả 
<pre lang="markdown">
  if ( (unsigned __int8)std::operator==<char>(
                              v13,
                              "ce49b6edd9ca1634525872f8289e576ab22efcf3cb3e02850c55a10a570b2afb9b968f82f8361eee3a5ab2ad6f"
                              "eba438d3b463f35bda45da3a4d8cf186ec4f3c") )
      {
</pre>
Lấy output của hàm f69 so sánh với chuỗi cho sẵn. Nếu trùng thì báo ```bash Correct! You unlocked the challenge.\n```
Vậy để biết được flag được xử lý như thế nào, ta sẽ phân tích hàm f69
### Phân tích hàm f69
Phân tích logic hoạt động của hàm f69
<pre lang="markdown">
  anonymous namespace'::sE3(v12, a2);
  `anonymous namespace'::M3c::M3c(v11, (int)v12);
</pre>
a2 chính là input1 -> chuỗi input này được đưa vào sE3 -> sau đó dùng để khởi tạo M3c
<pre lang="markdown">
  v20 = std::string::data(a3);
  v19 = std::string::size(a3);
  v18 = (v19 >> 4) + 1;
</pre>
a3 (input2) được chia thành các block 16 byte
<pre lang="markdown">
  if ( 16 * i < v19 )
    {
      v13 = v19 - v17;
      v14 = 16;
      Size = *(_DWORD *)std::min<unsigned int>(&v14, &v13);
      v3 = (const void *)(v20 + v17);
      v4 = (void *)std::array<unsigned char,16u>::data(v9);
      memcpy(v4, v3, Size);
    }
    `anonymous namespace'::M3c::jF5(v8, (int)v11, v9);
</pre>
Mỗi block có kích thước 16 byte, block này sau đó được đưa vào ```bash M3c::jF5``` để biến đổi dữ liệu bằng khóa đã sinh ra từ input1
<pre lang="markdown">
  v5 = std::array<unsigned char,16u>::size(v8);
    v6 = (const unsigned __int8 *)std::array<unsigned char,16u>::data(v8);
    `anonymous namespace'::nV1((_anonymous_namespace_ *)v15, v6, v5);
    std::operator<<<char>(v10, v15);
    std::string::~string(v15);
</pre>
Chuyển mỗi block -> string hex -> các string hex được nối liên tiếp tạo thành 1 chuỗi, chuỗi này được trả về để so sánh với chuỗi hardcore trong hàm main
Vậy để biết các block của input2 được xử lý như thế nào, ta sẽ phân tích hàm ``` bash M3c::jF5```
### Phân tích hàm M3c::jF5
Hàm M3c::jF5 thực hiện mã hóa AES-128 trên từng block 16 byte, cụ thể:
Chương trình thực hiện AddRoundKey đầu tiên bằng cách XOR block với round key 0
<pre lang="markdown">
  std::array<std::array<unsigned char,16u>,11u>::operator[](0);
  `anonymous namespace'::add_round_key();
</pre>
Sau đó block được xử lý qua 9 vòng mã hóa 
<pre lang="markdown">
  for ( i = 1; i <= 9; ++i )
  {
    `anonymous namespace'::sub_bytes((int)this);
    `anonymous namespace'::shift_rows(this);
    `anonymous namespace'::mix_columns(this);
    std::array<std::array<unsigned char,16u>,11u>::operator[](i);
    `anonymous namespace'::add_round_key();
  }
  `anonymous namespace'::sub_bytes((int)this);
  `anonymous namespace'::shift_rows(this);
  std::array<std::array<unsigned char,16u>,11u>::operator[](10);
  `anonymous namespace'::add_round_key();
</pre>
mỗi vòng gồm các bước: sub_bytes -> thay bytes,  shift_rows -> dịch hàng, mix_columns-> trộn cột, std::array<...,11>::operator[](i) -> XOR với round key tương ứng
Vòng mã hóa thứ 10 không có bước mix_columns
Sau khi mã hóa xong, block 16 byte này được trả về làm ciphertext
-> Ciphertext 16 byte của mỗi block được encode thành chuỗi hex và ghép lại trong hàm fG9
Để tái tạo lại quá trình mã hóa, ta cần phân tích hàm sE3, nơi chương trình sử dụng input để sinh ra khóa AES dùng trong M3c::jF5
### Phân tích hàm sE3
Hàm sE3 tạo khóa AES 16 byte từ input1
<pre lang="markdown">
  std::string::basic_string(v7, "753a13ad4a45f9df5a7e92f220dcd647bc71a0e4");
  std::operator+<char>(v6, v7, a2);
</pre>
Đầu tiên, Chương trình lấy một chuỗi cố định và ghép thêm input1 (a2) vào phía sau
<pre lang="markdown">
  `anonymous namespace'::kB2((int)v5, (int)v6);
</pre>
Sau đó, hàm kB2 thực hiện MD5 -> chuỗi hex MD5
<pre lang="markdown">
  for ( i = 0; i <= 0xF; ++i )
  {
    v3 = (_BYTE *)std::array<unsigned char,16u>::operator[](i);
    *v3 = *(_BYTE *)std::string::operator[](v5, i + 5);
  }
</pre>
Lấy 16 byte của chuỗi hex MD5, bắt đầu từ offset 5 => AES key dùng cho hàm M3c::jF5
Tuy nhiên, input1 (a2) - tham số dùng để sinh khóa AES không đến từ input bên ngoài. Khi lần ngược nguồn gốc của biến này, có thể thấy nó được tạo ra bởi một hàm riêng trong chương trình, đó là hàm vM7. Vậy ta sẽ phân tích hàm vM7
### Phân tích hàm vM7
Hàm vM7 có nhiệm vụ tạo ra input1 (a2) – chuỗi được dùng để sinh khóa AES trong hàm sE3.
<pre lang="markdown">
  v4 = 53;
  std::string::basic_string(this);
  std::string::reserve(this, 10);
</pre>
Khởi tạo biến v4 = 53  , cấp sẵn dung lượng 10 ký tự → cho thấy chuỗi kết quả dài đúng 10 byte
Vòng lặp sinh từng ký tự
<pre lang="markdown">
  for ( i = 0; i <= 9; ++i )
  {
    v1 = 33 * v4;
    v4 = 33 * v4 + i;
    std::string::push_back(
      this,
      (char)((v1 + i) ^ program[i])
    );
  }
</pre>
Trong mỗi vòng lặp, hàm sẽ:
+ Tính giá trị trung gian: v1 = 33 * v4 -> Cập nhật lại v4 = 33 * v4 + i
+ Ký tự thứ i của chuỗi được tạo bằng cách: Lấy (v1 + i) XOR với program[i]
+ Ta tìm được program[i] = [0xE5, 0x4F, 0x00, 0x0C, 0xAB, 0xB5, 0x3F, 0x69, 0x40, 0x9B]
<img width="977" height="22" alt="image" src="https://github.com/user-attachments/assets/2a8d089d-fa64-470e-a0b0-ce300fd9c2dd" />
Sau khi phân tích đầy đủ luồng xử lý của chương trình, có thể thấy toàn bộ quá trình kiểm tra flag bao gồm các bước: sinh input1 bằng hàm vM7, tạo khóa AES thông qua sE3, sau đó mã hóa input2 bằng AES-128 và so sánh với ciphertext hardcode => Ta có được script giải mã: 
<pre lang = "markdown">
import hashlib
from Crypto.Cipher import AES
program = [0xE5, 0x4F, 0x00, 0x0C, 0xAB, 0xB5, 0x3F, 0x69, 0x40, 0x9B]
v4 = 53
a2 = ""
for i in range(10):
    v1 = (33 * v4) & 0xFF
    v4 = (33 * v4 + i) & 0xFF
    a2 += chr((v1 + i) & 0xFF ^ program[i])

base_str = "753a13ad4a45f9df5a7e92f220dcd647bc71a0e4"
full_str = base_str + a2
md5_hex = hashlib.md5(full_str.encode()).hexdigest()
aes_key = md5_hex[5:21] 

target_hex = "ce49b6edd9ca1634525872f8289e576ab22efcf3cb3e02850c55a10a570b2afb9b968f82f8361eee3a5ab2ad6feba438d3b463f35bda45da3a4d8cf186ec4f3c"
cipher = AES.new(aes_key.encode(), AES.MODE_ECB)
flag = cipher.decrypt(bytes.fromhex(target_hex))
print(f"Flag: {flag.decode(errors='ignore')}")
</pre>
=> Flag: VSL{Passw0rd_H@sh1ng_W1th_A3S_B4ttl3s_1n_ECB_M0d3!}









