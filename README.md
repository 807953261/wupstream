# W�pstream

At [University W�rzburg](http://www1.informatik.uni-wuerzburg.de/):
Thomas C. van Dijk,
Tobias Greiner,
Nadja Henning,
Felix Klesen,
Andre L�ffler.  
At [ORTEC](http://www.ortec.com/):
Bas den Heijer.

## Description

W�pstream calculates upstream features in networks and was developed for the ACM SIGSPATIAL GIS Cup 2018.
See http://sigspatial2018.sigspatial.org/giscup2018/ for a general description of the upstream problem.

This repository contains a demonstration program - not a plug-and-play library.
However, it should be relatively easy to take the relevant parts for reuse in your own program.

**Algorithm.**
The upstream features in this challenge are highly related to the biconnected components of the network.
First we compute a block-cut tree (augmented with information about the controllers and starting nodes) and then solve the upstream-feature problem on this tree (using two depth first searches).
See the following paper for a somewhat more extensive description.

* *Thomas C. van Dijk, Tobias Greiner, Bas den Heijer, Nadja Henning, Felix
Klesen, and Andre L�ffler.* 2018. **W�pstream: Efficient Enumeration of Upstream
Features (GIS Cup).** In 26th ACM SIGSPATIAL International Conference
on Advances in Geographic Information Systems (SIGSPATIAL �18),
November 6�9, 2018, Seattle, WA, USA. ACM, New York, NY, USA, 4 pages.
https://doi.org/10.1145/3274895.3276475 (To appear.)

## Compiling the Demo

### Libraries

W�pstream uses Boost header-only libraries for small vector optimisation and memory pools.
See https://www.boost.org/.

W�pstream uses RapidJSON for parsing and docopt for handling commandline arguments.
Convenience copies have been provided in the repository.
See http://rapidjson.org/ and http://docopt.org/ for up to date versions.

### Linux

Compile using the following command in the src directory; it makes an executable in the bin directory.
If your compiler is not set up to find the Boost include files, add `-I/path/to/boost` with the root of your boost directory.

~~~
g++ -O3 -msse4.2 -std=c++11 *.cpp -o ../bin/wupstream
~~~

Substitute `clang++` to use Clang.

The resulting executable requires a processor with SSE4.2 support. If the executable does not run for this reason:

1. Remove `#define RAPIDJSON_SSE42` from rapidjson.h
2. Compile without `-msse4.2`
3. Be sad.

### Windows (Visual Studio)

Make a project file that has all .cpp files from the src directory and everything should be fine.
Depending on your processor, you may need to remove `#define RAPIDJSON_SSE42` from rapidjson.h.

### Experimental Parser

W�pstream contains an experimental parser specifically tuned for the GIS Cup file format (rather than any valid JSON).
It requires 38-character IDs and makes assumptions about the order of the fields.
Its use is not recommened.
To use it anyway, run with the `--experimental-parser` option.

## Running the Demo

W�pstream takes two or three arguments:

1. Filename of the network (json).
2. Starting nodes (txt).
3. Optionally, the output filename. Otherwise, output is given on stdout.

The output will likely contain the some IDs multiple times.

By default, the program does no logging, but you can change this in `Log.h` by changing `DefaultLogDestination`.
Setting it to `StdOut` will log to standard out; setting it to `LogFile` will log to a file called `log.txt`.
The log will contain some basic timing information of the various steps of the program.

### Linux

Compiling using the command suggested above results in `bin/wupstream`.
For example, run the following from the root directory:
~~~
bin/wupstream test/esri_naperville_electric/network.json test/esri_naperville_electric/start.txt
~~~
See the `test` directory for more example instances.

### Windows

Compile however you normally do and run as under linux.
Alternatively, configure the command line arguments in the project settings at
`Project > [project name] Properties ... > Debugging > Command Arguments`.
Example instances are available in the `test` directory.

# Running Tests

The `test` directory contains a Python 3 script to test the input/output behaviour of W�pstream.
Its output is colour-coded if the `termcolor` package is available.
(Windows may also require `colorama`.)

Run it as follows, where `<program>` is an *absolute* path to the W�pstream (or other) executable.
Relative paths may work depending on your system, but Python may claim the file is not found.

* Go to the `tests` directory.
* Run `python run_test.py <executable>`

This script reports `PASS` or `FAIL` for each test instance in `test_list.txt`.
Each individual test has a time limit of 1 second; this should be plenty, but if it is violated the test is reported as `TIME`.
For further options, see `run_test.py -h`.

There are several batches of test.

* The contest instances with 'officially' correct solutions.  These should all pass.
* Small instances with our manual solution.  This includes a number of weird corner cases.  These should all pass.
* The 'regression testing' instances with W�pstream's own solution.  These will all pass, but we do not vouch for correctness.  If you change anything about the program and one of these tests unexpectedly fails, you now know where to look.

# Libraries used

W�pstream uses:

* RapidJSON, which is freely available under an MIT license.
* docopt, which is freely available under an MIT license.
* Boost, which is freely available under the Boost Software License.

# License

W�pstream is available under the MIT License.