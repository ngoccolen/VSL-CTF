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
executeVM()
→ Toàn bộ logic xử lý và kiểm tra flag nằm ở đây
strcmp(mem, flag)
→ So sánh kết quả sau khi VM chạy với flag thật
## Phân tích hàm executeVM
Vòng lặp chính của VM
v21 = 0;
while (1) {
    opcode = code[v21];
    if (opcode == 0)
        return;
    ...
}
VM đọc từng opcode trong code
Gặp opcode 0 → kết thúc thực thi
Opcode 5
case 5:
    idx = readInt(code + v22);
    scanf("%c", mem + idx);
    v21 = v22 + 4;
VM đọc từng ký tự input và lưu vào mem[idx]
Opcode 1
case 1:
    idx = readInt(code + v22);
    mem[idx] += readByte(code + v22 + 4);
    v21 = v22 + 5;
Lấy mem[idx] rồi cộng thêm một số cố định
Opcode 2
case 2:
    idx = readInt(code + v22);
    mem[idx] -= readByte(code + v22 + 4);
    v21 = v22 + 5;
Lấy mem[idx] rồi trừ đi một số cố định
So sánh với flag thật
Sau khi executeVM kết thúc:
strcmp(mem, flag);
Flag thật được lưu trong .rodata:
.rodata:00000000004A0278
56 4C 75 5C 38 6D 39 58 ...
mem sau khi VM chạy phải trùng với mảng byte này.
Nhận xét
Từ các opcode trên có thể thấy:
VM thực hiện:
Nhập ký tự
Cộng / trừ hằng số
Output[i] = Input[i] + Delta[i]
Do đó có thể đảo ngược phép biến đổi.
7. Khai thác bằng GDB
7.1 Mở chương trình trong GDB
gdb ./vm
7.2 Đặt breakpoint tại strcmp trong main
b *0x40221d
7.3 Chạy chương trình
run code.pascal
7.4 Nhập input mẫu
pascalCTF{AAAAAAAAAAAAAAAAAAAAAAAAAAAAAA}
7.5 Dump dữ liệu khi dừng breakpoint
# Chuỗi target (flag thật)
x/40xb $rsi
# Chuỗi output sau VM
x/40xb $rdi
8. Giải mã flag bằng Python
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


