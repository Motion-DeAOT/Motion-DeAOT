## Installation

- Python 3.8 or higher
- PyTorch >= 1.7.0
- torchvision
- opencv-python
- Pillow
- NumPy
- SciPy
- FilterPy
- tqdm
- matplotlib

### Install Dependencies

```bash
pip install torch torchvision
pip install opencv-python Pillow numpy scipy filterpy tqdm matplotlib
```

### Install Pytorch Correlation Extension

Motion-DeAOT requires the Pytorch Correlation extension. We recommend installing it from source:

```bash
git clone https://github.com/ClementPinard/Pytorch-Correlation-extension.git
cd Pytorch-Correlation-extension
pip install .
cd ..
```

### Verify Installation

```python
import torch
import cv2
import filterpy
from correlation_package.correlation import Correlation
```

If no errors are raised, the installation was successful.
