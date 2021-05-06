[<img title="Algorand Payment Prompts" src="https://algorand-developer-portal.tryprism.com/static/Posts/2021/04/23%2011:10/042121-QR.jpg?w=2000&cmd=resize_then_crop&height=714&quality=100">](#credits)
# Algorand Payment QR Generator
A javascript module with a comprehensive set of tools to generates an standard Algorand URI (RFC 3986) and  QR code/2d barcode ,exportable to SVG, PNG and UTF8. Export media types are File, UTF8 text and DataURL. Works both in Node and modern browsers.

[![npm](https://img.shields.io/static/v1?label=Algorand&message=Draft&color=yellow&style=flat-square)](https://developer.algorand.org/solutions/algorand-payment-qr-generator/)
[![npm](https://img.shields.io/static/v1?label=npm&message=OK&color=green&style=flat-square)](https://www.npmjs.com/package/algorand-payment-qr)
[![npm](https://img.shields.io/npm/l/qrcode.svg?style=flat-square)](https://github.com/emg110/algorand-payment-qr/blob/master/license)

## Algorand Foundation Solution Pulication
#### This repository is made to accompany a solution publication for Algorand Foundation's developer ambassidor program
[<img title="Algorand Foundation" src="https://github.com/algorandfoundation/grow-algorand/raw/master/img/logo.png" height="auto" width="128">](https://algorand.foundation/)


## Technical notes
- Algorand URI reference specificatgion: [Algorand payment prompts specification](https://developer.algorand.org/docs/reference/payment_prompts/).
- Requires NodeJS version >= 10
- Since all the texts for Algorand URI fields is HTML Escaped and RFC 3986 encoded, for 



## Table of contents

- [Highlights](#highlights)
- [Algorand URI's ABNF Grammar](#algorand-uri-abnf-grammar)
- [Installation](#installation)
- [Usage](#usage)
- [Error correction level](#error-correction-level)
- [QR Code capacity](#qr-code-capacity)
- [Encoding Modes](#encoding-modes)
- [Binary data](#binary-data)
- [Multibyte characters](#multibyte-characters)
- [API](#api)
- [GS1 QR Codes](#gs1-qr-codes)
- [Used Trademarks](#used-trademarks)
- [Credits](#credits)
- [License](#license)


## Highlights
- This library can be built for browser, be imported or required in NodeJS or directly rendered in terminal.
- Supports RFC 3986 and Algorand URI ABNF Grammar
- Automatically escapes HTML
- CLI utility
- Save QR code as image (SVG, PNG, JPEG,...)
- Support for some styling and colors (dark , light,..)
- Support for Numeric, Alphanumeric, Kanji and Byte mode
- Support for mixed modes
- Support for chinese, cyrillic, greek and japanese characters
- Support for multibyte characters (like emojis :smile:)
- Auto generates optimized segments for best data compression and smallest QR Code size
- App agnostic readability, Generated QR Codes by definition are app agnostic

## Algorand URI ABNF Grammar

```javascript
    algorandurn     = "algorand://" algorandaddress [ "?" algorandparams ]
    algorandaddress = *base32
    algorandparams  = algorandparam [ "&" algorandparams ]
    algorandparam   = [ amountparam / labelparam / noteparam / assetparam / otherparam ]
    amountparam     = "amount=" *digit
    labelparam      = "label=" *qchar
    assetparam      = "asset=" *digit
    noteparam       = (xnote | note)
    xnote           = "xnote=" *qchar
    note            = "note=" *qchar
    otherparam      = qchar *qchar [ "=" *qchar ]
```


## Installation
Inside your project folder do:

```shell
npm install --save algorand-payment-qr
```
and then 

```shell
cd bin && node qrcode
```

or, install it globally to use `qrcode` from the command line to save Algorand URI qrcode and barcode   images or generate ones you can view in your terminal.

```shell
npm install -g algorand-payment-qr
```
and then 

```shell
qrcode [options]
```

## Usage
### CLI
#### Notice : when using Algorand key other extra parameters regarding Algorand URI for payment propmpt are required (see the following sections) 
```
Usage: qrcode [options]

Algorand switch:
  -x, --xnote Expects "xnote" instread of "note" for Algorand URI      [boolean]

Algorand options:
  -a, --amount Amount (in Micro Algos) of Algorand payment transaction  [number]
  -l, --label Label of Algorand payment transaction                     [string]
  -s, --asset Algorand asset id (in case of Algorand ASA transfer)      [string]
  -n, --note note/xnote (depends on -a | --xnote switch)                [string]
  -d, --dest Destination Wallet address (Algorand account address)      [string]

QR Code options:
  -v, --qversion  QR Code symbol version (1 - 40)                       [number]
  -e, --error     Error correction level           [choices: "L", "M", "Q", "H"]
  -m, --mask      Mask pattern (0 - 7)                                  [number]


Renderer options:
  -t, --type        Output type                  [choices: "png", "svg", "utf8"]
  -w, --width       Image width (px)                                    [number]
  -r, --ratio       Scale ratio factor                                  [number]
  -q, --qzone       Quiet zone size                                     [number]
  -b, --background  Light RGBA hex color
  -f, --foreground   Dark RGBA hex color
  -p, --puny  Output smaller QR code to terminal                       [boolean]
  -i, --inverse  Invert foreground and background colors               [boolean]

Options:
  -o, --output  Output file
  -h, --help    Show help                                              [boolean]
  --version     Show version number                                    [boolean]

Examples:
    - Draw in terminal window:
    node qrcode -a 1000000 -l "Coffee" -s 45 -n "This is a test transaction for Algorand payment QR code generator" -p
    - Save as png image:
    node qrcode -o algorandQR.png -a 1000000 -l "Coffee" -s 45 -n "This is a test transaction for Algorand payment QR code generator"
    - Use red as foreground color:
    node qrcode -f F00 -o algorandQR.png -a 1000000 -l "Coffee" -s 45 -x -n "This is a test transaction for Algorand payment QR code generator"
```
If not specified, output type is guessed from file extension.<br>
Recognized extensions are `png`, `svg` and `txt`.

### Browser
`algorand-payment-qr` can be used in browser through including the precompiled bundle present in `build/` folder.



```javascript
// index.js -> bundle.js
var QRCode = require('algorand-payment-qr')
var canvas = document.getElementById('canvas')

QRCode.toCanvas(canvas, 'sample text', function (error) {
  if (error) console.error(error)
  console.log('success!');
})
```

#### Precompiled bundle
```html
<canvas id="canvas"></canvas>

<script src="/build/qrcode.js"></script>
<script>
  QRCode.toCanvas(document.getElementById('canvas'), 'sample text', function (error) {
    if (error) console.error(error)
    console.log('success!');
  })
</script>
```

If you install through `npm`, precompiled files will be available in `node_modules/algorand-payment-qr/build/` folder.

The precompiled bundle have support for [Internet Explorer 10+, Safari 5.1+, and all evergreen browsers](https://browserl.ist/?q=defaults%2C+IE+%3E%3D+10%2C+Safari+%3E%3D+5.1).

### NodeJS
Require the module `algorand-payment-qr`

```javascript
var QRCode = require('algorand-payment-qr')

QRCode.toDataURL({}, function (err, url) {
  console.log(url)
})
```

render a qrcode for the terminal
```js
var QRCode = require('algorand-payment-qr')

QRCode.toString({type:'terminal'}, function (err, url) {
  console.log(url)
})
```

### ES6/ES7
Promises and Async/Await can be used in place of callback function.

```javascript
import QRCode from 'algorand-payment-qr'

// With promises
QRCode.toDataURL({})
  .then(url => {
    console.log(url)
  })
  .catch(err => {
    console.error(err)
  })

// With async/await
const generateQR = async text => {
  try {
    console.log(await QRCode.toDataURL({}))
  } catch (err) {
    console.error(err)
  }
}
```

## Error correction level
Error correction capability allows to successfully scan a QR Code even if the symbol is dirty or damaged.
Four levels are available to choose according to the operating environment.

Higher levels offer a better error resistance but reduce the symbol's capacity.<br>
If the chances that the QR Code symbol may be corrupted are low (for example if it is showed through a monitor)
is possible to safely use a low error level such as `Low` or `Medium`.

Possible levels are shown below:

| Level            | Error resistance |
|------------------|:----------------:|
| **L** (Low)      | **~7%**          |
| **M** (Medium)   | **~15%**         |
| **Q** (Quartile) | **~25%**         |
| **H** (High)     | **~30%**         |

The percentage indicates the maximum amount of damaged surface after which the symbol becomes unreadable.

Error level can be set through `options.errorCorrectionLevel` property.<br>
If not specified, the default value is `M`.

```javascript
QRCode.toDataURL({}, { errorCorrectionLevel: 'H' }, function (err, url) {
  console.log(url)
})
```

## QR Code capacity
Capacity depends on symbol version and error correction level. Also encoding modes may influence the amount of storable data.

The QR Code versions range from version **1** to version **40**.<br>
Each version has a different number of modules (black and white dots), which define the symbol's size.
For version 1 they are `21x21`, for version 2 `25x25` e so on.
Higher is the version, more are the storable data, and of course bigger will be the QR Code symbol.

The table below shows the maximum number of storable characters in each encoding mode and for each error correction level.

| Mode         | L    | M    | Q    | H    |
|--------------|------|------|------|------|
| Numeric      | 7089 | 5596 | 3993 | 3057 |
| Alphanumeric | 4296 | 3391 | 2420 | 1852 |
| Byte         | 2953 | 2331 | 1663 | 1273 |
| Kanji        | 1817 | 1435 | 1024 | 784  |

**Note:** Maximum characters number can be different when using [Mixed modes](#mixed-modes).

QR Code version can be set through `options.version` property.<br>
If no version is specified, the more suitable value will be used. Unless a specific version is required, this option is not needed.

```javascript
QRCode.toDataURL('some text', { version: 2 }, function (err, url) {
  console.log(url)
})
```

## Encoding modes
Modes can be used to encode a string in a more efficient way.<br>
A mode may be more suitable than others depending on the string content.
A list of supported modes are shown in the table below:

| Mode         | Characters                                                | Compression                               |
|--------------|-----------------------------------------------------------|-------------------------------------------|
| Numeric      | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9                              | 3 characters are represented by 10 bits   |
| Alphanumeric | 0–9, A–Z (upper-case only), space, $, %, *, +, -, ., /, : | 2 characters are represented by 11 bits   |
| Kanji        | Characters from the Shift JIS system based on JIS X 0208  | 2 kanji are represented by 13 bits        |
| Byte         | Characters from the ISO/IEC 8859-1 character set          | Each characters are represented by 8 bits |

Choose the right mode may be tricky if the input text is unknown.<br>
In these cases **Byte** mode is the best choice since all characters can be encoded with it. (See [Multibyte characters](#multibyte-characters))<br>
However, if the QR Code reader supports mixed modes, using [Auto mode](#auto-mode) may produce better results.

### Mixed modes
Mixed modes are also possible. A QR code can be generated from a series of segments having different encoding modes to optimize the data compression.<br>
However, switching from a mode to another has a cost which may lead to a worst result if it's not taken into account.
See [Manual mode](#manual-mode) for an example of how to specify segments with different encoding modes.

### Auto mode
By **default**, automatic mode selection is used.<br>
The input string is automatically splitted in various segments optimized to produce the shortest possible bitstream using mixed modes.<br>
This is the preferred way to generate the QR Code.

For example, the string **ABCDE12345678?A1A** will be splitted in 3 segments with the following modes:

| Segment  | Mode         |
|----------|--------------|
| ABCDE    | Alphanumeric |
| 12345678 | Numeric      |
| ?A1A     | Byte         |

Any other combinations of segments and modes will result in a longer bitstream.<br>
If you need to keep the QR Code size small, this mode will produce the best results.

### Manual mode
If auto mode doesn't work for you or you have specific needs, is also possible to manually specify each segment with the relative mode.
In this way no segment optimizations will be applied under the hood.<br>
Segments list can be passed as an array of object:

```javascript
  var QRCode = require('algorand-payment-qr')

  var segs = [
    { data: 'ABCDEFG', mode: 'alphanumeric' },
    { data: '0123456', mode: 'numeric' }
  ]

  QRCode.toDataURL(segs, function (err, url) {
    console.log(url)
  })
```

### Kanji mode
With kanji mode is possible to encode characters from the Shift JIS system in an optimized way.<br>
Unfortunately, there isn't a way to calculate a Shifted JIS values from, for example, a character encoded in UTF-8, for this reason a conversion table from the input characters to the SJIS values is needed.<br>
This table is not included by default in the bundle to keep the size as small as possible.

If your application requires kanji support, you will need to pass a function that will take care of converting the input characters to appropriate values.

An helper method is provided by the lib through an optional file that you can include as shown in the example below.

**Note:** Support for Kanji mode is only needed if you want to benefit of the data compression, otherwise is still possible to encode kanji using Byte mode (See [Multibyte characters](#multibyte-characters)).

```javascript
  var QRCode = require('algorand-payment-qr')
  var toSJIS = require('algorand-payment-qr/helper/to-sjis')

  QRCode.toDataURL(kanjiString, { toSJISFunc: toSJIS }, function (err, url) {
    console.log(url)
  })
```

With precompiled bundle:

```html
<canvas id="canvas"></canvas>

<script src="/build/qrcode.min.js"></script>
<script src="/build/qrcode.tosjis.min.js"></script>
<script>
  QRCode.toCanvas(document.getElementById('canvas'),
    'sample text', { toSJISFunc: QRCode.toSJIS }, function (error) {
    if (error) console.error(error)
    console.log('success!')
  })
</script>
```

## Binary data
QR Codes can hold arbitrary byte-based binary data. If you attempt to create a binary QR Code by first converting the data to a JavaScript string, it will fail to encode propery because string encoding adds additional bytes. Instead, you must pass a [`Uint8ClampedArray`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray) or compatible array, or a Node [Buffer](https://nodejs.org/api/buffer.html), as follows:

```javascript
// Regular array example
// WARNING: Element values will be clamped to 0-255 even if your data contains higher values.
const QRCode = require('algorand-payment-qr')
QRCode.toFile(
  'myAlgorandPaymentQR.png',
  [{ data: [253,254,255], mode: 'byte' }],
  ...options...,
  ...callback...
)
```

```javascript
// Uint8ClampedArray example
const QRCode = require('algorand-payment-qr')

QRCode.toFile(
  'myAlgorandPaymentQR.png',
  [{ data: new Uint8ClampedArray([253,254,255]), mode: 'byte' }],
  ...options...,
  ...callback...
)
```

```javascript
// Node Buffer example
// WARNING: Element values will be clamped to 0-255 even if your data contains higher values.
const QRCode = require('algorand-payment-qr')

QRCode.toFile(
  'myAlgorandPaymentQR.png',
  [{ data: Buffer.from([253,254,255]), mode: 'byte' }],
  ...options...,
  ...callback...
)
```

## Multibyte characters
Support for multibyte characters isn't present in the initial QR Code standard, but is possible to encode UTF-8 characters in Byte mode.

QR Codes provide a way to specify a different type of character set through ECI (Extended Channel Interpretation), but it's not fully implemented in this lib yet.

Most QR Code readers, however, are able to recognize multibyte characters even without ECI.

Note that a single Kanji/Kana or Emoji can take up to 4 bytes.

## API
Browser:
- [create()](#createtext-options)
- [toCanvas()](#tocanvascanvaselement-text-options-cberror)
- [toDataURL()](#todataurltext-options-cberror-url)
- [toString()](#tostringtext-options-cberror-string)

Server:
- [create()](#createtext-options)
- [toCanvas()](#tocanvascanvas-text-options-cberror)
- [toDataURL()](#todataurltext-options-cberror-url-1)
- [toString()](#tostringtext-options-cberror-string-1)
- [toFile()](#tofilepath-text-options-cberror)
- [toFileStream()](#tofilestreamstream-text-options)

### Browser API
#### `create([options])`
Creates QR Code symbol and returns a qrcode object.

##### `options`

See [QR Code options](#qr-code-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `returns`
Type: `Object`

```javascript
// QRCode object
{
  modules,              // Bitmatrix class with modules data
  version,              // Calculated QR Code version
  errorCorrectionLevel, // Error Correction Level
  maskPattern,          // Calculated Mask pattern
  segments              // Generated segments
}
```

<br>

#### `toCanvas(canvasElement, [options], [cb(error)])`
#### `toCanvas([options], [cb(error, canvas)])`
Draws qr code symbol to canvas.<br>
If `canvasElement` is omitted a new canvas is returned.

##### `canvasElement`
Type: `DOMElement`

Canvas where to draw QR Code.


##### `options`

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

##### Example
```javascript
QRCode.toCanvas('I am an Algorand Developer!', { errorCorrectionLevel: 'H' }, function (err, canvas) {
  if (err) throw err

  var container = document.getElementById('container')
  container.appendChild(canvas)
})
```

<br>

#### `toDataURL([options], [cb(error, url)])`
#### `toDataURL(canvasElement, [options], [cb(error, url)])`
Returns a Data URI containing a representation of the QR Code image.<br>
If provided, `canvasElement` will be used as canvas to generate the data URI.

##### `canvasElement`
Type: `DOMElement`

Canvas where to draw QR Code.

##### `options`

- ###### `type`
  Type: `String`<br>
  Default: `image/png`

  Data URI format.<br>
  Possible values are: `image/png`, `image/jpeg`, `image/webp`.<br>

- ###### `rendererOpts.quality`
  Type: `Number`<br>
  Default: `0.92`

  A Number between `0` and `1` indicating image quality if the requested type is `image/jpeg` or `image/webp`.

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

##### Example
```javascript
var opts = {
  errorCorrectionLevel: 'H',
  type: 'image/jpeg',
  quality: 0.3,
  margin: 1,
  color: {
    dark:"#010599FF",
    light:"#FFBF60FF"
  }
}

QRCode.toDataURL('I am an Algorand Developer!', opts, function (err, url) {
  if (err) throw err

  var img = document.getElementById('image')
  img.src = url
})
```
<br>

#### `toString([options], [cb(error, string)])`

Returns a string representation of the QR Code.<br>


##### `options`

- ###### `type`
  Type: `String`<br>
  Default: `utf8`

  Output format.<br>
  Possible values are: `terminal`,`utf8`, and `svg`.

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

##### Example
```javascript
QRCode.toString({}, function (err, string) {
  if (err) throw err
  console.log(string)
})
```

<br>


### Server API
#### `create([options])`
See [create](#createtext-options).<br>
See [Algorand URI options](#algorand-uri-params).

<br>

#### `toCanvas(canvas, [options], [cb(error)])`
Draws qr code symbol to [node canvas](https://github.com/Automattic/node-canvas).



##### `options`

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

<br>

#### `toDataURL([options], [cb(error, url)])`
Returns a Data URI containing a representation of the QR Code image.<br>
Only works with `image/png` type for now.


##### `options`
See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

<br>

#### `toString([options], [cb(error, string)])`
Returns a string representation of the QR Code.<br>
If choosen output format is `svg` it will returns a string containing xml code.

##### `options`

###### `type`
  Type: `String`<br>
  Default: `utf8`

  Output format.<br>
  Possible values are: `utf8`, `svg`, `terminal`.

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

##### Example
```javascript
QRCode.toString({}, function (err, string) {
  if (err) throw err
  console.log(string)
})
```

<br>

#### `toFile(path, [options], [cb(error)])`
Saves QR Code to image file.<br>
If `options.type` is not specified, the format will be guessed from file extension.<br>
Recognized extensions are `png`, `svg`, `txt`.

##### `path`
Type: `String`

Path where to save the file.


##### `options`

- ###### `type`
  Type: `String`<br>
  Default: `png`

  Output format.<br>
  Possible values are: `png`, `svg`, `utf8`.

- ###### `rendererOpts.deflateLevel` **(png only)**
  Type: `Number`<br>
  Default: `9`

  Compression level for deflate.

- ###### `rendererOpts.deflateStrategy` **(png only)**
  Type: `Number`<br>
  Default: `3`

  Compression strategy for deflate.

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).

##### `cb`
Type: `Function`

Callback function called on finish.

##### Example
```javascript
QRCode.toFile('path/to/filename.png', 'I am an Algorand Developer!', {
  color: {
    dark: '#00F',  // Blue dots
    light: '#0000' // Transparent background
  }
}, function (err) {
  if (err) throw err
  console.log('done')
})
```

<br>

#### `toFileStream(stream, [options])`
Writes QR Code image to stream. Only works with `png` format for now.

##### `stream`
Type: `stream.Writable`

Node stream.


##### `options`

See [All Options](#all-options).<br>
See [Algorand URI options](#algorand-uri-params).


### All Options

#### QR Code options

##### `version`
  Type: `Number`<br>

  QR Code version. If not specified the more suitable value will be calculated.
    
##### `errorCorrectionLevel`
  Type: `String`<br>
  Default: `M`

  Error correction level.<br>
  Possible values are `low, medium, quartile, high` or `L, M, Q, H`.

##### `maskPattern`
  Type: `Number`<br>

  Mask pattern used to mask the symbol.<br>
  Possible values are `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`.<br>
  If not specified the more suitable value will be calculated.

##### `toSJISFunc`
  Type: `Function`<br>

  Helper function used internally to convert a kanji to its Shift JIS value.<br>
  Provide this function if you need support for Kanji mode.
  
#### Algorand URI params

##### `xnote`
  Type: `Boolean`<br>

  Specifies if the xnote or note field is used for Algorand URI construction.
  
##### `amount`
  Type: `Number`<br>

  Amount of Algorand transaction in MicroAlgos or Standard Asset Unit.
  
##### `label`
  Type: `String`<br>

  Label of Algorand payment transaction.

##### `asset`
  Type: `String`<br>

  Asset Id of Algorand payment transaction if used. If not specified Algo will be used as payment fungible token.
  
##### `note`
  Type: `String`<br>

  note or xnote field content of Algorand payment transaction. The xnote option determines the name of the field with this content to be note or xnote.


#### Renderers options
##### `margin`
  Type: `Number`<br>
  Default: `4`

  Define how much wide the quiet zone should be.

##### `scale`
  Type: `Number`<br>
  Default: `4`

  Scale factor. A value of `1` means 1px per modules (black dots).

##### `small`
  Type: `Boolean`<br>
  Default: `false`

  Relevant only for terminal renderer. Outputs smaller QR code.

##### `width`
  Type: `Number`<br>

  Forces a specific width for the output image.<br>
  If width is too small to contain the qr symbol, this option will be ignored.<br>
  Takes precedence over `scale`.

##### `color.dark`
Type: `String`<br>
Default: `#000000ff`

Color of dark module. Value must be in hex format (RGBA).<br>
Note: dark color should always be darker than `color.light`.

##### `color.light`
Type: `String`<br>
Default: `#ffffffff`

Color of light module. Value must be in hex format (RGBA).<br>


## GS1 QR Codes
What defines a GS1 qrcode is a header with metadata that describes your gs1 information.
(Coming soon...)


## License
[MIT](https://github.com/emg110/algorand-payment-qr/blob/master/license)


## Credits
> This repository uses and appreciates great open source software and code from forking, extention and integration of:
-  [node-qrcode](https://github.com/soldair/node-qrcode)
-  [escape-html](https://github.com/component/escape-html)
-  [encodeurl](https://github.com/pillarjs/encodeurl)
-  The idea for this lib was inspired by:
   -  Algorand developers portal Article [Payment Prompts with Algorand Mobile Wallet](https://developer.algorand.org/articles/payment-prompts-with-algorand-mobile-wallet/ ) ,from Jason Paulos.
-  This lib is indirectly based on "QRCode for JavaScript", MIT licensed by Kazuhiko Arase.

## Banner image curtsey
-  [Payment Prompts with Algorand Mobile Wallet](https://developer.algorand.org/articles/payment-prompts-with-algorand-mobile-wallet/ ), Article's banner from Jason Paulos.


## Used Trademarks
"QR Code" from :<br>
[<img title="DENSO WAVE Incorporated" src="https://milliontech.com/wp-content/uploads/2017/01/Denso-Wave-Logo-300x102.png" height="auto" width="128">](https://www.denso-wave.com)



"Algorand" from:<br>
[<img title="Algorand Technologies" src="https://www.algorand.com/assets/media-kit/logos/full/png/algorand_full_logo_black.png" height="auto" width="128">](https://algorand.com)
