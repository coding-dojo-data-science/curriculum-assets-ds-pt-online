# How to Save Python Functions as Modules 

- ChatGPT Chat: https://chat.openai.com/share/d6d5daca-798a-405b-90de-8eb6e41435e1

### Importing Custom Modules Locally

>  ***What is the simplest way to save python functions so that they can be reused almost anywhere, without actually publishing a python package?***

The simplest way to save Python functions so that they can be reused almost anywhere is to put them into a .py file (often referred to as a Python module) and import this file when you need the functions.

Here's a step-by-step guide:

1. **Create a Python module**. Let's say you have a function to add two numbers:

   ```python
   def add_two_numbers(a, b):
       return a + b
   ```

   Save this function in a .py file. For example, save it as `my_functions.py`.

2. **Use the function in another script**. You can import this module into another Python script like this:

   ```python
   import my_functions
   
   result = my_functions.add_two_numbers(1, 2)
   print(result)  # Output: 3
   ```

   Note: The Python script and `my_functions.py` should be in the same directory for this to work. If they're not, you'd need to adjust the system's PYTHONPATH to include the directory where your module resides.

3. **For using in different projects across your machine**, a slightly more advanced way to reuse your functions is to create a special directory (e.g., ~/my_python_modules) where you will store such .py files. Then add this directory to the PYTHONPATH environment variable:

   - **On Unix/Linux/Mac**:
     - Open your terminal.
     - Open your shell's profile script (`.bashrc`, `.bash_profile`, or `.zshrc` for Bash/Zsh).
     - Add this line to the end of the file: `export PYTHONPATH="${PYTHONPATH}:~/my_python_modules"`
     - Save and close the text editor.
     - Either source the profile script (e.g., `source ~/.bashrc`) or close and re-open your terminal.
   - **On Windows**:
     - Open System Properties/Advanced/Environment Variables.
     - Choose New under System variables.
     - Enter PYTHONPATH as the variable name and the path to the directory as the variable value.
     - Click OK until you're out of the dialog. The changes will take effect after a restart.

   Now you can import your function in any Python script across your system like this:

   ```python
   import my_functions
   
   result = my_functions.add_two_numbers(1, 2)
   print(result)  # Output: 3
   ```

Remember, although this method is simple and works for small personal projects, if your module gets larger and more complex, or if you want to share your code with others, it's better to package it properly. Python packaging allows for versioning, easy distribution, and more flexible installation options.

___

### Update Custom Functions while using in a notebook

> ***How can I import my custom my_functions module in a Jupyter Notebook in a way that lets me continue to edit the .py file and have the module automatically reload itself with the updated code?***

If you're actively developing and changing a Python module, and you want those changes to be immediately available in your Jupyter Notebook, you can use the `autoreload` extension from IPython.

Here's a step-by-step guide:

1. **Load the `autoreload` extension**. In your Jupyter notebook, run the following code:

   ```python
   %load_ext autoreload
   ```

2. **Set autoreload mode**. There are two modes for autoreload:

   - `%autoreload 1`: Reloads modules imported with `%aimport`.
   - `%autoreload 2`: Reloads all modules (except those excluded by `%aimport`) every time before executing the Python code typed.

   So you'll probably want to use `%autoreload 2`:

   ```python
   %autoreload 2
   ```

3. **Import your module**. Now, you can import your module:

   ```python
   import my_functions as mf
   ```

Combined, the code would would look like the following (note that the extension is loaded **before** the import):

```python
%load_ext autoreload
%autoreload 2
import my_functions as mf
```

With this setup, changes made to the `my_functions.py` file will automatically be reflected in the Jupyter Notebook, without needing to restart the kernel or manually reimport the module. Note that this may not always work for certain types of changes, like changes to a class definition. It works best for function definitions.

One important thing to note is that while `autoreload` can be very useful during development, it might make your code behave in ways you don't expect, since the module state can change without you explicitly reloading it. Therefore, once your code is stable, it is a good practice to remove the `autoreload` statements and import your modules in the standard way.

___

### Common Issues & Gotcha's

> ***What tips or things to watch out for do you have for someone who is moving functions from their jupyter notebook to a .py file for the very first time? What are some of the common gotcha's/issues that people run into?***

Moving Python functions from a Jupyter Notebook to a .py file is generally a straightforward process, but there are some things you need to be mindful of. Here are some tips and common issues to watch out for:

1. **Hardcoded Paths**: If you have hardcoded paths for file I/O operations, these may no longer be correct when you move your function to a .py file in a different directory. Consider using relative paths or dynamically generating paths using Python's `os` module.
2. **Variable Dependencies**: In a Jupyter notebook, it's common to use globally defined variables within functions. If your function depends on global variables, ensure these are passed as arguments to the function in the .py file. Your functions should ideally not rely on global state.
3. **Order of Execution**: In Jupyter, you can run cells out of order. This can sometimes create dependencies that aren't immediately obvious. When you move your code to a .py file, everything is run sequentially from top to bottom, so you may need to rearrange your function definitions.
4. **Interactive Features**: Functions in Jupyter notebooks may leverage interactive features, such as magic commands (e.g., `%matplotlib inline`) or use `input()` to get user input. These may not work as expected in a .py file, and alternatives should be considered.
5. **Error Handling**: In a Jupyter Notebook, you may have become accustomed to writing quick-and-dirty code without much error handling, since you can interactively debug issues. In a .py file, you should add appropriate error handling to your functions.
6. **Imports**: Ensure that all necessary imports are included at the beginning of your .py file. You may have imported libraries in different cells of your notebook. It's important to consolidate these imports in your .py file.
7. **Refactoring**: This is a good opportunity to refactor your code. Make sure each function does one thing and does it well. Ensure your function names and variables are descriptive, and consider adding docstrings to your functions to explain what they do.
8. **Testing**: Make sure to test your functions after moving them to the .py file. Just because they worked in the notebook doesn't mean they'll still work in the new context.

Remember, code in a .py file should be self-contained and not depend on any hidden state, which is a common gotcha when moving from Jupyter notebooks to .py files.