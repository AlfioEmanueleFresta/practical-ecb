Conclusion
==========

Case study: Adobe
_________________

In 2013, Adobe announced that attackers were able to intrude in their
network and had stolen customer data. Later it was revealed that the customer
records contained in the leak belonged to hundreds of million users.

Unfortunately this is not the first attack of this kind that has
hit a multinational company, but according to Sophos, a cyber security
company who analysed a copy
of the leaked data circulating online[1], Adobe made a major mistake
in the way they stored users data. In particular, Adobe in their
disclosure wrote to its users:

  “The attackers may have gained access to your… encrypted password.”

This is a worrying statement, as encryption is not reccommended for storing passwords. In fact,
today's norm for password storage is using a one-way mathematical function
called a *hash* that generally depends on the password, but that can't be
inverted and used to retrieve the password.

Moreover, a random element called a *salt*
is often introduced so that identical passwords for different users are stored
as different *hashes* and no relationship in the data would be visible, even
if attackers were to steal all users' data.

Instead, Adobe's engineers decided to store passwords using ECB mode encryption,
encrypting all passwords with a secret key.

The block size used for the blocks was also very small: only 8 bytes.

Moreover, the password hints were stored in plain text and stored together
with the encrypted passwords.

The combinations of these mistakes meant that:

* All users that had the same passwords also had the same encrypted password.
  An attacker could use the combination of all those users' password hints to
  guess the password.

* Password parts could be decrypted using the same technique, as described
  by the XKCD comic shown below:

  .. figure:: images/xkcd.png

    From https://xkcd.com/1286/.


* As the same criptographic key was used to encrypt all of the users' passwords,
  if the key is leaked or brute-forced, all passwords could be decrypted
  in a matter of minutes.


Other Block Encryption Modes
____________________________

To overcome the issues of ECB mode encryption, you can use one of the many
different modes available.

For example, Cipher Block Chaining (CBC) mode uses the combinations of the
plain text block and the previous block's ciphertext to encrypt each block:

.. figure:: images/cbc-wikipedia.png

  Cipher Block Chaining (CBC) mode encryption. From Wikipedia, the free encyclopedia.


Moreover the Initialisation vector (IV) can be chosen to a different random
value for each message and can be stored in plain text together with the
encrypted message. This way, even identical messages won't be encrypted
to the same ciphertext, and a range of attacks will be unfeasible.


Summary
_______

It is imperative that software engineers analyse all aspects of the information
they want to encrypt, and fully understands the encryption methods and algorithms,
in order to be able to make an informed choice on the appropriate ones to use
for each case.



Ethical Issues
______________

As is common in security teaching, the techniques described here could be
used to attack systems but you must behave responsibly and ethically toward
other people, their data and systems. The writing or use of tools to gain
unauthorised access to systems is a criminal offence.

The Computer Misuse Act (1990) clearly states:

  [...]

  A person is guilty of an offence if—
    (a) he causes a computer to perform any function with intent to secure access to any program or data held in any computer, or to enable any such access to be secured;
    (b) the access he intends to secure, or to enable to be secured, is unauthorised;

  [...]

You can learn more about the Act at http://www.legislation.gov.uk/ukpga/1990/18/contents.



**References**

1. "Anatomy of a password disaster -- Adobe's giant-sized cryptographic blunder", Paul Ducklin. Sophos.
   https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/
