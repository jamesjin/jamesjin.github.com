HTML5 parsing
=============

Clickjacking
A page that provides users with an interface to perform actions that the user might not wish to perform needs to be designed so as to avoid the possibility that users can be tricked into activating the interface.
One way that a user could be so tricked is if a hostile site places the victim site in a small iframe and then convinces the user to click, for instance by having the user play a reaction game. Once the user is playing the game, the hostile site can quickly position the iframe under the mouse cursor just as the user is about to click, thus tricking the user into clicking the victim site's interface.

To avoid this, sites that do not expect to be used in frames are encouraged to only enable their interface if they detect that they are not in a frame (e.g. by comparing the window object to the value of the top attribute).

HttpOnly

IE Sp1


.. author:: default
.. categories:: none
.. tags:: none
.. comments::
