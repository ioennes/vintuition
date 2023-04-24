# vintuition
### CSC320: Computer Organization Project
##### By John Chidiac and Nanar Lilas Aintablian

```
Display Height: 512
Unit Width:     4
Unit Height:    4
Base Address:   0x10008000 ($gp)
```
Be sure to use MMIO for keyboard inputs (necessary for movement).
Programmed with MIPS in MARS.
Game designed to make users more comfortable using vi editors. Move with

```
                          k
                        h   l
                          j     
```
                         
## Documentation

### Creating the Menu, Win Screen, and Play Area

To create these screens, we had to convert 128x128 images into hex arrays. Then, we had to loop through each element of these arrays and store their results in the base address and its offsets. The bitmap simultaneously reads the values and displays the appropriate colors to the screen.

### Creating the Character

To create the character, we can't just load an image array into the screen and load it, this would cause multiple difficulties.
1. Unaligned addresses
2. Very complex nested for loops
3. Block representations without transparency

So, to create the characters, we had to manually load pixels one by one into their appropriate positions (starting at char_pos)

### Character Movement

For character movement, we simply had to modify the register holding char_pos. However, just moving the character would create an annoying trail on the screen, so for every move, we have to reload the whole map, targets, and score to clear the character's originally occupied space.

### Creating the Targets

To create the targets, we use `li $v0, 42` to load a random number into the `$a0` register, these numbers are then stored in `$s4, $s5, $s6` and the function that generated them is never to be accessed after initialization. This way, the positions are random and unique every playthrough.

To draw the targets, we employ the same method as drawing the character.

### Creating, Updating the Score, and Character Detection

To draw the scores, we employ the same method as drawing the character. We track a counter that updates everytime the character is on/near a target, once the counter changes, the target is teleported between the game name and the score and is never to be accessed again. To detect the characters, for every move we update certain registers so they hold appropriate values of the character's position, then we subtract and compare the values with the value of `BASE_ADDRESS + target_position`, if the absolute value of the difference is less than 100, the target is captured.
