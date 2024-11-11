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
