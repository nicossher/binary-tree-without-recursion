#include <iostream>
using namespace std;

template<typename T> struct Nodo {
    T num;
    Nodo<T>* der = NULL;
    Nodo<T>* izq = NULL;
};

template<typename T> struct nodoPila{
    Nodo<T>* info;
    nodoPila<T>* sgte;
};

template<typename T> Nodo<T>* crearNodo(T n){
    Nodo<T>* nuevo_nodo = new Nodo<T>();
    nuevo_nodo -> num = n;
    return nuevo_nodo;
}

template<typename T> void push (nodoPila<T>* &pila, Nodo<T>* a){
    nodoPila<T>* p = new nodoPila<T>();
    p->info = a;
    p->sgte = pila;
    pila = p;
}

template<typename T> Nodo<T>* pop (nodoPila<T>* &pila){
    nodoPila<T>* p = pila;
    pila = p->sgte;
    Nodo<T>* x = new Nodo<T>();
    x = p->info;
    delete(p);
    return x;
}

template<typename T> void cargar (Nodo<T>* &raiz,T n){
    Nodo<T>* nuevo_nodo = crearNodo(n);
        if (raiz == NULL){
            raiz = nuevo_nodo;            
        }
        else{      
            Nodo<T>* anterior = raiz; 
            Nodo<T>* sig = NULL;   
            if(n<raiz->num){
                sig = raiz->izq; //voy por el lado izq del arbol
            }
            else{
                sig = raiz->der; //voy por el lado derecho
            }
            while (sig != NULL){
                anterior = sig;
                if (n<sig->num){
                    sig = sig->izq;
                }
                else {
                    sig = sig->der;
                }
            }
            if (n<anterior->num){
                    anterior->izq = nuevo_nodo;
            }
            else{
                anterior ->der = nuevo_nodo;
            }
        }
}

template<typename T> void mostrar (Nodo<T>* raiz){
    nodoPila<T>* pila = NULL;
    Nodo<T>* guia = raiz; 
    while (guia != NULL || pila != NULL){
        while (guia != NULL){
            push(pila, guia);
            guia = guia->izq;
        }
        Nodo<T>* m = pop(pila);
        cout << m->num <<" ";
        guia = m->der;
    }
}

int main (){
    int n=11;
    int vector[n]={8,5,13,2,7,10,20,1,3,9,16};
    Nodo<int>* raiz = NULL;

//cargar arbol
    for (int i=0;i<n;i++){
        cargar<int> (raiz,vector[i]);
    }

//mostrar arbol en in-order
    mostrar<int>(raiz);
    return 0;
}