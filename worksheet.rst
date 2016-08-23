Worksheet
========================================================================

Preparation
___________

Start the Virtual Machine and use your favourite browser to navigate to the
web shell (port 12319) and Usermin (port 12323).

In the ``ecb`` folder you can find the ``exercise1.py`` file. Edit this file
using Usermin. You can execute the script by connecting to the web shell and
typing:

.. code:: bash

  cd ecb/
  python3 exercise1.py


In this file the following two functions have been imported:

.. code:: python

  from cp_ecb import encrypt_image_file, decrypt_image_file


They both take three parameters:

1. ``input_file`` the name of an input image file. You can use ``tux.png``
   which is already provided in the ``ecb`` folder or you can upload your
   own PNG image to the ``ecb`` folder using Usermin. If you decide to do so,
   please use a small image -- as the functions are very slow on large images.
   Also, keep in mind that for the purpose of this practical,
   drawings are generally better than pictures.

2. ``function``. This is a function which takes as the only argument a
   Python bytes literal, (e.g. ``b'This is a bytes literal'``), encrypts or
   decrypts it (depending on which of the two functions you're using) and
   returns another bytes literal.

3. ``output_file`` the name to use to save the output image file, once processed
   its contents using ``function``. This should end in ``.png``. Please note that
   if you use the name of an existing image, this will be overwritten.


The function will operate on bytes. Each pixel is represented as three bytes, one for the value of the
red colour component, one for the green colour and one for the blue component
(R, G, B). Then, pixels are serialised rows first, i.e. for an image of size
(width=N, height=M), the image is serialised as a single long bytes literal
similar to:

::

  R(0, 0), G(0, 0), B(0, 0), R(1, 0), G(1, 0), B(1, 0) ... R(N, 0), G(N, 0), B(N, 0),
      ...                                                                 ...
  R(0, M), G(0, M), B(0, M), R(1, M), G(1, M), B(1, M) ... R(N, M), G(N, M), B(N, M)

Please note that position (0, 0) is the top left corner, and (N, M) is the
bottom right corner. This is commonplace with computer images.


For example, if we want to negativise an image, we want to update the value of
each pixel to:

  NEW_VALUE = 0xFF - NEW_VALUE

Where hexadecimal FF (255 in decimal, 11111111 in binary) is the maximum value for a byte.

We can write a function which returns a list of inverted values, e.g.:

.. code:: python

  def inverter(image):
    output = []
    for pixel in image:
      output += [255 - pixel]
    return output

Or, using Python 3's generator syntax:

.. code:: python

  def inverter(image):
    return [255 - pixel for pixel in image]


Finally, we can invert an image by using:

.. code:: python

  encrypt_image_file("tux.png", inverter, "inverted.png")


One this code has been executed, a new image ``inverted.png`` should appear
showing the penguin with inverted colours.



Caesar Cipher
-------------

One of the first attempts at cryptography is the Caesar Cipher, named after
Julius Caesar, who is said to have invented the method,
dictator of the Roman Republic up until the year 44 BC.

.. topic:: Exercise 0

  Look at the following message:

  ::
    G  I  F  O  I     V  M  T  P  I

  Can you immediately recognise what this message says?


The method relies on shifting the alphabet by a number of positions, and replacing
the letters in the message with the ones in the shifted alphabet.

For example, if we shift the roman alphabet by **5 characters**, we get
the following correspondences table:

::

  PLAIN TEXT:  A  B  C  D  E  F  G  H  I  K  L  M  N  O  P  Q  R  S  T  V  X  Y  Z
   ENCRYPTED:  T  V  X  Y  Z  A  B  C  D  E  F  G  H  I  K  L  M  N  O  P  Q  R  S

This table can be used to encipher the mesage:

::

  PLAIN TEXT:  M  O  L  T  O     B  R  A  V  O
   ENCRYPTED:  G  I  F  O  I     V  M  T  P  I

To decrypt the message, just shift the roman alphabet by **-5 characters**, e.g.:

::

  PLAIN TEXT:  A  B  C  D  E  F  G  H  I  K  L  M  N  O  P  Q  R  S  T  V  X  Y  Z
   ENCRYPTED:  F  G  H  I  K  L  M  N  O  P  Q  R  S  T  V  X  Y  Z  A  B  C  D  E


And use the above table to decipher the message:

::

   ENCRYPTED:  G  I  F  O  I     V  M  T  P  I
  PLAIN TEXT:  M  O  L  T  O     B  R  A  V  O

Clearly, this cipher makes it hard for humans to read encrypted text, but is
nonetheless very weak.

In fact, you can simply try all 22 possible shift combinations
very easily and quickly identify the correct one:

::

   0:   G  I  F  O  I     V  M  T  P  I   <- Ciphertext
   1:   H  K  G  P  K     X  N  V  Q  K
   2:   I  L  H  Q  L     Y  O  X  R  L
   3:   K  M  I  R  M     Z  P  Y  S  M
   4:   L  N  K  S  N     A  Q  Z  T  N
   5:   M  O  L  T  O     B  R  A  V  O   <-- Plain text
   6:   N  P  M  V  P     C  S  B  X  P
   7:   O  Q  N  X  Q     D  T  C  Y  Q
   8:   P  R  O  Y  R     E  V  D  Z  R
   9:   Q  S  P  Z  S     F  X  E  A  S
  10:   R  T  Q  A  T     G  Y  F  B  T
  11:   S  V  R  B  V     H  Z  G  C  V
  12:   T  X  S  C  X     I  A  H  D  X
  13:   V  Y  T  D  Y     K  B  I  E  Y
  14:   X  Z  V  E  Z     L  C  K  F  Z
  15:   Y  A  X  F  A     M  D  L  G  A
  16:   Z  B  Y  G  B     N  E  M  H  B
  17:   A  C  Z  H  C     O  F  N  I  C
  18:   B  D  A  I  D     P  G  O  K  D
  19:   C  E  B  K  E     Q  H  P  L  E
  20:   D  F  C  L  F     R  I  Q  M  F
  21:   E  G  D  M  G     S  K  R  N  G
  22:   F  H  E  N  H     T  L  S  O  H


This shows how it is not straightforward to distinguish strong from weak encryption
simply by looking at cyphertext. Or is it?


.. topic:: Exercise 1.1

  Starting from the ``inverter`` function shown above, write a function called
  ``caesar`` which shifts the text by a number of positions of your choice,
  effectively using Caesar's Cipher.

  Remember that each pixel has a value between 0 and 255, and your
  function should also return values between 0 and 255.


.. topic:: Exercise 1.2

  Now use the following code to generate an encrypted image using Caesar's Cipher:

  .. code:: python

    encrypt_image_file("tux.png", caesar, "caesar.png")

  Using Usermin, look at the ``caesar.png`` image.


You can probably see that the content of the image is still very easily
recognisable. In fact, you should immediately be able to recognise the
image's contents, even if the colours look weird.


Strong cryptography
___________________

For comparison, you may want to see what an image encrypted using a modern
technique generally looks like.

The library includes a function which can be used to encrypt an image using
a stream cipher. This is weaker - but more practical - variation of
One-Time Pad encryption which uses a key to encrypt some data, and can
also decrypt using the same key.

You can get a function which works on bytes using the following code:

.. code:: python

  # Create a cipher function given a secret key.
  >>> from cp_ecb import get_stream_cipher
  >>> cipher = get_stream_cipher(seed="My secret key")

  # Encrypting a secret mesage.
  >>> ciphertext = cipher(b'This is a secret message')
  >>> ciphertext
  b'\x0f\xef\x8e\xa9c\x9co\xd0\xect\xf7\x0cE\xa0\xaev\xe6\xd2\x8c?\x90\x0e{\xd2'

  # Decrypting the ciphertext, using the same function and consequently, the same key.
  >>> cipher(ciphertext)
  b'This is a secret message'


.. topic:: Exercise 2

  Encrypt the image ``tux.png`` using a stream cipher and save it with the
  filename ``stream-cipher.png``.

  Then, use Usermin to view the image you just created.


This image should look completely random, and it should be impossible to
recognise immediately its original contents.


Block Modes
___________



One big issue with block-mode encryption is that each block will always
encrypt to the same ciphertext.

.. code:: python

  >> from cp_ecb import get_ecb_encrypter
  >> encrypter = get_ecb_encrypter(key="My secret key")

  >>> encrypter(b'This is a very long message, which contains similar blocks. This is the first message.')
  b'P\xf9\xcd\x07\x16\x0c ... \xc9\xd0\xe8z\xf7\x93] ... \x98(\x88\x1d'
       ^   ^   ^   ^   ^  ...   ^                          ^ ^  ^   ^

  >>> encrypter(b'This is a very long message, which contains similar blocks. This is the secon message.')
  b'P\xf9\xcd\x07\x16\x0c ... \xc9\xd0\xe8z\xf7;\xd6 ... \x98(\x88\x1d'
       ^   ^   ^   ^   ^  ...   ^                          ^ ^  ^   ^


You can see that the first and the last part of the message, which is equal in the plaintext, is also equal
in the ciphertext.


......... TODO .........



.. topic:: Exercise 3

  Use
