# Compute Sum (Question 1)

This is a assembly program that computes the sum of 1 and 2 and prints the result to stdout

## Build

nasm -f elf32 sum.asm -o sum.o
ld -m elf_i386 sum.o -o sum

## Run

./sum

## Source code

section .data
    buffer db 0
    nl db 10

section .text
    global _start

_start:
   
    mov eax, 1
    add eax, 2        

    
    add eax, '0'      
    mov [buffer], al  

    ; Print result
    mov eax, 4        
    mov ebx, 1       
    mov ecx, buffer   
    mov edx, 1        
    int 0x80

    ; Print newline
    mov eax, 4
    mov ebx, 1
    mov ecx, nl
    mov edx, 1
    int 0x80

    ; Exit
    mov eax, 1
    xor ebx, ebx
    int 0x80

## Output

3

---

# Capitalize String (Question 2) 

This is a assembly program that reads a line from standard input, converts all lowercase letters to uppercase, and prints the result.

## Build

nasm -f elf32 capt_string.asm -o capt_string.o
ld -m elf_i386 capt_string.o -o string

## Run

./string

## Source code

section .data
    newline db 10

section .bss
    buffer resb 100
    len    resd 1

section .text
    global _start

_start:
    ; Read input
    mov eax, 3
    mov ebx, 0
    mov ecx, buffer
    mov edx, 100
    int 0x80

    cmp eax, 0
    jle exit_program

    mov [len], eax
    mov edi, eax
    mov esi, buffer

capitalize_loop:
    cmp edi, 0
    je print_result

    mov al, [esi]

    cmp al, 'a'
    jb skip_char
    cmp al, 'z'
    ja skip_char
    sub al, 0x20
    mov [esi], al

skip_char:
    inc esi
    dec edi
    jmp capitalize_loop

print_result:
    mov eax, 4
    mov ebx, 1
    mov ecx, buffer
    mov edx, [len]
    int 0x80

    mov eax, 4
    mov ebx, 1
    mov ecx, newline
    mov edx, 1
    int 0x80

exit_program:
    mov eax, 1
    xor ebx, ebx
    int 0x80
## Output

manipal information security team
MANIPAL INFORMATION SECURITY TEAM
