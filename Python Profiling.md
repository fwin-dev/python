# Profiling a Python Script on OSX using qcachegrind

These instructions assume you have already installed Python 2.7


## How to Profile - `profilestats`

Generates python and qcachegrind profile files

### Installation

1. Run: `sudo pip install profilestats`

### Usage

To profile any python function, decorate it like so:

	from profilestats import profile

	@profile
	def myFunc():
		# your code


## `qcachegrind`

Pros:
- Identical interface for people used to profiling C/C++ applications in kcachegrind
- Well tested because of its wide usage

Cons:
- Long installation procedure which requires xcode and macports as prerequisites
- Functions will have extremely long names in the display because the full module path is included with the function. Also, functions are not sortable by class or by file because `profilestats` does not include this information properly.

### Installation

1. Download and install Qt 4.x using the installer from: http://qt-project.org/downloads
2. Download and extract kcachegrind (which contains qcachegrind) from: http://kcachegrind.sourceforge.net/html/Download.html
3. Open a terminal in the extracted directory
4. Run: `qmake -spec 'macx-g++'`
5. Run: `make`
6. In the `qcachegrind` folder, move `qcachegrind.app` to your `Applications` folder, or a subfolder
7. Run: `sudo pip install profilehooks`
8. Run: `sudo port install graphviz`
9. Symlink `dot`: `sudo ln -s /opt/local/bin/dot /usr/bin/dot`
10. Symlink `twopi`: `sudo ln -s /opt/local/bin/dot /usr/bin/twopi`

### Usage

- profilestats will generate 2 files, one being `cachegrind.out.profilestats` which can be opened inside the application


## RunSnakeRun

Pros:
- Similar to qcachegrind
- Does not pollute GUI with long file names like qcachegrind
- Can view memory dumps

Cons:
- Not as feature-full as `qcachegrind`

### Installation

1. Run: `sudo pip install RunSnakeRun`
2. Install wxPython from here: http://www.wxpython.org/download.php
  - Make sure you get the "cocoa" release, which may still be in the "development release" section. This is required for running wxpython in 64 bit, which is the default mode for python OSX installs.

### Usage

1. Run: `python -c "from runsnakerun import runsnake; runsnake.main()" &`
2. `profilestats` will generate 2 files, one being `profilestats.prof` which can be opened with any python profiling compatible tool, including `RunSnakeRun`


## SnakeViz

Pros:
- Fancy web interface
- Easy install

Cons:
- The time graph takes a while to render for not-so-complex programs
- The time graph causes a memory leak (2.5GB in 20 seconds)

### Installation

1. Run: `sudo pip install snakeviz`

Optionally:

1. Open: `/Library/Python/2.7/site-packages/snakeviz/upload.py`
2. Search for `self._timeout = 10` and replace `10` with the desired numbered of graph rendering timeout, in seconds

### Usage

1. Run: `snakeviz profilestats.prof`
2. Type Ctrl+C in the terminal to exit and stop the web server


## `pycallgraph`

Pros:
- No known bugs
- Simple graph layout which works

Cons:
- No longer maintained

### Installation

1. Run: `pip install pycallgraph`

### Usage

1. Run: `pycallgraph file.py`

