# DOCUMENTACIÓN DE CÓDIGO 1

## Estructura de Datos - Sergio Hernández García

### Interfaz _(IntCell.h)_

--- 
Para comenzar nos dan la introduccion al codigo los preprocesadores ifndef y define; ayudandonos a evitar 
problemas de inclusion multiple en nuestro código:

    #ifndef INTCELL_H
    #define INTCELL_H 

Para posteriormente comenzar con la declaracion de la clase llamada **IntCell** del tipo **público**:
    
    class IntCell {                        // definicion de clase
    public:                                // se determina que esta secci[on sera publica
    IntCell() = default;                   // contructor predeterminado (default)
    IntCell(int newValue = 0);             // constructor con valor inicial opcional (0)
    IntCell(const IntCell &rhs);           // call-by-constant reference (&)
    IntCell(IntCell &&rhs) noexcept;       // call-by-rvalue-reference (&&)
    ~IntCell() = default;                  // declaracion del destructor (~)

Ahora basandonos en el codigo mostrado; como ya mencione se nos declara la clase IntCell con datos que seran del 
tipo publico, lo que significa que pueden ser usados por agentes externos a nuestra clase. 

Despues es definido un valor para IntCell por **default/predeterminado** por C++, luego gracias al **newValue** 
se le asigna un valor especifico que en este caso es 0. En las siguientes dos lineas estan los operadores _&_ y _&&_;
los cuales son llamadas de referencia, uno haciendo referencia constante a nuestro objeto lo que evita la creacion 
de copias inncesarias y el otro devuelve la referencia esta vez ya **modificada** al objeto, respectivamente
(lo cual es util para semanticas de movimiento y optimizacion de recursos).

Siguiendo la informacion al basarnos en el libro _"DATA STRUCTURES AND ALGORITHM ANALYSIS IN C++ 4th Edition
by M.A. Weiss"_ dentro del capitulo 1, paginas 1-48.
[Libro](https://drive.google.com/file/d/1qT9N4BUQjUet9uXe1oQWa9y7nHmqtVhJ/view)

O en el resumen que hicimos los alumnos de Ing. Programacion de Videojuegos para esta materia, en mi caso
es el siguiente 
[Resumen](https://docs.google.com/document/d/1oRxZ3c_Je0YSugHmVQpi7CuPER-okAb34F36fzpUUjE/view); teniendo ambos como
referencia para la documentacion de este codigo.

Continuando con la documentacion, encontramos un destructor, el cual libera recursos de forma predeterminada
que esten asociados a IntCell.

    ~IntCell() = default;



Dentro de la misma interfaz estan las siguientes lineas:
    
    IntCell &operator=(const IntCell &rhs);        // asigna valores de otras instacias al actual, gracias a una copia
    IntCell &operator=(IntCell &&rhs) noexcept;    // asigna un mov pero la funcion no producira ninguna excepcion                                                           
    IntCell &operator=(int rhs);                   // asigna valor entero a IntCell
    IntCell &operator+(const IntCell &rhs);        // es una suma entre los valores de IntCell y devolvera el resultado 
    IntCell &operator+(int rhs);                   // suma entre valor actual de IntCell y un valor entero 
    IntCell &operator-(const IntCell &rhs);
    IntCell &operator-(int rhs);                   //estas dos lineas son similares a las anteriores pero en resta

La siguiente linea es metodo que devolvera el valor que esta almacenado en IntCell actualmente por medio de un 
**getValue** pero no lo modificara internamente. Seguido de la modificacion directa de IntCell y su valor almacenado.
    
    int getValue() const;                 //devuleve el valor 
    void setValue(int newValue);          //da un nuevo valor

Por otro lado, tambien tenemos valores de tipo **privado**, o sea; solo sera accesible dentro de la clase:

    private:                // esta seccion sera privada
    int storedValue;        // valor principal para IntCell
    int storedValue2;
    int storedValue3;       //valores almacenados adicionales
    };

Terminando la **INTERFAZ** con un endif, lo que nos dice que es el final del bloque.
    
    #endif  // INTCELL_H


### Codigo Completo Interfaz 

    #ifndef INTCELL_H
    #define INTCELL_H
    
    class IntCell {
    public:
    IntCell() = default;
    IntCell(int newValue = 0);
    IntCell(const IntCell &rhs);
    IntCell(IntCell &&rhs) noexcept;
    ~IntCell() = default;

    IntCell &operator=(const IntCell &rhs);
    IntCell &operator=(IntCell &&rhs) noexcept;
    IntCell &operator=(int rhs);
    IntCell &operator+(const IntCell &rhs);
    IntCell &operator+(int rhs);
    IntCell &operator-(const IntCell &rhs);
    IntCell &operator-(int rhs);

    int getValue() const;
    void setValue(int newValue);

    private:
    int storedValue;
    int storedValue2;
    int storedValue3;
    };
    
    #endif  // INTCELL_H

---
### Implementacion _(IntCell.cpp)_

    #include "IntCell.h" // permitw el uso del codigo anterior

La implementacion inicia con #include, lo que permite que podamos usar el codigo dentro del IntCell.h.
Para despues mostrar la implementacion por constructores para el valor inicial, una copia y un moviiento basados en
el valor dado dentro de los parametros _privados_ de _IntCell.h_.

    IntCell::IntCell(int newValue) : storedValue(newValue) {}                       //valor inicial
    IntCell::IntCell(const IntCell &rhs) : storedValue(rhs.storedValue) {}          //crea una copia de IntCell
    IntCell::IntCell(IntCell &&rhs) noexcept : storedValue(rhs.storedValue) {}      // transfiere datos de storedvalue
                                                                                    a una nueva         
Tambien contamos con operadores por asignacion...
* Operator=:

    
    //Copia los valores de IntCell a otra
    IntCell &IntCell::operator=(const IntCell &rhs) {
    if (this != &rhs) {
    storedValue = rhs.storedValue;
    }
    return *this;
    }

    //Mueve por transferencia los valores de IntCell a otra
    IntCell &IntCell::operator=(IntCell &&rhs) noexcept {
    if (this != &rhs) {
    storedValue = rhs.storedValue;
    rhs.storedValue = 0;
    }
    return *this;
    }
    
    //Asigna un valor entero a IntCell
    IntCell &IntCell::operator=(int rhs) {
    storedValue = rhs;
    return *this;
    }

* Operator+:

    
    
    //suma dos intancias 
    IntCell &IntCell::operator+(const IntCell &rhs) {
    storedValue = storedValue + rhs.storedValue;
    return *this;
    }
    
    //Suma un valor directo al storedvalue
    IntCell &IntCell::operator+(int rhs) {
    storedValue = storedValue + rhs;
    return *this;
    }

* Operator-:
    
    
    //Similar al +, pero en resta
    IntCell &IntCell::operator-(const IntCell &rhs) {
    storedValue = storedValue - rhs.storedValue;
    return *this;
    }
    
    IntCell &IntCell::operator-(int rhs) {
    storedValue = storedValue - rhs;
    return *this;
    }

Y para terminar estan 

    int IntCell::getValue() const {
    return storedValue;
    }
    
    void IntCell::setValue(int newValue) {
    storedValue = newValue;
    }

1.  int IntCell::getValue() const{} -> devuelve el valor storedvalue sin modificaciones
2.  void IntCell::setValue(int newValue) {} -> da un valor a storedvalue

### Codigo Completo Implementacion
    #include "IntCell.h"
    IntCell::IntCell(int newValue) : storedValue(newValue) {}
    
    IntCell::IntCell(const IntCell &rhs) : storedValue(rhs.storedValue) {}
    
    IntCell::IntCell(IntCell &&rhs) noexcept : storedValue(rhs.storedValue) {}
    
    IntCell &IntCell::operator=(const IntCell &rhs) {
    if (this != &rhs) {
    storedValue = rhs.storedValue;
    }
    return *this;
    }
    
    IntCell &IntCell::operator=(IntCell &&rhs) noexcept {
    if (this != &rhs) {
    storedValue = rhs.storedValue;
    rhs.storedValue = 0;
    }
    return *this;
    }
    
    IntCell &IntCell::operator=(int rhs) {
    storedValue = rhs;
    return *this;
    }
    
    IntCell &IntCell::operator+(const IntCell &rhs) {
    storedValue = storedValue + rhs.storedValue;
    return *this;
    }
    
    IntCell &IntCell::operator+(int rhs) {
    storedValue = storedValue + rhs;
    return *this;
    }
    
    IntCell &IntCell::operator-(const IntCell &rhs) {
    storedValue = storedValue - rhs.storedValue;
    return *this;
    }
    
    IntCell &IntCell::operator-(int rhs) {
    storedValue = storedValue - rhs;
    return *this;
    }

    int IntCell::getValue() const {
    return storedValue;
    }
    
    void IntCell::setValue(int newValue) {
    storedValue = newValue;
    }

---
### C[omo afectan a la RAM?


Ya que estamos definiendo los que sera nuestra clase en IntCell.h esto hace que los datos aqui
ocupen espacio en neustra RAM, aunque sea minimo lo hacen. Pero nos ayuda al momento de pasar a la implementacion, 
pues al haberlas declarado con anterioridad no agrega datos adicionales a la memoria. Ya que, operan directamente sobre 
datos ya existentes. 
Por otro aldo nuestras variables locales posiblemente si ocupen un espacio en las calls de ejecucion pero la memoria 
se liberara automaicamente al finalizarla.