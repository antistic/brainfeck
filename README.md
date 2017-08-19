# brainfeck

translates text into acceptably "readable" [brainfuck](https://en.wikipedia.org/wiki/Brainfuck) code.

[test it out here](http://antoniasiu.co.uk/brainfeck)

## tech

uses vuejs for ease of setup

## how it works

fairly simple algorithm. first it adds the nearest (round down) multiple of 10 to each cell. then it adds the remaining amount needed, then prints the character.

## improvements for another day

 - [ ] execute brainfuck code
 - [ ] ... with visualiser
 - [ ] optimise output
 - [ ] deal with unicode (surrogate pairs) :cold_sweat: :worried:
