# Google Colab Setup Guide for BITS Assignment Submissions

## Part 1: Creating a Google Account (if needed)

### Step 1.1: Go to Google Account Creation
- Visit: https://accounts.google.com/signup
- Enter your details:
  - First name: Your first name
  - Last name: Your last name
  - Gmail address: Create a new email (e.g., `firstname.lastname@gmail.com`)
  - Password: Create a strong password
- Complete the verification process

### Step 1.2: Verify Your Account
- Check your recovery email for verification link
- Click the link to verify your Google account

---

## Part 2: Setting Up Google Colab

### Step 2.1: Access Google Colab
1. Go to https://colab.research.google.com/
2. Sign in with your Google account
3. Click "New notebook" or upload an existing `.ipynb` file

### Step 2.2: Enable GPU (Free)
**Why GPU?** Training deep learning models (CNN, RNN, Transformer) on CPU is very slow. Google Colab provides **free NVIDIA T4 GPU** which trains models 10-20x faster.

**Steps:**
1. Open your Colab notebook
2. Click menu: **Runtime** → **Change runtime type**
3. Select **Hardware accelerator**: `GPU`
4. Click **Save**
5. Colab will restart with GPU enabled

**To verify GPU is available, run this in a code cell:**
```python
import tensorflow as tf
print("GPU Available:", len(tf.config.list_physical_devices('GPU')))
print("GPUs:", tf.config.list_physical_devices('GPU'))
```

---

## Part 3: Installing Required Libraries

### Step 3.1: Install Deep Learning Libraries
Run these commands in the first cell of your Colab notebook:

```python
# Install required packages
!pip install tensorflow keras scikit-learn pandas numpy matplotlib seaborn pillow opencv-python

# For time series and transformer work
!pip install statsmodels

# Verify installations
import tensorflow as tf
import keras
print(f"TensorFlow version: {tf.__version__}")
print(f"Keras version: {keras.__version__}")
```

### Step 3.2: Test GPU & Libraries
```python
# Test GPU availability
print("GPU Available:", len(tf.config.list_physical_devices('GPU')))

# Test Keras imports
from tensorflow import keras
from keras import layers, models
print("All imports successful!")
```

---

## Part 4: Working with Datasets in Colab

### Option A: Download from TensorFlow Datasets (Recommended)
```python
# For CNN: Download Cats vs Dogs dataset
import tensorflow_datasets as tfds

# Load dataset
ds = tfds.load('cats_vs_dogs', split='train', as_supervised=True)

# For image size adjustment
ds = ds.map(lambda x, y: (tf.image.resize(x, (224, 224)), y))
```

### Option B: Download from Kaggle
1. Create Kaggle account: https://www.kaggle.com/
2. Get API key: Settings → API → Download `kaggle.json`
3. Upload to Colab:
```python
# Upload kaggle.json file in Colab
from google.colab import files
files.upload()

# Setup Kaggle
!mkdir -p ~/.kaggle
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
!kaggle datasets download -d dataset-name
```

### Option C: Use Public URLs
```python
# Download dataset from URL
!wget https://data-url/dataset.zip
!unzip dataset.zip
```

---

## Part 5: Uploading Your Code to Colab

### Method 1: Upload Notebook File
1. Click **File** → **Upload notebook**
2. Select your `.ipynb` file
3. Opens in Colab ready to edit

### Method 2: Create from GitHub
1. Click **File** → **Open notebook**
2. Select **GitHub** tab
3. Paste GitHub repository URL
4. Select the notebook from list

### Method 3: Create New & Paste Code
1. Click **File** → **New notebook**
2. Copy-paste your Python code into cells
3. Run each cell to verify

---

### Screenshot Tips:
- Include your email/username visible at top right
- Include the code cells with output
- Include notebook name at top
- Make sure the GPU indicator shows "GPU" is enabled
- Paint out or blur sensitive information if needed

---

## Part 7: Uploading Files from Colab

### Download Completed Notebook
```python
# After completing the assignment
from google.colab import files

# Download as .ipynb (will auto-export)
# The notebook is already saved, just click File → Download
```

**Steps:**
1. Complete all cells in your notebook
2. Run **Kernel** → **Restart and run all**
3. Click **File** → **Download** → **Download .ipynb**
4. Or export to HTML: **File** → **Print** → Save as PDF/HTML

---

## Part 8: Common Issues & Solutions

### Issue 1: GPU Not Available
**Problem**: Running on CPU (slow training)
**Solution**:
- Click: **Runtime** → **Change runtime type**
- Select: **Hardware accelerator** = `GPU`
- Click: **Save** and restart

### Issue 2: Out of Memory (OOM) Error
**Problem**: Dataset too large or batch size too big
**Solutions**:
- Reduce batch size: `batch_size=16` instead of `batch_size=32`
- Reduce image size: `(128, 128)` instead of `(224, 224)`
- Use fewer samples for testing
- Restart runtime to clear memory

### Issue 3: Libraries Not Found
**Problem**: `ModuleNotFoundError: No module named '...'`
**Solution**:
- Run: `!pip install package-name`
- Restart kernel after installation

### Issue 4: Dataset Download Takes Too Long
**Problem**: Slow internet connection
**Solutions**:
- Use smaller dataset subset
- Download once and cache it
- Use preloaded TensorFlow datasets

---

## Part 9: Complete Workflow Example

### Step-by-Step Workflow:
```
1. Create Google Account
   ↓
2. Go to colab.research.google.com
   ↓
3. Click "New Notebook"
   ↓
4. First cell: Install packages
   !pip install tensorflow keras scikit-learn pandas numpy matplotlib ...
   ↓
5. Enable GPU: Runtime → Change runtime type → Select GPU
   ↓
6. Second cell: Test GPU
   import tensorflow as tf
   print(tf.config.list_physical_devices('GPU'))
   ↓
7. Load and preprocess dataset
   ↓
8. Build models (CNN or RNN)
   ↓
9. Train models
   ↓
10. Generate metrics and visualizations

```

---

## Part 10: Tips for Success

### ✅ Do This:
- Save your notebook regularly (Ctrl+S in Colab)
- Test with small dataset first before full training
- Add markdown cells explaining your decisions
- Include visualizations of training progress
- Add screenshots showing your environment
- Run entire notebook before submission
- Test file download/conversion

### ❌ Don't Do This:
- Don't use pre-trained models from HuggingFace (for RNN assignment)
- Don't shuffle time series data (for RNN assignment)
- Don't shuffle image classification data (will lose class balance)
- Don't include data files in submission (only .ipynb and .html)
- Don't mix frameworks (use only Keras OR PyTorch, not both)
- Don't skip positional encoding in transformer
- Don't forget to track initial and final loss

---

## Quick Reference: GPU Performance Gains

| Task | CPU Time | GPU Time | Speedup |
|------|----------|----------|---------|
| Train CNN (50 epochs) | 2-3 hours | 8-12 min | **15-20x faster** |
| Train RNN (50 epochs) | 1-2 hours | 5-10 min | **10-15x faster** |
| Train Transformer (50 epochs) | 3-4 hours | 15-20 min | **12-15x faster** |

---

## Support Resources

- **Google Colab Docs**: https://colab.research.google.com/#faq
- **TensorFlow Docs**: https://www.tensorflow.org/tutorials
- **Keras Docs**: https://keras.io/
- **Colab GPU Limits**: Each session = 12 hours max (then restart)

---

**Ready to start? Go to colab.research.google.com and follow Part 2 to create your first notebook!**
