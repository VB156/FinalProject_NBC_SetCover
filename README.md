# Vertex Graph Junction Generator

This Python project processes Boolean CNF formulas in DIMACS format to generate an NBC-compatible output file containing all the necessary network-based computation (NBC) data required to solve the given problem.

## Project Structure

```
├── VertesCover.py                                # Main script: reads DIMACS, computes junctions, writes output
├── VertexGraph1.dimacs                           # Sample DIMACS input
└── output_<inputBaseName>_<YYYYMMDD_HHMMSS>.txt  # Generated output (per run)
```

### Prerequisites

- Python 3.x

### How to Run

1. Clone this repository or download the project files.
2. Place your DIMACS CNF file in the same directory (samples provided).
3. Run the program with the input file path:

```bash
python3 VertesCover.py /home/vika/Documents/FinalProject_NBC_SetCover/VertexGrapth_from_michal_positive.dimacs
```
4. The results are saved to a uniquely named file: `output_<inputBaseName>_<YYYYMMDD_HHMMSS>.txt`.


## Example Output (generated output)

```
12 + 8 + 7 + 2 + 1
[x,y] in [[0, 0], [1, 0], [1, 1], ...]
[x,y] in [[9, 7], [8, 7], ...]


CTLSPEC NAME    ctl_base    := EF(row = <rows> & column = <columns>);
LTLSPEC NAME    ltl_base    := G!(row = <rows> & column = <columns>);
CTLSPEC NAME    ctl_z_0     := EF(row = <rows> & column = <columns> & z_split = 0);
...
CTLSPEC NAME    ctl_z_N     := EF(row = <rows> & column = <columns> & z_split = N);
```

- **Header line**: Dimension of the NBC based on the given problem
- **Second line**: List of computed split junctions.
- **Third line**: List of reset-false junctions.

## DIMACS File Format

This tool expects a valid `.dimacs` file in CNF form where clauses contain only positive literals (vertex IDs). The format must contain:
- A header line beginning with `p cnf <num_vars> <num_clauses>`
- Clause lines with only positive integers (1-based vertex IDs), ending with `0`

Example:
```
p cnf 7 8
1 3 0
1 2 0
2 3 0
3 7 0
2 4 0
4 6 0
4 5 0
5 6 0
```

Notes:
- Negative literals from classic DIMACS are omitted in the positive-only variant.
- The header counts (`num_vars`, `num_clauses`) remain the same.

## Output Details

- Header line: the per-vertex integer patterns joined by ` + `.
- Next two lines: `[x,y]` split junctions and reset-false junctions.
- Then two blank lines, followed by CTL/LTL specs:
  - rows = sum of vertex pattern values
  - columns = 2^num_vars - 1
  - z specs emitted for z_split in [0, num_vars]
