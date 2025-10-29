# Binary Mask to YOLO Format Converter

This Python script converts binary segmentation mask images into YOLO format labels for both segmentation and bounding box detection tasks.

## Overview

The script processes binary mask images (where objects are white/255 and background is black/0) and generates:
- **YOLO segmentation labels**: Normalized polygon coordinates
- **YOLO bounding box labels**: Normalized bounding box coordinates (x_center, y_center, width, height)

## Requirements

```bash
pip install opencv-python numpy
```

## Directory Structure

### Input
Your mask images should be organized as follows:
```
masks/
├── image_01_mask.png
├── image_02_mask.png
├── image_03_mask.png
└── image_04_mask.png
```

### Output
After running the script, labels will be generated in the specified directories:
```
labels_seg/
├── image_01.txt
├── image_02.txt
└── ...

labels_box/
├── image_01.txt
├── image_02.txt
└── ...
```

## Usage

### Basic Example

```python
from binmask_converter import binmask_to_yolo

# Convert masks to YOLO format
binmask_to_yolo(
    binmask_path="path/to/masks",
    output_seg_path="path/to/labels_seg",
    output_box_path="path/to/labels_box"
)
```

### Generate Only Segmentation Labels

```python
binmask_to_yolo(
    binmask_path="path/to/masks",
    output_seg_path="path/to/labels_seg",
    output_box_path=None  # Skip bounding box generation
)
```

### Generate Only Bounding Box Labels

```python
binmask_to_yolo(
    binmask_path="path/to/masks",
    output_seg_path=None,  # Skip segmentation generation
    output_box_path="path/to/labels_box"
)
```

## Function Parameters

### `binmask_to_yolo(binmask_path, output_seg_path=None, output_box_path=None)`

- **binmask_path** (str, required): Directory containing binary mask images (`.png` or `.jpg`)
- **output_seg_path** (str, optional): Directory where YOLO segmentation labels will be saved
- **output_box_path** (str, optional): Directory where YOLO bounding box labels will be saved

## Input Requirements

- **Image format**: PNG or JPG
- **Mask type**: Binary masks where:
  - Objects = white (pixel value 255)
  - Background = black (pixel value 0)
- **File naming**: Files with `_mask` in the name will have this suffix removed in output labels

## Output Format

### Segmentation Labels (`.txt`)
Each line represents one object:
```
class_id x1 y1 x2 y2 x3 y3 ... xn yn
```
Where:
- `class_id`: Always set to `1` (modify in code if needed)
- `x, y`: Normalized coordinates (0.0 to 1.0) of polygon points

### Bounding Box Labels (`.txt`)
Each line represents one object:
```
class_id x_center y_center width height
```
Where:
- `class_id`: Always set to `0` (class_id - 1)
- All values are normalized (0.0 to 1.0)

## Notes

- The script automatically creates output directories if they don't exist
- Contours with fewer than 3 points are skipped (YOLO requirement)
- Coordinates are normalized and rounded to 6 decimal places
- Progress messages are printed for each processed image

## Example Output

For a mask image `foam_mask.png` (1920x1080), the script generates:

**foam.txt** (segmentation):
```
1 0.245833 0.462963 0.252604 0.469907 0.259375 0.476852 ...
```

**foam.txt** (bounding box):
```
0 0.5 0.5 0.3 0.2
```

## Troubleshooting

- **No contours found**: Ensure your masks are binary (black and white only)
- **Empty output files**: Check that mask pixel values are exactly 255 for objects
- **Missing output**: Verify that output directories are writable

## Command Line Usage

### Option 1: Using Python Module Directly

```bash
python -c "from binmask_converter import binmask_to_yolo; binmask_to_yolo('masks/', 'labels_seg/', 'labels_box/')"
```

### Option 2: Create a CLI Script

Add this to the end of your Python file or create a new file `binmask_to_yolo.py`:

```python
if __name__ == "__main__":
    import argparse
    
    parser = argparse.ArgumentParser(description='Convert binary masks to YOLO format')
    parser.add_argument('input', help='Path to directory containing mask images')
    parser.add_argument('--seg', '--segmentation', dest='seg_output', 
                        help='Output directory for segmentation labels')
    parser.add_argument('--box', '--bbox', dest='box_output',
                        help='Output directory for bounding box labels')
    
    args = parser.parse_args()
    
    if not args.seg_output and not args.box_output:
        parser.error('At least one output directory (--seg or --box) must be specified')
    
    binmask_to_yolo(args.input, args.seg_output, args.box_output)
```

Then run from bash:

```bash
# Generate both segmentation and bounding boxes
python binmask_to_yolo.py masks/ --seg labels_seg/ --box labels_box/

# Generate only segmentation labels
python binmask_to_yolo.py masks/ --seg labels_seg/

# Generate only bounding boxes
python binmask_to_yolo.py masks/ --box labels_box/
```

### Option 3: Create a Bash Wrapper Script

Create `binmask_to_yolo.sh`:

```bash
#!/bin/bash

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

if [ $# -lt 2 ]; then
    echo "Usage: $0 <input_dir> <output_seg_dir> [output_box_dir]"
    echo "Example: $0 masks/ labels_seg/ labels_box/"
    exit 1
fi

INPUT_DIR="$1"
OUTPUT_SEG="${2:-}"
OUTPUT_BOX="${3:-}"

python3 -c "
from binmask_converter import binmask_to_yolo
binmask_to_yolo('$INPUT_DIR', '$OUTPUT_SEG' if '$OUTPUT_SEG' else None, '$OUTPUT_BOX' if '$OUTPUT_BOX' else None)
"
```

Make it executable:

```bash
chmod +x binmask_to_yolo.sh
./binmask_to_yolo.sh masks/ labels_seg/ labels_box/
```

## License

This code is provided as-is for educational and commercial use.