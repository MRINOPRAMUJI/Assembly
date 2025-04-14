![image](https://github.com/user-attachments/assets/e2c52945-528d-47f8-a53b-b6847b8bde94)
![image](https://github.com/user-attachments/assets/d4b960ac-3c3e-4d37-8fb1-fd5494c10e46)

Penjelasan Program Assembly: Input dan Output (Versi Lengkap)
Program ini ditulis menggunakan bahasa Assembly (NASM) dan dijalankan pada sistem operasi Linux berbasis 32-bit. Tujuan dari program ini adalah untuk menerima input teks dari pengguna melalui keyboard, lalu menampilkannya kembali ke layar. Program menggunakan syscall Linux dengan interrupt `int 0x80` untuk melakukan operasi input/output dan keluar dari program.
Kode Program

section .data
    promptMsg       db  'Ketik sesuatu: ', 0xA
    promptLen       equ $ - promptMsg
    responseMsg     db  'Kamu mengetik: ', 0xA
    responseLen     equ $ - responseMsg

section .bss
    userInput resb  100     ; buffer untuk input huruf

section .text
    global _start

_start:
    ; Tampilkan prompt
    mov eax, 4
    mov ebx, 1
    mov ecx, promptMsg
    mov edx, promptLen
    int 0x80

    ; Baca dari keyboard (stdin)
    mov eax, 3
    mov ebx, 0
    mov ecx, userInput
    mov edx, 100
    int 0x80

    ; Tampilkan pesan respons
    mov eax, 4
    mov ebx, 1
    mov ecx, responseMsg
    mov edx, responseLen
    int 0x80

    ; Tampilkan kembali input dari user
    mov eax, 4
    mov ebx, 1
    mov ecx, userInput
    mov edx, 100
    int 0x80

    ; Keluar dari program
    mov eax, 1
    xor ebx, ebx
    int 0x80

Penjelasan Lengkap
1. Bagian `.data`:
   - Di bagian ini, kita menyimpan pesan-pesan teks tetap yang ingin ditampilkan ke layar.
   - `promptMsg` berisi string 'Ketik sesuatu: ' diikuti dengan `0xA` yang merupakan newline (enter).
   - `promptLen` menghitung panjang string `promptMsg` secara otomatis.
   - `responseMsg` adalah string yang akan ditampilkan sebelum hasil input pengguna.
   - `responseLen` juga menghitung panjang dari `responseMsg`.

2. Bagian `.bss`:
   - Bagian ini digunakan untuk mendeklarasikan variabel tanpa nilai awal.
   - Di sini kita menyediakan buffer bernama `userInput` sebesar 100 byte, yang akan menyimpan input dari keyboard.
3. Bagian `.text`:
   - Bagian ini adalah tempat instruksi utama program dijalankan.
   - `global _start` menunjukkan titik awal eksekusi program.
   - Pertama, program menampilkan `promptMsg` ke layar menggunakan syscall nomor 4 (write).
   - Kemudian, program membaca input dari keyboard (stdin) menggunakan syscall nomor 3 (read), dan hasilnya disimpan ke dalam buffer `userInput`.
   - Setelah itu, program menampilkan `responseMsg` sebagai respons.
   - Lalu, input yang telah dimasukkan oleh pengguna akan ditampilkan kembali.
   - Terakhir, program keluar menggunakan syscall nomor 1 (exit).
Cara Menjalankan Program
Ikuti langkah-langkah berikut untuk meng-compile dan menjalankan program di sistem Linux 32-bit:

1. Simpan kode program ke dalam file dengan nama `input.asm`
2. Buka terminal, lalu jalankan perintah-perintah berikut:
   - `nasm -f elf32 input.asm -o input.o` → Untuk mengompilasi kode sumber menjadi object file.
   - `ld -m elf_i386 input.o -o input` → Untuk melakukan linking dan menghasilkan file executable.
   - `./input` → Untuk menjalankan program.

Pastikan NASM dan linker (ld) telah terinstal di sistem kamu. Jika kamu menggunakan sistem 64-bit, kamu perlu memastikan bahwa dukungan 32-bit tersedia.
