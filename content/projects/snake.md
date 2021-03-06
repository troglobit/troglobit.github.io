---
name: Snake
title: "Micro Snake"
date: 2013-07-07 10:28:00 +02:00
aliases: /snake.html
---
<img class="center" src="/images/micro-snake.png" />

Welcome to Micro Snake, based on an original implementation by
[Simon Huggins](http://www.simonhuggins.com/courses/cbasics/course_notes/snake.htm). This
version of the
[snake game](http://en.wikipedia.org/wiki/Snake_%28video_game%29) is
very small, utilizing only ANSI escape sequences to draw the board, no
external library dependencys other than a standard C-library, like
[uClibc](http://www.uclibc.org/).  Hence, it is very suitable for
todays small embedded devices.

The aim of the game is to collect the gold ($), avoid cactuses (*),
borders, and colliding with the snake itself. As you collect gold, the
snake gets longer, thus increasing the likelihood of crashing into
yourself. When you have collected all gold you are abruptly hauled to
the next level. For each new level the snake gets longer and the
amount of gold and cactuses increases.

You get scored according to the length of the snake and the number of
cactuses on the screen. The speed increases every 5 levels.

You get a bonus of 1000 points when you complete each level.

There is no concept of lives. Once you hit an obstacle, that's it,
game over.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/snake)
   * [Issue Tracker](http://github.com/troglobit/snake/issues)
   * [snake-1.0.1.tar.bz2](ftp://ftp.troglobit.com/snake/snake-1.0.1.tar.bz2),
     [MD5](ftp://ftp.troglobit.com/snake/snake-1.0.1.tar.bz2.md5)

See also the [Free(code) page](http://freecode.com/projects/micro-snake).

    a    Up,
    z    Down,
    o    Left
    p    Right
    
    f    Left turn
    j    Right turn
    
    q    Quit

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
