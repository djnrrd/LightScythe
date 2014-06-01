# LightScythe v3 #
## Credits ##
This project could not exist were it not for the work of [The Mechatronics Guy][mech],
[and his update][mech2].  The work I am doing here is largely based on his list of
improvements.  Thanks dude.  Credit also has to be given to [Adafruit][adafruit] for
their code and components - you guys saved me a lot of hard work!

## Hardware ##
As I didn't want to get involved with wiring buttons directly to the GPIO pins on my Pi
and it seemed that for images to be selected some form of display would be needed, I
opted for the [Adafruit 16x2 LCD plate with built in buttons][ada]. Assembly was simple
but it does require a certain level of dexterity with a soldering iron as there are many
pins in a row that need to be soldered precisely.

Of course, for the strip I used 2 metres of the [Adafruit Digital LED Strip][ada2], so
I guess the next step for this is making the scythe itself. Oh, I do so love making things
&lt;/sarcasm&gt;

But the star of the show is the [Raspberry Pi][rpi] - I have done and intend to do so many
things with this device. I run Raspbian, and for any of this to work you need to enable I2C
(for the display and buttons) and SPI (for the light strip).  To do this you will first
need to remove the modules from the blacklist (/etc/modprobe.d/raspi-blacklist.conf):

    # blacklist spi and i2c by default (many users don't need them)

    #blacklist spi-bcm2708
    #blacklist i2c-bcm2708

And then you will need to enable the modules you need in /etc/modules:

    snd-bcm2835
    i2c-bcm2708
    i2c-dev

In order to allow non-root access to the [I2C][i2c] and [SPI][spi] pins:

    sudo adduser pi i2c

Give the beast a reboot and she's good to go.

## Software ##
Well, this is what I have so far.  Despite my total lack of Python knowledge I seem to be
doing OK. Everything seems to be it the right place, and I have got automatic image resizing,
and I have hooked in the original image parsing code.  Once you have everything setup, you just
need to run:

    ./Scythe.py

On the first run, it will create a settings.ini file in the project root which will expect the
images to be found in /home/pi/images - if it cannot find either the directory or any images the
script will terminate.  When it loads, any images found in that directly will be automatically 
resized to the correct height, and the resized image will be saved in a "preocessed" sub-directory.
You can also set the display colour and timeout here, using the constants found in Adafruit_CharLCDPlate.py

[mech]: https://sites.google.com/site/mechatronicsguy/lightscythe
[mech2]: https://sites.google.com/site/mechatronicsguy/lightscythe-v2
[adafruit]: https://learn.adafruit.com/light-painting-with-raspberry-pi
[ada]: https://learn.adafruit.com/adafruit-16x2-character-lcd-plus-keypad-for-raspberry-pi
[ada2]: https://learn.adafruit.com/digital-led-strip
[rpi]: http://www.raspberrypi.org/
[i2c]: http://skpang.co.uk/blog/archives/575
[spi]: http://quick2wire.com/non-root-access-to-spi-on-the-pi/
