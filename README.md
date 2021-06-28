# ProximityEffect.js

v3.0.0

Bulk modify CSS properties on elements based on mouse pointer or other arbitrary element proximity. Very customisable, definitely overdeveloped. A fun toy project originally from the Flash days, remade in JS as a practice project. Version 3 has had an API makeover and is a little more flexible. More importantly, it can be a LOT more flexible in the future, and shouldn't see any more drastic changes to the syntax. 

[View live demos](http://lab.adasha.com/components/proximity-effect)

## Roadmap
- (for 3.1) Per-property parameters
- (for 3.2) Multiple-value CSS properties
- (for 3.3) Multi-point animations

## Installation

### npm

```
npm install --save proximity-effect
```

### CDN
```html
<script src="https://unpkg.com/proximity-effect"></script>
```

### Vanilla
Latest ES6+ version is in `src`, ES5/minified versions are in `dist`. Download your version of choice and embed in your HTML:
```html
<script src="ProximityEffect.min.js"></script>
```

## To use


### Add some content to affect
In your `<body>` content add some elements you want to affect:
```html
<div>
   <div class="foo">...</div>
   <div class="foo">...</div>
   <div class="foo">...</div>
   ...
</div>
```

### Set-up
Remaining set-up should be done after content has loaded. Store a reference to the chosen target:
```javascript
let elements = document.querySelectorAll("*.foo"); // requires NodeList
```

Then define parameters in an object. All parameters are optional, but without setting at least a value for `threshold` or `runoff` you won't see anything. Nearly all parameters can also be accessed as properties after instantiation.

|Parameter |Type |Details |
| :---: | :---: | :--- |
| `attack` and `decay` | Number | Sets the rate of change when approaching (`attack`) or receding (`decay`), giving an effect of inertia. 1 is full speed,  0 is no movement (effectively disabling the animation). Default is 1. |
| `invert` | Boolean | Reverse the `values` array, effectively swapping near and far distances. Default is `false`. |
| `threshold` | Number | The minimum distance (from element's mathematical centre) before effect starts, in pixels. Can be any positive number, default is 0. |
| `runoff` | Number | The distance over which styles are interpolated, in pixels. Default is 0. |
| `direction` | String | The coordinates axis/axes to use for distance calculations. Can be `"both"`, `"horizontal"` or `"vertical"`. Default is `"both"`. |
| `offsetX` and `offsetY` | Number | Global horizontal (`offsetX`) and vertical (`offsetY`) centre-point offset. Default is 0. |
| `jitter`, `jitterX` and `jitterY` | Number | Random offset per element, in pixels. `jitter` affects both X and Y values, while `jitterX` and `jitterY` affect only their respective axis. All three values stack. Default is 0. |
| `jitterMethod` | String | Random generation method for jitter values. Accepts `"uniform"` or `"gaussian"`. Default is `"uniform"`. |

For example:

```javascript
let params = {
   attack:     0.8,
   decay:      0.7,
   threshold: 40,
   runoff:   100,
   jitter:    35,
   direction: 'both'
}
```

Next create a ProximityEfect instance, feeding in the list of nodes and the parameters:

```javascript
let myEffect = new ProximityEffect(elements, params);
```

Parameters can also be accessed as individual properties on the ProximityEffect instance after-the-fact, e.g.:

```javascript
myEffect.invert = true;
```

Finally, add animation properties to the ProximityEffect instance as you see fit. The `addEffect` method is used for this:

```javascript
ProximityEffect.addEffect (property, values, [params]);
```

The first argument defines the CSS property that will be affected. This can be a string that is the name of a pre-defined CSS property, or can be an object defining a property of your own. The second argument must be an array defining the start and end values of this property when animated. Only the first and last values of the array are read currently; eventually more fine-grained animations will be possible.

For example:

```javascript
myEffect.addEffect('opacity', [100, 50]);
myEffect.addEffect('scale',   [  1,  2]);
myEffect.addEffect('blur',    [  0, 10]);
```

Here are some example of defining custom properties:

```javascript
myEffect.addEffect({rule: 'left', unit: 'em'}, [100, 50]);
myEffect.addEffect({rule: 'transform', func: 'perspective', unit: 'px'},  [100, 50]);
...
```

ProximityEffect comes pre-defined with [most permitted functions](https://github.com/Adasha/proximity-effect/wiki/API-reference#supported-effects) of the `transform` and `filter` style rules. It currently only supports custom properties with single numerical values in them, so rgb() and the like can't be used yet.

The `values` array can also contain objects with a `value` key and other optional properties, including a `scatter` value:

```javascript
myEffect.addEffect('translateX', [0, {value: 50, scatter: 15}]);
myEffect.addEffect({rule: 'padding', unit: 'px'}, [{value: 20, scatter: 30}, {value: 100, scatter: 50}]);
```

## API
Full details on the API are forthcoming, for now there is only an unfinished [page on the wiki](https://github.com/Adasha/proximity-effect/wiki/API-reference).



## License:

This software is provided under the Mozilla Public License 2.0. You can freely use the code in your own projects, using any license, without limitation, but if you modify the code base those changes must be pushed back under the same MPL2 license. Any copyright/credits must be left intact.
