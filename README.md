## ReKernel: Room 386

I'm trying to build a Linux kernel using the last of the 386 support
to use on my Toshiba T5200 386 luggable with 4MB of RAM. The last
version of Linux that will actually build is v3.3.8.

You'll need to have a copy of the Linux kernel source for v3.3.8
somewhere.

Do `make -C docker` to create the necessary Docker container, based on
Debian 7 wheezer. Once built, `docker/start ./billdit` will build the
Linux kernel.

I'm definitely going to be installing Linux on my Toshiba T5200, that
was one of the reasons I got it. Why not a v2.x kernel? Why not use
some old distro? I don't know. [I was able to get a v5.x kernel going
on a
486](https://www.insentricity.com/a.cl/283/booting-a-486-from-floppy-with-the-most-up-to-date-stable-linux-kernel)
so I have this need to get the most recent possible going on a 386 I
guess.

---

fozztexx@fozztexx.com
