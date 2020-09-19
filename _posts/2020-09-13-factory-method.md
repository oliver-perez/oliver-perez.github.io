---
title: "Factory Method Design Pattern"
date: 2020-09-13
tags: [design patterns, swift, factory method, creational pattern]
excerpt: "Design Patterns, Factory Method, Swift"
---
Imagina que necesitas implementar una aplicación musical con diferentes tipos de instrumentos virtuales, para lo cual, es necesario contar con una forma simple de crear instancias de éstos instrumentos. Veamos como resolver este problema utilizando el Factory Method.

### **¿En qué consiste el patrón** Factory Method**?**

Factory method es uno de los patrones creacionales más populares, simples y útiles que puedes tener en tu arsenal. Este patrón de diseño **define un protocolo para crear un objecto, pero cede la responsabilidad a quién conforme a este protocolo decidir que tipo de producto instanciar.


### ¿Quiénes son los participanes?

- **Product:** Protocolo que define las características de un producto.
- **ConcreteProduct:** Clase o estructura que conforma al protocolo Product.
- **Factory:** Protocolo que declara el **factory method, es decir, un método que regresa un objeto de tipo Product. Este protocolo puede definir también una implementación por default del factory method que regrese un ConcreteProduct por default.
- **ConcreteFactory:** Clase o estructura que conforma al protocolo Creator y provee una implementación del factory method.

![image](https://user-images.githubusercontent.com/35386069/93418999-402e7200-f871-11ea-9f50-fc3378867202.png)
