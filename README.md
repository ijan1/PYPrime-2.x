# PYPrime-2.x Linux
I wanted to benchmark the RAM on my Linux system, but this only ran on Windows machines
without any tweaks. So I quickly hacked together this version capable of running on
Linux.

Install steps are mostly similar.

```bash
pip install cython # if you don't have cython installed
cython --embed -3 -o PYPrime.c PYPrime.pyx # compile to 'C' file

# pkg-config can be used here, however it doesn't detect the library for me
# but an invocation like so will do the job if yours works
# ${CC} -O3 `pkg-config --cflags --libs python3` PYPrime.c -o PYPrime.out
${CC} -O3 -I{path/to/python-headers.h} -l{python/library} PYPrime.c -o PYPrime.out

./PYPrime.out 2B # For a single run and primes up to 2 billion
```

There is most definitely some more work to be done to make it more robust, however
it should work out of the box (I hope).

# PYPrime-2.x
PYPrime 2 is an open source python based benchmark that scales well with RAM clock, timings and overall latency, less so with CPU speed.

When compiling it yourself you should first install cython

    pip install cython

after that you can cythonize the .pyx file

    cython --embed -3 -o PYPrime.c PYPrime.pyx

Now you can compile the file, on windows use something like this, this will be slightly different on your PC,

    cl PYPrime.c /O2 /Oi /Ot /GL /Gy /fp:fast /I "[Path to python]\include" /link /OPT:REF,ICF /LIBPATH:"[Path to python]\libs"

On linux you can use either gcc or Clang, I chose Clang as the performance is closer to what you would get on windows*
You might have to replace "python3.7" with later versions depending on what you have currently installed

    clang -O3 -I /usr/include/python3.7 PYPrime.c -lpython3.7m -o PYPrime


*Keep in mind that you can't use this version directly on Linux since uses Query Performance Counter from Kernel32.dll, which is only available on Windows, you can replace those lines of code with time.perf_counter()



On macOS the procedure isn't as straight forward, first you have to install python 3 and Xcode, then you have to edit the output file (PYPrime.c)
I decided to use Python 3.9 as it will also support arm based macs, but you can use basically whatever version you want

you will find something akin to this

    #include "Python.h"

For the compilation to work you'll have to change it to the directory of your Python 3 installation, for example:

    #include "/Library/Frameworks/Python.framework/Versions/3.9/include/python3.9/Python.h"

Then you may compile the program:

    clang -O3 -L /Library/Frameworks/Python.framework/Versions/3.9/lib -lpython3.9 -I /Library/Frameworks/Python.framework/Versions/3.9/include/python3.9  PYPrime.c -o PYPrime


You can find precompiled binaries for most architectures and OSes here:

https://github.com/monabuntur/PYPrime-2.x/releases


A huge thanks goes to the guys at BenchMate without whom this wouldn't have been possible!
