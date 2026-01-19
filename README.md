# Dataset: SPITTLEBUG_DB
This dataset was created to support the publication of the accompanying article [1] and to make the data publicly available. It contains 365 images in .PNG and .JPG formats, captured at various resolutions using three different devices: an Intel RealSense D435, a Canon EOS1100D, and an iPhone 11. The images are intended for research in computer vision and machine learning, particularly for applications in pest monitoring and automated detection systems.

## Subject Area
- **Computer science**: artificial intelligence, computer vision, computer science applications, pattern recognition  
- **Data science**: applied machine learning  
- **Agricultural sciences**: agronomy and crop science  
- **Biological sciences**: entomology and insect science

## Specific Subject Area
Image dataset of juvenile Aphrophoridae’s foam to train deep learning models for early identification of *Xylella fastidiosa* vectors.

## Type of Data
- **Data format**: .PNG, .JPG
- **Number of images**: 365
  - 211 annotated with:
    - PNG binary masks (semantic segmentation)
    - YOLO format labels (bounding boxes, segmentation)
  - 154 unannotated (testing purposes)
- **Image resolutions**:
  - 1280×720 (Intel RealSense D435)
  - 3024×4032 (iPhone 11)
  - 4272×2848 (Canon EOS1100D)

## Data Collection
- **Campaign dates**: April 2024 and April 2025
- **Location**: Uncultivated 600 m² area in Valenzano, Apulia, Italy  
- **GPS coordinates**: 41°01’40’’N, 16°54’14’’E
- **Devices used**:
  - Intel RealSense D435
  - iPhone 11
  - Canon EOS1100D
- **Collection method**:
  - Cameras manually deployed at varying distances (30–50 cm)
  - Varied camera angles (frontal to orthogonal)
  - Lighting conditions: sunny and cloudy, with light changes and shadows

## Data Source Location
Valenzano, Apulia, Italy (70010 BA)  
**GPS coordinates**: 41°01’40’’N, 16°54’14’’E

## Mask conversion code
The Python script `binmask_to_yolo.py` converts binary segmentation masks to YOLO format for segmentation and detection tasks. See `binmask_to_yolo.md` for details.

## Usage for object detection and segmentation

Once the dataset has been processed and converted to the YOLO format, it can be used for either the segmentation or the detection task. To this end, the [Ultralytics](https://github.com/ultralytics/ultralytics) library can be used. A sample script for object detection is the following:

```py
from ultralytics import YOLO

# Load the pretrained YOLO11 nano model
model = YOLO("yolo11n.pt")
# Fine tune the model on the dataset by specifying its relative or absolute path.
model.train("path/to/converted/dataset.yaml")
```

A base experiment for the segmentation task can be achieved by modifying the targeted model, for example selecting the `yolov11n-seg.pt` version.

## Acknowledgments
This work was funded by the European Union-NextGenerationEU under the research program PNRR MUR Missione 4, Componente C2, Investimento 1.1 NextGenerationEU - PRIN 2022 “Sustainable physical management system and automated detection of juvenile Aphrophoridae vectors of Xylella fastidiosa” (Grant N. 20227F7J5W, CUP H53D23005130006, B53D23017280006).
 
## Credits
Experiments were conceived and supervised by Annalisa Milella (CNR-STIIMA) and Simone Pascuzzi (UNIBA). Data acquisitions were performed by Annalisa Milella, Michele Elia, Arianna Rana, and Antonio Petitti (CNR-STIIMA). Annalisa Milella, Michele Elia, Angelo Cardellicchio, and Vito Renò (CNR-STIIMA) contributed to data annotation, data maintenance, and software development. The technical support from Michele Paradiso and Giuseppe Veronico is also acknowledged.
<br/>
<br/>
Contact person: Annalisa Milella - annalisa.milella@cnr.it
<br/>
<br/>
National Research Council of Italy (CNR), Institute of Intelligent Systems and Technologies for Advanced Manufacturing (STIIMA), via Amendola 122 D/O, 70126, Bari, Italy

## Cite
M. Elia, A. Cardellicchio, M. Paradiso, G. Veronico, A. Rana, A. Petitti, V. Renò, S. Pascuzzi, A. Milella, "Towards sustainable management of Xylella fastidiosa vectors: an annotated image dataset for automated in-field detection of Aphrophoridae foam," Data in Brief, 2026, 112477, ISSN 2352-3409, https://doi.org/10.1016/j.dib.2026.112477.


