![Gambar WhatsApp 2025-04-14 pukul 14 37 32_8559536e](https://github.com/user-attachments/assets/70376a57-d8b4-4927-9762-c2d27e8f5d13)
![Gambar WhatsApp 2025-04-14 pukul 14 37 55_75e91fa1](https://github.com/user-attachments/assets/e160dc5a-c76c-4725-9874-3c56182166df)
; ============================================
; Program: Input dan Tampilkan Kembali (x86 Assembly)
; Bahasa: NASM (Intel Syntax)
; Platform: Linux 32-bit
; Deskripsi: Program ini meminta pengguna mengetik sesuatu,
;           lalu menampilkannya kembali ke layar.
; ============================================

section .data
    promptMsg       db  'Ketik sesuatu: ', 0xA          ; Pesan prompt (dengan newline)
    promptLen       equ $ - promptMsg                   ; Panjang pesan prompt

    responseMsg     db  'Kamu mengetik: ', 0xA          ; Pesan respons
    responseLen     equ $ - responseMsg                 ; Panjang pesan respons

section .bss
    userInput       resb 100                            ; Alokasi buffer untuk input (100 byte)

section .text
    global _start

_start:
    ; --------------------------------------------
    ; 1. Tampilkan pesan prompt ke layar
    ; syscall: write(fd=1, buf=promptMsg, len=promptLen)
    ; --------------------------------------------
    mov eax, 4              ; syscall number untuk write
    mov ebx, 1              ; file descriptor 1 (stdout)
    mov ecx, promptMsg      ; alamat buffer pesan
    mov edx, promptLen      ; panjang pesan
    int 0x80                ; panggil kernel

    ; --------------------------------------------
    ; 2. Baca input dari keyboard (stdin)
    ; syscall: read(fd=0, buf=userInput, len=100)
    ; --------------------------------------------
    mov eax, 3              ; syscall number untuk read
    mov ebx, 0              ; file descriptor 0 (stdin)
    mov ecx, userInput      ; simpan input di sini
    mov edx, 100            ; maksimal 100 byte
    int 0x80                ; panggil kernel

    ; --------------------------------------------
    ; 3. Tampilkan pesan "Kamu mengetik:"
    ; syscall: write(fd=1, buf=responseMsg, len=responseLen)
    ; --------------------------------------------
    mov eax, 4
    mov ebx, 1
    mov ecx, responseMsg
    mov edx, responseLen
    int 0x80

    ; --------------------------------------------
    ; 4. Tampilkan kembali input yang diketik user
    ; syscall: write(fd=1, buf=userInput, len=100)
    ; --------------------------------------------
    mov eax, 4
    mov ebx, 1
    mov ecx, userInput
    mov edx, 100            ; tampilkan 100 byte (termasuk newline)
    int 0x80

    ; --------------------------------------------
    ; 5. Keluar dari program
    ; syscall: exit(code=0)
    ; --------------------------------------------
    mov eax, 1              ; syscall number untuk exit
    xor ebx, ebx            ; exit code 0
    int 0x80
