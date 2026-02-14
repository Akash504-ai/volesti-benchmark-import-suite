📝 Note 01 — Repository Structure Observed

Command Run:

ls


Observed Structure:

CITATION.cff
CONTRIBUTING.md
README.md
docs/
external/
include/
examples/
test/
build/

📝 Note 02 — Build Directory Status

Command Run:

cd build
ls


Output:

(empty)

📝 Note 03 — Build System Type Identified

Command Run:

cmake ..


Error:

The source directory ".../volesti" does not appear to contain CMakeLists.txt.

📝 Note 04 — volesti Uses CMake (Not Classic Makefile)

From the README badges:

You can clearly see:

cmake-gcc.yml
cmake-clang.yml


That means:

👉 volesti does use CMake
👉 But probably not from root directory

📝 Note 05 — volesti Is Header-Based, Examples Use CMake

Command Run:

find . -name "CMakeLists.txt"

📝 Note 06 — Example Structure Identified

Location:

examples/hpolytope-volume


Files found:

CMakeLists.txt
README.md
build/
data/
hpolytopeVolume.cpp

📝 Note 07 — Example Successfully Compiled

Location:

examples/hpolytope-volume/build


Files observed:

CMakeCache.txt
CMakeFiles/
Makefile
cmake_install.cmake
compile_commands.json
liblp_solve.a
hpolytopeVolume   ← executable

📝 Note 08 — volesti Successfully Executed

Command Run:

./hpolytopeVolume


Output:

Polytope HP1:
 4 2 double
1 0 <= 1
0 1 <= 1
-1 0 <= 1
0 -1 <= 1

Volume of HP1:

📝 Note 10 — How HPolytope Is Constructed

From main():

There are two construction paths.

🚨 Important for GSoC Project

This line:

HPOLYTOPE HP2(Pin);

📝 Note 11 — .ine File Format Identified

File:

cube10.ine


First lines:

cube10.ine
H-representation
begin
 20 11 real
 1 1 0 0 0 0 0 0 0 0 0
 1 0 1 0 0 0 0 0 0 0 0
 ...

 🚨 This Is VERY Important For Your GSoC Project

volesti does NOT directly use:

𝐴
𝑥
≤
𝑏
Ax≤b

It reads cdd-style format and internally converts.

So your import layer must:

Either produce .ine format

Or construct matrix properly for HPolytope

📝 Note 13 — How .ine Data Becomes Ax ≤ b

The critical constructor:

HPolytope(std::vector<std::vector<NT>> const& Pin)
{
    _d = Pin[0][1] - 1;
    A.resize(Pin.size() - 1, _d);
    b.resize(Pin.size() - 1);

    for (unsigned int i = 1; i < Pin.size(); i++) {
        b(i - 1) = Pin[i][0];
        for (unsigned int j = 1; j < _d + 1; j++) {
            A.coeffRef(i - 1, j - 1) = -Pin[i][j];
        }
    }
}

🚨 Critical Observations (Very Important for GSoC)
❌ No validation of:

Row length consistency

Empty input

Wrong metadata

Mismatch between declared dimension and row size

Numeric format errors

Everything is assumed correct.

📝 Note 14 — Direct (A, b) Construction Works
What You Did

You constructed:

HPOLYTOPE HP(d, A, b);


Where:

A =
[ 1  0
  0  1
 -1  0
  0 -1 ]

b =
[1 1 1 1]

Output:
Manual Polytope:
 4 2 double
1 0 <= 1
0 1 <= 1
-1 0 <= 1
0 -1 <= 1


✔ Matrix A stored correctly
✔ Vector b stored correctly
✔ Print format matches internal representation
✔ No .ine parsing involved

🔥 This Is Big

You have now verified:

1️⃣ volesti internally uses
𝐴
𝑥
≤
𝑏
Ax≤b
2️⃣ The constructor
HPolytope(d, A, b)


is the cleanest interface.

3️⃣ .ine parsing is just a wrapper that converts to (A,b)
🧠 Critical Insight for Your GSoC Proposal

Your import layer should NOT:

Depend on .ine

Produce fragile cdd-format

Rely on read_pointset

Instead it should:

LP / metabolic model
        ↓
Extract A, b
        ↓
Validate dimensions
        ↓
Construct HPolytope(d, A, b)


Direct. Clean. Explicit.

That aligns perfectly with the project description.

🎯 Where We Are Now

You have completed:

✔ Phase 1 — Run volesti
✔ Phase 2 — Understand HPolytope constructor
✔ Phase 3 — Manual construction test

You now truly understand the pipeline.