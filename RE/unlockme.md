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







