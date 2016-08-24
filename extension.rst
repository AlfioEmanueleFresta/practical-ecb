Extension Challenge
===================


Steganography
_____________

Steganography is the practice of concealing a secret message within another.
For example, the hidden message may be in invisible ink.

In images, there are many ways of hiding content. One of the easiest ways
is to hide the information in the lowest order bits of each byte.



.. topic:: Extension Exercise

  Now look at the image ``tux-secret.png``. This image looks very similar to
  ``tux.png`` but the difference is that ``tux-secret.png`` contains an hidden
  secret image.

  Create a decryption function to use with ``decrypt_image_file`` to uncover
  the hidden content in ``tux-secret.png``.

  Hint:
    You can think of each byte in the input of your decryption function
    as a Python number between 0 and 255.

  Hint:
    How does changing the least significant bit affect each byte, numerically?
