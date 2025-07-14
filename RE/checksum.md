# CHECKSUM
## Mô tả thử thách
<img width="711" height="464" alt="image" src="https://github.com/user-attachments/assets/cdd61cc3-10d2-408b-b79f-cff679e4c146" />

## Chạy chương trình
<img width="1486" height="761" alt="image" src="https://github.com/user-attachments/assets/d040f5f4-bf55-42f1-a81a-6ef33e784d56" />
Ta thấy chương trình chạy ngẫu nhiên các phép tính, nhập đúng hết tất cả các phép tính thì chương trình sẽ hiện lên dòng yêu cầu nhập checksum. Nếu sai phép tính hoặc sai checksum thì chương trình sẽ kết thúc

## Phân tích chương trình
Mở chương trình bằng ghidra
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4f9d0250-096d-41ed-b8e7-2819f4384d7b" />
Ta thấy có rất nhiều hàm main ở đây, trước tiên ta mở hàm main.main và phân tích nó
<pre lang="markdown"> 
void main::main.main(void)
{
  math/rand::math/rand.Seed
            ((int)(sdword)((dword)tVar6.wall & 0x3fffffff) + iVar2 * 1000000000 +
             -0x5e4dfc14c2e60000);
  iVar2 = math/rand::math/rand.Intn(6);
  val = 0;
  while ((int)val < (int)(iVar2 + 5U)) {
    val = val + 1;
    local_58 = piVar3;
    pvStack_50 = (void *)uVar4;
    local_48 = piVar3;
    pvStack_40 = (void *)uVar4;
    pvStack_50 = runtime::runtime.convT64(val);
    local_58 = &int___Int_type;
    pvStack_40 = runtime::runtime.convT64(iVar2 + 5U);
    local_48 = &int___Int_type;
    w_04.data = os.Stdout;
    w_04.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
    sVar5.len = 0xf;
    sVar5.str = &DAT_004d5c16;
    a_08.len = 2;
    a_08.array = (interface {} *)&local_58;
    a_08.cap = 2;
    fmt::fmt.Fprintf(w_04,sVar5,a_08);
    bVar1 = main.s();
    if (!bVar1) {
      message.len = 0x2e;
      message.str = (uint8 *)"Mathematical precision required! Try again! ;)";
      main.j(message);
    }
    local_68 = &string___String_type;
    local_60 = &PTR_DAT_004fd910;
    w_02.data = os.Stdout;
    w_02.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
    a_02.len = 1;
    a_02.array = (interface {} *)&local_68;
    a_02.cap = 1;
    fmt::fmt.Fprintln(w_02,a_02);
    local_78 = &string___String_type;
    local_70 = &PTR_DAT_004fd920;
    w_03.data = os.Stdout;
    w_03.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
    a_03.len = 1;
    a_03.array = (interface {} *)&local_78;
    a_03.cap = 1;
    fmt::fmt.Fprintln(w_03,a_03);
  }
  local_88 = &string___String_type;
  local_80 = &PTR_DAT_004fd930;
  w_05.data = os.Stdout;
  w_05.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_04.len = 1;
  a_04.array = (interface {} *)&local_88;
  a_04.cap = 1;
  fmt::fmt.Fprintln(w_05,a_04);
  local_98 = &string___String_type;
  local_90 = &PTR_DAT_004fd900;
  w_06.data = os.Stdout;
  w_06.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_05.len = 1;
  a_05.array = (interface {} *)&local_98;
  a_05.cap = 1;
  fmt::fmt.Fprintln(w_06,a_05);
  local_a8 = &string___String_type;
  local_a0 = &PTR_DAT_004fd940;
  w_07.data = os.Stdout;
  w_07.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_06.len = 1;
  a_06.array = (interface {} *)&local_a8;
  a_06.cap = 1;
  fmt::fmt.Fprint(w_07,a_06);
  sVar5 = main.g();
  main.h(sVar5);
  bVar1 = main.i(sVar5);
  if (!bVar1) {
    message_00.len = 0x1d;
    message_00.str = (uint8 *)"Invalid checksum signature...";
    main.j(message_00);
  }
  local_b8 = &string___String_type;
  local_b0 = &PTR_DAT_004fd950;
  w_08.data = os.Stdout;
  w_08.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_07.len = 1;
  a_07.array = (interface {} *)&local_b8;
  a_07.cap = 1;
  fmt::fmt.Fprintln(w_08,a_07);
  main.k(sVar5);
  return;
}
</pre>
Đầu tiên, hàm sẽ tạo ra 5 đến 10 phép toán ngẫu nhiên dùng hàm main.s. Nếu trả lời đúng hêt các phép toán thì tiếp tục đến bước nhập checksum, nếu sai thì hàm sẽ gọi main.j để thoát với thông báo lỗi
<img width="1458" height="134" alt="image" src="https://github.com/user-attachments/assets/e75ba014-2f0c-4a90-9ea0-b0fe0deb5a4d" />
Vậy giờ ta cần tìm chuỗi checksum đúng, phân tích hàm tiếp ta thấy có hàm ```bash bVar1 = main.i(sVar5);``` để kiểm tra checksum. Nếu đúng thì chương trình sẽ chạy tiếp hàm main.k
Giờ ta hãy phân tích hàm main.i để xem điều kiện của checksum là gì
### hàm main.i
<pre lang="markdown"> 
bool main::main.i(string input)
{
  bool bVar1;
  unsafe.Pointer pvVar2;
  uint x;
  uintptr *puVar3;
  int len;
  int iVar4;
  string sVar5;
  multireturn{[]uint8;error} mVar6;
  []uint8 src;
  string input_spill;
  
  iVar4 = input.len;
  while (&stack0x00000000 <= CURRENT_G.stackguard0) {
    runtime::runtime.morestack_noctxt();
  }
  if (iVar4 == 0) {
    return false;
  }
  mVar6 = encoding/hex::encoding/hex.DecodeString(input);
  puVar3 = mVar6.~r0.array;
  len = mVar6.~r0.len;
  if ((mVar6.~r1.tab != (internal/abi.ITab *)0x0) &&
     (puVar3 = (uintptr *)input.str, len = iVar4, (uintptr *)input.str == (uintptr *)0x0)) {
    puVar3 = &runtime.zerobase;
  }
  pvVar2 = runtime::runtime.makeslice((internal/abi.Type *)&uint8___Uint8_type,len,len);
  iVar4 = 0;
  while( true ) {
    if (len <= iVar4) {
      src.len = len;
      src.array = (uint8 *)pvVar2;
      src.cap = len;
      sVar5 = encoding/base64::encoding/base64.(*Encoding).EncodeToString
                        ((encoding/base64.Encoding *)encoding/base64.StdEncoding,src);
      if (sVar5.len == 0x58) {
        bVar1 = runtime::runtime.memequal
                          (sVar5.str,
                           "ZNc6dY7k+caeaHof3F08RsFslfKEUTmoxaYLzJ8jJm9M2r2HftI7UGCuKCm8pv1+lMCkFeBf O67vigYXHUAo2Q=="
                           ,0x58);
      }
      else {
        bVar1 = false;
      }
      return bVar1;
    }
    x = iVar4 + (iVar4 / 7 + (iVar4 >> 0x3f)) * -7;
    if (6 < x) break;
    *(byte *)((int)pvVar2 + iVar4) = *(byte *)((int)puVar3 + iVar4) ^ (&DAT_004d4399)[x];
    iVar4 = iVar4 + 1;
  }
                    /* WARNING: Subroutine does not return */
  runtime::runtime.panicIndex(x,iVar4);
}
</pre>
Hàm này kiểm tra xem chuỗi input nhập vào có hợp lệ hay không bằng cách giải mã chuỗi hex ta nhập thành 1 mảng các byte, mỗi byte sẽ bị XOR với key -> kết quả được mã hóa bằng base64 -> so sánh với chuỗi base64 ```bash ZNc6dY7k+caeaHof3F08RsFslfKEUTmoxaYLzJ8jJm9M2r2HftI7UGCuKCm8pv1+lMCkFeBf O67vigYXHUAo2Q==``` trong hàm. Ta có thể dịch ngược hàm, rồi viết script để tìm chuỗi input đúng
Trước tiên ta tìm XOR key từ ```bash &DAT_004d4399```
<img width="759" height="164" alt="image" src="https://github.com/user-attachments/assets/f6fa0f23-8738-4560-94a4-633949d5e6da" />
Viết script để lấy được chuỗi input đúng
<pre lang="markdown"> 
import base64

key = [0x56, 0x53, 0x4C, 0x32, 0x30, 0x32, 0x35]

target = "ZNc6dY7k+caeaHof3F08RsFslfKEUTmoxaYLzJ8jJm9M2r2HftI7UGCuKCm8pv1+lMCkFeBfO67vigYXHUAo2Q=="

xored = base64.b64decode(target)

decoded = bytearray()
for i in range(len(xored)):
    decoded.append(xored[i] ^ key[i % 7])

hex_input = decoded.hex()
print(hex_input)
</pre>
Và ta được 1 chuỗi như này
<img width="1194" height="51" alt="image" src="https://github.com/user-attachments/assets/61441190-876f-4e6f-b299-857a464a219a" />
Nhập chuỗi này vào chương trình thì ta được thông báo thành công
<img width="1442" height="92" alt="image" src="https://github.com/user-attachments/assets/72b15e3b-0c3c-43e8-b154-9b83e89d3147" />
Phân tích hàm main.k ta thấy nếu nhập checksum đúng thì chương trình sẽ in ra 1 bức ảnh ```bash sVar9.str = (uint8 *)"READ_VSL_FLAG_%s.png";```
Mở bức ảnh này trong đường dẫn cùng với checksum.exe ta được flag
<img width="1416" height="714" alt="image" src="https://github.com/user-attachments/assets/744db231-9949-4052-a018-34766a84602c" />
VSL{X0r_1s_Th3_K3y_And_M4th_Rul3z_6722}









 
