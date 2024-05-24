# DOCUMENTACIÓN DE CÓDIGO 2
## Estructura de Datos - Sergio Hernández García

### Listas Enlazadas Simples _(SLList)_

---
## 1. Interfaz < SLList.h >
    
Gracias a la documentacion del codigo anterior, donde vimos el IntCell; sabemos que el siguiente codigo empieza con 
prepocesadores los culaes nos ayudan a evitar poblemas de inclusion multiple: 

    #ifndef SLLIST_H
    #define SLLIST_H

En este caso tenemos nuevas declaraciones las cuales son conocidas como librerias. Comenzando por la libreria/biblioteca 
**'iostream'** y segun Learn by Microsoft funciona para la declaracion de objetos que puedan controlar tanto la lectura 
como la escritura, siendo el unico encabezado necesario para realizar entradas y salidas de un programa manejado con C++.

Sin embargo, tambien hay existen algunas otras como:
1. ios - Define tipos y funciones basicas relacionadas con la entrada y salida de datos.
2. streambuf - Funciona para gestionar buffers de entrada y salida de datos.
3. istream - Proporciona clases y funciones para la gestion de entrada de datos.
4. ostream - Propociona clases y funciones para la gesntion de salida de datos.

Pero se suele y se prefiere usar 'iostream'; ya que, contiene todo lo anterior mecionado aunque sea de manera basica 
ademas, al momento de su ejecucion suele ser mas veloz ya que no agrega funcionalidades extras que no seran necesarias 
dentro de un manejo de codigo simple, pues esta misma libreria ya nos permite el correcto manejo de entradas y salidas
de datos al incluir las intrucciones **cin** y **cout**.

    #include <iostream>
    #include <utility>
    #include "SLList.cpp"

Tambien tenemos a la libreria **'utility'**; que en caso personal es la primera vez que la veo. Pero basandonos en la 
informacion dentro de _"Learn by Microsoft, en el tema Conceptos Basicos"_ [Learn](https://learn.microsoft.com/en-us/cpp/standard-library/utility?view=msvc-170)
y _"GeekforGeeks en el tema C++"_ [GeekforGeeks](https://www.geeksforgeeks.org/utility-in-c/?ref=header_search), 
esta funciona para definir funciones, tipos y operadores los cuales facilitaran la administracion y creacion de objetos 
pares, los cuales seran tratados como uno solo. Especificamente algunas de sus funciones son

1. Intercambiar el valor de dos objetos (swap)
2. Creacion de pares de elementos (make_pair)
3. Permite la transferencia de recursos entre objetos (move)
4. Etc.


Por ultimo dentro de los header esta la inclusion del .cpp el cual contiene la deficion de los metodos de la clase 
SLList; sin embargo aqui quiero resaltar un punto, pues no estoy seguro si este .cpp esta implementado de forma 
correcta dentro del .h.
Pues al estar investigando encontre que no es una forma ideonea, pues esto puede afectar en los tiempos de compilacion; 
ademas de que se puede confundir con una mala practica al programar ya que hace redundante el tener separados ambos 
codigos, pues si se queria tomar el cpp dentro del .h, entonces mejor tenerlos juntos. Quizas asi se tenia el codigo en 
inicio y al momento de separarlo, se fue este dentro del .h.

Vuelvo a decir, no estoy del todo seguro; y me pareceria bien que se mencionara esto dentro de la clase, pues esto que 
menciono es tocado en foros de discusion como _Reddit, Quora_ y _Stack Overflow_. En los cuales se menciona que quizas sea 
una forma de simplificacion al momento de codificar, pero que esto vuelve que el codigo se demore mas al compilar o que 
simplemente no compile y se vuelva una **mala practica**.

Volviendo al codigo, se nos presenta en primer instacion una plantilla de tipo objeto; la cual nos permitira crear 
instancias de la Clase **'SLList'** con una variedad de tipos de datos _(int. string, double)_.
Y gracias a su nombre podemos darnos cuenta que trabajamos con una Lista Simplemente Enlazada.

    template<typename Object>
    class SLList {

Seguido de la instanciacion de una clase de tipo publico, que a grandes rasgos usa la **definicion de una estructura Node** 
para poder crear nodos con valores por default o dados por el usurio, permitiendo a su vez establecer punteros al o los 
siguientes nodos, proporcionando flexibilidad para **crear** y/o **manipular** los nodos de neustra lista.
En este caso podemos ver que se hace referencia a un objeto d y al nodo n, para seguir con su manipulacion al mover 
dicho objeto **(&&d)** para despues instanciarlo en el siguiente nodo con un valor por defecto **(*n = nullptr)**.

    public:                     //esta seccion sera publica
    struct Node {               //se define la estrucutra Node
    Object data;                //este sera nuestro dato almacenado en el nodo
    Node *next;                 // puntero al siguiente nodo
    
            Node(const Object &d = Object{}, Node *n = nullptr); // constructor con valor default
    
            Node(Object &&d, Node *n = nullptr);                 //contrcutro de movimiento
    };

Definiendo asi, los nodos individuales que compondran nuestra lista.

La siguiente clase es mas sencilla de comprender o leer, pues en la codificacion anterior ya se manejaron los operadores,
los cuales tambien estan presentes en este codigo, dentro de una **Clase 'Iterator'**; teniendo operadoes de 
1. indireccion (_operator*_) - perimite acceder al objeto al que se le hace referencia
2. preincremento (_operator++_) y postincremento (_operator++(int)_) - ambos permiten avanzar a los siguientes nodos
3. igualdad (_operator==_) y desigualdad (_operator!=_) - permiten comparar iteraciones

Lo cual nos permitira recorrer nuestra lsita de principio a fin, pues nos muestra iteradores para avanzar al siguiente dato,
acceder al acutal y la comparacion entre si.

    public:
    class iterator {                                         //definicion de la clase iterator
    public:
    iterator();                                              //default
    
            Object &operator*();                             //operador indireccion
    
            iterator &operator++();                          //operador preincremento
    
            const iterator operator++(int);                  //operador postincremento
    
            bool operator==(const iterator &rhs) const;      //operador igualdad
    
            bool operator!=(const iterator &rhs) const;      //operador desigualdad

Dentro de esta misma clase **'Iterator'** tenemos una seccion privada:
    
        private:                                             //esta seccion sera privada
            Node *current;                                   //crea un puntero al nodo actual
    
            iterator(Node *p);                               // este construcor resibe un puntero a un nodo
    
            friend class SLList<Object>;                     //la clase SLList, sera tomada como amiga
        };
    
Bien, la parte diferente al resto aqui es **friend class** esto lo que hace es declarar la clase SLList como clase amiga/aliada
permitiendole con esta accion que pueda acceder a los datos privados de esta clase. Ademas de que tambien la clase Iterator 
podra acceder a los privados de SLList, lo que quiere decir que ambas clases podran comunicarse y colaborar entre si.

Casi para concluir con el .h se hace la definicion de los miembros publicos de la Clase SLList, dentro de esta se toman
destructores, contructores y metodos que podrian **manipualr nuestro lista enlazada** al dar con el tamaño, verificar 
si esta llena o vacia, limpiarla, acceder para insertar/eliminar elementos y el poder imprimir dicha lista.

    public:
    SLList();                                                     //constructor default
    
        SLList(std::initializer_list <Object> init_list);         //constructor con una lista inicial
    
        ~SLList();                                                //desctructor de SLList
    
        iterator begin();
        iterator end();                                           //metodos para obtener un iterador al inicio y final de la lista
    
        int size() const;                                         //checa el tamaño
    
        bool empty() const;                                       //verifica si esta vacia
    
        void clear();                                             //borra todos los elementos de la lsita
    
        Object &front();                                          //ayuda a obtener el 1er elemento
        void pop_front();                                         //borra el 1er elemento
        
        void push_front(const Object &x);
        void push_front(Object &&x);                              //metodo para agregar un elemento al inicio de la lista, pero uno lo hace mediante mov

        iterator insert(iterator itr, const Object &x);
        iterator insert(iterator itr, Object &&x);                //tambien agregan elementos pero lo hacen en una posicion especifica
    
        iterator erase(iterator itr);                             //esta elimina elementos en una pos especifica
    
        void print();                                             //imprime los elementos

Esta seccion tambien contiene su parte privada donde se incluyen punteros que referencian directamente al ultimo y primer
nodo de nuestra lista, la posibilidad de almacenar su tamaño y la inicializacion.
    
    private:
    Node *head;                                    //hace referencia al primer nodo
    Node *tail;                                    //al ultimo
    int theSize;                                   //Tamaño 
    
        void init();                               //Metodo privado para arrancar la lista
    };
    
Y finalmente el preprocesador **#endif** que nos aclara que este es el final del bloque:

    #endif


### Codigo Completo .h

    #ifndef SLLLIST_H
    #define SLLLIST_H
    
    #include <iostream>
    #include <utility>
    
    template<typename Object>
    class SLList {
    public:
    struct Node {
    Object data;
    Node *next;
    
            Node(const Object &d = Object{}, Node *n = nullptr);
    
            Node(Object &&d, Node *n = nullptr);
        };
    
    public:
    class iterator {
    public:
    iterator();
    
            Object &operator*();
    
            iterator &operator++();
    
            const iterator operator++(int);
    
            bool operator==(const iterator &rhs) const;
    
            bool operator!=(const iterator &rhs) const;
    
        private:
            Node *current;
    
            iterator(Node *p);
    
            friend class SLList<Object>;
        };
    
    public:
    SLList();
    
        SLList(std::initializer_list <Object> init_list);
    
        ~SLList();
    
        iterator begin();
    
        iterator end();
    
        int size() const;
    
        bool empty() const;
    
        void clear();
    
        Object &front();
    
        void push_front(const Object &x);
    
        void push_front(Object &&x);
    
        void pop_front();
    
        iterator insert(iterator itr, const Object &x);
    
        iterator insert(iterator itr, Object &&x);
    
        iterator erase(iterator itr);
    
        void print();
    
    private:
    Node *head;
    Node *tail;
    int theSize;
    
        void init();
    };
    
    #include "SLList.cpp"
    
    #endif

--- 
## 2. Implementacion <SLList.cpp>

    #include "SLList.h"
    
Gracias a la inclusion del .h es que el siguiente codigo puede acceder a las definiciones previamente hechas de la clase 
SLList, Iterator, la estrucutra Node, funciones miembro y datos que ahi se encuentren. 

Comenzando por la definicion de constructores para los nodes, en primer lugar se esta la referencia a un objeto y puntero n, 
donde 'data' es iniciado con el valor 'd', gracias a una lista con el formato **data{d}, next{n}**.

    template<typename Object>
    SLList<Object>::Node::Node(const Object &d, Node *n)
    : data{d}, next{n} {}
    
Y el segundo constructor de la estructura Node, hace referencia a un rvalue, en esta ocasion el 'data' se inicia al usar un 
std::mode(d), permitiendo asi el movimiento de recursos.

    template<typename Object>
    SLList<Object>::Node::Node(Object &&d, Node *n)
    : data{std::move(d)}, next{n} {}
    
Bien, para no tener que estar repitiendo esto; el codigo que se muestra aqui depende de plantillas con el tipo de objeto 
'Object', antes de instanciar el constructor se muestra la plantilla **template'<typename Object'**, bajo mi punto de vista 
era algo redundante tener que instancias siempre el metodo y el constructor, pero al investigar un poco me di cuenta que es
necesario para que la clase SLList se vuelva flexible a poder trabajar con cualquier tipo de dato. Ademas, como estamos iniciando
con la programacion creo que es una forma sencilla de poder trabajar con listas sin tener que recurrir a codificacion "mas complicada"
como la declaracion dentro del .h, para que todo dentro del .cpp sea tomado como 'Object' y que al final por ahorranos codigo
tengamos muchos otros errores.
   
     template<typename Object>
    SLList<Object>::iterator::iterator() : current{nullptr} {}

En este caso el SLList(Object); define el constructor por default de la clase 'Iterator' el cual esta anidado a la clase 
principal, para luego inicial el puntero current con un valor nulo, indicando que el interador no apunto a una posicion
especifica. 

Siguiendo con la clase Iterator:

    template<typename Object>
    Object &SLList<Object>::iterator::operator*() {
    if(current == nullptr)
    throw std::logic_error("Trying to dereference a null pointer.");
    return current->data;
    }

Aqui se sobrecarga al operador (*), para poder acceder al objeto que es almacenado en el nodo actual al que el iterador esta 
apuntando, en caso de que el puntero **'current'** se fije a **'nullptr'** se lanzara un mensaje que indique que el puntero
esta intentenado desreferenciar a un puntero nulo.

    template<typename Object>
    typename SLList<Object>::iterator &SLList<Object>::iterator::operator++() {
    if(current)
    current = current->next;
    else
    throw std::logic_error("Trying to increment past the end.");
    return *this;
    }

Por otro lado, al usar el operador **++**, hace que nuestro iterador avance al siguiente nodo de la lista. Si el puntero es nulo,
significa que ya estamos al final de la lsita, por lo que en esta ocasion se lanzara un mensaje que indica "Se esta intendando
incrementar mas alla del final"

A diferencia del anterior, en el siguiente fragmento de codigo se usa un operador con valor entero **++(int)**; lo que crea
una copia del interador actual _(old)_, para luego incrementar al original y finalmente devuelve la copia, perimitiendonos
mantener el valor original del iterador antes de que lo incrementemos.

    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::iterator::operator++(int) {
    iterator old = *this;
    ++(*this);
    return old;
    }

Luego entramos al manejo de operadores de igualdad y desigualdad, dentro de nuestra SLList. 

    template<typename Object>
    bool SLList<Object>::iterator::operator==(const iterator &rhs) const {
    return current == rhs.current;
    }

Este oeprador de igualdad hace una comparacion entre dos interadores devolviendo un 'true', si ambos apuntan al mismo nodo 
de nuestra lista enlazada. **Nota: Se dice que devuelve un valor verdadero cuando los punteros current y rhs.current apuntan
ambos a la misma ubicacion, siginificando que nuestros punteros son iguales. En caso de que esto no sea asi, o sea, que apunten
a diferentes ubicaciones sera un 'false'.**
    
    template<typename Object>
    bool SLList<Object>::iterator::operator!=(const iterator &rhs) const {
    return !(*this == rhs);
    }

En este caso, al tener un operador de desigualdad, se devolvera un **'true'** si los punteros current son diferentes, ya que 
se puede concluir que los iteradores no apuntan al mismo elemento por lo que nuestra desigualdad es verdadera. Y si son iguales 
un false. 

Para continuar, el constructor de la Clase **'Iterator'** recibira como punturo a un nodo como argumento para luego asignarlos 
al 'current', logrando crear iteradores que apunten a nodos especificos en la lista.
    
    template<typename Object>
    SLList<Object>::iterator::iterator(Node *p) : current{p} {}
    
Seguidamente de la creacion de una lista vacia, al iniciar **head** y **tail** con nuevos nodos.

    template<typename Object>
    SLList<Object>::SLList() : head(new Node()), tail(new Node()), theSize(0) {
    head->next = tail;
    }

La siguiente trabaja con un constructor con valor Object, para crear una lsita con los elementos enteriormente mencionados; 
comenzando con la inicilizacion de head y tail, para recorrerla e insertar cada elemento al frende de la lista al usar 
**'push_front'**.

    template<typename Object>
    SLList<Object>::SLList(std::initializer_list <Object> init_list) {
    head = new Node();
    tail = new Node();
    head->next = tail;
    theSize = 0;
    for(const auto& x : init_list) {
    push_front(x);
    }
    }

Despues se crea un destructor, el cual liberara la memoria por medio del 'clear' y posteriormente, eliminar los nodos 'head'
y 'tail'

    template<typename Object>
    SLList<Object>::~SLList() {
    clear();
    delete head;
    delete tail;
    }
    
Las siguientes lineas son funciones que proporcionaran acceso a la lista y tendran operaciones sobre ella.
    
    //Begin - permite iterar sobre la lista empezando desde el primer elemento
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::begin() { return {head->next}; }
    
    //End - identifica el final de la lista
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::end() { return {tail}; }
    
    //Size - devuelve el tamaño actual 
    template<typename Object>
    int SLList<Object>::size() const { return theSize; }
    
    //Empty - verifica si la lista esta vacia
    template<typename Object>
    bool SLList<Object>::empty() const { return size() == 0; }
    
    //El clear elimina elementos, pero al contrar con un while y el pop_front; hace que el clear sea llamado repetidamente
    asegurandonos asi que si estara vacia por completo
    template<typename Object>
    void SLList<Object>::clear() { while (!empty()) pop_front(); }
    
Aqui tenemos varios elementos... primero el **front** devuelve la referencia al primer elemento de la lista, verificando si esta
vacia, si si lo esta lanzara un mensaje "Lista Vacia", en caso de que no lo este hara referencia al primer elemento.

    template<typename Object>
    Object &SLList<Object>::front() {
    if(empty())
    throw std::logic_error("List is empty.");
    return *begin();
    }
    
Luego insertara un nuevo elemento con el valor x al inicio de la lista.

    template<typename Object>
    void SLList<Object>::push_front(const Object &x) { insert(begin(), x); }
    
Esto es similar al anterior pero la insercion del nuevo valor, sera por movimiento.

    template<typename Object>
    void SLList<Object>::push_front(Object &&x) { insert(begin(), std::move(x)); }
    
En este fragmento eliminara el primer elemento de la lista; si ya esta vacio lanzara un mensaje en caso de que no lo este. 
Usara la funcion **erase** para eliminarlo.

    template<typename Object>
    void SLList<Object>::pop_front() {
    if(empty())
    throw std::logic_error("List is empty.");
    erase(begin());
    }
    
Despues, se insertara un nuevo elemento justo despues del elemento al que el iterador apunta con el valor x. Creando un nuevo
nodo con ese valor, ubicandose entre el actual y el siguiente nodo dentro de la lista, por lo que incrementara el tamaño. 
**Apuntando al nuevo nodo**.

    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, const Object &x) {
    Node *p = itr.current;
    Node *newNode = new Node{x, p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
    }


A grandes rasgos, se inserta un nuevo nodo en la lista despues del nodo al que se apunta por el 'itr', moviendo el valor x
al nuevo para despues actualizar los punteros para que asi se inserte de manera correcta ese nuevo nodo para finalmente 
apuntarlo con un iterador.

    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, Object &&x) {
    Node *p = itr.current;
    Node *newNode = new Node{std::move(x), p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
    }
    
La funcion **erase(iterator itr)**. Elimina el nodo apuntado por itr, al primero verificar si nuestro iterador esta al final 
de la lista, gracias a esto es que toma al penultimo para poder borrar al que esta al final, para despues liberar la memoria
que este ocupaba y asi devolver un iterador al penultimo (que ahora es el ultimo). 

    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::erase(iterator itr) {
    if (itr == end())
    throw std::logic_error("Cannot erase at end iterator");
    Node *p = head;
    while (p->next != itr.current) p = p->next;
    Node *toDelete = itr.current;
    p->next = itr.current->next;
    delete toDelete;
    theSize--;
    return iterator(p->next);
    }
    
Para casi terminar el **print**, pues imprimira los elementos de la lista enlazada. Iterandola desde el inicio hasta el final, 
imprimiendo el valor de cada uno de los nodos que hemos estado trabajando en la consola, seguido de un leve espacio para 
tener un orden mas visual y que no este todo revuelto.

    template<typename Object>
    void SLList<Object>::print() {
    iterator itr = begin();
    while (itr != end()) {
    std::cout << *itr << " ";
    ++itr;
    }
    std::cout << std::endl;
    }
    
Ahora si por ultimo y para poder concluir con esta documentacion, es que esta la inicilaizacion de la lista; al establecerla
con un tamaño 0, lo cual nos ayuda a entender que la lista esta vacia. Luego hace una conexion entre los nodos head y tail, 
al asignarles un puntero next entre si. 


    template<typename Object>
    void SLList<Object>::init() {
    theSize = 0;
    head->next = tail;
    }

Esto nos asegura que la lista este **COMPLETAMENTE** vacia y lista para ser usada. 

## Codigo Completo .cpp

    #include "SLList.h"
    
    template<typename Object>
    SLList<Object>::Node::Node(const Object &d, Node *n)
    : data{d}, next{n} {}
    
    template<typename Object>
    SLList<Object>::Node::Node(Object &&d, Node *n)
    : data{std::move(d)}, next{n} {}
    
    template<typename Object>
    SLList<Object>::iterator::iterator() : current{nullptr} {}
    
    template<typename Object>
    Object &SLList<Object>::iterator::operator*() {
    if(current == nullptr)
    throw std::logic_error("Trying to dereference a null pointer.");
    return current->data;
    }
    
    template<typename Object>
    typename SLList<Object>::iterator &SLList<Object>::iterator::operator++() {
    if(current)
    current = current->next;
    else
    throw std::logic_error("Trying to increment past the end.");
    return *this;
    }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::iterator::operator++(int) {
    iterator old = *this;
    ++(*this);
    return old;
    }
    
    template<typename Object>
    bool SLList<Object>::iterator::operator==(const iterator &rhs) const {
    return current == rhs.current;
    }
    
    template<typename Object>
    bool SLList<Object>::iterator::operator!=(const iterator &rhs) const {
    return !(*this == rhs);
    }
    
    template<typename Object>
    SLList<Object>::iterator::iterator(Node *p) : current{p} {}
    
    template<typename Object>
    SLList<Object>::SLList() : head(new Node()), tail(new Node()), theSize(0) {
    head->next = tail;
    }
    
    template<typename Object>
    SLList<Object>::SLList(std::initializer_list <Object> init_list) {
    head = new Node();
    tail = new Node();
    head->next = tail;
    theSize = 0;
    for(const auto& x : init_list) {
    push_front(x);
    }
    }
    
    template<typename Object>
    SLList<Object>::~SLList() {
    clear();
    delete head;
    delete tail;
    }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::begin() { return {head->next}; }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::end() { return {tail}; }
    
    template<typename Object>
    int SLList<Object>::size() const { return theSize; }
    
    template<typename Object>
    bool SLList<Object>::empty() const { return size() == 0; }
    
    template<typename Object>
    void SLList<Object>::clear() { while (!empty()) pop_front(); }
    
    template<typename Object>
    Object &SLList<Object>::front() {
    if(empty())
    throw std::logic_error("List is empty.");
    return *begin();
    }
    
    template<typename Object>
    void SLList<Object>::push_front(const Object &x) { insert(begin(), x); }
    
    template<typename Object>
    void SLList<Object>::push_front(Object &&x) { insert(begin(), std::move(x)); }
    
    template<typename Object>
    void SLList<Object>::pop_front() {
    if(empty())
    throw std::logic_error("List is empty.");
    erase(begin());
    }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, const Object &x) {
    Node *p = itr.current;
    Node *newNode = new Node{x, p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
    }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::insert(iterator itr, Object &&x) {
    Node *p = itr.current;
    Node *newNode = new Node{std::move(x), p->next};
    p->next = newNode;
    theSize++;
    return iterator(newNode);
    }
    
    template<typename Object>
    typename SLList<Object>::iterator SLList<Object>::erase(iterator itr) {
    if (itr == end())
    throw std::logic_error("Cannot erase at end iterator");
    Node *p = head;
    while (p->next != itr.current) p = p->next;
    Node *toDelete = itr.current;
    p->next = itr.current->next;
    delete toDelete;
    theSize--;
    return iterator(p->next);
    }
    
    template<typename Object>
    void SLList<Object>::print() {
    iterator itr = begin();
    while (itr != end()) {
    std::cout << *itr << " ";
    ++itr;
    }
    std::cout << std::endl;
    }
    
    template<typename Object>
    void SLList<Object>::init() {
    theSize = 0;
    head->next = tail;
    }

Bien, entonces al analizar ambas partes del codigo; puedo decir que lo que hace es la creacion de una lista enlazada simple
la cual puede obtener elementos de cualquier tipo de dato. Por seperado, el .h nos brinda la declaracion de la Clase SLList 
junto con sus metodos y miembros, por otro lado el .cpp; contiene pues la implementacion a detalle de que es lo que va a suceder
con la lista, paso por paso o al menos asi lo interpreto, al estar de esa forma acomodada. Entonces juntos, nos permiten la creacion,
manipulacion y gestion de dicha lista.