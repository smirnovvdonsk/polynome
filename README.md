
# polynome
A polynome ```a+bx+cx²+dx³+...``` given by coefficients ```coeffs=[a,b,c,d...]``` with methods of finding roots, interpolation, differentiation and more.

Example:
```js
import Polynome from "@smirnovvdonsk/polynome"

// Create a new polynome 20+38x-35x²+6x³
// (there can be as many members and degrees as you want)
let expr1 = new Polynome([20, 38, -35, 6])

// Get all the roots of the polynome
let roots1 = expr1.roots
console.log(roots1) // [ -0.38..., 2.13..., 4.08... ]

// Get values of the polynome at the root points
let evals1 = expr1.evals(roots1)
console.log(evals1) // [ -6.66e-16, 0, 0 ]
// and they are very close to zeroes

// Get all the extremes of the polynome
let extremes1 = expr1.extremes
console.log(extremes1) // [ 0.65..., 3.24... ]

// Get values of the polynome at the extremes
let extremeEvals1 = expr1.evals(extremes1)
console.log(extremeEvals1) // [ 31.56..., -20.2... ]

///////
// Recreate a new polynome by interpolation from previous root and extreme points
// First prepare the input points 
let points = roots1
	.concat(extremes1)
	.sort()
	.map(function (abscissa) {
		return {
			x: abscissa,
			y: expr1.eval(abscissa)
		}
	})

let expr2 = Polynome.lagrangeInterpolate(points)

// Compare two polynomes: the original and interpolated from roots one
console.log(expr1.coeffs) // [ 20,       38,       -35, 6               ]
console.log(expr2.coeffs) // [ 20.00..., 38.00..., -35,	5.99..., 4.4e-16]
// they are very close to each other, and the last new extra member is very close to zero
```
## Constructor
* ```new Polynome(coeffs)``` Creates a new ```Polynome``` object.
  * ```coeffs``` ```Array.<number>``` ```<optional>``` Coefficients ```[a,b,c,d...]``` will give ```a+bx+cx²+dx³+...``` polynome. Default is ```[0]```
## Static methods
* ```Polynome.lagrangeInterpolate(points)``` → ```<Polynome>``` Creates a new instance generated by the Lagrange interpolation from points ```[{x,y},{x,y},...]```.
  * ```points``` ```Array.<Object>``` An array of interpolation points.
    * ```<Object>.x``` ```<number>``` Point abscissa.
    * ```<Object>.y``` ```<number>``` Point ordinate.
## Instance properties
* ```coeffs``` ```Array.<number>``` Coefficients ```[a,b,c,d...]``` will give ```a+bx+cx²+dx³+...``` polynome. This is the only property that contains the state of the object.
* ```clone``` ```<Polynome>```(getter) A new clone of instance.
* ```derivative``` ```<Polynome>```(getter) A new instance that is the first derivative of this.
* ```integral``` ```<Polynome>``` (getter) A new instance that is an indefinite intergal to this.
* ```extremums``` ```Array.<number>``` (getter) An array of polynome's extremes (an array of abscissas in which the maximums and minimums of the polynome are observed).
* ```extrems``` ```Array.<number>``` (getter) Sugar for ```extremums```.
* ```newtonRoots``` ```Array.<number>``` (getter) All the roots of the polynome.
* ```roots``` ```Array.<number>``` (getter) Sugar for ```newtonRoots```.
* ```length``` ```<number>``` The length of the ```coeffs``` array. When increasing, it appends zeroes to the tail.
* ```size``` ```<number>``` Sugar for ```length```.
* ```degree``` ```<number>``` The degree of the polynome. When increasing, it appends zeroes to the tail.
## Instance methods
* ```eval(abscissa)``` → ```<number>``` Returns the value of the polynome at ```abscissa``` point.
  * ```abscissa``` ```<number>```
* ```evals(abscissas)``` → ```Array.<number>``` Returns the array of values of the polynome at ```abscissas``` points.
  * ```abscissas``` ```Array.<number>``` Array of abscissas
* ```getNewtonRoot(x0)``` → ```<number>``` Returns the single root closest to ```x0``` for which the polynome is zero.
  * ```x0``` ```<number>``` ```<optional>``` Initial abscissa. Default is ```0```
* ```getNewtonRoots(xArray)``` → ```Array.<number>``` Returns the roots (not all) of the polynome, trying everything in the ```xArray``` as initial values.
  * ```xArray``` ```Array.<number>``` ```<optional>``` Array of initial abscissas. Default is ```[0]``` 
* ```setLength(length)``` → ```<Polynome>``` Changes the length of the ```coeffs``` array. When increasing, appends zeroes to the tail.
  * ```length``` ```<number>``` New length
* ```setSize(length)``` → ```<Polynome>``` Sugar for ```setLength(length)```.
* ```lagrangeInterpolate(points)``` → ```<Polynome>``` Do the same as the static method ```Polynome.lagrangeInterpolate(points)``` and transforms the instance to the result.
* ```add(polynome)``` → ```<Polynome>``` Adds another polynome to this polynome. Transforms the instance and returns it. For example ```[a,b,c] + [x,y] => [a+x,b+y,c+0]```.
  * ```polynome``` ```<Polynome>|Array.<number>``` A polynome for adding to the instance
* ```sub(polynome)``` → ```<Polynome>``` Subtracts another polynome from this polynome. Transforms the instance and returns it. For example ```[a,b,c] - [x,y] => [a-x,b-y,c-0]```.
  * ```polynome``` ```<Polynome>|Array.<number>``` A polynome for subtracting from the instance
* ```mult(polynome)``` → ```<Polynome>``` Multiplies this polynome by another polynome. Transforms the instance and returns it. For example ```(a+bx+cx²+dx³+...)(p+qx+rx²+sx³+...)``` given as ```([a,b,c,d],[p,q,r,s ])```.
  * ```polynome``` ```<Polynome>|Array.<number>``` A polynome for multiplying with the instance

