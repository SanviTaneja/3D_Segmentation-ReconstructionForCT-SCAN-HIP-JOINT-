# 3D_Segmentation-ReconstructionForCT-SCAN-HIP-JOINT
CT Scan to 3D STL Reconstruction Pipeline

Overview
This project presents a medical imaging pipeline for converting CT scan data in DICOM format into a 3D reconstructed STL model. The system loads raw CT scan slices, organizes them into an appropriate data structure, processes them using medical image preprocessing techniques, and reconstructs a 3D model suitable for visualization or further computational analysis.

The pipeline demonstrates a structured approach to:
Reading DICOM CT scan datasets
Loading DICOM IN 3D SLICER
Constructing a 3D volumetric representation
Segmenting hip joint out of the CT Scan of Pelvic Region
Applying Hounsfield Unit conversion
Performing bone segmentation
Generating 3D mesh models
Reduced Mesh Complexity
Exporting the reconstructed anatomy as an STL file

This model is designed as a foundational framework for medical image analysis and 3D anatomical reconstruction, which can later be integrated with deep learning pipelines for diagnostic or surgical planning applications.



Dataset Used
The dataset used in this project is obtained from
The Cancer Imaging Archive (TCIA): https://www.cancerimagingarchive.net/collection/pelvic-reference-data/
Specifically CT datasets containing pelvic / hip CT scan series.
Each CT scan series typically consists of 50–150 DICOM slices, which together represent a volumetric scan of the anatomical region.

Project Pipeline
The overall pipeline followed in this project is illustrated below:
DICOM CT Dataset
        │
        ▼
Load DICOM Files
        │
        ▼
Sort & Organize Slices
        │
        ▼
Construct 3D Volume (NumPy Array)
        │
        ▼
Convert Pixel Values → Hounsfield Units
        │
        ▼
Bone Segmentation
        │
        ▼
Volume Cropping
        │
        ▼
3D Surface Reconstruction
        │
        ▼
Mesh Generation
        │
        ▼
STL Export

Key Innovation in the Pipeline

This project introduces a structured data processing workflow for medical imaging, with emphasis on:
1. Cross Data Verification: Slice metadata is verified across multiple DICOM headers,Slice ordering is validated,Pixel spacing and slice thickness consistency is checkedThis ensures that 3D reconstruction is geometrically correct.
2. Structured Data Representation:The CT slices are converted into a 3D NumPy volume, which enables:
Efficient volumetric computation
Medical image processing
3. Automated 3D Reconstruction

Installation and Dependencies
Required Python libraries:
pip install numpy
pip install pydicom
pip install matplotlib
pip install scikit-image
pip install scipy
pip install trimesh
pip install tqdm

Code Workflow Explanation
1. Loading the DICOM Dataset
The program first scans the dataset directory and identifies CT scan series.
pydicom.dcmread(file)
This loads the DICOM metadata and pixel information.
2. Sorting Slices
CT slices must be ordered correctly based on Image Position Patient or Instance Number.This ensures that slices are stacked in the correct anatomical order.
3. Creating a 3D Volume
All slices are stacked together to create a 3D NumPy array.
Volume shape:
(Slices, Height, Width)
Example:
(153, 512, 512)
This structure represents the full volumetric CT scan.
4. Hounsfield Unit Conversion
CT scanners store pixel values that must be converted to Hounsfield Units (HU).
HU values help identify tissue density.
Example ranges:
Tissue	HU Range
Air	-1000
Soft Tissue	20 – 80
Bone	700+
5. Bone Segmentation
Bone structures are extracted using threshold segmentation.
Example logic:
Bone Threshold: HU > 300
This removes soft tissues and isolates the skeletal structure.
6. Cropping the Volume
The segmented volume is cropped to reduce unnecessary empty space.
Benefits:
Faster processing
Reduced memory consumption
Improved mesh generation
7. Surface Reconstruction
The segmented volume is converted into a 3D surface mesh using algorithms such as:
Marching Cubes:This generates vertices and faces that define the 3D object.
8. STL Model Generation
The mesh is exported as an STL file(output_model.stl)
This file can be opened in tools such as:
MeshLab,Blender,3D Slicer

Data fusion would require:
Multiple datasets would have led to Dataset normalization by correcting the Alignment of coordinate systems and thenRe-normalization after fusion.Because each dataset may have different voxel spacing and intensity ranges, normalization becomes a critical step.

This step was therefore postponed but can be implemented to increase the robustness of the model.

Current Processing Scope
The current code is designed to process:
One CT scan series at a time:Each series contained 50–60+ slices
The code currently stops after processing the first valid series using a break condition.
However, by removing this break statement, the system can process multiple CT series sequentially.
Important note:
1 CT Series → 1 STL Model

Future Work
Several improvements can be implemented in future iterations:
1. Deep Learning Integration
The volumetric NumPy array can be converted into a PyTorch tensor for deep learning models.
Example:NumPy Array → Torch Tensor

2. Multi-Dataset Data Fusion
Future versions can integrate data fusion techniques to combine information from multiple scans to increase higher resolution reconstruction

Applications
This pipeline can support several medical imaging applications:
3D anatomical visualization
Orthopedic analysis
Surgical planning
Medical research
Educational visualization
3D printing of bone structures
Structural Analysis(SOLID MODEL)

Conclusion
This project demonstrates a complete pipeline for converting raw CT scan DICOM datasets into 3D STL models using medical image processing techniques by segmenting and reconstructing the 3D STL model.The workflow establishes a structured framework for DICOM data ingestion,volumetric reconstruction,segmentation,3D mesh generation and even removing noises so as to make ut ideal for medical imaging.
