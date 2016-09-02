Answer Sheet
============

Caesar Cipher
-------------

.. code:: python

  from cp_ecb import encrypt_image_file


  KEY = 42

  def caesar(image):
    output = []

    for value in image:
      output.append((value + KEY) % 0xFF)

    return output


  encrypt_image_file("tux.png", caesar, "tux-caesar.png")


Stream Cipher
-------------

.. code:: python

  from cp_ecb import encrypt_image_file, get_stream_cipher


  # Get the cipher given the secret key (seed for the keystream generator)
  cipher = get_stream_cipher(seed="This is my secret key")

  encrypt_image_file("tux.png", cipher, "tux-stream-cipher.png")


ECB encryption
--------------

.. code:: python

  from cp_ecb import encrypt_image_file, get_ecb_encrypter


  # Get an ECB-encrypter function given the secret key
  encrypter = get_ecb_encrypter(key="This is my secret key")

  encrypt_image_file("tux.png", encrypter, "tux-ecb.png")



Extension Challenge
----------------------------------------

Changing the lowest order bit affects the parity of each byte. This can
be used to determine whether the byte contains information or not, e.g.:

.. code:: python

  from cp_ecb import decrypt_image_file


  # Define ON and OFF
  ON, OFF = 0xFF, 0x00

  # The decrypter function
  def decrypter(image):
    output = []

    for value in image:  # For each byte in the image
      if value % 2 == 0:  # Check parity
        output.append(ON)
      else:
        output.append(OFF)

    return output

  # Finally, decrypt the image.
  decrypt_image_file("tux-secret.png", decrypter, "answer.png")
