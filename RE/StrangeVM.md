# StrangeVM
## Mô tả challenge
<img width="627" height="573" alt="image" src="https://github.com/user-attachments/assets/f78be957-12e3-43e8-9f90-476e66f90b04" />

## Solution

## Challenge Overview
Challenge cung cấp:
- File thực thi: `vm`
- File bytecode: `code.pascal`
`vm` đọc bytecode từ `code.pascal`, xử lý input người dùng, sau đó so sánh kết quả với flag thật được lưu sẵn trong chương trình.
## Phân tích hàm `main`
Mình sẽ sử dụng IDA để phân tích hàm main
```c
initVM();
executeVM();
if (!strcmp(mem, flag))
    puts("Congratulations!");
```
Ta thấy có 2 hàm
executeVM()
→ Toàn bộ logic xử lý và kiểm tra flag nằm hàm này
strcmp(mem, flag)
→ So sánh kết quả sau khi VM chạy với flag thật
Trước tiên, ta hãy dump flag trong .rodata
<img width="559" height="24" alt="image" src="https://github.com/user-attachments/assets/8ff4cb0f-e766-43c4-bfe6-3b8e7df42589" />
Tại địa chỉ unk_4A0278:
<pre lang ="markdown">
.rodata:00000000004A0278 unk_4A0278      db  56h ; V             ; DATA XREF: .data:flag↓o
.rodata:00000000004A0279                 db  4Ch ; L
.rodata:00000000004A027A                 db  75h ; u
.rodata:00000000004A027B                 db  5Ch ; \
.rodata:00000000004A027C                 db  38h ; 8
.rodata:00000000004A027D                 db  6Dh ; m
.rodata:00000000004A027E                 db  39h ; 9
.rodata:00000000004A027F                 db  58h ; X
.rodata:00000000004A0280                 db  6Ch ; l
.rodata:00000000004A0281                 db  28h ; (
.rodata:00000000004A0282                 db  3Eh ; >
.rodata:00000000004A0283                 db  57h ; W
.rodata:00000000004A0284                 db  7Bh ; {
.rodata:00000000004A0285                 db  5Fh ; _
.rodata:00000000004A0286                 db  3Fh ; ?
.rodata:00000000004A0287                 db  54h ; T
.rodata:00000000004A0288                 db  44h ; D
.rodata:00000000004A0289                 db  5Bh ; [
.rodata:00000000004A028A                 db  71h ; q
.rodata:00000000004A028B                 db  20h
.rodata:00000000004A028C                 db  82h
.rodata:00000000004A028D                 db  1Bh
.rodata:00000000004A028E                 db  8Bh
.rodata:00000000004A028F                 db  50h ; P
.rodata:00000000004A0290                 db  80h
.rodata:00000000004A0291                 db  46h ; F
.rodata:00000000004A0292                 db  7Eh ; ~
.rodata:00000000004A0293                 db  15h
.rodata:00000000004A0294                 db  8Ah
.rodata:00000000004A0295                 db  57h ; W
.rodata:00000000004A0296                 db  7Dh ; }
.rodata:00000000004A0297                 db  5Ah ; Z
.rodata:00000000004A0298                 db  50h ; P
.rodata:00000000004A0299                 db  54h ; T
.rodata:00000000004A029A                 db  81h
.rodata:00000000004A029B                 db  51h ; Q
.rodata:00000000004A029C                 db  8Ch
.rodata:00000000004A029D                 db  0Ch
.rodata:00000000004A029E                 db  94h
.rodata:00000000004A029F                 db  44h ; D
.rodata:00000000004A02A0                 db    0
</pre>
=> target = [
    0x56, 0x4C, 0x75, 0x5C, 0x38, 0x6D, 0x39, 0x58,
    0x6C, 0x28, 0x3E, 0x57, 0x7B, 0x5F, 0x3F, 0x54,
    0x44, 0x5B, 0x71, 0x20, 0x82, 0x1B, 0x8B, 0x50,
    0x80, 0x46, 0x7E, 0x15, 0x8A, 0x57, 0x7D, 0x5A,
    0x50, 0x54, 0x81, 0x51, 0x8C, 0x0C, 0x94, 0x44
]
## Phân tích hàm executeVM
Vòng lặp chính của VM
<pre lang = "markdown">
v21 = 0;
while (1) {
    opcode = code[v21];
    if (opcode == 0)
        return;
}
</pre>
VM đọc từng opcode trong code
Gặp opcode 0 → kết thúc thực thi
-> phân tích từng opcode
Opcode 5
<pre lang="markdown">
case 5:
    idx = readInt(code + v22);
    scanf("%c", mem + idx);
    v21 = v22 + 4;
</pre>
VM đọc từng ký tự input và lưu vào mem[idx]
Opcode 1
<pre lang="markdown">
case 1:
    idx = readInt(code + v22);
    mem[idx] += readByte(code + v22 + 4);
    v21 = v22 + 5;
</pre>
Lấy mem[idx] rồi cộng thêm một số cố định
Opcode 2
<pre lang="markdown">
case 2:
    idx = readInt(code + v22);
    mem[idx] -= readByte(code + v22 + 4);
    v21 = v22 + 5;
</pre>
Lấy mem[idx] rồi trừ đi một số cố định
-> So sánh với flag thật
mem sau khi VM chạy phải trùng với mảng byte đã dump trong địa chỉ ```bash unk_4A0278```
=> Nhận xét
Từ các opcode trên có thể thấy: VM thực hiện: Nhập ký tự sau đó cộng / trừ hằng số
=> ``` bash Output[i] = Input[i] + Delta[i] ```
Do đó ta có thể đảo ngược phép biến đổi.
## Khai thác bằng GDB
### Mở chương trình trong GDB
gdb ./vm
### Đặt breakpoint tại strcmp trong main
b *0x40221d
### Chạy chương trình
run code.pascal
### Nhập input mẫu
pascalCTF{AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA}
### Dump dữ liệu khi dừng breakpoint
####  Chuỗi target 
x/40xb $rsi
####  Chuỗi output sau VM
x/40xb $rdi
<img width="934" height="283" alt="image" src="https://github.com/user-attachments/assets/4ec17336-0e64-4b05-8392-4d5118a9fec4" />
### viết script giải mã
<pre lang="markdown">
target = [
    0x56, 0x4C, 0x75, 0x5C, 0x38, 0x6D, 0x39, 0x58,
    0x6C, 0x28, 0x3E, 0x57, 0x7B, 0x5F, 0x3F, 0x54,
    0x44, 0x5B, 0x71, 0x20, 0x82, 0x1B, 0x8B, 0x50,
    0x80, 0x46, 0x7E, 0x15, 0x8A, 0x57, 0x7D, 0x5A,
    0x50, 0x54, 0x81, 0x51, 0x8C, 0x0C, 0x94, 0x44
]

output_gdb = [
    0x70, 0x60, 0x75, 0x60, 0x65, 0x67, 0x49, 0x4D,
    0x4E, 0x72, 0x4B, 0x36, 0x4D, 0x34, 0x4F, 0x32,
    0x51, 0x30, 0x53, 0x2E, 0x55, 0x2C, 0x57, 0x2A,
    0x59, 0x28, 0x5B, 0x26, 0x5D, 0x24, 0x5F, 0x22,
    0x61, 0x20, 0x63, 0x1E, 0x65, 0x1C, 0x67, 0x1A
]

fake_input = "pascalCTF{AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA}"

flag = ""
for i in range(len(target)):
    delta = output_gdb[i] - ord(fake_input[i])
    flag += chr((target[i] - delta) % 256)

print("Flag:", flag)
</pre>
<img width="1588" height="715" alt="image" src="https://github.com/user-attachments/assets/e6872d8c-8a3f-4dbe-bf56-9f1921c001df" />


