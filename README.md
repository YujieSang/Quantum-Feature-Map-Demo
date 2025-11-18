# Quantum Kernel SVM Demo

This notebook compares **classical** and **quantum** kernels for a simple binary classification task and is meant as a small playground for quantum kernel ideas (feature maps, circuit design, and decision boundaries).

## Contents

1. **Data generation**
   - Uses sklearn to create a 2D, nonlinearly-separable dataset.
   - Splits into train/test and standardizes features

2. **Classical baseline: RBF SVM**
   - Trains an rbf on the raw 2D data.
   - Serves as the reference.

3. **Quantum feature maps (kernel circuits)**
   - Several interchangeable feature-map circuits:
     - `feature_map_ry_rz_entangling(x, depth)`: RY + RZ + CZ-ring (data re-uploading).
     - `feature_map_ry_only(x, depth)`: simple RY-only encoding (no entanglement).
     - `feature_map_zz_kernel(x, reps, scale_single, scale_pair)`: ZZ-type feature map with pairwise interactions (Havlíček-style).
   - A single variable `CURRENT_FEATURE_MAP` selects which circuit is used everywhere.
   - The notebook visualizes the chosen circuit for a sample input.

4. **Quantum kernels**
   - We do both Statevector simulation and shot based simulation. Statevector simulation is cleaner while shot based simulation can be easily modified to run on real quantum hardware.
   - We calculate the kernel matrix and train the SVM.
   - Shows which support vectors the classifier relies on.

6. **Decision boundary plots**
   - Plots decision regions for:
     - Classical RBF SVM (high resolution).
     - Quantum kernel (statevector) (medium resolution).
     - Quantum kernel (shots) (low resolution, faster).
   - Points are categorized as:
     - Train, class 0 / class 1
     - Test, class 0 / class 1
   - All three plots use the same data split for direct visual comparison.

## How to switch quantum kernels / circuits

Edit the `CURRENT_FEATURE_MAP` line in the feature-map cell:

```python
# Example choices:

# RY + RZ + CZ-ring map
CURRENT_FEATURE_MAP = lambda x: feature_map_ry_rz_entangling(x, depth=2)

# Simple RY-only map
# CURRENT_FEATURE_MAP = lambda x: feature_map_ry_only(x, depth=1)

# ZZ feature map with pairwise terms (recommended to explore)
# CURRENT_FEATURE_MAP = lambda x: feature_map_zz_kernel(
#     x, reps=2, scale_single=np.pi/2, scale_pair=np.pi/2
# )
