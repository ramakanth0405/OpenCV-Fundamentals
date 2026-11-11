# OpenCV Fundamentals for Image Processing

[![Python 3.13+](https://img.shields.io/badge/python-3.13+-blue.svg)](https://www.python.org/downloads/)
[![OpenCV](https://img.shields.io/badge/OpenCV-5.0+-5C3EE8.svg?logo=opencv&logoColor=white)](https://opencv.org/)
[![NumPy](https://img.shields.io/badge/NumPy-2.0+-013243.svg?logo=numpy&logoColor=white)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.10+-11557c.svg)](https://matplotlib.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Learning computer vision fundamentals using **OpenCV**, **NumPy**, and **Matplotlib**.

---

## Table of Contents

- [Overview & Outcomes](#overview--outcomes)
- [Project Structure](#project-structure)
- [Prerequisites & Environment Setup](#prerequisites--environment-setup)
- [Verify Your Environment](#verify-your-environment)
- [Key Topics](#key-topics)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Overview & Outcomes

This laboratory guides developers and beginners through essential 2D image processing techniques. Every operation is explored interactively with immediate visual feedback directly inside the notebook.

### Key Learning Outcomes
- **Image Representation**: Understand multi-dimensional NumPy arrays, coordinate systems, and data types (`uint8`).
- **Color Spaces**: Navigate between BGR (OpenCV native), RGB (Matplotlib/web native), and Grayscale intensity maps.
- **Saving & Resizing**: Export processed images to disk and scale images using appropriate interpolation methods.
- **Geometric Slicing**: Crop regions of interest (ROI) using 2D NumPy array slicing.
- **Filtering & Denoising**: Smooth images with Gaussian blur and eliminate impulse noise with Median filtering.
- **Segmentation & Edge Detection**: Isolate features using Global Binary Thresholding, Adaptive Gaussian Thresholding, and Canny edge detection.
- **Drawing & Annotation**: Overlay geometric primitives (lines, rectangles, circles) and stamp text labels.
- **Morphological Operations**: Refine binary masks using erosion and dilation with custom structuring kernels.

---

## Project Structure

```text
Open_CV/
├── open_cv_code.ipynb    # Interactive Jupyter Notebook for hands-on experiments
├── sample.jpg            # Standard calibration target image (400x600)
├── main.py               # Standalone Python entry point
├── pyproject.toml        # Declarative dependency specification (OpenCV, NumPy, Matplotlib)
├── uv.lock               # Deterministic dependency lockfile
├── LICENSE               # MIT License
└── README.md             # Project documentation & reference
```

---

## Prerequisites & Environment Setup

### Method A: Ultra-Fast Setup with `uv` (Recommended)

If you have [`uv`](https://docs.astral.sh/uv/) installed:

```bash
# 1. Clone or navigate to the project directory
cd Open_CV

# 2. Sync the virtual environment and install all pinned dependencies
uv sync

# 3. Launch VS Code or JupyterLab
uv run jupyter notebook open_cv_code.ipynb
```

### Method B: Standard Python Virtual Environment

```bash
# 1. Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# 2. Install required packages
pip install "opencv-python>=5.0.0" "numpy>=2.0.0" "matplotlib>=3.10.0"
```

---

## Verify Your Environment

Before starting the notebook exercises, run this quick fail-fast sanity check in your terminal:

```bash
uv run python -c "
import cv2, numpy as np, matplotlib
img = cv2.imread('sample.jpg')
assert img is not None, 'sample.jpg missing or unreadable!'
print(f'✅ OpenCV {cv2.__version__} initialized | Image shape: {img.shape} | Dtype: {img.dtype}')
"
```

**Expected output:**
```text
✅ OpenCV 5.0.0 initialized | Image shape: (400, 600, 3) | Dtype: uint8
```

---

## Key Topics

These 14 core topics correspond directly to the sections in [`open_cv_code.ipynb`](open_cv_code.ipynb):

| # | Topic | Key OpenCV API | Purpose & Typical Use Case |
|---|---|---|---|
| **01** | Reading and Displaying an Image | `cv2.imread()`, `plt.imshow()` | Load image from disk and display inline |
| **02** | Inspecting Image Properties | `img.shape`, `img.dtype`, `img.size` | Check height, width, channels, and data type |
| **03** | Converting to Grayscale | `cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)` | Simplify 3-channel color image into a 1-channel brightness map |
| **04** | Saving an Image | `cv2.imwrite()` | Export modified NumPy array to disk (`.png`, `.jpg`) |
| **05** | Resizing an Image | `cv2.resize()` | Scale down (`INTER_AREA`) or scale up (`INTER_LINEAR`) |
| **06** | Cropping an Image | NumPy slice `img[y1:y2, x1:x2]` | Extract rectangular regions of interest (ROI) |
| **07** | Blurring with Gaussian Blur | `cv2.GaussianBlur(img, (k, k), sigma)` | Suppress high-frequency noise and soften details |
| **08** | Median Blur | `cv2.medianBlur(img, k)` | Remove salt-and-pepper / impulse noise |
| **09** | Binary Thresholding | `cv2.threshold(gray, thresh, maxval, TYPE)` | Separate high-contrast foreground objects from background |
| **10** | Adaptive Thresholding | `cv2.adaptiveThreshold()` | Segment images with non-uniform lighting and shadows |
| **11** | Edge Detection with Canny | `cv2.Canny(gray, low, high)` | Detect structural gradient boundaries |
| **12** | Drawing Lines, Rectangles, Circles | `cv2.line()`, `cv2.rectangle()`, `cv2.circle()` | Overlay geometric annotations and bounding shapes |
| **13** | Adding Text to an Image | `cv2.putText()` | Stamp labels, text, and numerical metrics |
| **14** | Morphological Operations: Erosion and Dilation | `cv2.erode()`, `cv2.dilate()` | Erode small foreground noise or dilate/bridge broken edges |

---

## Troubleshooting

> [!CAUTION]
> ### Issue 1: `cv2.imshow()` in Jupyter Notebooks Causes Kernel Crashes
> - **Symptom**: `The Kernel crashed while executing code in the current cell or a previous cell.`
> - **The Rule**: **Never use `cv2.imshow()`, `cv2.waitKey()`, or `cv2.destroyAllWindows()` inside a Jupyter Notebook.**  
> - **Correct Approach**:
>   ```python
>   # In Jupyter, always display inline with Matplotlib:
>   import matplotlib.pyplot as plt
>
>   img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
>   plt.figure(figsize=(6, 4))
>   plt.imshow(img_rgb)
>   plt.axis("off")
>   plt.show()
>   ```

> [!WARNING]
> ### Trap 2: The BGR vs RGB Color Inversion
> - **Symptom**: Skin tones look blue; reds appear dark blue; greens shift unexpectedly.
> - **Why It Happens**: OpenCV reads images in **BGR** order, while Matplotlib and web standards expect **RGB** order.
> - **Fix**: Always call `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` prior to `plt.imshow()`.
> - **Grayscale Notice**: For 2D single-channel images, specify `plt.imshow(gray, cmap="gray")` to prevent Matplotlib from applying its default viridis (green-yellow) colormap.

> [!NOTE]
> ### Trap 3: Coordinate Systems: `[y, x]` vs `(x, y)`
> - **NumPy Array Slicing**: Uses `[row, col]` $\rightarrow$ `img[y1:y2, x1:x2]` (Height first, Width second).
> - **OpenCV Geometric Functions**: Uses `(x, y)` $\rightarrow$ `cv2.resize(img, (width, height))`, `cv2.circle(img, (x_center, y_center), radius, ...)`.

| Error / Symptom | Root Cause | Solution |
|---|---|---|
| `The Kernel crashed while executing code` | Executed `cv2.imshow()` or `cv2.waitKey()` in Jupyter | Replace GUI window calls with `plt.imshow()` and `plt.show()` |
| `AttributeError: 'NoneType' object has no attribute 'shape'` | `cv2.imread()` failed to locate image (returns `None`) | Verify file path: check spelling, current working directory, or pass absolute path |
| Image appears inverted/blue in notebook | Unconverted BGR array passed to Matplotlib | Convert color space: `img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` |
| Grayscale image displays in green and yellow | Matplotlib default `viridis` colormap applied to 2D matrix | Explicitly specify colormap: `plt.imshow(gray, cmap="gray")` |
| `cv2.error: (-215:Assertion failed)` in `Canny` | Image has 3 color channels instead of 1 | Convert to single-channel first using `cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)` |
| Drawing functions overwrite original image | `cv2.line` / `cv2.circle` modify NumPy arrays **in-place** | Create a copy first: `canvas = img.copy()`, then draw on `canvas` |

---

## License

This project is open-source and available under the [MIT License](LICENSE).
