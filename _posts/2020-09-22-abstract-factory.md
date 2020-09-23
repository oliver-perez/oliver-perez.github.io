---
title: "Abstract Factory Design Pattern"
date: 2020-09-13
tags: [Design Pattern]
excerpt: "The Abstract Factory is the most popular creational design pattern. It's very useful to handle the creation of families of related objects."
---

Consider that you're developing a game in which you have to build structures of different materials, just like Fortnite. There are walls, stairs and roofs, and the material can be wood, brick or metal. Well, this seems a perfect use case for the Abstract Factory design pattern!

### What is the Abstract Factory Design Pattern?

The Abstract Factory is the most popular creational design pattern, some folks even call it the king of creational patterns. It consists in defining a protocol for creating families of related objects without specifying their concrete classes.

### Structure

The following UML diagram represents the structure of this pattern.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46c13e89-2031-4d0f-8a18-6918877468ef/Screen_Shot_2020-09-20_at_22.22.48.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46c13e89-2031-4d0f-8a18-6918877468ef/Screen_Shot_2020-09-20_at_22.22.48.png)

### Participants

- **Abstract Factory Protocol:** Declares an interface for methods that create product type objects.

    Note: It's called *Abstract* because this pattern was originally meant to use Abstract classes in object oriented languages like C++, but we don't have abstract classes in Swift, instead we use protocols, but I like to keep using the term *Abstract* because it will make easier for other programmers that look at my code to recognize at first glance that I'm using this pattern.

- **Concrete Factory:** Object that conforms to the Abstract Factory Protocol and makes an implementation of its methods to create concrete products.
- **Product Protocol:** Declares an interface for a type of product object.
- **Concrete Product:** Defines a product object that conforms to the Product Protocol to be created by the corresponding concrete factory.
- **Client:** Object that uses only interfaces declared by Abstract Factory Protocol or Product Protocols to create of objects of a given family.

Thats all the theory, now let's start coding!

### Implementation

Let's see how to apply this pattern to solve our initial problem.

First, let's define our Abstract Factory Protocol to create different types of structures, and the protocols that represent the types of structures that we'll create:

```swift
protocol StructureAbstractFactory {
  func makeWall(with numberOfWindows: Int) -> Wall
  func makeStairs(with numberOfSteps: Int) -> Stairs
  func makeRoof(with shape: RoofShape) -> Roof
}

enum RoofShape {
  case pyramid
  case flat
}

protocol Wall {
  var strengthLevel: Int { get set }
  var numberOfWindows: Int { get set }
}

protocol Stairs {
  var strengthLevel: Int { get set }
  var numberOfSteps: Int { get set }
}

protocol Roof {
  var strengthLevel: Int { get set }
  var shape: RoofShape { get set }
}

enum RoofShape {
  case pyramid
  case flat
}
```

Now, let's define the concrete structures that conform to the previously defined protocols.

```swift
// Walls
struct WoodWall: Wall {
  var strengthLevel: Int
  var numberOfWindows: Int
}

struct BrickWall: Wall {
  var strengthLevel: Int
  var numberOfWindows: Int
}

struct MetalWall: Wall {
  var strengthLevel: Int
  var numberOfWindows: Int
}

// Stairs
struct WoodStairs: Stairs {
  var strengthLevel: Int
  var numberOfSteps: Int
}

struct BrickStairs: Stairs {
  var strengthLevel: Int
  var numberOfSteps: Int
}

struct MetalStairs: Stairs {
  var strengthLevel: Int
  var numberOfSteps: Int
}

// Roofs
struct WoodRoof: Roof {
  var strengthLevel: Int
  var shape: RoofShape
}

struct BrickRoof: Roof {
  var strengthLevel: Int
  var shape: RoofShape
}

struct MetalRoof: Roof {
  var strengthLevel: Int
  var shape: RoofShape
}
```

Alright, now it's time to create our concrete Factories of each kind of material to create concrete families of structures.

```swift
// Factories
class WoodStructureFactory: StructureAbstractFactory {

  private let woodStrength = 500

  func makeWall(with numberOfWindows: Int = 0) -> Wall {
    return WoodWall(strengthLevel: woodStrength, numberOfWindows: numberOfWindows)
  }

  func makeStairs(with numberOfSteps: Int = 0) -> Stairs {
    return WoodStairs(strengthLevel: woodStrength, numberOfSteps: numberOfSteps)
  }

  func makeRoof(with shape: RoofShape = .pyramid) -> Roof {
    return WoodRoof(strengthLevel: woodStrength, shape: shape)
  }

}

class BrickStructureFactory: StructureAbstractFactory {

  private let brickStrength = 500

  func makeWall(with numberOfWindows: Int = 0) -> Wall {
    return BrickWall(strengthLevel: brickStrength, numberOfWindows: numberOfWindows)
  }

  func makeStairs(with numberOfSteps: Int = 0) -> Stairs {
    return BrickStairs(strengthLevel: brickStrength, numberOfSteps: numberOfSteps)
  }

  func makeRoof(with shape: RoofShape = .pyramid) -> Roof {
    return BrickRoof(strengthLevel: brickStrength, shape: shape)
  }

}

class MetalStructureFactory: StructureAbstractFactory {

  private let metalStrength = 500

  func makeWall(with numberOfWindows: Int = 0) -> Wall {
    return MetalWall(strengthLevel: metalStrength, numberOfWindows: numberOfWindows)
  }

  func makeStairs(with numberOfSteps: Int = 0) -> Stairs {
    return MetalStairs(strengthLevel: metalStrength, numberOfSteps: numberOfSteps)
  }

  func makeRoof(with shape: RoofShape = .pyramid) -> Roof {
    return MetalRoof(strengthLevel: metalStrength, shape: shape)
  }

}
```

We're almost done. We just need to create our Client class to make use of our factories, typically the Client class isn't called "Client", but rather is called the same as the Abstract Factory protocol but without the "Abstract" part in its name. So, in this example our client will be called StructureFactory.

```swift
// This is our Client
class StructureFactory {

  private var factory: StructureAbstractFactory

  init(factory: StructureAbstractFactory) {
    self.factory = factory
  }

  func makeWall(numberOfWindows: Int = 0) -> Wall {
    return factory.makeWall(numberOfWindows: numberOfWindows)
  }

  func makeStairs(numberOfSteps: Int = 0) -> Stairs {
    return factory.makeStairs(numberOfSteps: numberOfSteps)
  }

  func makeRoof(shape: RoofShape = .pyramid) -> Roof {
    return factory.makeRoof(shape: shape)
  }

  func setFactory(factory: StructureAbstractFactory) {
    self.factory = factory
  }

}
```

As you can see, the Client doesn't know any concrete implementation of the factories or products. It only knows about the protocols that represent them.  And that's all, now you can create families of structures very easily.

```swift
// Creating wooden structures
let structureFactory = StructureFactory(factory: WoodStructureFactory())
structureFactory.makeWall()
structureFactory.makeStairs()
structureFactory.makeRoof()

// Creating brick structures
structureFactory.setFactory(factory: BrickStructureFactory())
structureFactory.makeWall()
structureFactory.makeStairs()
structureFactory.makeRoof()

// Creating metal structures
structureFactory.setFactory(factory: MetalStructureFactory())
structureFactory.makeWall()
structureFactory.makeStairs()
structureFactory.makeRoof()
```

**When to use this pattern?**

- When you need to create multiple families of products.
- A family of related products is designed to be used together and you need to enforce this constraint.

**Advantages of this pattern**

- *Low coupling between the participants.* The use of protocols hides implementation details. Factories encapsulates the details of creating product objects, so the client doesn't know about concrete types. And remember, low coupling is always a good thing!
- *Makes exchanging product families easy.* As we saw in our example, **Its very simple to change the concrete factory that the client uses, and the whole family of products changes at once.
- *Consistency among products.* When a family of products are designed to work together, it's important that the objects are used from only one family at once. This pattern makes this easy to enforce. In our example, when the player as selected an specific type of material to build, all the structures will be created from that family of structures until the user changes that configuration.

**Disadvantages**

- *Supporting new kinds of products is difficult.* The main disadvantage of this pattern is that The Abstract Factory fixes the set of products that can be created. Supporting new kinds of products requires adding new methods to the Abstract Factory Protocol, which involves changing all the factories that conform to that protocol.

**Resources**

This post was based from the Abstract Factory chapter from the book Design Patterns: Elements of Reusable Object-Oriented Software, by the gang of four. You should definitely read this book if your serious about your software development career.
