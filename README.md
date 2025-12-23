# **Visual Language Maps (VLMaps)**
### **REIMPLEMENTATION**

## **Abstract**

This repository presents a reimplementation of **Visual Language Maps (VLMaps)**, a framework
for language-conditioned robot navigation that fuses geometric mapping with open-vocabulary
semantic understanding. The project demonstrates how visual–language embeddings can be
projected into a spatial map, enabling robots to ground free-form natural language queries
directly into navigable environments. Using RGB-D observations in simulated indoor scenes,
the implementation constructs persistent top-down maps that support zero-shot object-goal
navigation and semantic reasoning.

---

## **Introduction**

Traditional robot navigation systems rely heavily on geometric maps generated via SLAM.
While effective for localization and obstacle avoidance, these systems lack semantic
understanding and cannot interpret natural language instructions such as “go to the chair next to the table.”

Conversely, modern vision–language models excel at associating images with text but operate
on isolated observations and lack persistent spatial memory.

**Visual Language Maps (VLMaps)** bridge this gap by integrating visual–language embeddings
with spatial mapping. Each region of the environment stores a semantic embedding, enabling
robots to localize and reason about objects and landmarks using free-form language without
predefined categories or retraining.

---

## **Mathematical Model**

Let an RGB-D observation at time step t be:
$(I_t, D_t, T_t)$
where $I_t$ is the RGB image, $D_t$ the depth image, and $T_t$ the camera-to-world transformation.

### Visual–Language Encoding
Each RGB image is encoded as:
$f_t = E_v(I_t)$

A language query $q$ is encoded as:
$l_q = E_l(q)$

Both embeddings lie in a shared semantic space.

### 3D Back-Projection
Depth pixels are projected into 3D using camera intrinsics:
$P_k = D(u) K^{-1} u~$

and transformed into the world frame:
$P_W = T_Wk P_k$

### VLMap Representation
The environment is discretized into a 2D grid:
$M$ in $R^{H x W x C}$

Each grid cell stores the averaged embedding:
$M(x,y) = (1/n) sum_i f_i$

### Semantic Matching
Language grounding is performed via cosine similarity:
$s(x,y) = cos(M(x,y), l_q)$

Cells with the highest similarity scores correspond to language-grounded landmarks.

---

## **Procedure**

1. Dataset Preparation  
   RGB images, depth maps, and camera poses are collected from a simulated indoor
   environment.

2. Feature Extraction  
   LSeg and CLIP are used to extract dense visual–language embeddings from RGB frames.

3. 3D Reconstruction & Projection  
   Depth data is back-projected into 3D and mapped into a global coordinate frame.

4. VLMap Construction  
   Embeddings are aggregated into a top-down grid map, producing:
   - visual–language embedding maps
   - obstacle maps
   - semantic grids
   - color top-down maps

5. Language Querying  
   Free-form language prompts are encoded and matched against the VLMap to identify
   semantic landmarks.

---

## **Output & Discussion**

The generated VLMaps demonstrate that:
- visual–language embeddings can be reliably fused into spatial representations,
- landmarks can be identified using open-vocabulary language queries,
- navigation-relevant maps can be constructed without predefined object classes.

Results show clear semantic separation of indoor objects such as chairs, sofas, tables,
and sinks, along with obstacle-aware free-space maps suitable for navigation planning.
The framework supports zero-shot generalization and provides a strong foundation for
language-guided robotic navigation.

To have a look at authors' output, have a look at the notebook PDF in the $notebook$ folder.

---

## **Repository Structure**

```text
vlmaps-reimplementation/
│
├── notebook/
│   ├── vlmaps_reimplementation.ipynb
│   └── vlmaps_reimplementation.pdf
│
├── .gitignore
├── .gitattributes
├── requirements.txt
├── README.md             # this file
└── LICENSE
```

---

## **How to Run**

### 1. Create a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Notebook

```bash
cd notebook
jupyter notebook vlmaps_reimplementation.ipynb
```

**Run all cells sequentially.**

**Note:**  
Parts of the simulation stack **(Habitat-Sim / Habitat-Lab)** require **Ubuntu/Linux** for full functionality.

---

## **Contributors**

> **Aman Chandak** (https://github.com/Aman-Chandak)

> **Ayushman Mishra** (https://github.com/aymisxx)

---

## **Citation**

```bibtex
@inproceedings{huang23vlmaps,
  title     = {Visual Language Maps for Robot Navigation},
  author    = {Chenguang Huang and Oier Mees and Andy Zeng and Wolfram Burgard},
  booktitle = {Proceedings of the IEEE International Conference on Robotics and Automation (ICRA)},
  year      = {2023},
  address   = {London, UK}
}

Project website: https://vlmaps.github.io/
```

Reimplementation Repository: https://github.com/aymisxx/vlmaps-reimplementation

---