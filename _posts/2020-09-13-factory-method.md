---
title: "Factory Method Design Pattern"
date: 2020-09-13
tags: [design patterns, swift, factory method, creational pattern]
excerpt: "Design Patterns, Factory Method, Swift"
---
Imagine that you're working on a musical app, like Garage Band, and you need a simple way to instantiate different virtual instruments. Let's see how you can use the Factory Method design pattern to solve this requirement.

### Whats is the Factory Method design pattern?

The Factory Method is one of the most popular creational design patterns. It's very simple and useful and you should definitely have it in your tool box. This pattern consists in defining a protocol to create an object type, but letting the classes that conform to that protocol decide which concrete object to instantiate.


### Structure

The following UML diagram represents the structure of this pattern:
![image](https://user-images.githubusercontent.com/35386069/93418999-402e7200-f871-11ea-9f50-fc3378867202.png)

### Participants
- **Product Protocol:** Protocol that defines the interface of objects the factory method creates.
- **Concrete Product:** Class or struct that conforms to the Product protocol.
- **Factory:** Protocol that declares the factory method, which returns an object of type Product. It may also define a default implementation for the factory method that returns an instance of a concrete Product.
- **Concrete Factory:** Class or struct that conforms to the Factory protocol and provides an implementation for the factory method that returns an instance of a concrete Product.

Well, that was enough theory. Let's start coding!

### Implementation
We're going to solve our initial problem using what we've just learned. First of all, let's declare a protocol to represent our musical instruments. This protocol corresponds to our ProductProtocol of the previous UML diagram.

```swift
// Product Protocol
protocol MusicalInstrument {
    func playNotes()
}
```

Now we can create several "concrete" instrument classes that conform to the MusicalInstrument protocol.

```swift
// Concrete product 1
class Piano: MusicalInstrument {
    func playNotes() {
        // Playing notes
    }
}

// Concrete product 2
class Guitar: MusicalInstrument {
    func playNotes() {
        // Playing notes
    }
}

// Concrete product 3
class ElectricPiano: MusicalInstrument {
    func playNotes() {
        // Playing notes
    }
}

// Concrete product 4
class ElectricGuitar: MusicalInstrument {
    func playNotes() {
        // Playing notes
    }
}
```

Next step will be to create a protocol that defines our factory method that will allow us to create any object that conforms to our MusicalInstrument protocol. This factory method will be called `create`. You can instead call it `make` if you want, but wherever name you choose, be consistent with the naming in your whole app.

``` swift
// Factory
protocol MusicalInstrumentFactory {
    func create(instrument: InstrumentsCatalog) -> MusicalInstrument
}

enum InstrumentCatalog {
    case piano
    case guitar
}
```

For this particular example, to keep things simple we're declaring an enum as a catalog for the different instruments that we'll be creating. The Factory Method pattern doesn't tells you how to handle the logic for instantiating your objects, so you'll have to figure out for yourself how to do it.

We're almost done. We can create now one of several objects that conform to our MusicalInstrumentFactory protocol and implement our factory method. Let's define an Acoustic Instrument Factory and an Electric Instrument Factory.

```swift
// Concrete factory 1
class AcousticInstrumentFactory: MusicalInstrumentFactory {

    func create(instrument: InstrumentsCatalog) -> MusicalInstrument {
        switch instrument {
        case .piano:
            return Piano()
        case .guitar:
            return Guitar()
        }
    }

}

// Concrete factory 2
class ElectricInstrumentFactory: MusicalInstrumentFactory {

    func create(instrument: InstrumentsCatalog) -> MusicalInstrument {
        switch instrument {
        case .piano:
            return ElectricPiano()
        case .guitar:
            return ElectricGuitar()
        }
    }
}
```

Finally, let's call our factory methods!

```swift
class ViewController: UIViewController {

    // MARK: - Factories
    let acousticInstrumentFactory: MusicalInstrumentFactory = AcousticInstrumentFactory()
    let electricInstrumentFactory: MusicalInstrumentFactory = ElectricInstrumentFactory()

		// MARK: - Lifecycle
     override func viewDidLoad() {
        super.viewDidLoad()
    }

		// MARK: - User selection handling methods
    func selectAcusticInstrument(instrument: InstrumentsCatalog) -> MusicalInstrument {
        return acousticInstrumentFactory.create(instrument: instrument)
    }

    func selectElectricInstrument(instrument: InstrumentsCatalog) -> MusicalInstrument {
        return electricInstrumentFactory.create(instrument: instrument)
    }

}
```

### When to use the Factory Method pattern?

Use the Factory Method pattern when:

- There are several similar products, i.e., that can conform to the same protocol or inherit from the same class.
- A decision needs to be made about which one of the products to instantiate based in one or more criteria.

### Conclusion

This design pattern allow us to create similar objects in a simple and decoupled manner. With the use of protocols maintains hidden the implementation details of each concrete product and its factory methods. This are always a good things, since makes easy to create testing mocks and decoupled modules. However, it has an important  downside, and it is that adding new products or factory methods implies changes in several places, so you have to craft carefully your design.
