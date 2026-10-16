# 03 — Stereo Disparity Estimation

Baseline stereo: compute the horizontal disparity between a left/right image
pair two different ways — both implemented from scratch.

![pair 1: left | right | disparity](outputs/pair1_demo.png)

## Block matching

Each pixel window in the left image is matched against candidate windows on
the same scanline in the right image, using two cost functions:

- **SAD** — sum of absolute differences
- **SSD** — sum of squared differences

evaluated at window sizes `w ∈ {1, 5, 9}` (6 disparity maps per pair). Larger
windows trade noise for blurrier boundaries — visible across the three maps
below:

| Pair | SAD | SSD |
|---|---|---|
| 1 | ![pair1 sad](outputs/pair1_sad.png) | ![pair1 ssd](outputs/pair1_ssd.png) |
| 2 | ![pair2 sad](outputs/pair2_sad.png) | ![pair2 ssd](outputs/pair2_ssd.png) |
| 3 | ![pair3 sad](outputs/pair3_sad.png) | ![pair3 ssd](outputs/pair3_ssd.png) |

## Dynamic programming

Per-scanline optimal alignment with occlusion handling: pixels may be matched
(cost `d_ij = (I_l(i) − I_r(j))² / σ²`) or skipped (constant cost `c₀`), solved
with the classic `D(i,j)` recurrence and backtracked to recover the disparity
maps for both views:

| Pair | Left + right disparity maps |
|---|---|
| 1 | ![pair1 dp](outputs/pair1_dp.png) |
| 2 | ![pair2 dp](outputs/pair2_dp.png) |
| 3 | ![pair3 dp](outputs/pair3_dp.png) |

Includes the bonus visualization — the optimal alignment path through the
cost matrix for a single scanline (diagonal = match, horizontal/vertical =
occlusion skip):

![scanline alignment](outputs/scanline_1.png)

## Files

| Path | Contents |
|---|---|
| `stereo_disparity.ipynb` | Block matching + DP, run on all three stereo pairs |
| `assets/` | `l1–l3.png`, `r1–r3.png` stereo pairs |
| `outputs/` | Disparity maps and alignment plots — `pair{N}_{sad,ssd,dp}.png` |

## Running

```bash
pip install -r ../../requirements.txt
jupyter notebook stereo_disparity.ipynb
```

Note: block matching is implemented as an explicit per-pixel search — it's
instructive but slow (tens of seconds per map).
