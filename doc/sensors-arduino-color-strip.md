## LED Color Strip

We'll use an Adafruit library called `Adafruit_Neopixel` to control the color strip. As of November 2024, our color strip has 55 LED lights and 13 LED pins. WE'll explain how to write a program that 1) defines the state of the LED strip (so that our code matches the hardware) and 2) controls the LED lights on the color strip.

### Writing a basic Arduino program to control the LED color strip
In your Arduino program, first include the header file for that library:
```
#include <Adafruit_Neopixel.h>
```
This allows us to instantiate an `Adafruit_Neopixel` object that we'll call strip:
```
Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);
```
Here, `LED_COUNT` and `LED_PIN` are integer values that we define, but `NEO_GRB` and `NEO_KHZ800` are available from the `Adafruit_Neopixel` library that we `#include`d above.

As mentioned above, our Arduino program needs both a `setup()` and a `loop()` function. The `setup()` code is run once and configures the color strip for the life of the program. The `loop()` code runs for as long as we'd like (forever, if you have nothing else to do!). For the color strip, we need to turn it on, set some configurations, and then display it. You can do that like this:
```
void setup() {
  strip.begin();                                // Start up the color strip
  strip.setBrightness(<INTEGER BRIGHTNESS>);    // Set a value for the brightness
  strip.show();                                 // Display what is in the `loop()`...
}
```

The body of the work is done repeatedly inside the `loop()` function. We'll write some functions that set the colors of individual LED lights which `loop()` will call over and over again. That means, our code should look something like this:
```
void loop() {
    setColors(<POSITION>, <COLOR>);
}
```
If we ran our Arduino with the code above with some definition of `<POSITION>` and `<COLOR>`, then the LED strip would light up for one color at one position and never change. Fortunately, we can do better.

### A better program to control the LED color strip
Rather than set the color for one LED, let's set it for a whole group of LEDs. Recall from earlier that we have 55 LED lights, which are numbered 0,...,54 (remember, C++ uses 0-based indexing). A simple approach would be to choose both a starting and ending light and then set every light in between them with some color. Let's define the beginning and end of our range:
```
int begin{0};
int end{19};
```
Now, to set our range of lights with a for loop:
```
for (int i = begin; i <= end; i++){  
    strip.setPixelColor(i, <COLOR>);
}
strip.show();
```
The `.setPixelColor()` member function from our `strip` object is provided by the library we `#include`d earlier. Same with `.show()`. But we haven't defined `<COLOR>` yet!

### Defining colors
One common way to represent colors on displays is RGB format (for red-green-blue). RGB uses 3 values that describe the proportion of red, green, and blue that, when added together, make up a color. So, black could be represented as `(0,0,0)` and any other color would be take some value strictly greater than 0 for at least one of the red, green, and blue values. The `strip` object we instantiated earlier has a `.Color()` member function that will allow us to define our own colors. Here are some examples:
```
uint32_t black  = strip.Color(0, 0, 0);     // as before
uint32_t red    = strip.Color(255, 0, 0);
uint32_t green  = strip.Color(0, 255, 0);
uint32_t blue   = strip.Color(0, 0, 255);
```
When we use a color like `blue` in place of `<COLOR>` above, our for loop will set the LED lights from `begin` to `end` (inclusive) to `blue`.

### Conclusion
Now you should have all of the pieces to get started. Here are some ideas of things you could do to make your code better:
* Define different variables for different ranges of `begin` and `end` that will allow more fine-grained control of which LED lights you control.
* Add more colors!
* Wrap the code that *sets* the lights (i.e., the for loop above) into a function so that you can control any range of lights with different colors.
* Add a `delay(<SECONDS>);` after you set the LED lights for some number of `<SECONDS>` so that the colors remain on the strip for a certain period of time (otherwise they'll just blink and you might miss it!). A good value is 500, but you should play around with whatever you like.