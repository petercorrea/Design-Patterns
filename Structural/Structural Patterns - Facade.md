---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Facade
created: "2021-05-08T17:39:58.584Z"
modified: "2021-05-08T17:42:47.941Z"
---

// FACADE
// Intent:
// text
// Design:
// provides a simple interface to a library, framework, or complex set of classes

# Structural Patterns - Facade

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Facade provides a simplified interface to a library, a framework, or any other complex set of classes.

#### Objects

- **Facade** provides convenient access to a particular part of the subsystem’s functionality. It knows where to direct the client’s request and how to operate all the moving parts.

- **Additional Facade** class can be created to prevent polluting a single facade with unrelated features that might make it yet another complex structure. Additional facades can be used by both clients and other facades.

-**Complex Subsystem** consists of dozens of various objects. To make them all do something meaningful, you have to dive deep into the subsystem’s implementation details, such as initializing objects in the correct order and supplying them with data in the proper format. Subsystem classes aren’t aware of the facade’s existence. They operate within the system and work with each other directly.

- **Client** uses the facade instead of calling the subsystem objects directly.

<img src="https://refactoring.guru/images/patterns/diagrams/facade/structure-indexed.png" width="500" />

#### Application

- Use the Facade pattern when you need to have a limited but straightforward interface to a complex subsystem.
- Use the Facade when you want to structure a subsystem into layers.

#### Code

```typescript

// These are some of the classes of a complex 3rd-party video
// conversion framework. We don't control that code, therefore
// can't simplify it.

class VideoFile
// ...

class OggCompressionCodec
// ...

class MPEG4CompressionCodec
// ...

class CodecFactory
// ...

class BitrateReader
// ...

class AudioMixer
// ...


// We create a facade class to hide the framework's complexity
// behind a simple interface. It's a trade-off between
// functionality and simplicity.
class VideoConverter is
    method convert(filename, format):File is
        file = new VideoFile(filename)
        sourceCodec = new CodecFactory.extract(file)
        if (format == "mp4")
            destinationCodec = new MPEG4CompressionCodec()
        else
            destinationCodec = new OggCompressionCodec()
        buffer = BitrateReader.read(filename, sourceCodec)
        result = BitrateReader.convert(buffer, destinationCodec)
        result = (new AudioMixer()).fix(result)
        return new File(result)

// Application classes don't depend on a billion classes
// provided by the complex framework. Also, if you decide to
// switch frameworks, you only need to rewrite the facade class.
class Application is
    method main() is
        convertor = new VideoConverter()
        mp4 = convertor.convert("funny-cats-video.ogg", "mp4")
        mp4.save()

        // The facade pattern is used to simplify a client’s interaction with a system.
// It can also be used when you want to interact with the methods present in a library without knowing the processing that happens in the background.

class Inventory {
	constructor() {
		this.inventory = {
			shampoo: 5,
			conditioner: 3,
			serum: 0,
		};
	}

	isAvaliable(product) {
		if (product.amount <= this.inventory[product.productName]) {
			return true;
		}

		return false;
	}
}

class BuyingProduct extends Inventory {
	buyProduct(product) {
		let order;
		if (this.isAvaliable(product)) {
			order = new BuyProduct();
			return order.message(product);
		}

		order = new PreOrderProduct();

		return order.message(product);
	}
}

class BuyProduct {
	message(product) {
		console.log(
			`${product.amount} bottles of ${product.productName} are available. Click on "buy" to purchase them.`
		);
	}
}

class PreOrderProduct {
	message(product) {
		console.log(
			console.log(
				`${product.amount} bottles of ${product.productName} are not available. You can Pre-order them on the next page.`
			)
		);
	}
}

var customer = new BuyingProduct();
customer.buyProduct({ productName: "shampoo", amount: 2 });
customer.buyProduct({ productName: "serum", amount: 2000 });

```
