# Index
<details>
  <summary><b>Click to expand Table of Contents</b></summary>

1. [What is Jupyter Notebook](#what-is-a-jupyter-notebook)
2. [Using Google Colab (browser option)](#using-google-colab-browser-option)
 

</details>

# What is a Jupyter Notebook?

During the lab we will work in **Jupyter notebooks**, either in Google Colab or VS Code.

A Jupyter notebook is an interactive document that can contain:

- Code cells (that you can run)  
- Text cells (explanations, instructions, notes)  
- Outputs (tables, charts, printed results)

You run each code cell independently and see the result immediately below it.  
We’ll use notebooks to experiment with NumPy and to complete hands-on exercises.

If you have never seen a notebook before:

- Watch a short introduction video (any 5–10 min overview is fine), or  
- Read a brief introduction from the Jupyter or Colab docs.

You do **not** need to understand everything yet; just get familiar with:

- What a “cell” is  
- How to run a cell  
- How to see output

---

# Using Google Colab (browser option)

If you plan to use **Google Colab** in the browser:

## 2.1. Requirements

- A Google account (Gmail or Workspace)  
- Google Chrome browser

## 2.2. Install the “Open in Colab” Chrome extension

This extension lets you open GitHub-hosted notebooks in Colab with one click. [chromewebstore.google](https://chromewebstore.google.com/detail/open-in-colab/iogfkhleblhcpcekbiedikdehleodpjo?hl=en-US&utm_source=ext_sidebar)

1. Open the Chrome Web Store page for **Open in Colab**. [chromewebstore.google](https://chromewebstore.google.com/detail/open-in-colab/iogfkhleblhcpcekbiedikdehleodpjo?hl=en-US&utm_source=ext_sidebar)  
2. Click **Add to Chrome** and confirm.  
3. After installation, you should see the extension icon in your Chrome toolbar.

What it does:

- On GitHub, you’ll see an “Open in Colab” button for notebooks.  
- Clicking it will open the notebook directly in Colab so you can run the code. [chromewebstore.google](https://chromewebstore.google.com/detail/open-in-colab/iogfkhleblhcpcekbiedikdehleodpjo?hl=en-US&utm_source=ext_sidebar)

## 2.3. Test Colab

Before class:

1. Go to [https://colab.research.google.com](https://colab.research.google.com)  
     
2. Click **New notebook**.  
     
3. In the first cell, type:

```py
import numpy as np
np.__version__
```

4. Click **Run** (the ▶ button on the left of the cell) and confirm it runs without errors.

If this works, your Colab setup is ready.

---


# 3. Using VS Code (desktop option)

If you prefer to work in **VS Code**, you can either run notebooks **locally** or connect VS Code to **Google Colab** as the execution environment.

## 3.1. Install required VS Code extensions

In VS Code:

1. Open the **Extensions** panel (left sidebar, square icon).  
2. Install these extensions:  
   - **Python** (by Microsoft)  
   - **Jupyter** (by Microsoft) – adds notebook support in VS Code  
   - **Colab** (VS Code extension) – lets you use Google Colab as a compute environment from VS Code

After installation, restart VS Code if prompted.

## 3.2. Test Jupyter notebooks in VS Code (local Python)

1. Open VS Code.  
     
2. Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on macOS).  
     
3. Type **Jupyter: Create New Jupyter Notebook** and select it.  
     
4. In the first cell, type:

```py
import numpy as np
np.__version__
```

5. Run the cell (click the ▶ button).  
     
6. Confirm that the output shows a NumPy version and no error.

If this works, your local Jupyter environment in VS Code is ready.

## 3.3. (Optional) Use Colab as the environment in VS Code

If you installed the **Colab** extension, you can run notebooks on Colab from within VS Code:

1. Sign into your Google account in the Colab extension if prompted.  
     
2. Open a `.ipynb` notebook in VS Code.  
     
3. Use the Colab extension UI to select Colab as the kernel / compute environment (the exact command name may vary slightly depending on extension version).  
     
4. Run a test cell:

```py
import numpy as np
np.__version__
```

If this runs successfully, you are ready to use Colab’s backend while working in the VS Code interface.


---

# 4\. Choose your environment for class

For the lab, please **choose one** environment:

- **Option A – Google Colab (recommended for beginners):**  
    
  - Chrome \+ “Open in Colab” extension installed  
  - You have successfully run a Colab notebook with `import numpy as np`


- **Option B – VS Code with Jupyter:**  
    
  - Python 3.x installed  
  - VS Code installed with Python and Jupyter extensions  
  - You have successfully run a notebook cell with `import numpy as np`

You do **not** need to install NumPy separately if you use Colab; it is already included.

---

# 5\. What you do *not* need before class

You do **not** need to:

- Know advanced Python features  
- Know linear algebra

We will build everything step by step in class, using guided exercises.

---
