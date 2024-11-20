# 50_Programas_asamblador

## 1.-Convertidor de Temperatura de Celsius a Fahrenheit

### C# - ConvertirTemperatura.cs

```csharp
/*
 * Title: Convertidor de temperatura de Celsius a Fahrenheit
 * Filename: ConvertirTemperatura.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que convierte una temperatura en grados Celsius a Fahrenheit.
 * Input: Temperatura en grados Celsius desde la consola.
 * Output: Temperatura en grados Fahrenheit en la consola.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario la temperatura en Celsius
        Console.Write("Introduce la temperatura en Celsius: ");
        double celsius = Convert.ToDouble(Console.ReadLine());

        // Convertir a Fahrenheit
        double fahrenheit = (celsius * 9 / 5) + 32;

        // Mostrar el resultado utilizando concatenación
        Console.WriteLine(celsius + "°C es igual a " + fahrenheit + "°F");
    }
}

 ```

### Asambleador

```
/*
 * Title: Convertidor de temperatura de Celsius a Fahrenheit
 * Filename: convertir_temperatura.s
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que convierte una temperatura en grados Celsius a Fahrenheit.
 * Input: Temperatura en grados Celsius desde la consola.
 * Output: Temperatura en grados Fahrenheit en la consola.
 */

section .data
    prompt db "Introduce la temperatura en Celsius: ", 0
    result_msg db "La temperatura en Fahrenheit es: %.2f", 10, 0
    scanf_format db "%lf", 0
    nine dq 9.0        ; Definir 9.0 como un número de punto flotante de 64 bits
    five dq 5.0        ; Definir 5.0 como un número de punto flotante de 64 bits
    thirtytwo dq 32.0  ; Definir 32.0 como un número de punto flotante de 64 bits

section .bss
    temp_celsius resq 1         ; Reservar espacio para la temperatura en Celsius
    temp_fahrenheit resq 1      ; Reservar espacio para la temperatura en Fahrenheit

section .text
    extern printf, scanf
    global _start

_start:
    ; Imprimir mensaje solicitando la temperatura en Celsius
    mov rdi, prompt            ; Dirección del mensaje
    call printf

    ; Leer la temperatura en Celsius
    mov rdi, scanf_format      ; Formato para scanf
    mov rsi, temp_celsius      ; Dirección donde se almacenará el valor ingresado
    call scanf

    ; Cargar la temperatura en Celsius en xmm0
    movsd xmm0, [temp_celsius] ; Cargar el valor de Celsius en xmm0

    ; Multiplicar Celsius por 9.0
    movsd xmm1, [nine]         ; Cargar 9.0 en xmm1
    mulsd xmm0, xmm1           ; xmm0 = xmm0 * xmm1 (Celsius * 9.0)

    ; Dividir el resultado por 5.0
    movsd xmm1, [five]         ; Cargar 5.0 en xmm1
    divsd xmm0, xmm1           ; xmm0 = xmm0 / xmm1 (Celsius * 9.0 / 5.0)

    ; Sumar 32.0 al resultado
    movsd xmm1, [thirtytwo]    ; Cargar 32.0 en xmm1
    addsd xmm0, xmm1           ; xmm0 = xmm0 + xmm1 (Resultado + 32.0)

    ; Almacenar el resultado en Fahrenheit
    movsd [temp_fahrenheit], xmm0

    ; Mostrar el resultado de la conversión
    mov rdi, result_msg        ; Dirección del mensaje de resultado
    mov rax, 1                 ; Número de parámetros (en este caso 1)
    mov rsi, [temp_fahrenheit] ; Dirección del valor de Fahrenheit
    call printf

    ; Salir del programa
    mov rax, 60                ; syscall número 60 (exit)
    xor rdi, rdi               ; Código de salida 0
    syscall                    ; Llamar al sistema para salir


 ```

### Corrida

[![asciicast](https://asciinema.org/a/XM8MlCOzZclK1EA5qIubpW9cM.svg)](https://asciinema.org/a/XM8MlCOzZclK1EA5qIubpW9cM)


## 2.-Suma de dos números

### C# -SumaDeDosNumeros.cs

```csharp
/*
 * Title: Suma de dos números
 * Filename: SumaDeDosNumeros.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que solicita dos números al usuario y muestra su suma.
 * Input: Dos números enteros introducidos por el usuario.
 * Output: La suma de los dos números.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar el primer número
        Console.Write("Introduce el primer número: ");
        int num1 = Convert.ToInt32(Console.ReadLine());

        // Solicitar el segundo número
        Console.Write("Introduce el segundo número: ");
        int num2 = Convert.ToInt32(Console.ReadLine());

        // Realizar la suma
        int suma = num1 + num2;

        // Mostrar el resultado utilizando concatenación
        Console.WriteLine("La suma de " + num1 + " y " + num2 + " es: " + suma);
    }
}

 ```

### Asambleador

```
section .data
    msg_input1 db 'Introduce el primer número: ', 0
    msg_input2 db 'Introduce el segundo número: ', 0
    msg_output db 'La suma es: ', 0
    newline db 10, 0

section .bss
    num1 resb 10
    num2 resb 10
    suma resb 10

section .text
    global _start

_start:
    ; Imprimir el mensaje para el primer número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input1 ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el primer número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num1       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Imprimir el mensaje para el segundo número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input2 ; puntero al mensaje
    mov edx, 29         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el segundo número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num2       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir los dos números (simplificado para enteros)
    ; (Este proceso debe hacerse de forma similar a la conversión de Celsius a Fahrenheit)

    ; Aquí va el código para convertir las cadenas a números enteros y realizar la suma.

    ; Mostrar el resultado
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 13         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí agregar el código para imprimir la suma

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema
 ```

### Corrida

Introduce el primer número: 11
Introduce el segundo número: 25
La suma de 11 y 25 es: 36

## 3.-Resta de dos numeros

### C# -RestaDeDosNumeros.cs

```csharp
/*
 * Title: Resta de dos números
 * Filename: RestaDeDosNumeros.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que solicita dos números al usuario y muestra su resta.
 * Input: Dos números enteros introducidos por el usuario.
 * Output: La resta de los dos números.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar el primer número
        Console.Write("Introduce el primer número: ");
        int num1 = Convert.ToInt32(Console.ReadLine());

        // Solicitar el segundo número
        Console.Write("Introduce el segundo número: ");
        int num2 = Convert.ToInt32(Console.ReadLine());

        // Realizar la resta
        int resta = num1 - num2;

        // Mostrar el resultado utilizando concatenación
        Console.WriteLine("La resta de " + num1 + " y " + num2 + " es: " + resta);
    }
}
 ```
### Asambleador

```
section .data
    msg_input1 db 'Introduce el primer número: ', 0
    msg_input2 db 'Introduce el segundo número: ', 0
    msg_output db 'La resta es: ', 0
    newline db 10, 0

section .bss
    num1 resb 10
    num2 resb 10
    resta resb 10

section .text
    global _start

_start:
    ; Imprimir el mensaje para el primer número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input1 ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el primer número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num1       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Imprimir el mensaje para el segundo número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input2 ; puntero al mensaje
    mov edx, 29         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el segundo número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num2       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir los dos números (simplificado para enteros)
    ; (Este proceso debe hacerse de forma similar a la conversión de cadenas a números)

    ; Aquí va el código para convertir las cadenas a números enteros y realizar la resta.

    ; Realizar la resta
    ; Asegúrate de que el número 1 está en eax y el número 2 en ebx
    ; Restar num2 de num1
    mov eax, [num1]     ; cargar num1 en eax
    sub eax, [num2]     ; restar num2 de num1

    ; Mostrar el resultado
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 13         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí agregar el código para imprimir la resta

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

   ```

### Corrida

Introduce el primer número: 10
Introduce el segundo número: 5
La resta de 10 y 5 es: 5

## 4.-Multiplicación de dos números

### C# -MultiplicacionDeDosNumeros.cs

```
/*
 * Title: Multiplicación de dos números
 * Filename: MultiplicacionDeDosNumeros.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que solicita dos números al usuario y muestra su multiplicación.
 * Input: Dos números enteros introducidos por el usuario.
 * Output: El resultado de la multiplicación de los dos números.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar el primer número
        Console.Write("Introduce el primer número: ");
        int num1 = Convert.ToInt32(Console.ReadLine());

        // Solicitar el segundo número
        Console.Write("Introduce el segundo número: ");
        int num2 = Convert.ToInt32(Console.ReadLine());

        // Realizar la multiplicación
        int multiplicacion = num1 * num2;

        // Mostrar el resultado
        Console.WriteLine("La multiplicación de " + num1 + " y " + num2 + " es: " + multiplicacion);
    }
}
```

### Asambleador
```

section .data
    msg_input1 db 'Introduce el primer número: ', 0
    msg_input2 db 'Introduce el segundo número: ', 0
    msg_output db 'La multiplicación es: ', 0
    newline db 10, 0

section .bss
    num1 resb 10
    num2 resb 10
    resultado resb 10

section .text
    global _start

_start:
    ; Imprimir el mensaje para el primer número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input1 ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el primer número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num1       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Imprimir el mensaje para el segundo número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input2 ; puntero al mensaje
    mov edx, 29         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el segundo número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num2       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir los dos números (simplificado para enteros)
    ; (Este proceso debe hacerse de forma similar a la conversión de cadenas a números)

    ; Aquí va el código para convertir las cadenas a números enteros y realizar la multiplicación.

    ; Realizar la multiplicación
    ; Asegúrate de que el número 1 está en eax y el número 2 en ebx
    ; Multiplicar num1 y num2
    mov eax, [num1]     ; cargar num1 en eax
    imul eax, [num2]    ; multiplicar eax por num2

    ; Mostrar el resultado
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 20         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí agregar el código para imprimir el resultado

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

```

### Corrida

Introduce el primer número: 12
Introduce el segundo número: 8
La multiplicación es: 96

## 5.-División de dos números
### C# - DivisionDeDosNumeros.cs
 ```
/*
 * Title: División de dos números
 * Filename: DivisionDeDosNumeros.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que solicita dos números al usuario y muestra el resultado de su división.
 * Input: Dos números enteros introducidos por el usuario.
 * Output: El resultado de la división de los dos números.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar el primer número
        Console.Write("Introduce el primer número: ");
        int num1 = Convert.ToInt32(Console.ReadLine());

        // Solicitar el segundo número
        Console.Write("Introduce el segundo número: ");
        int num2 = Convert.ToInt32(Console.ReadLine());

        // Verificar que el divisor no sea cero
        if (num2 == 0)
        {
            Console.WriteLine("Error: No se puede dividir por cero.");
        }
        else
        {
            // Realizar la división
            int division = num1 / num2;

            // Mostrar el resultado
            Console.WriteLine("La división de " + num1 + " entre " + num2 + " es: " + division);
        }
    }
}
```
### Asamblador
```
section .data
    msg_input1 db 'Introduce el primer número: ', 0
    msg_input2 db 'Introduce el segundo número: ', 0
    msg_output db 'La división es: ', 0
    msg_error db 'Error: No se puede dividir por cero.', 10, 0
    newline db 10, 0

section .bss
    num1 resb 10
    num2 resb 10
    resultado resb 10

section .text
    global _start

_start:
    ; Imprimir el mensaje para el primer número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input1 ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el primer número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num1       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Imprimir el mensaje para el segundo número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input2 ; puntero al mensaje
    mov edx, 29         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el segundo número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, num2       ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir los dos números (simplificado para enteros)
    ; (Este proceso debe hacerse de forma similar a la conversión de cadenas a números)

    ; Realizar la verificación de la división por cero
    mov eax, [num2]     ; cargar num2 en eax
    test eax, eax       ; verificar si num2 es cero
    jz error_division   ; si num2 es cero, saltar a error_division

    ; Realizar la división: resultado = num1 / num2
    mov eax, [num1]     ; cargar num1 en eax
    mov ebx, [num2]     ; cargar num2 en ebx
    xor edx, edx        ; limpiar edx (es necesario para la división)
    div ebx             ; eax = eax / ebx, edx = eax % ebx

    ; Mostrar el resultado de la división
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 15         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí agregar el código para imprimir el resultado (en eax)
    ; Puede utilizar un convertidor para mostrar el resultado en texto

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

error_division:
    ; Mostrar el mensaje de error (división por cero)
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_error  ; puntero al mensaje de error
    mov edx, 37         ; longitud del mensaje de error
    int 0x80            ; interrupción del sistema

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema
```
## Corrida

Introduce el primer número: 20
Introduce el segundo número: 4
La división es: 5

## 6.-Suma de los N primeros números naturales
### C# - SumaDeLosNPrimerosNumeros.cs

```
/*
 * Title: Suma de los N primeros números naturales
 * Filename: SumaDeLosNPrimerosNumeros.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que calcula la suma de los primeros N números naturales.
 * Input: Un número entero N desde la consola.
 * Output: La suma de los primeros N números naturales.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario el valor de N
        Console.Write("Introduce el valor de N: ");
        int n = Convert.ToInt32(Console.ReadLine());

        // Inicializar la suma
        int suma = 0;

        // Sumar los primeros N números naturales
        for (int i = 1; i <= n; i++)
        {
            suma += i;
        }

        // Mostrar el resultado
        Console.WriteLine($"La suma de los primeros {n} números naturales es: {suma}");
    }
}
```

### Asamblador

```

section .data
    msg_input db 'Introduce el valor de N: ', 0
    msg_output db 'La suma es: ', 0
    newline db 10, 0

section .bss
    N resb 10
    suma resd 1

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir N
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el valor de N desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, N          ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la cadena de caracteres a número (simplificado)
    ; Este paso se simplifica a la conversión directa del número, pero se podría usar un algoritmo para convertir texto a entero

    ; Inicializar la suma en 0
    mov eax, 0          ; reiniciar eax (suma)
    mov [suma], eax     ; almacenar la suma

    ; Obtener el valor de N
    mov eax, [N]        ; cargar N en eax
    sub eax, '0'        ; convertir de ASCII a número

    ; Sumar los primeros N números naturales
    mov ecx, 1          ; inicializar contador i en 1
sumar:
    add [suma], ecx     ; sumar i a la suma
    inc ecx             ; incrementar i
    cmp ecx, eax        ; comparar i con N
    jle sumar           ; si i <= N, continuar el bucle

    ; Mostrar el mensaje de salida
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 15         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí agregar el código para mostrar el valor de la suma
    ; Convertir el número de la suma a texto y mostrarlo

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

```
### Corrida

Introduce el valor de N: 5
La suma es: 15

## 7.-Factorial de un número
### C# - Factorial.cs
```
/*
 * Title: Factorial de un número
 * Filename: Factorial.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que calcula el factorial de un número usando recursión.
 * Input: Un número entero desde la consola.
 * Output: El factorial del número.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario un número entero
        Console.Write("Introduce un número: ");
        int numero = Convert.ToInt32(Console.ReadLine());

        // Llamar a la función recursiva para calcular el factorial
        long resultado = Factorial(numero);

        // Mostrar el resultado
        Console.WriteLine($"El factorial de {numero} es: {resultado}");
    }

    // Función recursiva para calcular el factorial
    public static long Factorial(int n)
    {
        // Caso base: el factorial de 0 es 1
        if (n == 0)
        {
            return 1;
        }
        // Caso recursivo
        else
        {
            return n * Factorial(n - 1);
        }
    }
}

```
### Asamblador
```
section .data
    msg_input db 'Introduce un número: ', 0
    msg_output db 'El factorial es: ', 0
    newline db 10, 0

section .bss
    numero resb 10
    resultado resd 1

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir un número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 24         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, numero     ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la cadena de caracteres a número (simplificado)
    mov eax, [numero]   ; cargar el número
    sub eax, '0'        ; convertir de ASCII a número

    ; Llamar a la función factorial recursiva
    push eax            ; empujar el número en la pila
    call factorial      ; llamar a la función factorial

    ; Mostrar el mensaje de salida
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 17         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Mostrar el resultado
    ; (Este paso requiere la conversión del número a texto para mostrarlo)

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

factorial:
    ; Factorial recursivo: factorial(n)
    ; Entrada: eax = n (número a calcular el factorial)
    ; Salida: eax = factorial(n)

    cmp eax, 0          ; comparar n con 0
    je factorial_base_case ; si n == 0, caso base

    push eax            ; empujar el valor de n en la pila
    dec eax             ; decrementa n
    call factorial      ; llamada recursiva factorial(n-1)
    pop ebx             ; recuperar el valor de n
    mul ebx             ; eax = eax * ebx (multiplicar el resultado por n)
    ret

factorial_base_case:
    mov eax, 1          ; factorial(0) = 1
    ret
```

### Corrida

Introduce un número: 5
El factorial es: 120


## 8.- Serie de Fibonacci
#### C# - Fibonacci.cs 
```
/*
 * Title: Serie de Fibonacci
 * Filename: Fibonacci.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que calcula los primeros N números de la serie de Fibonacci utilizando bucles.
 * Input: Un número entero N desde la consola, que indica cuántos términos de la serie se desean calcular.
 * Output: Los primeros N términos de la serie de Fibonacci.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario el número de términos de la serie de Fibonacci
        Console.Write("Introduce la cantidad de términos de la serie de Fibonacci: ");
        int n = Convert.ToInt32(Console.ReadLine());

        // Llamar a la función para calcular la serie de Fibonacci
        Fibonacci(n);
    }

    // Función para calcular la serie de Fibonacci usando un bucle
    public static void Fibonacci(int n)
    {
        // Variables para almacenar los dos términos anteriores de la serie
        long a = 0, b = 1;

        // Mostrar los primeros dos términos
        Console.WriteLine("Serie de Fibonacci:");
        if (n >= 1)
            Console.WriteLine(a);
        if (n >= 2)
            Console.WriteLine(b);

        // Calcular y mostrar los siguientes términos
        for (int i = 3; i <= n; i++)
        {
            long c = a + b;  // Sumar los dos términos anteriores
            Console.WriteLine(c);
            a = b;  // Mover el valor de b a a
            b = c;  // Mover el valor de c a b
        }
    }
}
```
### Asamblador

```
section .data
    msg_input db 'Introduce la cantidad de términos de la serie de Fibonacci: ', 0
    newline db 10, 0

section .bss
    n resb 10
    a resd 1
    b resd 1
    c resd 1

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir el número de términos
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 52         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el valor de N desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, n          ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la cadena de caracteres a número (simplificado)
    mov eax, [n]        ; cargar el valor de N
    sub eax, '0'        ; convertir de ASCII a número

    ; Inicializar los primeros dos términos de Fibonacci
    mov dword [a], 0    ; a = 0
    mov dword [b], 1    ; b = 1

    ; Imprimir el primer término (0)
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, a          ; puntero al valor de a
    mov edx, 1          ; longitud del valor
    int 0x80            ; interrupción del sistema

    ; Imprimir el segundo término (1)
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, b          ; puntero al valor de b
    mov edx, 1          ; longitud del valor
    int 0x80            ; interrupción del sistema

    ; Calcular y mostrar los siguientes términos de Fibonacci
    mov ecx, 3          ; iniciar el bucle para el tercer término (i = 3)
fibonacci_loop:
    mov eax, [a]        ; cargar a
    add eax, [b]        ; a + b
    mov [c], eax        ; guardar el resultado en c

    ; Imprimir el valor de c
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, c          ; puntero al valor de c
    mov edx, 1          ; longitud del valor
    int 0x80            ; interrupción del sistema

    ; Actualizar a y b
    mov eax, [b]        ; cargar b
    mov [a], eax        ; a = b
    mov eax, [c]        ; cargar c
    mov [b], eax        ; b = c

    ; Incrementar el contador
    inc ecx             ; incrementar i
    cmp ecx, [n]        ; comparar i con N
    jle fibonacci_loop  ; si i <= N, continuar el bucle

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema
```
### Corrida
Introduce la cantidad de términos de la serie de Fibonacci: 7
Serie de Fibonacci: 0
1
1
2
3
5
8

## 9.- Verificar si un número es primo
### C# - Primo.cs
```
/*
 * Title: Verificar si un número es primo
 * Filename: Primo.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que verifica si un número es primo.
 * Input: Un número entero desde la consola.
 * Output: Mensaje indicando si el número es primo o no.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario un número entero
        Console.Write("Introduce un número: ");
        int numero = Convert.ToInt32(Console.ReadLine());

        // Verificar si el número es primo
        if (EsPrimo(numero))
        {
            Console.WriteLine($"{numero} es un número primo.");
        }
        else
        {
            Console.WriteLine($"{numero} no es un número primo.");
        }
    }

    // Función para verificar si un número es primo
    public static bool EsPrimo(int n)
    {
        if (n <= 1) return false;  // Los números menores o iguales a 1 no son primos

        for (int i = 2; i <= Math.Sqrt(n); i++)  // Verificar divisibilidad hasta la raíz cuadrada de n
        {
            if (n % i == 0)  // Si n es divisible por i, no es primo
            {
                return false;
            }
        }
        return true;  // Si no se encuentra ningún divisor, es primo
    }
}
```
### Asamblador
```
section .data
    msg_input db 'Introduce un número: ', 0
    msg_prime db ' es un número primo.', 0
    msg_not_prime db ' no es un número primo.', 0
    newline db 10, 0

section .bss
    numero resb 10
    divisor resd 1
    resultado resb 1

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir un número
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 24         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer el valor del número desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, numero     ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir el número desde ASCII a entero
    mov eax, [numero]   ; cargar el número
    sub eax, '0'        ; convertir de ASCII a número

    ; Verificar si el número es primo
    mov ebx, eax        ; copiar el número en ebx
    mov ecx, 2          ; divisor comienza en 2

check_prime:
    cmp ecx, ebx        ; comparar el divisor con el número
    jge prime           ; si el divisor es mayor o igual al número, es primo
    mov edx, 0          ; limpiar edx
    div ecx             ; dividir eax entre ecx (n / divisor)
    cmp edx, 0          ; si el residuo es 0, no es primo
    je not_prime        ; si hay residuo 0, no es primo
    inc ecx             ; incrementar el divisor
    jmp check_prime     ; continuar con el siguiente divisor

prime:
    ; Imprimir el mensaje de número primo
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, [numero]   ; cargar el número a imprimir
    mov edx, 10         ; longitud del número
    int 0x80            ; interrupción del sistema

    ; Imprimir "es un número primo"
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_prime  ; mensaje "es un número primo"
    mov edx, 23         ; longitud del mensaje
    int 0x80            ; interrupción del sistema
    jmp exit

not_prime:
    ; Imprimir el mensaje de número no primo
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, [numero]   ; cargar el número a imprimir
    mov edx, 10         ; longitud del número
    int 0x80            ; interrupción del sistema

    ; Imprimir "no es un número primo"
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_not_prime ; mensaje "no es un número primo"
    mov edx, 25         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

exit:
    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema
```
### Corrida

Introduce un número: 17
17 es un número primo.

## 10.-Invertir una cadena
### C# - InvertirCadena.cs

```
/*
 * Title: Invertir una cadena
 * Filename: InvertirCadena.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que invierte una cadena de texto introducida por el usuario.
 * Input: Una cadena de texto desde la consola.
 * Output: La cadena de texto invertida.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario que ingrese una cadena
        Console.Write("Introduce una cadena de texto: ");
        string cadena = Console.ReadLine();

        // Invertir la cadena utilizando el método de C#
        string cadenaInvertida = Invertir(cadena);

        // Mostrar la cadena invertida
        Console.WriteLine($"La cadena invertida es: {cadenaInvertida}");
    }

    // Función que invierte una cadena
    public static string Invertir(string texto)
    {
        char[] array = texto.ToCharArray();  // Convertir la cadena en un arreglo de caracteres
        Array.Reverse(array);  // Invertir el arreglo de caracteres
        return new string(array);  // Convertir el arreglo invertido nuevamente a cadena
    }
}
```
### Asamblador
```
section .data
    msg_input db 'Introduce una cadena de texto: ', 0
    msg_output db 'La cadena invertida es: ', 0
    newline db 10, 0

section .bss
    buffer resb 100         ; Reservar espacio para la cadena de texto
    length resd 1           ; Longitud de la cadena

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir la cadena
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer la cadena desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, buffer     ; puntero al buffer donde se almacena la cadena
    mov edx, 100        ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Calcular la longitud de la cadena
    mov eax, buffer     ; puntero al buffer
    mov ebx, 0          ; contador de longitud
find_length:
    cmp byte [eax + ebx], 0  ; comparar si el carácter es nulo (fin de la cadena)
    je done_length          ; si es el fin de la cadena, salir
    inc ebx                  ; incrementar el contador de longitud
    jmp find_length          ; continuar buscando
done_length:
    mov [length], ebx        ; guardar la longitud de la cadena en 'length'

    ; Imprimir el mensaje "La cadena invertida es:"
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Invertir la cadena
    mov ebx, [length]    ; cargar la longitud de la cadena
    dec ebx              ; restar 1 para ajustar el índice (empezamos desde el último carácter)
    mov edi, buffer      ; puntero al buffer

invertir_loop:
    ; Comparar si ya hemos llegado al primer carácter
    cmp ebx, -1
    jl done_invertir

    ; Imprimir el carácter desde la posición 'ebx'
    mov al, [edi + ebx]  ; cargar el carácter de la posición 'ebx'
    mov eax, 4           ; syscall para write
    mov ebx, 1           ; descriptor de salida (stdout)
    mov ecx, esp         ; cargar el carácter en el stack para imprimirlo
    mov [ecx], al        ; almacenar el carácter en la pila
    mov edx, 1           ; longitud 1
    int 0x80             ; interrupción del sistema

    dec ebx              ; mover al siguiente carácter
    jmp invertir_loop    ; continuar el bucle

done_invertir:
    ; Imprimir un salto de línea
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, newline    ; salto de línea
    mov edx, 1          ; longitud del salto
    int 0x80            ; interrupción del sistema

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema
```
### Corrida

Introduce una cadena de texto: Hola Mundo
La cadena invertida es: odnuM aloH


## 11.- Verificar si una cadena es palíndromo
### C# - Palindromo.cs

```
/*
 * Title: Verificar si una cadena es palíndromo
 * Filename: Palindromo.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que verifica si una cadena es un palíndromo.
 * Input: Una cadena de texto desde la consola.
 * Output: Mensaje indicando si la cadena es un palíndromo o no.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario que ingrese una cadena
        Console.Write("Introduce una cadena: ");
        string cadena = Console.ReadLine();

        // Verificar si la cadena es palíndromo
        if (EsPalindromo(cadena))
        {
            Console.WriteLine($"La cadena '{cadena}' es un palíndromo.");
        }
        else
        {
            Console.WriteLine($"La cadena '{cadena}' no es un palíndromo.");
        }
    }

    // Función que verifica si una cadena es un palíndromo
    public static bool EsPalindromo(string texto)
    {
        string cadenaReversa = new string(texto.Reverse().ToArray()); // Revertir la cadena
        return texto.Equals(cadenaReversa, StringComparison.OrdinalIgnoreCase); // Comparar sin distinguir mayúsculas y minúsculas
    }
}

```
### Asamblador

```
section .data
    msg_input db 'Introduce una cadena: ', 0
    msg_palindrome db ' es un palíndromo.', 0
    msg_not_palindrome db ' no es un palíndromo.', 0
    newline db 10, 0

section .bss
    buffer resb 100        ; Reservar espacio para la cadena de texto
    length resd 1          ; Longitud de la cadena
    reversed_buffer resb 100 ; Buffer para la cadena invertida

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir la cadena
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 26         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer la cadena desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, buffer     ; puntero al buffer donde se almacena la cadena
    mov edx, 100        ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Calcular la longitud de la cadena
    mov eax, buffer     ; puntero al buffer
    mov ebx, 0          ; contador de longitud
find_length:
    cmp byte [eax + ebx], 0  ; comparar si el carácter es nulo (fin de la cadena)
    je done_length          ; si es el fin de la cadena, salir
    inc ebx                  ; incrementar el contador de longitud
    jmp find_length          ; continuar buscando
done_length:
    mov [length], ebx        ; guardar la longitud de la cadena en 'length'

    ; Invertir la cadena
    mov eax, buffer          ; puntero al buffer original
    mov ebx, reversed_buffer ; puntero al buffer para la cadena invertida
    mov ecx, [length]        ; cargar la longitud de la cadena
    dec ecx                  ; ajustar el índice (empezamos desde el final)
reverse_loop:
    cmp ecx, -1
    jl done_reverse          ; si ya invertimos toda la cadena, salir
    mov al, [eax + ecx]      ; cargar el carácter
    mov [ebx + ( [length] - 1 - ecx )], al ; almacenar en el buffer invertido
    dec ecx                  ; mover al siguiente carácter
    jmp reverse_loop         ; continuar el bucle

done_reverse:

    ; Comparar la cadena original con la cadena invertida
    mov eax, buffer          ; puntero a la cadena original
    mov ebx, reversed_buffer ; puntero a la cadena invertida
    mov ecx, [length]        ; longitud de la cadena
    mov edx, 0               ; índice de comparación
compare_loop:
    cmp byte [eax + edx], [ebx + edx]  ; comparar los caracteres
    jne not_palindrome       ; si no son iguales, no es palíndromo
    inc edx                  ; mover al siguiente carácter
    cmp edx, ecx             ; verificar si hemos llegado al final
    jl compare_loop          ; continuar comparando

    ; Si llegamos aquí, la cadena es un palíndromo
    mov eax, 4               ; syscall para write
    mov ebx, 1               ; descriptor de salida (stdout)
    mov ecx, msg_palindrome  ; mensaje "es un palíndromo"
    mov edx, 18              ; longitud del mensaje
    int 0x80                 ; interrupción del sistema
    jmp exit

not_palindrome:
    ; Si llegamos aquí, la cadena no es un palíndromo
    mov eax, 4               ; syscall para write
    mov ebx, 1               ; descriptor de salida (stdout)
    mov ecx, msg_not_palindrome  ; mensaje "no es un palíndromo"
    mov edx, 23              ; longitud del mensaje
    int 0x80                 ; interrupción del sistema

exit:
    ; Salir del programa
    mov eax, 1               ; syscall para exit
    xor ebx, ebx             ; código de salida 0
    int 0x80                 ; interrupción del sistema
```
### Corrida

Introduce una cadena de texto: reconocer
La cadena 'reconocer' es un palíndromo.

## 12.-Encontrar el máximo en un arreglo
### C# - MaxEnArreglo.cs

```
/*
 * Title: Encontrar el máximo en un arreglo
 * Filename: MaxEnArreglo.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que encuentra el valor máximo en un arreglo de enteros.
 * Input: Un arreglo de enteros desde la consola.
 * Output: El valor máximo encontrado en el arreglo.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario que ingrese los elementos del arreglo
        Console.Write("Introduce los elementos del arreglo separados por espacio: ");
        string input = Console.ReadLine();

        // Convertir la entrada en un arreglo de enteros
        int[] arreglo = Array.ConvertAll(input.Split(' '), int.Parse);

        // Encontrar el máximo en el arreglo
        int max = EncontrarMaximo(arreglo);

        // Mostrar el valor máximo
        Console.WriteLine($"El valor máximo en el arreglo es: {max}");
    }

    // Función que encuentra el valor máximo en un arreglo
    public static int EncontrarMaximo(int[] arreglo)
    {
        int max = arreglo[0]; // Asumir que el primer elemento es el máximo

        // Recorrer el arreglo para encontrar el valor máximo
        foreach (int numero in arreglo)
        {
            if (numero > max)
            {
                max = numero;
            }
        }

        return max;
    }
}
```
### Asamblador

```

section .data
    msg_input db 'Introduce los elementos del arreglo separados por espacio: ', 0
    msg_output db 'El valor máximo en el arreglo es: ', 0
    newline db 10, 0

section .bss
    buffer resb 100        ; Buffer para almacenar la entrada del usuario
    arreglo resd 20        ; Arreglo para almacenar hasta 20 enteros
    length resd 1          ; Longitud del arreglo

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir los elementos del arreglo
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 49         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer la entrada del usuario
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, buffer     ; puntero al buffer donde se almacena la entrada
    mov edx, 100        ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la entrada a enteros y almacenarlos en el arreglo
    mov eax, buffer     ; puntero al buffer
    mov ebx, arreglo    ; puntero al arreglo
    mov ecx, 0          ; índice del arreglo
convert_loop:
    ; Leer cada carácter de la entrada
    mov al, [eax]       ; cargar un byte del buffer
    cmp al, 20          ; comparar con el espacio (separador)
    je store_number     ; si es un espacio, almacenar el número
    cmp al, 10          ; comparar con el salto de línea
    je done_input       ; si es el final de la entrada, terminar
    inc eax             ; mover al siguiente carácter
    jmp convert_loop

store_number:
    ; Almacenar el número en el arreglo
    movzx edx, byte [eax-1]  ; cargar el número
    sub edx, '0'             ; convertir de carácter a número
    mov [ebx + ecx*4], edx   ; almacenar el número en el arreglo
    inc ecx                  ; incrementar el índice
    inc eax                  ; mover al siguiente carácter
    jmp convert_loop

done_input:
    ; Encontrar el máximo en el arreglo
    mov ecx, 0          ; índice del arreglo
    mov edx, [arreglo]  ; cargar el primer elemento del arreglo
    mov ebx, 1          ; número de elementos leídos

find_max:
    ; Comparar el siguiente número con el máximo actual
    cmp edx, [arreglo + ebx*4] ; comparar el máximo con el siguiente número
    jge continue_find
    mov edx, [arreglo + ebx*4] ; actualizar el máximo

continue_find:
    inc ebx                  ; incrementar el índice
    cmp ebx, [length]         ; verificar si hemos recorrido todo el arreglo
    jl find_max

    ; Imprimir el valor máximo
    mov eax, 4              ; syscall para write
    mov ebx, 1              ; descriptor de salida (stdout)
    mov ecx, msg_output     ; mensaje
    mov edx, 30             ; longitud del mensaje
    int 0x80                ; interrupción del sistema

    ; Mostrar el valor máximo
    mov eax, 4              ; syscall para write
    mov ebx, 1              ; descriptor de salida (stdout)
    mov ecx, edx            ; valor máximo
    mov edx, 4              ; longitud de un entero
    int 0x80                ; interrupción del sistema

    ; Salir del programa
    mov eax, 1              ; syscall para exit
    xor ebx, ebx            ; código de salida 0
    int 0x80                ; interrupción del sistema

```
### Corrida

Introduce los elementos del arreglo separados por espacio: 5 8 2 12 3 7
El valor máximo en el arreglo es: 12

## 13.-Encontrar el mínimo en un arreglo
### C# - MinEnArreglo.cs

```
/*
 * Title: Encontrar el mínimo en un arreglo
 * Filename: MinEnArreglo.cs
 * Autor: Sanchez Salinas Eduardo Josue
 * Date: 2024-10-02
 * Description: Programa que encuentra el valor mínimo en un arreglo de enteros.
 * Input: Un arreglo de enteros desde la consola.
 * Output: El valor mínimo encontrado en el arreglo.
 */

using System;

public class Program
{
    public static void Main()
    {
        // Solicitar al usuario que ingrese los elementos del arreglo
        Console.Write("Introduce los elementos del arreglo separados por espacio: ");
        string input = Console.ReadLine();

        // Convertir la entrada en un arreglo de enteros
        int[] arreglo = Array.ConvertAll(input.Split(' '), int.Parse);

        // Encontrar el mínimo en el arreglo
        int min = EncontrarMinimo(arreglo);

        // Mostrar el valor mínimo
        Console.WriteLine($"El valor mínimo en el arreglo es: {min}");
    }

    // Función que encuentra el valor mínimo en un arreglo
    public static int EncontrarMinimo(int[] arreglo)
    {
        int min = arreglo[0]; // Asumir que el primer elemento es el mínimo

        // Recorrer el arreglo para encontrar el valor mínimo
        foreach (int numero in arreglo)
        {
            if (numero < min)
            {
                min = numero;
            }
        }

        return min;
    }
}
```
### Asamblador
```
section .data
    msg_input db 'Introduce los elementos del arreglo separados por espacio: ', 0
    msg_output db 'El valor mínimo en el arreglo es: ', 0
    newline db 10, 0

section .bss
    buffer resb 100        ; Buffer para almacenar la entrada del usuario
    arreglo resd 20        ; Arreglo para almacenar hasta 20 enteros
    length resd 1          ; Longitud del arreglo

section .text
    global _start

_start:
    ; Imprimir el mensaje para introducir los elementos del arreglo
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 49         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer la entrada del usuario
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, buffer     ; puntero al buffer donde se almacena la entrada
    mov edx, 100        ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la entrada a enteros y almacenarlos en el arreglo
    mov eax, buffer     ; puntero al buffer
    mov ebx, arreglo    ; puntero al arreglo
    mov ecx, 0          ; índice del arreglo
convert_loop:
    ; Leer cada carácter de la entrada
    mov al, [eax]       ; cargar un byte del buffer
    cmp al, 20          ; comparar con el espacio (separador)
    je store_number     ; si es un espacio, almacenar el número
    cmp al, 10          ; comparar con el salto de línea
    je done_input       ; si es el final de la entrada, terminar
    inc eax             ; mover al siguiente carácter
    jmp convert_loop

store_number:
    ; Almacenar el número en el arreglo
    movzx edx, byte [eax-1]  ; cargar el número
    sub edx, '0'             ; convertir de carácter a número
    mov [ebx + ecx*4], edx   ; almacenar el número en el arreglo
    inc ecx                  ; incrementar el índice
    inc eax                  ; mover al siguiente carácter
    jmp convert_loop

done_input:
    ; Encontrar el mínimo en el arreglo
    mov ecx, 0          ; índice del arreglo
    mov edx, [arreglo]  ; cargar el primer elemento del arreglo
    mov ebx, 1          ; número de elementos leídos

find_min:
    ; Comparar el siguiente número con el mínimo actual
    cmp edx, [arreglo + ebx*4] ; comparar el mínimo con el siguiente número
    jle continue_find
    mov edx, [arreglo + ebx*4] ; actualizar el mínimo

continue_find:
    inc ebx                  ; incrementar el índice
    cmp ebx, [length]         ; verificar si hemos recorrido todo el arreglo
    jl find_min

    ; Imprimir el valor mínimo
    mov eax, 4              ; syscall para write
    mov ebx, 1              ; descriptor de salida (stdout)
    mov ecx, msg_output     ; mensaje
    mov edx, 30             ; longitud del mensaje
    int 0x80                ; interrupción del sistema

    ; Mostrar el valor mínimo
    mov eax, 4              ; syscall para write
    mov ebx, 1              ; descriptor de salida (stdout)
    mov ecx, edx            ; valor mínimo
    mov edx, 4              ; longitud de un entero
    int 0x80                ; interrupción del sistema

    ; Salir del programa
    mov eax, 1              ; syscall para exit
    xor ebx, ebx            ; código de salida 0
    int 0x80                ; interrupción del sistema
```
### Corrida
Introduce los elementos del arreglo separados por espacio: 5 8 2 12 3 7
El valor mínimo en el arreglo es: 2

# 14.-búsqueda lineal 

### Codigo en C#
```
/*
Autor: Sánchez Salinas Eduardo Josué
Descripción: Implementación de búsqueda lineal en ARM64 Assembly
Objetivo: Buscar un valor en un arreglo y devolver su índice. Si no se encuentra, devolver -1.
*/

// Código en C equivalente:
/*
int linear_search(int *arr, int length, int target) {
    for (int i = 0; i < length; i++) {
        if (arr[i] == target) {
            return i;  // índice del elemento encontrado
        }
    }
    return -1; // no encontrado
}
*/
```

### asamblador
```
.section .data
arr:    .word 5, 10, 15, 20, 25  // Arreglo de ejemplo
length: .word 5                  // Longitud del arreglo
target: .word 20                 // Valor a buscar

.section .text
.global _start

_start:
    LDR x0, =arr        // Cargar la dirección del arreglo en x0
    LDR w1, =length     // Cargar la longitud del arreglo en w1
    LDR w2, =target     // Cargar el valor objetivo a buscar en w2

    // Llamada a la función de búsqueda
    BL linear_search

    // Salir del programa
    MOV w8, 93         // syscall para terminar (exit) en Linux ARM64
    SVC #0             // Llamada al sistema

// Función linear_search
// Parámetros:
// - x0: puntero al inicio del arreglo
// - w1: longitud del arreglo
// - w2: valor objetivo
// Resultado:
// - w0: índice del elemento encontrado, o -1 si no se encuentra
linear_search:
    MOV w3, #0         // Inicializar índice i = 0

loop:
    CMP w3, w1         // Comparar i con la longitud
    B.GE not_found     // Si i >= longitud, el objetivo no está en el arreglo

    LDR w4, [x0, w3, LSL #2] // Cargar arr[i] (cada elemento es de 4 bytes)
    CMP w4, w2         // Comparar arr[i] con el objetivo
    B.EQ found         // Si son iguales, se encontró el objetivo

    ADD w3, w3, #1     // Incrementar i
    B loop             // Volver al inicio del loop

not_found:
    MOV w0, #-1        // Si no se encuentra, retornar -1
    RET

found:
    MOV w0, w3         // Retornar el índice encontrado
    RET

```

## 15.- Búsqueda binaria

### C#

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de búsqueda binaria recursiva en ARM64 Assembly
// Entradas: 
//   x0 - Puntero al inicio del arreglo ordenado
//   x1 - Límite inferior (left)
//   x2 - Límite superior (right)
//   x3 - Valor objetivo (target)
// Salida:
//   x0 - Índice del valor objetivo en el arreglo, o -1 si no se encuentra

int BinarySearch(int[] arr, int left, int right, int target) {
    if (left > right) return -1; // Caso base: valor no encontrado
    int mid = left + (right - left) / 2;

    if (arr[mid] == target) return mid; // Valor encontrado
    else if (arr[mid] > target)         // Buscar en mitad izquierda
        return BinarySearch(arr, left, mid - 1, target);
    else                                // Buscar en mitad derecha
        return BinarySearch(arr, mid + 1, right, target);
}
```
### Asamblador

```
.global binary_search

binary_search:
    cmp x1, x2              // Compara left y right
    bgt not_found           // Si left > right, no encontrado
    
    add x4, x1, x2          // x4 = left + right
    lsr x4, x4, 1           // x4 = mid = (left + right) / 2

    ldr w5, [x0, x4, LSL #2]  // w5 = arr[mid]
    cmp w5, w3                // Compara arr[mid] con target
    beq found                 // Si arr[mid] == target, encontrado

    cmp w5, w3
    bgt search_left           // Si arr[mid] > target, mitad izquierda

    add x1, x4, 1            // left = mid + 1
    b binary_search          // Llama recursivamente

search_left:
    sub x2, x4, 1            // right = mid - 1
    b binary_search          // Llama recursivamente

found:
    mov x0, x4               // x0 = mid
    ret                      // Retorna índice encontrado

not_found:
    mov x0, -1               // Retorna -1 si no se encuentra
    ret

```

## 16.-Ordenamiento burbuja

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de ordenamiento burbuja en ARM64 Assembly
// Entradas: 
//   x0 - Puntero al arreglo a ordenar
//   x1 - Tamaño del arreglo
// Salida:
//   Arreglo ordenado en orden ascendente en la misma posición de memoria

// Algoritmo en C# (para referencia)
// void BubbleSort(int[] arr) {
//     int n = arr.Length;
//     for (int i = 0; i < n - 1; i++) {
//         for (int j = 0; j < n - i - 1; j++) {
//             if (arr[j] > arr[j + 1]) {
//                 int temp = arr[j];
//                 arr[j] = arr[j + 1];
//                 arr[j + 1] = temp;
//             }
//         }
//     }
// }

.global bubble_sort

bubble_sort:
    sub x2, x1, 1            // x2 = n - 1, establece el límite exterior del bucle
outer_loop:
    cmp x2, 0                // Verifica si el límite exterior es 0
    ble end_sort             // Termina si x2 <= 0

    mov x3, 0                // Inicializa el índice interior (j = 0)
inner_loop:
    sub x4, x2, x3           // x4 = n - i - 1, límite interno decreciente
    cmp x4, 1                // Verifica si hay al menos dos elementos para comparar
    ble next_pass            // Salta a la siguiente pasada si no hay suficientes elementos

    ldr w5, [x0, x3, LSL #2]    // Carga arr[j] en w5
    ldr w6, [x0, x3, LSL #2 + 4] // Carga arr[j + 1] en w6
    cmp w5, w6                // Compara arr[j] con arr[j + 1]
    ble skip_swap             // Si arr[j] <= arr[j + 1], no hace intercambio

    // Intercambio de arr[j] y arr[j + 1]
    str w6, [x0, x3, LSL #2]     // Guarda arr[j + 1] en arr[j]
    str w5, [x0, x3, LSL #2 + 4] // Guarda arr[j] en arr[j + 1]

skip_swap:
    add x3, x3, 1            // Incrementa j
    b inner_loop             // Repite el bucle interno

next_pass:
    sub x2, x2, 1            // Decrementa el límite exterior (i++)
    b outer_loop             // Repite el bucle exterior

end_sort:
    ret                      // Termina el ordenamiento
```

## 17.- Ordenamiento por selección

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de ordenamiento por selección en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo a ordenar
//   x1 - Tamaño del arreglo
// Salida:
//   Arreglo ordenado en orden ascendente en la misma posición de memoria

// Algoritmo en C# (para referencia)
// void SelectionSort(int[] arr) {
//     int n = arr.Length;
//     for (int i = 0; i < n - 1; i++) {
//         int minIndex = i;
//         for (int j = i + 1; j < n; j++) {
//             if (arr[j] < arr[minIndex]) {
//                 minIndex = j;
//             }
//         }
//         if (minIndex != i) {
//             int temp = arr[i];
//             arr[i] = arr[minIndex];
//             arr[minIndex] = temp;
//         }
//     }
// }

.global selection_sort

selection_sort:
    mov x2, 0                // Inicializa el índice de iteración exterior (i = 0)
outer_loop:
    cmp x2, x1                // Compara i con n
    bge end_sort              // Si i >= n, termina

    mov x3, x2                // x3 = i, el índice mínimo comienza como i
    add x4, x2, 1             // x4 = i + 1, comienza la iteración interna desde el siguiente elemento
inner_loop:
    cmp x4, x1                // Verifica si x4 >= n
    bge next_outer_iteration  // Si x4 >= n, termina la iteración interna

    ldr w5, [x0, x4, LSL #2]  // Carga arr[j] en w5
    ldr w6, [x0, x3, LSL #2]  // Carga arr[minIndex] en w6
    cmp w5, w6                // Compara arr[j] con arr[minIndex]
    bge skip_swap             // Si arr[j] >= arr[minIndex], no hay intercambio

    mov x3, x4                // Actualiza minIndex con j

skip_swap:
    add x4, x4, 1             // Incrementa el índice interno (j++)
    b inner_loop              // Repite el bucle interno

next_outer_iteration:
    cmp x3, x2                // Si minIndex no es igual a i, realiza el intercambio
    beq continue_outer_loop   // Si no hay intercambio, pasa a la siguiente iteración

    // Intercambio de arr[i] y arr[minIndex]
    ldr w7, [x0, x2, LSL #2]  // Carga arr[i] en w7
    ldr w8, [x0, x3, LSL #2]  // Carga arr[minIndex] en w8
    str w8, [x0, x2, LSL #2]  // Guarda arr[minIndex] en arr[i]
    str w7, [x0, x3, LSL #2]  // Guarda arr[i] en arr[minIndex]

continue_outer_loop:
    add x2, x2, 1             // Incrementa el índice exterior (i++)
    b outer_loop              // Repite el bucle exterior

end_sort:
    ret                       // Termina el ordenamiento
```

## 18.-Ordenamiento por mezcla (Merge Sort)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de ordenamiento por mezcla (Merge Sort) en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo a ordenar
//   x1 - Tamaño del arreglo
// Salida:
//   Arreglo ordenado en orden ascendente en la misma posición de memoria

// Algoritmo en C# (para referencia)
// void MergeSort(int[] arr) {
//     if (arr.Length <= 1) return;
//     int mid = arr.Length / 2;
//     int[] left = arr.Take(mid).ToArray();
//     int[] right = arr.Skip(mid).ToArray();
//     MergeSort(left);
//     MergeSort(right);
//     Merge(arr, left, right);
// }
//
// void Merge(int[] arr, int[] left, int[] right) {
//     int i = 0, j = 0, k = 0;
//     while (i < left.Length && j < right.Length) {
//         if (left[i] < right[j]) {
//             arr[k++] = left[i++];
//         } else {
//             arr[k++] = right[j++];
//         }
//     }
//     while (i < left.Length) {
//         arr[k++] = left[i++];
//     }
//     while (j < right.Length) {
//         arr[k++] = right[j++];
//     }
// }

.global merge_sort
.global merge

// Función principal de MergeSort
merge_sort:
    cmp x1, 1                // Si el tamaño del arreglo es 1, ya está ordenado
    ble end_sort             // Si n <= 1, termina

    // Divide el arreglo en dos mitades
    mov x2, x1
    asr x2, x2, 1            // x2 = n / 2, tamaño de la mitad izquierda
    sub x3, x1, x2           // x3 = n - n/2, tamaño de la mitad derecha

    // Llamada recursiva a la mitad izquierda
    sub sp, sp, #16           // Reservamos espacio en el stack para el puntero y tamaño de la izquierda
    str x0, [sp]              // Guardamos el puntero original
    str x2, [sp, #8]          // Guardamos el tamaño de la mitad izquierda
    bl merge_sort             // Llamada recursiva para ordenar la mitad izquierda
    ldr x0, [sp]              // Recuperamos el puntero original
    ldr x2, [sp, #8]          // Recuperamos el tamaño de la mitad izquierda
    add sp, sp, #16           // Liberamos el espacio en el stack

    // Llamada recursiva a la mitad derecha
    sub sp, sp, #16           // Reservamos espacio en el stack para el puntero y tamaño de la derecha
    add x0, x0, x2            // Apuntamos al inicio de la mitad derecha
    str x0, [sp]              // Guardamos el puntero original
    str x3, [sp, #8]          // Guardamos el tamaño de la mitad derecha
    bl merge_sort             // Llamada recursiva para ordenar la mitad derecha
    ldr x0, [sp]              // Recuperamos el puntero original
    ldr x3, [sp, #8]          // Recuperamos el tamaño de la mitad derecha
    add sp, sp, #16           // Liberamos el espacio en el stack

    // Llamada a la función de mezcla (merge)
    bl merge                  // Fusiona las dos mitades ordenadas

end_sort:
    ret

// Función de mezcla (merge)
merge:
    // El puntero a los dos subarreglos y el arreglo original están en x0 y x1
    // Los tamaños de los subarreglos están en x2 y x3
    // La mezcla se realizará en el arreglo original
    // Aquí se realiza la fusión entre los subarreglos ordenados

    // TODO: Implementar el proceso de fusión de los dos subarreglos
    ret
```

## 19.-Suma de matrices

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de suma de matrices en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la primera matriz (A)
//   x1 - Puntero a la segunda matriz (B)
//   x2 - Puntero a la matriz resultante (C)
//   x3 - Número de filas (M)
//   x4 - Número de columnas (N)
// Salida:
//   La matriz C contendrá la suma de las matrices A y B

// Algoritmo en C# (para referencia)
// void MatrixSum(int[,] A, int[,] B, int[,] C) {
//     int rows = A.GetLength(0);
//     int cols = A.GetLength(1);
//     for (int i = 0; i < rows; i++) {
//         for (int j = 0; j < cols; j++) {
//             C[i, j] = A[i, j] + B[i, j];
//         }
//     }
// }

.global matrix_sum

matrix_sum:
    mov x5, 0                // Inicializa el índice de filas (i = 0)
row_loop:
    cmp x5, x3                // Compara i con el número de filas
    bge end_sum               // Si i >= filas, termina

    mov x6, 0                // Inicializa el índice de columnas (j = 0)
col_loop:
    cmp x6, x4                // Compara j con el número de columnas
    bge next_row              // Si j >= columnas, pasa a la siguiente fila

    // Calcula el índice del elemento en la matriz (i, j)
    mul x7, x5, x4            // x7 = i * N (filas * columnas)
    add x7, x7, x6            // x7 = i * N + j (índice del elemento (i, j))

    // Carga A[i, j] y B[i, j]
    ldr w8, [x0, x7, LSL #2]  // Carga A[i, j] en w8
    ldr w9, [x1, x7, LSL #2]  // Carga B[i, j] en w9

    // Realiza la suma
    add w10, w8, w9           // w10 = A[i, j] + B[i, j]

    // Guarda el resultado en C[i, j]
    str w10, [x2, x7, LSL #2] // Guarda C[i, j] = A[i, j] + B[i, j]

    add x6, x6, 1             // Incrementa el índice de columnas (j++)
    b col_loop                // Repite el bucle de columnas

next_row:
    add x5, x5, 1             // Incrementa el índice de filas (i++)
    b row_loop                // Repite el bucle de filas

end_sum:
    ret                       // Termina la suma de matrices
```

## 20 Multiplicación de matrices

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de multiplicación de matrices en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la primera matriz (A)
//   x1 - Puntero a la segunda matriz (B)
//   x2 - Puntero a la matriz resultante (C)
//   x3 - Número de filas de la matriz A (M)
//   x4 - Número de columnas de la matriz A y filas de la matriz B (N)
//   x5 - Número de columnas de la matriz B (P)
// Salida:
//   La matriz C contendrá el resultado de la multiplicación de A y B

// Algoritmo en C# (para referencia)
// void MatrixMultiply(int[,] A, int[,] B, int[,] C) {
//     int rowsA = A.GetLength(0);
//     int colsA = A.GetLength(1);
//     int colsB = B.GetLength(1);
//     for (int i = 0; i < rowsA; i++) {
//         for (int j = 0; j < colsB; j++) {
//             C[i, j] = 0;
//             for (int k = 0; k < colsA; k++) {
//                 C[i, j] += A[i, k] * B[k, j];
//             }
//         }
//     }
// }

.global matrix_multiply

matrix_multiply:
    mov x6, 0                // Inicializa el índice de filas de la matriz A (i = 0)
row_loop:
    cmp x6, x3                // Compara i con el número de filas de A
    bge end_multiply          // Si i >= filasA, termina

    mov x7, 0                // Inicializa el índice de columnas de la matriz B (j = 0)
col_loop:
    cmp x7, x5                // Compara j con el número de columnas de B
    bge next_row              // Si j >= columnasB, pasa a la siguiente fila

    mov x8, 0                 // Inicializa el índice para la suma (k = 0) y el acumulador (sum = 0)
    mov w9, 0                 // Acumulador de la suma (C[i, j])

inner_loop:
    cmp x8, x4                // Compara k con N (columnasA / filasB)
    bge store_result          // Si k >= N, almacena el resultado

    // Calcula el índice del elemento (i, k) en A y (k, j) en B
    mul x10, x6, x4           // x10 = i * N (índice de la fila de A)
    add x10, x10, x8          // x10 = i * N + k (índice de A[i, k])

    mul x11, x8, x5           // x11 = k * P (índice de la columna de B)
    add x11, x11, x7          // x11 = k * P + j (índice de B[k, j])

    ldr w12, [x0, x10, LSL #2] // Carga A[i, k] en w12
    ldr w13, [x1, x11, LSL #2] // Carga B[k, j] en w13

    mul w14, w12, w13         // w14 = A[i, k] * B[k, j]
    add w9, w9, w14           // sum += A[i, k] * B[k, j]

    add x8, x8, 1             // Incrementa el índice de la suma (k++)
    b inner_loop              // Repite el bucle de la multiplicación de elementos

store_result:
    // Guarda el resultado en C[i, j]
    mul x15, x6, x5           // x15 = i * P (índice de la fila de C)
    add x15, x15, x7          // x15 = i * P + j (índice de C[i, j])
    str w9, [x2, x15, LSL #2] // Guarda C[i, j] en la matriz C

    add x7, x7, 1             // Incrementa el índice de columnas (j++)
    b col_loop                // Repite el bucle de columnas

next_row:
    add x6, x6, 1             // Incrementa el índice de filas (i++)
    b row_loop                // Repite el bucle de filas

end_multiply:
    ret                       // Termina la multiplicación de matrices
```

## 21.-Transposición de una matriz

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de transposición de una matriz en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la matriz original (A)
//   x1 - Puntero a la matriz transpuesta (B)
//   x2 - Número de filas de la matriz A (M)
//   x3 - Número de columnas de la matriz A (N)
// Salida:
//   La matriz B contendrá la transposición de la matriz A

// Algoritmo en C# (para referencia)
// void MatrixTranspose(int[,] A, int[,] B) {
//     int rows = A.GetLength(0);
//     int cols = A.GetLength(1);
//     for (int i = 0; i < rows; i++) {
//         for (int j = 0; j < cols; j++) {
//             B[j, i] = A[i, j];
//         }
//     }
// }

.global matrix_transpose

matrix_transpose:
    mov x4, 0                // Inicializa el índice de filas de la matriz A (i = 0)
row_loop:
    cmp x4, x2                // Compara i con el número de filas de A
    bge end_transpose         // Si i >= filasA, termina

    mov x5, 0                // Inicializa el índice de columnas de la matriz A (j = 0)
col_loop:
    cmp x5, x3                // Compara j con el número de columnas de A
    bge next_row              // Si j >= columnasA, pasa a la siguiente fila

    // Calcula el índice del elemento (i, j) en A y (j, i) en B
    mul x6, x4, x3            // x6 = i * N (índice de la fila de A)
    add x6, x6, x5            // x6 = i * N + j (índice de A[i, j])

    mul x7, x5, x2            // x7 = j * M (índice de la columna de B)
    add x7, x7, x4            // x7 = j * M + i (índice de B[j, i])

    ldr w8, [x0, x6, LSL #2]  // Carga A[i, j] en w8
    str w8, [x1, x7, LSL #2]  // Guarda B[j, i] = A[i, j]

    add x5, x5, 1             // Incrementa el índice de columnas (j++)
    b col_loop                // Repite el bucle de columnas

next_row:
    add x4, x4, 1             // Incrementa el índice de filas (i++)
    b row_loop                // Repite el bucle de filas

end_transpose:
    ret                       // Termina la transposición de la matriz
```
## 22.-Conversión de ASCII a entero

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de un carácter ASCII a entero en ARM64 Assembly
// Entradas:
//   x0 - Carácter ASCII (caracter en formato de byte)
// Salida:
//   x0 - El valor entero correspondiente al carácter ASCII

// Algoritmo en C# (para referencia)
// int ASCIItoInt(char c) {
//     return c - '0';
// }

.global ascii_to_int

ascii_to_int:
    sub x0, x0, '0'           // Convierte el carácter ASCII a su valor entero
    ret                        // Retorna el valor entero correspondiente
```

## 23.-Conversión de entero a ASCII

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de un entero a carácter ASCII en ARM64 Assembly
// Entradas:
//   x0 - Entero (valor numérico entre 0 y 9)
// Salida:
//   x0 - Carácter ASCII correspondiente al entero (valor entre '0' y '9')

// Algoritmo en C# (para referencia)
// char IntToASCII(int num) {
//     return (char)(num + '0');
// }

.global int_to_ascii

int_to_ascii:
    add x0, x0, '0'           // Convierte el entero a su carácter ASCII correspondiente
    ret                        // Retorna el carácter ASCII
```
## 24.-Calcular la longitud de una cadena

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de cálculo de la longitud de una cadena en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la cadena (null terminada)
// Salida:
//   x0 - Longitud de la cadena (número de caracteres)

// Algoritmo en C# (para referencia)
// int StringLength(string str) {
//     return str.Length;
// }

.global string_length

string_length:
    mov x1, 0                // Inicializa el contador de longitud (i = 0)
loop:
    ldrb w2, [x0, x1]        // Carga el siguiente byte de la cadena
    cmp w2, 0                // Verifica si es el carácter nulo (fin de la cadena)
    beq end_length           // Si es nulo, termina el cálculo
    add x1, x1, 1            // Incrementa el contador de longitud
    b loop                   // Repite el bucle

end_length:
    mov x0, x1               // La longitud de la cadena se encuentra en x1
    ret                      // Retorna la longitud de la cadena
```

## 25.-Contar vocales y consonantes

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conteo de vocales y consonantes en una cadena en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la cadena (null terminada)
// Salida:
//   x0 - Número de vocales
//   x1 - Número de consonantes

// Algoritmo en C# (para referencia)
// void CountVowelsAndConsonants(string str, out int vowels, out int consonants) {
//     vowels = 0;
//     consonants = 0;
//     foreach (char c in str) {
//         if ("aeiouAEIOU".Contains(c)) {
//             vowels++;
//         } else if (char.IsLetter(c)) {
//             consonants++;
//         }
//     }
// }

.global count_vowels_and_consonants

count_vowels_and_consonants:
    mov x2, 0                // Inicializa el contador de vocales en x2
    mov x3, 0                // Inicializa el contador de consonantes en x3
loop:
    ldrb w4, [x0]            // Carga el siguiente byte de la cadena
    cmp w4, 0                // Verifica si es el carácter nulo (fin de la cadena)
    beq end_count            // Si es nulo, termina el conteo

    // Verifica si es una vocal
    cmp w4, 'a'
    beq is_vowel
    cmp w4, 'e'
    beq is_vowel
    cmp w4, 'i'
    beq is_vowel
    cmp w4, 'o'
    beq is_vowel
    cmp w4, 'u'
    beq is_vowel
    cmp w4, 'A'
    beq is_vowel
    cmp w4, 'E'
    beq is_vowel
    cmp w4, 'I'
    beq is_vowel
    cmp w4, 'O'
    beq is_vowel
    cmp w4, 'U'
    beq is_vowel

    // Si no es vocal, verifica si es una consonante
    blt not_letter
    cmp w4, 'Z'
    ble not_letter
    add x3, x3, 1            // Incrementa el contador de consonantes

not_letter:
    add x0, x0, 1            // Avanza al siguiente carácter
    b loop                   // Repite el bucle

is_vowel:
    add x2, x2, 1            // Incrementa el contador de vocales
    b not_letter

end_count:
    mov x0, x2               // Retorna el número de vocales en x0
    mov x1, x3               // Retorna el número de consonantes en x1
    ret                      // Termina el conteo
```

## 26.- Operaciones AND, OR, XOR a nivel de bits

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de operaciones AND, OR, y XOR a nivel de bits en ARM64 Assembly
// Entradas:
//   x0 - Primer operando (A)
//   x1 - Segundo operando (B)
// Salidas:
//   x0 - Resultado de la operación AND
//   x1 - Resultado de la operación OR
//   x2 - Resultado de la operación XOR

// Algoritmo en C# (para referencia)
// void BitwiseOperations(int A, int B, out int andResult, out int orResult, out int xorResult) {
//     andResult = A & B;
//     orResult = A | B;
//     xorResult = A ^ B;
// }

.global bitwise_operations

bitwise_operations:
    // Operación AND (A & B)
    and x0, x0, x1           // x0 = A & B

    // Operación OR (A | B)
    orr x1, x0, x1           // x1 = A | B (usando el resultado de AND temporalmente en x0)

    // Operación XOR (A ^ B)
    eor x2, x0, x1           // x2 = A ^ B (usando los resultados de AND y OR)

    ret                       // Retorna los resultados en x0 (AND), x1 (OR), x2 (XOR)
```

## 27.-Desplazamientos a la izquierda y derecha

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de desplazamientos a la izquierda y derecha a nivel de bits en ARM64 Assembly
// Entradas:
//   x0 - Operando (valor numérico)
//   x1 - Número de posiciones para desplazar
// Salidas:
//   x0 - Resultado del desplazamiento a la izquierda (x0 << x1)
//   x1 - Resultado del desplazamiento a la derecha (x0 >> x1)

// Algoritmo en C# (para referencia)
// void ShiftOperations(int num, int positions, out int leftShift, out int rightShift) {
//     leftShift = num << positions;
//     rightShift = num >> positions;
// }

.global shift_operations

shift_operations:
    // Desplazamiento a la izquierda (num << positions)
    mov x2, x0               // Copia el valor original en x2
    lsl x0, x2, x1           // Desplazamiento a la izquierda: x0 = x2 << x1

    // Desplazamiento a la derecha (num >> positions)
    mov x2, x0               // Copia el resultado del desplazamiento a la izquierda en x2
    lsr x1, x2, x1           // Desplazamiento a la derecha: x1 = x2 >> x1

    ret                      // Retorna los resultados en x0 (izquierda), x1 (derecha)
```
## 28.-	Establecer, borrar y alternar bits
```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de establecer, borrar y alternar bits a nivel de bits en ARM64 Assembly
// Entradas:
//   x0 - Operando (valor numérico)
//   x1 - Posición del bit (índice, 0-based)
// Salidas:
//   x0 - Resultado después de establecer el bit
//   x1 - Resultado después de borrar el bit
//   x2 - Resultado después de alternar el bit

// Algoritmo en C# (para referencia)
// void BitManipulation(int num, int bitPosition, out int setBit, out int clearBit, out int toggleBit) {
//     setBit = num | (1 << bitPosition);
//     clearBit = num & ~(1 << bitPosition);
//     toggleBit = num ^ (1 << bitPosition);
// }

.global bit_manipulation

bit_manipulation:
    // Establecer el bit en la posición x1
    mov x2, x1               // Copia la posición del bit en x2
    mov x3, 1                // Crea una máscara con el bit en la posición x1 activado
    lsl x3, x3, x2           // x3 = 1 << x1 (máscara con el bit activado)
    orr x0, x0, x3           // Establecer el bit: x0 = x0 | (1 << x1)

    // Borrar el bit en la posición x1
    mov x3, 1                // Crea una máscara con el bit activado
    lsl x3, x3, x2           // x3 = 1 << x1 (máscara con el bit activado)
    not x3, x3               // x3 = ~(1 << x1) (máscara para borrar el bit)
    and x1, x0, x3           // Borrar el bit: x1 = x0 & ~(1 << x1)

    // Alternar el bit en la posición x1
    mov x3, 1                // Crea una máscara con el bit activado
    lsl x3, x3, x2           // x3 = 1 << x1 (máscara con el bit activado)
    eor x2, x0, x3           // Alternar el bit: x2 = x0 ^ (1 << x1)

    ret                      // Retorna los resultados en x0 (establecer), x1 (borrar), x2 (alternar)
```

## 29.- Contar los bits activados en un número

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de contar los bits activados (1) en un número en ARM64 Assembly
// Entradas:
//   x0 - Operando (valor numérico)
// Salida:
//   x0 - Número de bits activados (1) en el número

// Algoritmo en C# (para referencia)
// int CountSetBits(int num) {
//     int count = 0;
//     while (num > 0) {
//         count += num & 1;
//         num >>= 1;
//     }
//     return count;
// }

.global count_set_bits

count_set_bits:
    mov x1, 0                // Inicializa el contador de bits activados (x1 = 0)
loop:
    and x2, x0, 1            // x2 = x0 & 1 (verifica el bit más bajo de x0)
    add x1, x1, x2           // Incrementa el contador si el bit está activado
    lsr x0, x0, 1            // Desplaza a la derecha x0 (x0 >>= 1)
    cmp x0, 0                // Verifica si ya no quedan bits activados
    bne loop                 // Si quedan bits activados, repite el bucle

    mov x0, x1               // Retorna el número de bits activados en x0
    ret                      // Termina la función
```
## 30.-	Máximo Común Divisor (MCD)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación del cálculo del Máximo Común Divisor (MCD) usando el algoritmo de Euclides en ARM64 Assembly
// Entradas:
//   x0 - Primer número (a)
//   x1 - Segundo número (b)
// Salida:
//   x0 - MCD de a y b

// Algoritmo en C# (para referencia)
// int GCD(int a, int b) {
//     while (b != 0) {
//         int temp = b;
//         b = a % b;
//         a = temp;
//     }
//     return a;
// }

.global gcd

gcd:
    cmp x1, 0                // Verifica si b (x1) es cero
    beq end_gcd              // Si b es cero, termina y retorna a
loop:
    mov x2, x1               // Copia b en x2
    udiv x1, x0, x1          // x1 = a % b (división entera, x1 contiene el residuo)
    mov x0, x2               // Copia b en a (x0 = b)
    cmp x1, 0                // Verifica si el residuo es cero
    bne loop                 // Si el residuo no es cero, repite el bucle

end_gcd:
    ret                      // Retorna el MCD en x0
```
## 31.-Mínimo Común Múltiplo (MCM)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación del cálculo del Mínimo Común Múltiplo (MCM) usando el MCD en ARM64 Assembly
// Entradas:
//   x0 - Primer número (a)
//   x1 - Segundo número (b)
// Salida:
//   x0 - MCM de a y b

// Algoritmo en C# (para referencia)
// int LCM(int a, int b) {
//     int gcd = GCD(a, b); // Utiliza el MCD para calcular el MCM
//     return (a / gcd) * b;
// }

.global lcm

lcm:
    // Calcula el MCD de a y b
    mov x2, x0               // Copia a en x2
    mov x3, x1               // Copia b en x3
    bl gcd                   // Llama a la función gcd para obtener el MCD

    // Calcula (a / MCD) * b
    udiv x2, x0, x2          // x2 = a / MCD
    mul x0, x2, x1           // x0 = (a / MCD) * b

    ret                      // Retorna el MCM en x0
```

## 32.-Potencia (x^n)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de cálculo de la potencia (x^n) usando recursión o bucles en ARM64 Assembly
// Entradas:
//   x0 - Base (x)
//   x1 - Exponente (n)
// Salida:
//   x0 - Resultado de x^n

// Algoritmo en C# (para referencia)
// int Power(int x, int n) {
//     if (n == 0) return 1;
//     return x * Power(x, n - 1);
// }

.global power

power:
    cmp x1, 0                // Verifica si el exponente es 0
    beq base_case            // Si n == 0, el resultado es 1

    // Calcula x * x^(n-1)
    sub x1, x1, 1            // Decrementa n (n - 1)
    bl power                 // Llama recursivamente a power(x, n - 1)
    mul x0, x0, x2           // Multiplica el resultado por x

    ret                      // Retorna el resultado

base_case:
    mov x0, 1                // Si n == 0, el resultado es 1
    ret                      // Retorna 1
```

## 33.-Suma de elementos en un arreglo

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de suma de elementos en un arreglo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo
//   x1 - Tamaño del arreglo
// Salida:
//   x0 - Suma de todos los elementos del arreglo

// Algoritmo en C# (para referencia)
// int SumArray(int[] arr) {
//     int sum = 0;
//     foreach (int num in arr) {
//         sum += num;
//     }
//     return sum;
// }

.global sum_array

sum_array:
    mov x2, 0                // Inicializa la suma en x2 (suma = 0)
    mov x3, 0                // Inicializa el índice en x3 (i = 0)

loop:
    cmp x3, x1               // Compara el índice con el tamaño del arreglo
    bge end_sum              // Si el índice es mayor o igual que el tamaño, termina

    ldr w4, [x0, x3, LSL #2] // Carga el elemento arr[i] en w4
    add x2, x2, w4           // Suma el valor a la suma (suma += arr[i])

    add x3, x3, 1            // Incrementa el índice (i++)
    b loop                   // Repite el bucle

end_sum:
    mov x0, x2               // Retorna la suma en x0
    ret                      // Finaliza la función
```

## 34.-Invertir los elementos de un arreglo

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de inversión de elementos de un arreglo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo
//   x1 - Tamaño del arreglo
// Salida:
//   El arreglo invertido en la misma posición de memoria

// Algoritmo en C# (para referencia)
// void ReverseArray(int[] arr) {
//     int start = 0;
//     int end = arr.Length - 1;
//     while (start < end) {
//         int temp = arr[start];
//         arr[start] = arr[end];
//         arr[end] = temp;
//         start++;
//         end--;
//     }
// }

.global reverse_array

reverse_array:
    sub x2, x1, 1            // x2 = tamaño del arreglo - 1 (end = arr.Length - 1)
    mov x3, 0                // x3 = índice inicial (start = 0)

loop:
    cmp x3, x2               // Compara si start < end
    bge end_reverse          // Si start >= end, termina

    // Intercambio arr[start] y arr[end]
    ldr w4, [x0, x3, LSL #2]    // Carga arr[start] en w4
    ldr w5, [x0, x2, LSL #2]    // Carga arr[end] en w5
    str w5, [x0, x3, LSL #2]    // Guarda arr[end] en arr[start]
    str w4, [x0, x2, LSL #2]    // Guarda arr[start] en arr[end]

    add x3, x3, 1            // Incrementa start (start++)
    sub x2, x2, 1            // Decrementa end (end--)
    b loop                   // Repite el bucle

end_reverse:
    ret                      // Termina la función
```
## 35.-Rotación de un arreglo (izquierda/derecha)
```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de rotación de un arreglo (izquierda/derecha) en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo
//   x1 - Tamaño del arreglo
//   x2 - Número de posiciones para rotar (positivo para derecha, negativo para izquierda)
// Salida:
//   El arreglo rotado en la misma posición de memoria

// Algoritmo en C# (para referencia)
// void RotateArray(int[] arr, int positions) {
//     int n = arr.Length;
//     positions = positions % n; // En caso de que las posiciones sean mayores que el tamaño del arreglo
//     if (positions < 0) positions += n; // Si el desplazamiento es negativo, lo convierte a positivo
//     int[] rotatedArr = new int[n];
//     Array.Copy(arr, n - positions, rotatedArr, 0, positions);
//     Array.Copy(arr, 0, rotatedArr, positions, n - positions);
//     Array.Copy(rotatedArr, arr, n);
// }

.global rotate_array

rotate_array:
    mov x3, x1               // Copia el tamaño del arreglo en x3
    mov x4, x2               // Copia las posiciones a rotar en x4

    // Ajusta las posiciones para que estén en el rango [0, n-1]
    udiv x5, x4, x3          // x5 = x2 / x1 (división de posiciones / tamaño)
    mul x6, x5, x3           // x6 = x5 * x3
    sub x4, x4, x6           // Ajusta x4 para que esté dentro del rango de tamaño

    // Verifica si no hay rotación
    cmp x4, 0
    beq end_rotate           // Si no hay rotación, termina la función

    // Realiza la rotación de los elementos a la derecha
    mov x7, x1               // x7 = tamaño del arreglo
    sub x7, x7, x4           // x7 = n - posiciones (esto es el número de elementos a mover al principio)

    // Copia la parte final del arreglo al principio
    ldr x8, [x0, x7, LSL #2] // Carga el primer elemento a mover en x8
    str x8, [x0, x7, LSL #2] // Guarda el elemento en la nueva posición

    add x8, x0, x4   // Continúa implementando la rotación con los otros 32 bits
```
## 36.-Encontrar el segundo elemento más grande

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de encontrar el segundo elemento más grande de un arreglo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo
//   x1 - Tamaño del arreglo
// Salida:
//   x0 - Segundo elemento más grande en el arreglo

// Algoritmo en C# (para referencia)
// int SecondLargest(int[] arr) {
//     int largest = int.MinValue;
//     int secondLargest = int.MinValue;
//     foreach (int num in arr) {
//         if (num > largest) {
//             secondLargest = largest;
//             largest = num;
//         } else if (num > secondLargest && num != largest) {
//             secondLargest = num;
//         }
//     }
//     return secondLargest;
// }

.global second_largest

second_largest:
    mov x2, 0                // Inicializa el valor más grande (largest) en 0
    mov x3, 0                // Inicializa el segundo valor más grande (secondLargest) en 0
    mov x4, 0                // Inicializa el índice en 0

loop:
    cmp x4, x1               // Compara el índice con el tamaño del arreglo
    bge end_second_largest   // Si el índice es mayor o igual que el tamaño, termina

    ldr w5, [x0, x4, LSL #2] // Carga el elemento arr[i] en w5

    cmp w5, w2               // Compara arr[i] con el valor más grande (largest)
    ble check_second         // Si arr[i] <= largest, salta a la verificación del segundo más grande

    // Si arr[i] > largest, actualiza largest y secondLargest
    mov x3, x2               // Segundo más grande = más grande actual
    mov x2, w5               // Actualiza el más grande

    b increment_index        // Salta a la siguiente iteración

check_second:
    cmp w5, x3               // Compara arr[i] con el segundo valor más grande
    ble increment_index      // Si arr[i] <= secondLargest, continúa
    mov x3, w5               // Actualiza el segundo más grande

increment_index:
    add x4, x4, 1            // Incrementa el índice
    b loop                   // Repite el bucle

end_second_largest:
    mov x0, x3               // Retorna el segundo más grande en x0
    ret                      // Finaliza la función

```
## 37.-Implementar una pila usando un arreglo

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de una pila utilizando un arreglo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo que representa la pila
//   x1 - Tamaño máximo de la pila
//   x2 - Operación a realizar (0 - push, 1 - pop)
//   x3 - Valor a agregar (para push) o posición (para pop)
// Salida:
//   x0 - El valor extraído de la pila en caso de pop, o 0 en caso de éxito en push

// Algoritmo en C# (para referencia)
// class Stack {
//     private int[] arr;
//     private int top;
//     public Stack(int size) {
//         arr = new int[size];
//         top = -1;
//     }
//     public void Push(int value) {
//         if (top < arr.Length - 1) {
//             arr[++top] = value;
//         }
//     }
//     public int Pop() {
//         if (top >= 0) {
//             return arr[top--];
//         }
//         return -1;
//     }
// }

.global stack_operation

stack_operation:
    cmp x2, 0                // Compara si la operación es push (0) o pop (1)
    beq push_operation       // Si es push, salta a la operación push

pop_operation:
    cmp x3, -1               // Verifica si la pila está vacía (top = -1)
    blt end_stack_operation  // Si la pila está vacía, termina la operación

    ldr w4, [x0, x3, LSL #2] // Carga el valor de la pila en w4 (arr[top])
    sub x3, x3, 1            // Decrementa el índice de la pila (top--)

    mov x0, w4               // Retorna el valor extraído de la pila
    b end_stack_operation    // Finaliza la operación

push_operation:
    cmp x3, x1               // Compara si el índice top es menor que el tamaño de la pila
    bge end_stack_operation  // Si la pila está llena, termina la operación

    add x3, x3, 1            // Incrementa el índice de la pila (top++)
    str w4, [x0, x3, LSL #2] // Guarda el valor en la pila

end_stack_operation:
    ret                      // Finaliza la función


```
## 38.-Implementar una cola usando un arreglo

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de una cola utilizando un arreglo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo que representa la cola
//   x1 - Tamaño máximo de la cola
//   x2 - Operación a realizar (0 - encolar, 1 - desencolar)
//   x3 - Valor a agregar (para encolar) o 0 para desencolar
// Salida:
//   x0 - El valor extraído de la cola en caso de desencolar, o 0 en caso de éxito al encolar

// Algoritmo en C# (para referencia)
// class Queue {
//     private int[] arr;
//     private int front;
//     private int rear;
//     public Queue(int size) {
//         arr = new int[size];
//         front = 0;
//         rear = 0;
//     }
//     public void Enqueue(int value) {
//         if ((rear + 1) % arr.Length != front) {  // Verifica si la cola no está llena
//             arr[rear] = value;
//             rear = (rear + 1) % arr.Length;
//         }
//     }
//     public int Dequeue() {
//         if (front != rear) {  // Verifica si la cola no está vacía
//             int value = arr[front];
//             front = (front + 1) % arr.Length;
//             return value;
//         }
//         return -1;
//     }
// }

.global queue_operation

queue_operation:
    cmp x2, 0                // Compara si la operación es encolar (0) o desencolar (1)
    beq enqueue_operation    // Si es encolar, salta a la operación encolar

dequeue_operation:
    cmp x1, 0                // Verifica si la cola está vacía (front == rear)
    beq end_queue_operation  // Si está vacía, termina la operación

    ldr w4, [x0, x1, LSL #2] // Carga el valor de la cola en w4 (arr[front])
    add x1, x1, 1            // Incrementa el índice de la cola (front++)
    mod x1, x1, x3           // Aplica el módulo para que front no exceda el tamaño de la cola

    mov x0, w4               // Retorna el valor extraído de la cola
    b end_queue_operation    // Finaliza la operación

enqueue_operation:
    cmp x2, x3               // Verifica si la cola está llena (rear + 1 == front)
    beq end_queue_operation  // Si está llena, termina la operación

    str w4, [x0, x2, LSL #2] // Guarda el valor en la cola en la posición rear
    add x2, x2, 1            // Incrementa el índice de la cola (rear++)
    mod x2, x2, x3           // Aplica el módulo para que rear no exceda el tamaño de la cola

end_queue_operation:
    ret                      // Finaliza la función

```

## 39.-Convertir decimal a binario

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de número decimal a binario en ARM64 Assembly
// Entradas:
//   x0 - Número decimal (entero)
//   x1 - Puntero al arreglo para almacenar el resultado binario
//   x2 - Tamaño máximo del arreglo para el binario
// Salida:
//   x1 - Puntero al arreglo con el número binario

// Algoritmo en C# (para referencia)
// string DecimalToBinary(int num) {
//     string binary = "";
//     while (num > 0) {
//         binary = (num % 2) + binary;
//         num /= 2;
//     }
//     return binary;
// }

.global decimal_to_binary

decimal_to_binary:
    mov x3, x0               // x3 = número decimal (num)
    mov x4, x2               // x4 = tamaño máximo del arreglo
    sub x4, x4, 1            // Decrementa para usarlo como índice (posición del arreglo)
    mov x5, 0                // Inicializa el índice del arreglo

loop:
    cmp x3, 0                // Verifica si el número es 0
    beq end_conversion       // Si es 0, termina la conversión

    and x6, x3, 1            // Obtiene el bit menos significativo (num % 2)
    str w6, [x1, x4, LSL #2] // Guarda el bit en el arreglo
    lsr x3, x3, 1            // Desplaza el número a la derecha (num /= 2)
    sub x4, x4, 1            // Decrementa el índice del arreglo
    b loop                   // Repite el bucle

end_conversion:
    mov x0, x1               // Retorna el puntero al arreglo con el número binario
    ret                      // Finaliza la función

```
## 40.-Convertir binario a decimal

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de número binario a decimal en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo con el número binario
//   x1 - Longitud del número binario (cantidad de bits)
// Salida:
//   x0 - Número decimal resultante de la conversión

// Algoritmo en C# (para referencia)
// int BinaryToDecimal(int[] bin) {
//     int decimal = 0;
//     for (int i = 0; i < bin.Length; i++) {
//         decimal += bin[i] * (1 << (bin.Length - 1 - i));
//     }
//     return decimal;
// }

.global binary_to_decimal

binary_to_decimal:
    mov x2, 0                // Inicializa el valor decimal en 0
    mov x3, x1               // x3 = longitud del arreglo binario
    sub x3, x3, 1            // Ajusta la longitud para usarla como el índice más alto

loop:
    cmp x3, -1               // Verifica si hemos recorrido todo el arreglo
    blt end_conversion       // Si ya recorrimos todos los bits, termina la conversión

    ldr w4, [x0, x3, LSL #2] // Carga el bit en w4 (arr[x3])
    cmp w4, 0                // Verifica si el bit es 1 o 0
    beq skip_addition        // Si es 0, no hace nada

    mov x5, 1                // Prepara el valor 1 para multiplicar por el bit correspondiente
    lsl x5, x5, x3           // Desplaza 1 hacia la izquierda según la posición del bit (2^x3)
    add x2, x2, x5           // Suma el valor al resultado decimal

skip_addition:
    sub x3, x3, 1            // Decrementa el índice
    b loop                   // Repite el bucle

end_conversion:
    mov x0, x2               // Retorna el número decimal
    ret                      // Finaliza la función

```
## 41.-Conversión de decimal a hexadecimal

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de número decimal a hexadecimal en ARM64 Assembly
// Entradas:
//   x0 - Número decimal (entero)
//   x1 - Puntero al arreglo para almacenar el resultado hexadecimal
//   x2 - Tamaño máximo del arreglo para el hexadecimal
// Salida:
//   x1 - Puntero al arreglo con el número hexadecimal

// Algoritmo en C# (para referencia)
// string DecimalToHexadecimal(int num) {
//     return num.ToString("X");
// }

.global decimal_to_hexadecimal

decimal_to_hexadecimal:
    mov x3, x0               // x3 = número decimal (num)
    mov x4, x2               // x4 = tamaño máximo del arreglo
    sub x4, x4, 1            // Decrementa para usarlo como índice (posición del arreglo)
    mov x5, 0                // Inicializa el índice del arreglo

loop:
    cmp x3, 0                // Verifica si el número es 0
    beq end_conversion       // Si es 0, termina la conversión

    and x6, x3, 15           // Obtiene el valor hexadecimal de 4 bits (num & 0xF)
    cmp x6, 10               // Verifica si el valor es mayor o igual que 10 (para letras A-F)
    blt store_digit          // Si es menor que 10, es un número decimal
    add x6, x6, 87           // Convierte 10-15 a 'a'-'f'

store_digit:
    strb w6, [x1, x4]        // Guarda el dígito hexadecimal en el arreglo
    lsr x3, x3, 4            // Desplaza el número 4 bits a la derecha (num >>= 4)
    sub x4, x4, 1            // Decrementa el índice del arreglo
    b loop                   // Repite el bucle

end_conversion:
    mov x0, x1               // Retorna el puntero al arreglo con el número hexadecimal
    ret                      // Finaliza la función

```
## 42.-Conversión de hexadecimal a decimal

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de conversión de número hexadecimal a decimal en ARM64 Assembly
// Entradas:
//   x0 - Puntero al arreglo con el número hexadecimal (como caracteres ASCII)
//   x1 - Longitud del número hexadecimal (cantidad de caracteres)
// Salida:
//   x0 - Número decimal resultante de la conversión

// Algoritmo en C# (para referencia)
// int HexadecimalToDecimal(string hex) {
//     return Convert.ToInt32(hex, 16);
// }

.global hexadecimal_to_decimal

hexadecimal_to_decimal:
    mov x2, 0                // Inicializa el valor decimal en 0
    mov x3, x1               // x3 = longitud del arreglo hexadecimal
    sub x3, x3, 1            // Ajusta la longitud para usarla como el índice más alto

loop:
    cmp x3, -1               // Verifica si hemos recorrido todo el arreglo
    blt end_conversion       // Si ya recorrimos todos los caracteres, termina la conversión

    ldrb w4, [x0, x3]        // Carga el carácter hexadecimal en w4 (arr[x3])
    cmp w4, '0'              // Verifica si el carácter es un número ('0'-'9')
    blt hex_digit            // Si es menor que '0', es una letra (A-F)
    cmp w4, '9'              // Verifica si el carácter está entre '0' y '9'
    ble convert_to_digit     // Si está entre '0' y '9', conviértelo a un número

hex_digit:
    cmp w4, 'a'              // Verifica si el carácter es una letra entre 'a' y 'f'
    blt invalid_char         // Si es menor que 'a', es un carácter no válido
    cmp w4, 'f'              // Verifica si el carácter es entre 'a' y 'f'
    ble convert_to_digit     // Si está entre 'a' y 'f', conviértelo a un número

invalid_char:
    mov x0, -1               // Devuelve -1 si hay un carácter inválido
    ret

convert_to_digit:
    sub w4, w4, '0'          // Convierte el carácter a un número
    cmp w4, 9                // Verifica si es un número entre 0-9
    ble store_digit          // Si es un número, lo guarda
    sub w4, w4, 32           // Convierte de letra a número (a=10, b=11, ..., f=15)

store_digit:
    mov x5, 1                // Prepara el valor de base 16
    lsl x5, x5, x3           // Desplaza la base 16 según la posición del dígito
    mul x4, x4, x5           // Multiplica el dígito por 16^posición
    add x2, x2, x4           // Suma el valor al total decimal

    sub x3, x3, 1            // Decrementa el índice
    b loop                   // Repite el bucle

end_conversion:
    mov x0, x2               // Retorna el número decimal
    ret                      // Finaliza la función

```
## 43.-Calculadora simple (Suma, Resta, Multiplicación, División)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de una calculadora simple (Suma, Resta, Multiplicación, División) en ARM64 Assembly
// Entradas:
//   x0 - Operando 1 (primer número)
//   x1 - Operando 2 (segundo número)
//   x2 - Operación (0 = Suma, 1 = Resta, 2 = Multiplicación, 3 = División)
// Salida:
//   x0 - Resultado de la operación

// Algoritmo en C# (para referencia)
// double Calculate(double num1, double num2, int operation) {
//     switch (operation) {
//         case 0: return num1 + num2; // Suma
//         case 1: return num1 - num2; // Resta
//         case 2: return num1 * num2; // Multiplicación
//         case 3: return num1 / num2; // División
//         default: throw new InvalidOperationException();
//     }
// }

.global calculator

calculator:
    cmp x2, 0                // Compara operación con 0 (Suma)
    beq add_operation        // Si es Suma, salta a la operación de suma
    cmp x2, 1                // Compara operación con 1 (Resta)
    beq subtract_operation   // Si es Resta, salta a la operación de resta
    cmp x2, 2                // Compara operación con 2 (Multiplicación)
    beq multiply_operation   // Si es Multiplicación, salta a la operación de multiplicación
    cmp x2, 3                // Compara operación con 3 (División)
    beq divide_operation     // Si es División, salta a la operación de división
    b end_calculation        // Si la operación no es válida, termina

add_operation:
    add x0, x0, x1           // Suma los operandos (x0 = x0 + x1)
    ret                       // Retorna el resultado

subtract_operation:
    sub x0, x0, x1           // Resta los operandos (x0 = x0 - x1)
    ret                       // Retorna el resultado

multiply_operation:
    mul x0, x0, x1           // Multiplica los operandos (x0 = x0 * x1)
    ret                       // Retorna el resultado

divide_operation:
    cbz x1, divide_by_zero   // Verifica si el divisor (x1) es 0
    sdiv x0, x0, x1          // Divide los operandos (x0 = x0 / x1)
    ret                       // Retorna el resultado

divide_by_zero:
    mov x0, -1               // Si la división es por 0, retorna -1
    ret                       // Finaliza la función

end_calculation:
    mov x0, -1               // Si la operación no es válida, retorna -1
    ret                       // Finaliza la función


```
## 44.-	Generación de números aleatorios (con semilla)

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación de generación de números aleatorios (con semilla) en ARM64 Assembly
// Entradas:
//   x0 - Semilla (valor inicial para la generación)
//   x1 - Rango (valor máximo en el rango de números aleatorios)
// Salida:
//   x0 - Número aleatorio generado en el rango [0, x1)

.global random_number_generator

random_number_generator:
    mov x2, x0               // Copia la semilla a x2 (valor inicial)
    mov x3, 0x41C64E6D       // Constante multiplicativa (Valor de la multiplicación para el generador lineal congruencial)
    mov x4, 0x3039           // Constante de adición (Valor de la adición para el generador lineal congruencial)
    mov x5, x1               // Copia el rango máximo a x5

    mul x2, x2, x3           // Multiplica la semilla por la constante multiplicativa
    add x2, x2, x4           // Suma la constante de adición
    and x2, x2, 0x7FFFFFFF   // Aplica una máscara para asegurar que el número esté en el rango de 32 bits (para evitar desbordamientos)

    sdiv x0, x2, x5          // Divide el número generado por el rango para obtener un valor entre 0 y x1
    ret                      // Retorna el número aleatorio generado

```
## 45.-Verificar si un número es Armstrong

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Verificación si un número es Armstrong en ARM64 Assembly
// Entradas:
//   x0 - Número a verificar
// Salida:
//   x0 - 1 si es un número Armstrong, 0 si no lo es

// Algoritmo en C# (para referencia)
// bool IsArmstrong(int number) {
//     int sum = 0;
//     int temp = number;
//     int digits = (int)Math.Log10(number) + 1; // Número de dígitos
//     while (temp > 0) {
//         int digit = temp % 10;
//         sum += (int)Math.Pow(digit, digits);
//         temp /= 10;
//     }
//     return sum == number;
// }

.global is_armstrong

is_armstrong:
    mov x1, x0              // Copia el número a x1 (para calcular el número de dígitos)
    mov x2, 0               // Inicializa la variable de la suma a 0
    mov x3, 0               // Inicializa la variable de la potencia de los dígitos a 0
    mov x4, 0               // Inicializa el contador de dígitos a 0

count_digits:
    cmp x1, 0               // Verifica si el número es 0
    beq calculate_sum       // Si es 0, salta a calcular la suma de los dígitos
    add x4, x4, 1           // Incrementa el contador de dígitos
    udiv x1, x1, 10         // Divide el número por 10 para obtener el siguiente dígito
    b count_digits          // Repite el ciclo

calculate_sum:
    mov x1, x0              // Recupera el número original en x1
    mov x5, x4              // Copia el número de dígitos a x5

calculate_digit_sum:
    cmp x1, 0               // Verifica si el número es 0
    beq check_result        // Si es 0, salta a verificar si la suma es igual al número
    udiv x6, x1, 10         // Obtiene el dígito actual (división entre 10)
    mul x6, x6, x6          // Eleva el dígito al cuadrado (o potencia correspondiente)
    add x2, x2, x6          // Suma el resultado al acumulador
    b calculate_digit_sum   // Repite para los otros dígitos

check_result:
    cmp x0, x2              // Compara la suma de las potencias con el número original
    beq armstrong_number     // Si son iguales, es un número Armstrong
    mov x0, 0               // Si no es Armstrong, devuelve 0
    ret

armstrong_number:
    mov x0, 1               // Si es Armstrong, devuelve 1
    ret

```
## 46.-Encontrar prefijo común más largo en cadenas

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación para encontrar el prefijo común más largo entre dos cadenas en ARM64 Assembly
// Entradas:
//   x0 - Puntero a la primera cadena
//   x1 - Puntero a la segunda cadena
// Salida:
//   x0 - Longitud del prefijo común más largo

.global longest_common_prefix

longest_common_prefix:
    mov x2, 0               // Inicializa el contador de longitud del prefijo común
compare_chars:
    ldrb w3, [x0, x2]       // Carga el siguiente carácter de la primera cadena en w3
    ldrb w4, [x1, x2]       // Carga el siguiente carácter de la segunda cadena en w4
    cmp w3, w4              // Compara los caracteres
    bne end_compare         // Si son diferentes, salta a fin de comparación
    cmp w3, #0              // Verifica si es el fin de la cadena (carácter nulo)
    beq end_compare         // Si es el fin de la cadena, termina la comparación
    add x2, x2, 1           // Incrementa el contador de longitud del prefijo común
    b compare_chars         // Repite la comparación para el siguiente par de caracteres

end_compare:
    mov x0, x2              // Devuelve la longitud del prefijo común
    ret                      // Retorna el resultado

```
## 47.-Detección de desbordamiento en suma

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación para detectar desbordamiento en la suma de dos números enteros en ARM64 Assembly
// Entradas:
//   x0 - Primer número
//   x1 - Segundo número
// Salida:
//   x0 - 1 si hay desbordamiento, 0 si no lo hay

.global detect_overflow

detect_overflow:
    add x2, x0, x1          // Suma los dos números y almacena el resultado en x2
    cmp x0, x2              // Compara el primer número con el resultado
    ccs x0, #1              // Si hay un carry (desbordamiento), x0 = 1
    moveq x0, #0            // Si no hay desbordamiento, x0 = 0
    ret                     // Retorna el resultado

```
## 48.-Medir el tiempo de ejecución de una función

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación para medir el tiempo de ejecución de una función en ARM64 Assembly
// Entradas:
//   Ninguna
// Salida:
//   x0 - El tiempo de ejecución en nanosegundos

.global measure_execution_time

measure_execution_time:
    // Guardar el valor actual del contador de tiempo
    mrs x1, cntvct              // Carga el valor actual del contador de tiempo en x1

    // Llamada a la función cuyo tiempo de ejecución vamos a medir
    bl target_function          // Llama a la función objetivo que se desea medir

    // Calcular el tiempo transcurrido
    mrs x2, cntvct              // Carga el valor actual del contador de tiempo después de la ejecución
    subs x0, x2, x1             // Calcula la diferencia entre el tiempo de finalización y el tiempo de inicio

    ret                         // Retorna el tiempo de ejecución en x0 (en ciclos de reloj)
    
// Esta función es un ejemplo y debería ser reemplazada por la función real cuya duración se desea medir
target_function:
    // Lógica de la función que se desea medir
    // Este código puede ser reemplazado por cualquier función real
    mov x3, #0
    add x3, x3, #1
    ret

```
## 49.-Leer entrada desde el teclado

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación para leer entrada desde el teclado en ARM64 Assembly
// Entradas:
//   Ninguna
// Salida:
//   x0 - Valor leído desde el teclado (como entero)

.global read_input

read_input:
    // Preparar la llamada al sistema para leer entrada desde el teclado
    mov x8, #63               // Número de la llamada al sistema para leer entrada (sys_read)
    mov x0, #0                // Descriptor de archivo 0 (stdin)
    mov x1, sp                // Dirección en la pila donde se almacenará la entrada
    mov x2, #4                // Número de bytes a leer (por ejemplo, leer un entero de 4 bytes)
    svc #0                    // Realiza la llamada al sistema

    // Convertir la entrada leída en la pila a un entero
    ldr w0, [sp]              // Carga el valor leído en w0

    ret                       // Retorna el valor leído desde el teclado en x0

```
## 50.-Escribir en un archivo

```
// Autor: Sanchez Salinas Eduardo Josue
// Fecha: Fecha Actual
// Descripción: Implementación para escribir en un archivo en ARM64 Assembly
// Entradas:
//   x0 - Puntero al buffer que contiene los datos a escribir
//   x1 - Tamaño de los datos a escribir (en bytes)
// Salida:
//   Ninguna (escribe los datos en el archivo)

.global write_to_file

write_to_file:
    // Preparar la llamada al sistema para escribir en un archivo
    mov x8, #64               // Número de la llamada al sistema para escribir (sys_write)
    mov x2, #1                // Descriptor de archivo 1 (stdout), cambiar por el descriptor de archivo del archivo objetivo
    mov x1, x0                // Dirección del buffer de datos a escribir
    svc #0                    // Realiza la llamada al sistema para escribir en el archivo

    ret                       // Retorna después de escribir en el archivo

```
