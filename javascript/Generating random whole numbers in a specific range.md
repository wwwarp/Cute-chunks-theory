There are some examples on the [Mozilla Developer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) Network page:

``` js
/**
 * Returns a random number between min (inclusive) and max (exclusive)
 */
function getRandomArbitrary(min, max) {
    return Math.random() * (max - min) + min;
}

/**
 * Returns a random integer between min (inclusive) and max (inclusive)
 * Using Math.round() will give you a non-uniform distribution!
 */
function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

Here's the logic behind it. It's a simple rule of three:

Math.random() returns a Number between 0 (inclusive) and 1 (exclusive). 
So we have an interval like this:

``` bash
[0 .................................... 1)
Now, we'd like a number between min (inclusive) and max (exclusive):

[0 .................................... 1)
[min .................................. max)
We can use the Math.random to get the correspondent in the [min, max) interval. 
But, first we should factor a little bit the problem by subtracting min from the second interval:

[0 .................................... 1)
[min - min ............................ max - min)
This gives:

[0 .................................... 1)
[0 .................................... max - min)
We may now apply Math.random and then calculate the correspondent. Let's choose a random number:

                Math.random()
                    |
[0 .................................... 1)
[0 .................................... max - min)
                    |
                    x (what we need)
So, in order to find x, we would do:

x = Math.random() * (max - min);
Don't forget to add min back, so that we get a number in the [min, max) interval:

x = Math.random() * (max - min) + min;
That was the first function from MDN. The second one, returns an integer between min and max, both inclusive.

Now for getting integers, you could use round, ceil or floor.

You could use Math.round(Math.random() * (max - min)) + min, this however gives a non-even distribution. Both, min and max only have approximately half the chance to roll:

min...min+0.5...min+1...min+1.5   ...    max-0.5....max
└───┬───┘└────────┬───────┘└───── ... ─────┘└───┬──┘   ← Math.round()
   min          min+1                          max
With max excluded from the interval, it has an even less chance to roll than min.

With Math.floor(Math.random() * (max - min +1)) + min you have a perfectly even distribution.

min.... min+1... min+2 ... max-1... max.... max+1 (is excluded from interval)
|        |        |         |        |        |
└───┬───┘└───┬───┘└─── ... ┘└───┬───┘└───┬───┘   ← Math.floor()
   min     min+1               max-1    max
You can't use ceil() and -1 in that equation because max now had a slightly less chance to roll, but you can roll the (unwanted) min-1 result too.
```

**Author:** Ionuț G. Stan
