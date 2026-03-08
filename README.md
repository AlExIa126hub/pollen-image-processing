# Pollen Particle Segmentation and Shape Analysis

Final project for the **Digital Image Processing (DIP)** course.  
This project applies classical image processing techniques to microscopic images of pollen in order to segment pollen particles and analyse their geometric properties.

The workflow includes image preprocessing, segmentation, boundary extraction, and the computation of both boundary-based and region-based descriptors.

---

# Project Structure

```
pollen-particle-analysis/
│
├── notebooks/
│   └── pollen_analysis.ipynb
│
├── data/
│   ├── original_rgb/
│   │   ├── T54_28.02.jpg
│   │   ├── T55_28.02.jpg
│   │   └── T91_28.02.jpg
│   │
│   ├── rgb_masks/
│   │   └── selected regions extracted from the original images
│   │
│   ├── binary_masks/
│   │   └── grayscale / binarised versions of the selected regions
│   │
│   └── cropped_particles/
│       └── manually segmented binary masks of 10 individual pollen particles
│
└── README.md
```

---

# Dataset

The microscopic images used in this project were provided during the course and originate from the research project:

**“Holographic microscopy- and artificial intelligence-based digital pathology for the next generation of cytology in veterinary medicine (VetCyto)”**

The dataset consists of microscopy images containing:

- pollen particles (round yellowish objects)
- background noise
- artefacts such as spills of the embedding medium

Because these artefacts interfere with segmentation tasks, specific image regions were selected for further processing.

---

# Project Workflow

The analysis follows a multi-stage image processing pipeline.

---

## 1. Region Selection

The original microscopy images were visually inspected and several **regions of interest** were selected.

The selected regions contain:

- multiple pollen particles
- limited background artefacts
- relatively uniform illumination

These regions were extracted from the original images and stored in the `rgb_masks` folder for further processing.

---

## 2. Grayscale Conversion and Histogram Analysis

Each selected region was converted to **grayscale** to simplify the image processing pipeline.

For every region:

- a grayscale image was generated
- the intensity histogram was computed

Histogram analysis helps understand the distribution of pixel intensities and identify possible thresholds for segmentation.

---

## 3. Spatial Filtering

Several spatial filters were applied to reduce background noise:

- Mean (averaging) filter
- Gaussian smoothing filter
- Median filter

The filters were compared visually.

Observations:

- The **mean filter** blurred pollen particle boundaries and produced the worst results.
- **Gaussian** and **median filters** preserved particle shapes better.
- Increasing kernel size reduced noise slightly but also blurred the particles.

Overall, spatial filtering had only a limited impact on improving segmentation quality.

---

## 4. Segmentation

Two segmentation approaches were tested.

### Manual Thresholding

Several threshold values were manually tested with and without prior filtering.

### Otsu Thresholding

Otsu’s automatic thresholding algorithm was applied to:

- filtered images
- unfiltered images

After comparing the results, **Otsu thresholding without prior filtering** produced the most compact and well-defined pollen particle regions.

Therefore, this method was selected for the remaining analysis.

---

## 5. Boundary Extraction

Due to strong background noise and artefacts introduced by the embedding medium, thresholding alone did not always produce clean segmentation masks.

To obtain reliable boundaries, **10 pollen particles were manually segmented**.

Processing steps:

1. Cropping individual pollen particles from the images
2. Converting masks to grayscale
3. Converting grayscale images to boolean masks
4. Filling internal gaps to ensure solid object regions

Boundaries were extracted using **morphological erosion**.

The erosion operation slightly shrinks the object.  
By subtracting the eroded image from the original mask, the removed pixels form a thin boundary around the object.

---

## 6. Chain Code Representation

The extracted boundaries were represented using **Freeman chain codes**.

Freeman chain code records the direction of movement between consecutive contour pixels using an **8-neighbourhood representation**.

The boundary is therefore encoded as a sequence of numbers between **0 and 7** representing directions.

Two normalization methods were applied:

- **Minimum magnitude normalization** – rotates the sequence so that it begins with the smallest direction value.
- **First difference normalization** – records changes between consecutive directions.

These transformations help make the representation more invariant and comparable between different shapes.

---

## 7. Boundary Descriptors

Several boundary-based descriptors were computed for each pollen particle:

- Boundary length
- Diameter
- Major axis

The results show variation in pollen particle size across the dataset.

Because all particles were cropped to the same image dimensions, the descriptors are directly comparable between samples.

---

## 8. Region-Based Descriptors

Region-based shape descriptors were also computed:

- Area
- Perimeter
- Compactness
- Circularity
- Eccentricity

Observations:

- **Area and perimeter** vary significantly between particles, reflecting differences in particle size.
- **Compactness** values are relatively similar, indicating that most particles have fairly regular shapes.
- **Circularity** values suggest the particles are moderately circular.
- **Eccentricity** indicates slight variation in particle elongation.

These descriptors provide quantitative measurements of pollen morphology.

---

# Technologies Used

- Python
- NumPy
- OpenCV
- scikit-image
- Matplotlib
- Jupyter Notebook

---

# Running the Project

Clone the repository:

```
git clone <repository-url>
```

Install dependencies:

```
pip install numpy opencv-python scikit-image matplotlib
```

Launch Jupyter Notebook:

```
jupyter notebook
```

Open and run the notebook:

```
notebooks/pollen_analysis.ipynb
```

---

# Concepts Demonstrated

This project demonstrates several important digital image processing concepts:

- Image preprocessing
- Histogram analysis
- Spatial filtering
- Threshold-based segmentation
- Morphological operations
- Boundary extraction
- Chain code representation
- Shape descriptors
- Region descriptors
