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
  piVar3 = (internal/abi.Type *)0x0;
  uVar4 = 0;
  while (&local_90 <= CURRENT_G.stackguard0) {
    runtime::runtime.morestack_noctxt();
  }
  tVar6 = time::time.Now();
  iVar2 = tVar6.ext;
  if ((int)tVar6.wall < 0) {
    iVar2 = ((tVar6.wall << 1) >> 0x1f) + 0xdd7b17f80;
  }
  math/rand::math/rand.Seed
            ((int)(sdword)((dword)tVar6.wall & 0x3fffffff) + iVar2 * 1000000000 +
             -0x5e4dfc14c2e60000);
  local_18 = &string___String_type;
  local_10 = &PTR_DAT_004fd8e0;
  w.data = os.Stdout;
  w.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a.len = 1;
  a.array = (interface {} *)&local_18;
  a.cap = 1;
  fmt::fmt.Fprintln(w,a);
  local_28 = &string___String_type;
  local_20 = &PTR_DAT_004fd8f0;
  w_00.data = os.Stdout;
  w_00.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_00.len = 1;
  a_00.array = (interface {} *)&local_28;
  a_00.cap = 1;
  fmt::fmt.Fprintln(w_00,a_00);
  local_38 = &string___String_type;
  local_30 = &PTR_DAT_004fd900;
  w_01.data = os.Stdout;
  w_01.tab = (io.Writer_itab *)&os::*os.File__implements__io.Writer__itab;
  a_01.len = 1;
  a_01.array = (interface {} *)&local_38;
  a_01.cap = 1;
  fmt::fmt.Fprintln(w_01,a_01);
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



 
