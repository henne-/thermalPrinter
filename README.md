# Control the Adafruit/Sparkfun thermal printer from node.js

Largely inspired by http://electronicfields.wordpress.com/2011/09/29/thermal-printer-dot-net/

You can print images, but they need to be 384px wide.

It's a fluent API, so you can chain functions, but don't forget to call `print` at the end to actually print something!

## Crappy schematics

You'll need an USB/Serial converter.

![schematics](/images/schema.png)


## Usage
- install with `npm install adafruitThermalPrinter --save` 
- check the demo sample:

```js
var SerialPort = require('serialport').SerialPort,
	serialPort = new SerialPort('/dev/ttyUSB0', {
		baudrate: 19200
	}),
	Printer = require('../src/printer');

var path = __dirname + '/images/nodebot.png';

serialPort.on('open',function() {
	var printer = new Printer(serialPort);
	printer.on('ready', function() {
		printer
			.indent(10)
			.horizontalLine(16)
			.bold(true)
			.indent(10)
			.printLine('first line')
			.bold(false)
			.inverse(true)
			.big(true)
			.right()
			.printLine('second line')
			.printImage(path)
			.print(function() {
				console.log('done');
				process.exit();
			});
	});
});
```