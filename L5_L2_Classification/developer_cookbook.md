# Developer Cookbook — PAX_ARC_SOLVER
**Stack:** Python 3.11, numpy, PAX 27B, constraint programming, AIOSS_FORMAT

## Core Usage Patterns

## Solve an ARC-style grid problem
```python
from pax_arc_solver import ARCSolver
solver = ARCSolver(pax_model="./pax-27b-q4.gguf")
solution = solver.solve(
    input_grid=[[0,1,0],[1,0,1],[0,1,0]],
    examples=[{"input": ..., "output": ...}]
)
print(solution.grid, solution.confidence)
```

## Constraint satisfaction problem
```python
from pax_arc_solver import CSPSolver
csp = CSPSolver(pax_model="./pax-27b-q4.gguf")
result = csp.solve(
    variables=["x", "y", "z"],
    domains={"x": range(10), "y": range(10), "z": range(10)},
    constraints=["x + y == z", "x < y"]
)
```

## Multi-attempt with AIOSS trace
```python
solution = solver.solve_with_audit(problem, max_attempts=5, aioss_chain="./arc.aioss")
print(f"Solved in {solution.attempts} attempts. Chain: {solution.chain_hash}")
```

## AIOSS Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```

## Performance
Set max_attempts=5 for PAX 27B — beyond that, problem likely requires reformulation. Cache constraint validators; don't recompile per attempt. Numpy for grid manipulation is 100x faster than pure Python.

## Integration
Used by PAX_REASONING and PAX_PLANNING. Feeds verified solutions into PAX_KNOWLEDGE_GRAPH. Integrated with KANTOR_K5 for benchmark problem lookup.
