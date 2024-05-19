## Multiplicacion  de dos numeros enteros, usando  registros de 8 bits

Solucion:
```

section .data
    num1 db 4
    num2 db 5

segment .bss
    resultado resb 2

section .text


global _start

_start:

    mov al, [num1];mover el valor de num1 al registro al
    mov bl, [num2]; mover el valor de num2 al registro bl
    mul bl; implicitament multiplica los registros al * bl
    mov ah,0 
    mov bl, 10; 
    div bl
    
    add al, '0';convierte de ascii a numero
    add ah, '0'
    ;asignacion del valor a resultado
    mov [resultado], al
    mov [resultado +1], ah
    ;mostrar en pantalla
    mov eax,4
    mov ebx,1
    mov ecx, resultado
    mov edx, 2
    int 0x80
    ;finalizar ejecucion
    mov eax,1
    mov ebx,0
    int 0x80
    
````
**Nota**: el ejercicio funciona con operandos (num1 y num2) multiplicando y multiplicador en un rango de 1-9, caso contrario el resultado no sera el correcto.

## Division  de dos numeros enteros, usando registros de 32 bits

Solución:

````
section .data
    num1 dd 50
    num2 dd 5
    cociente dd 0
    msg db "Cociente: ", 0

section .bss
    cociente_str resb 12

section .text
    global _start

_start:
    ;asignar valores a los registros
    mov eax, [num1]
    mov ebx, [num2]

    ;Proceso de division
    mov edx, 0     
    div ebx     

    ; Guardar el cociente y el residuo en variables
    mov [cociente], eax   ; Guardar el cociente
 
    ; Imprimir el mensaje
    mov eax, 4
    mov ebx, 1
    mov ecx, msg
    mov edx, 10
    int 0x80

    ; Convertir el cociente a cadena
    mov eax, [cociente]
    mov edi, eax
    mov ebx, cociente_str
    call int_to_string

    ; Imprimir el cociente
    mov eax, 4
    mov ebx, 1
    mov ecx, cociente_str
    mov edx, 12
    int 0x80
    
    ; Salir del programa
    call finalizar

int_to_string:
    mov esi, edi    
    mov ecx, 10     
    mov edi, 0    
    mov ebp, ebx    

loop:
    mov edx, 0    
    div ecx         
    add dl, '0'     
    push edx        
    inc edi         
    test eax, eax   
    jnz loop        

    mov ecx, edi    
    mov edi, ebp    

pop_loop:
    pop eax         
    stosb           
    loop pop_loop  
    mov byte [edi], 0  

    ret           
    
finalizar:
    mov eax, 1
    mov ebx, 0
    int 0x80
````
**Nota:**
- La rutina **int_to_string** llevaron a la busqueda de registros de proposito general(esi, edi) para el uso de datos temporales

-La rutina **loop**: basicamente busca convertir un numero a su equivalente en ascii, basandose en el uso de registro de 8 bits como lo es dl.

-La rutina  **pop_loop**:Elimina el ultimo elemento de la pila.La razón por la cual se elimina el último elemento de la pila (pop eax) en el bucle porque el último dígito convertido es el primer dígito que se desapiló.


Nombre: José Franklin Angel Guevara 
GT: 01
Carnet AG19030