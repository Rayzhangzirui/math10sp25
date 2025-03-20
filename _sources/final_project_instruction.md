# Final Project Instruction

## Goal

The purpose of the final project is to submit a notebook project which analyzes a dataset of your choice. This type of project could look great in a portfolio when applying to internships and jobs.


## Submission
* Due date: 11:59pm, Wednesday of the Finals Week.
* Before making the submission, Restart kernel>Run all to make sure there is no error.
* Submit .ipynb to Gradescope
* Format: the first cell of the notebook should be a markdown cell with the following format:
```
# [Title]

Author: [Name]

Course Project, UC Irvine, Math 10, Spring 25

I would like to post my notebook on the course's website. [Yes/No]
```
Answer the question regarding whether you want your project posted in the course notes. If the answer is "yes", you can also email me to update or remove the notebook later.

## Requirements
* This is an individual project.
* The project should clearly build on the Math 10 material. 
* The primary focus of the project must be on something involving data, and primarily using one or more datasets that weren't covered in Math 10.
* Anything that is taken from another source (either the idea for the project or a piece of code, even if you edit that code) should be explicitly referenced with a link.  (For example, you could write, "The configuration of this Altair chart was adapted from ...").



## Rubric
The course project is worth 20% of the course grade, and we will grade the project out of 20 total points.

* **Clarity** (4 points)
    - The notebook is well-formatted, with appropriate use of section headers, markdown, and text to guide the reader.
    - The project has a relevant title
    - Datasets are clearly described with sources provided.

* **Code Quality** (4 points)
    - Code runs without errors.
    - Code is well-commented and easy to follow.

* **Data Exploration and Visualization** (4 points)
    - Pandas is used appropriately for data exploration.
    - Data cleaning, analysis, or feature engineering is performed where needed, and is clearly explained.
    - A variety of charts are included that provide insights into the data.
    - The charts are well-labeled and easy to understand (e.g., appropriate titles, axis labels, legends).

* **Analysis** (4 points)
    - Machine learning models are used appropriately where relevant.
    - Key data analysis concepts such as overfitting, cross-validation, or bias-variance tradeoff are discussed where relevant.
    - Results are clearly interpreted with appropriate conclusions drawn.
    
* **Extra** (4 points)
    - The project has new algorithms (tree-based models, support vector machines, etc.) that go beyond the course material. Training neural networks count as an extra.

## Examples
Here are examples of student projects: 
[Fall 2024](https://rayzhangzirui.github.io/math10fa24/final_projects/intro.html), 
[Spring 2024](https://rayzhangzirui.github.io/math10sp24/final_project_demo/intro.html), 
[Spring 2023](https://christopherdavisuci.github.io/UCI-Math-10-S23/Proj/StudentProjects.html), 
[Spring 2022](https://christopherdavisuci.github.io/UCI-Math-10-S22/Proj/StudentProjects.html), 
[Winter 2022](https://christopherdavisuci.github.io/UCI-Math-10-W22/Proj/StudentProjects.html), 
[Fall 2022](https://christopherdavisuci.github.io/UCI-Math-10-F22/Proj/StudentProjects.html).

## Project Advice

Here is some general advice:

- **Data Source**: You can use datasets that are built in to a Python library like Seaborn or scikit-learn, or you can use dataset from [Kaggle](https://https://kaggle.com/), [openml.org](https://www.openml.org/), [UC Irvine Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), etc. You also also welcome to use your own dataset, but make sure that privacy is not violated.
- **Avoid Repetition:** Don’t repeat the same technique multiple times unless essential. A variety of different charts is better than the same chart for different datasets.
- **Don't Oversearch:** Don’t spend too much time searching for the perfect dataset or idea. Exploring a dataset in an interesting way is more important than having a perfect dataset.
- **Be Reasonable:** Keep your statements reasonable. It’s better to say "Therefore, we didn’t see any relationship between A and B," than to make a claim that isn’t supported by the data.
- **Show Your Learning:** The main point is to demonstrate what you’ve learned in Math 10. A project based on material outside of what was covered in class won’t score well, even if it's advanced.
- **Use Markdown:** Include many markdown cells to explain what you are doing. These can be very short, even just one sentence. Use `**bold text**` in markdown cells for emphasis, not in Python comments.
- **Reference Everything:** Reference all sources clearly. If your project is based on an idea or tutorial, include a clear link to the original source. Your project will be graded on how well you explain and build on the tutorial content.


## Frequently Asked Questions

**Q: Is there a length requirement?**
- No specific length, but aim to spend about 12 productive hours on the project. Productive means time spent actively working on the project, not just browsing tutorials or datasets.

**Q: What if I'm worried my project is too short?**
- It's fine to switch topics halfway through. If you finish your original plan and want to start something new with a different dataset, go ahead.

**Q: What if much of my work involved data cleaning, but it's not in the project?**
- Include all data cleaning steps in the project. Work done outside Python, like in Excel, won't count.

**Q: Can I use a different plotting library?**
- Yes, the content of the figure is more important than the library used. But choosing a different plotting library does not count as an "extra" for the project.

**Q: Do I need to post my project in the course notes?**
- Posting is optional. It can be useful for applications to internships or grad school. You can email me to update or remove it later.

**Q: How can I use an Excel file instead of a CSV file?**
- Use `pd.read_excel` for Excel files. Alternatively, open the Excel file in Excel or Google Sheets, save it as a CSV, and then upload that CSV file.

**Q: What if the dataset is large and the algorithm takes a long time to run?**
- Scaling can improve convergence of some algorithm. Features with larger scales will dominate the gradients, causing slower convergence. he optimization trajectory might “zig-zag” rather than smoothly progressing toward the minimum.
- You can reduce the dimensionality of the data and work with a smaller dataset.
- It's fine to use a subset of the data for the project. Just mention that you did so.

**Q: I can't intall pytorch or tensorflow on my computer.**
- Installing pytorch or tensorflow can be tricky sometime.
- You can use Google Colab, which is a free Jupyter notebook environment that runs on Google's cloud servers. It comes with many libraries pre-installed, including PyTorch and TensorFlow, and have free access to GPUs.