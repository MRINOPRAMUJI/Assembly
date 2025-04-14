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
    mov ebx, 0          ; 0 = stdin
    mov ecx, userInput  ; simpan di sini
    mov edx, 100        ; baca max 100 byte
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
    mov edx, 100        ; tampilkan 100 byte (bisa kamu ubah sesuai input sebenarnya)
    int 0x80

    ; Keluar dari program
    mov eax, 1
    xor ebx, ebx
    int 0x80
  # Assembly
