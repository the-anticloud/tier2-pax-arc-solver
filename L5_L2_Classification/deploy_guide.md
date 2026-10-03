# Deploy Guide — PAX_ARC_SOLVER
**Platform:** Anticloud PAX 27B harness | Air-gap capable

## Prerequisites
Python 3.11+, numpy 1.26+, constraint (python-constraint), PAX 27B weights

## Environment
16GB RAM. GPU recommended. CPU inference for small constraint sets is feasible.

## AIOSS Integration
```bash
aioss init --module PAX_ARC_SOLVER --output ./pax_arc_solver.aioss
aioss append --chain ./pax_arc_solver.aioss --payload ./output.bin --module PAX_ARC_SOLVER
aioss verify --chain ./pax_arc_solver.aioss
```

## Air-Gap Deployment
```bash
pip download -r requirements.txt -d ./wheels/
# Transfer to air-gap host
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(model_path="./pax-27b-q4.gguf", module="PAX_ARC_SOLVER",
                     aioss_chain="./pax_arc_solver.aioss",
                     classification="L5_NARROW_L2_GENERAL")
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./pax_arc_solver.aioss --verbose
python -m pax_arc_solver.tests.smoke
```
