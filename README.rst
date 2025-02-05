Introduction
============


.. image:: https://readthedocs.org/projects/adafruit-circuitpython-gc9a01a/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/gc9a01a/en/latest/
    :alt: Documentation Status


.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/adafruit/Adafruit_CircuitPython_GC9A01A/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_GC9A01A/actions
    :alt: Build Status


.. image:: https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json
    :target: https://github.com/astral-sh/ruff
    :alt: Code Style: Ruff

displayio driver for GC9A01A displays.


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`_
or individual libraries can be installed using
`circup <https://github.com/adafruit/circup>`_.


`Adafruit 1.28" 240x240 Round TFT GC9A01A <http://www.adafruit.com/products/6178>`_

Installing from PyPI
=====================

On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-gc9a01a/>`_.
To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-gc9a01a

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-gc9a01a

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .venv
    source .env/bin/activate
    pip3 install adafruit-circuitpython-gc9a01a

Installing to a Connected CircuitPython Device with Circup
==========================================================

Make sure that you have ``circup`` installed in your Python environment.
Install it with the following command if necessary:

.. code-block:: shell

    pip3 install circup

With ``circup`` installed and your CircuitPython device connected use the
following command to install:

.. code-block:: shell

    circup install adafruit_gc9a01a

Or the following command to update an existing version:

.. code-block:: shell

    circup update

Usage Example
=============

.. code-block:: python

    import board
    import displayio
    import terminalio
    from adafruit_display_text.bitmap_label import Label
    from fourwire import FourWire
    from vectorio import Circle

    from adafruit_gc9a01a import GC9A01A

    spi = board.SPI()
    tft_cs = board.D5
    tft_dc = board.D6
    tft_reset = board.D9

    displayio.release_displays()

    display_bus = FourWire(spi, command=tft_dc, chip_select=tft_cs, reset=tft_reset)
    display = GC9A01A(display_bus, width=240, height=240)

    # Make the display context
    main_group = displayio.Group()
    display.root_group = main_group

    bg_bitmap = displayio.Bitmap(240, 240, 2)
    color_palette = displayio.Palette(2)
    color_palette[0] = 0x00FF00  # Bright Green
    color_palette[1] = 0xAA0088  # Purple

    bg_sprite = displayio.TileGrid(bg_bitmap, pixel_shader=color_palette, x=0, y=0)
    main_group.append(bg_sprite)

    inner_circle = Circle(pixel_shader=color_palette, x=120, y=120, radius=100, color_index=1)
    main_group.append(inner_circle)

    # Draw a label
    text_group = displayio.Group(scale=2, x=50, y=120)
    text = "Hello World!"
    text_area = Label(terminalio.FONT, text=text, color=0xFFFF00)
    text_group.append(text_area)  # Subgroup for text scaling
    main_group.append(text_group)

    while True:
        pass


Documentation
=============
API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/gc9a01a/en/latest/>`_.

For information on building library documentation, please check out
`this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_GC9A01A/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.
