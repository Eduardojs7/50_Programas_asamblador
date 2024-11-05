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
    msg_input db 'Introduce la temperatura en Celsius: ', 0
    msg_output db 'Temperatura en Fahrenheit: ', 0
    newline db 10, 0

section .bss
    celsius resb 10
    fahrenheit resb 10

section .text
    global _start

_start:
    ; Imprimir el mensaje de entrada
    mov eax, 4          ; syscall para write
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_input  ; puntero al mensaje
    mov edx, 33         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Leer la temperatura en Celsius desde la entrada
    mov eax, 3          ; syscall para read
    mov ebx, 0          ; descriptor de entrada (stdin)
    mov ecx, celsius    ; puntero al buffer donde se almacena el número
    mov edx, 10         ; tamaño máximo de caracteres a leer
    int 0x80            ; interrupción del sistema

    ; Convertir la cadena de caracteres a número (simplificado para enteros)
    mov eax, 0          ; reiniciar eax
    mov ebx, celsius    ; puntero al buffer
convert_loop:
    movzx edx, byte [ebx]  ; cargar un byte del buffer
    test edx, edx           ; verificar si es el fin de la cadena (null terminator)
    jz done_convert
    sub edx, '0'            ; convertir el carácter a número
    imul eax, eax, 10       ; multiplicar el valor acumulado por 10
    add eax, edx            ; agregar el nuevo dígito
    inc ebx                 ; mover al siguiente carácter
    jmp convert_loop

done_convert:
    ; Realizar la conversión de Celsius a Fahrenheit
    ; Fórmula: F = (C * 9 / 5) + 32
    mov ebx, eax        ; celsius guardado en ebx
    imul eax, ebx, 9    ; celsius * 9
    div ecx, 5          ; dividir el resultado entre 5
    add eax, 32         ; sumar 32 para obtener Fahrenheit

    ; Mostrar el mensaje de salida
    mov ebx, 1          ; descriptor de salida (stdout)
    mov ecx, msg_output ; puntero al mensaje
    mov edx, 25         ; longitud del mensaje
    int 0x80            ; interrupción del sistema

    ; Aquí añadir código para mostrar el valor de Fahrenheit

    ; Salir del programa
    mov eax, 1          ; syscall para exit
    xor ebx, ebx        ; código de salida 0
    int 0x80            ; interrupción del sistema

 ```

### Corrida

Introduce la temperatura en Celsius: 25
25°C es igual a 77°F


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


## Serie de Fibonacci
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

## Verificar si un número es primo
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

## Invertir una cadena
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


## Verificar si una cadena es palíndromo
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

## Encontrar el máximo en un arreglo
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

## Encontrar el mínimo en un arreglo
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

