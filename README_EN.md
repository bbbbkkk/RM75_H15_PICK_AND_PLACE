# RM75 + H15 Three-Layer Block Manipulation

[中文](README_CN.md)

This demo is designed to build and rearrange a three-block tower with a RealMan RM75 arm and a PaXini H15 hand. The thumb, index finger, and middle finger grasp three equal-sized cubes, identified in the initial design by their red, blue, and purple colors. A Python application uses the vendor SDKs directly. A fixed RGB-D camera locates the blocks, and fingertip sensing during a short test lift checks whether the grasp holds.

**Status.** The implementation specification and acceptance criteria are available. The table describes the intended hardware behavior. The first hardware trial will pick one block, lift it, place it in an empty slot, and verify its position after the hand withdraws. Cycle time and success rate still need to be measured.

| Demo task | Expected behavior |
|---|---|
| Stack in a chosen order | Measure the actual top face of the supporting block before placing the next one. If the support is stable and within tolerance, update the placement target to match. |
| Remove, dismantle, or reorder | To remove the blue middle block, park the purple top block in a buffer, remove blue, and place purple back on red. Layer swaps and tower dismantling reuse the same pick-and-place operation. |
| Recover a dropped block | Pause and rescan the dropped block and any tower layers it may have struck. Retry if the block can be located and reached, or let the operator return it to the recovery area before resuming. |

**Sensing and calibration.** The fixed RGB-D camera provides the global position and checks placement and layer height after release. The H15 palm RGB camera can check alignment at the pre-grasp pose. If a small correction is needed, the arm moves once and the camera checks again. Lateral transport starts only after the test lift confirms the grip. The separate hand–eye calibration tool is specified to collect 35 valid poses per camera used for geometric positioning, with 30 poses for fitting and five for independent validation.

**Code layout.** Source files are planned under `src/`. Workstation settings and accepted calibration results belong in `config/`. The planned entry point is `src/app.py`. Task sequencing belongs in `src/tasks.py` and single-block moves in `src/pick_place.py`. The other four modules cover devices (`src/devices.py`), vision (`src/vision.py`), scene state (`src/scene.py`), and calibration (`src/calibrate.py`).

**Photos and videos.** Workstation photos, interface screenshots, and calibration diagrams belong in `docs/images/`. Hardware recordings of stacking, layer swaps, and drop recovery belong in `videos/`. Both directories are ready for footage from hardware trials. [Images](docs/images/) · [Videos](videos/)
