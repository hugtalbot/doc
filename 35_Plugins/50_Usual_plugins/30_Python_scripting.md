SofaPython3
-----------

The SofaPython3 plugin has its own [GitHub repository](https://github.com/sofa-framework/sofapython3/). A dedicated documentation for the plugin is available online: [SofaPython3 > readthedoc](https://sofapython3.readthedocs.io/en/latest/index.html).

This page provides guidelines to:
- install SOFA within a Python environment
- execute a Python script
- write new bindings

For more, check out the [Jupyter notebook](https://github.com/sofa-framework/Tutorials), providing a tutorial to build a SOFA simulation.


## Install

The SofaPython3 plugin allows you to embed a Python 3 interpreter into an existing SOFA application (e.g., runSofa) and to create/launch SOFA simulations from a Python environment.
Depending on your use case, you may need to follow different steps to set up a fully working SofaPython3 environment.


### Get SofaPython3

The latest version of the SofaPython3 plugin is now shipped with the binary SOFA release supported by the SOFA Consortium.
All you need to do is download these SOFA binaries from the [SOFA website](https://www.sofa-framework.org/download/).


### Get Python Installed

First, ensure you have the same version of Python installed on your computer as the one used in the binary version.

<details>
<summary><b>Ubuntu</b></summary>

Run in a terminal:

```bash
sudo add-apt-repository ppa\:deadsnakes/ppa
sudo apt install libpython3.12 python3.12 python3-pip
python3.12 -m pip install numpy
```

If you want to launch runSofa:

```bash
sudo apt install libopengl0
```

</details>


<details>
<summary><b>MacOS</b></summary>

Run in a terminal:

```bash
brew install python@3.12
export PATH="/usr/local/opt/python@3.12/bin/:\$PATH"
```

**BigSur only:**

```bash
pip3 install --upgrade pip
python3.12 -m pip install numpy
```

**Catalina only:**

```bash
pip3 install numpy
```
</details>


<details>
<summary><b>Windows</b></summary>

Download and install [Python 3.12 64-bit](https://www.python.org/ftp/python/3.12.1/python-3.12.1-amd64.exe).

</details>


### Setup Your Environment

#### Using runSofa

Using SofaPython3 in runSofa requires loading the SofaPython3 plugin in your runSofa environment. If you downloaded and installed SOFA from the SOFA website (as explained above), you can load the SofaPython3 plugin using the PluginManager (in the GUI) or by auto-loading the plugin in runSofa:
Simply copy the file **`plugin_list.conf.default`** in `<SOFA_build>/lib`, rename it **`plugin_list.conf`**, and add the line:

```text
SofaPython3 NO_VERSION
```

> **Note:** Adding the line to the file **`plugin_list.conf.default`** in `<SOFA_build>/lib` would work, but you would need to add the line every time you compile the code.

Having the SofaPython3 plugin active will allow you to open scene files using the `.py`, `.py3`, `.pyscn`, or `.pyscn3` file extensions in runSofa with the command:

```bash
<SOFA_build>/bin/runSofa <your_python_file>
```



#### Using Python 3

Before running your simulations, ensure you define the following environment variables:

<details>
<summary><b>Ubuntu</b></summary>

Run in a terminal:

```bash
export SOFA_ROOT=/path/to/SOFA_install
export PYTHONPATH=/path/to/SofaPython3/lib/python3/site-packages:\$PYTHONPATH
```

</details>


<details>
<summary><b>MacOS</b></summary>

Run in a terminal:

```bash
export SOFA_ROOT=/path/to/SOFA_install
export PYTHONPATH=/path/to/SofaPython3/lib/python3/site-packages:\$PYTHONPATH
export PATH="/usr/local/opt/python@3.12/bin/:\$PATH"
```

</details>


<details>
<summary><b>Windows</b></summary>

- Create a system variable **`SOFA_ROOT`** and set it to `<SOFA-install-directory>`.
- Create a system variable **`PYTHON_ROOT`** and set it to `<Python3-install-directory>`.
- Create a system variable **`PYTHONPATH`** and set it to `%SOFA_ROOT%\plugins\SofaPython3\lib\python3\site-packages`.
- Edit the system variable **`Path`** and add at the end:
  `;%PYTHON_ROOT%;%PYTHON_ROOT%\DLLs;%PYTHON_ROOT%\Lib;%SOFA_ROOT%\bin;`
- Open a Console (`cmd.exe`) and run:
  ```bash
  python -V && python -m pip install numpy scipy
  ```

After that, all you need to do is open a Console (`cmd.exe`) and run:
```bash
runSofa -lSofaPython3
```

⚠️ **Note:** Depending on the plugins you use, you might need to add the `site-packages/` paths associated with these plugins to your `PYTHONPATH`. These are usually located in `/path_to_plugin/lib/python3/site-packages`.
For example, if you use the binary install of SOFA and want to use both SofaPython3 and ImGui plugins in Python, you could define:
```bash
export PYTHONPATH=\$SOFA_ROOT/plugins/SofaPython3/lib/python3/site-packages/:\$SOFA_ROOT/plugins/SofaImGui/lib/python3/site-packages/
```

To discover how to use SOFA in any Python 3 interpreter, refer to the [First Steps section](https://sofapython3.readthedocs.io/en/latest/content/FirstSteps.html#with-the-python3-interpreter).

</details>


### Get Support

🙋 To get free technical assistance from the community, join the [SofaPython3 GitHub forum](https://github.com/sofa-framework/sofapython3/discussions) and post your questions there.

👨‍🏫 To quickly level up on SOFA, request [SOFA training sessions](https://www.sofa-framework.org/sofa-events/sofa-training-sessions/).



## Executing a Python script

### With runSofa

💡 To ensure runSofa can load a scene described by Python, you can either:
- Open runSofa without any argument once and then add `libSofaPython3` in the Plugin Manager.
- Or add `-l SofaPython3` to the command line.

For more information, refer to the documentation: [Plugin loading](https://sofa-framework.github.io/doc/plugins/what-is-a-plugin/#plugin_loading).

Once the SofaPython3 plugin is loaded, you can load a simulation from a Python script directly in runSofa.
Assuming you want to run a script named `example.py`, use the following command:

```shell
runSofa example.py
```

Let's see how this script `example.py` should look.
The first important thing is that to be compatible with SOFA, a Python script must define the `createScene(root: Sofa.Core.Node)` function. This function is the entry point of your simulation and is automatically called by the runSofa application when a Python file is loaded. It is responsible for describing and building the SOFA scene graph.

In the section ["Create a new simulation"](https://sofapython3.readthedocs.io/en/latest/content/FirstSteps.html#create-a-new-simulation), we detail how to implement this `createScene()`.


### With the Python 3 Interpreter

SOFA simulations can also be executed from a Python environment (including Jupyter Notebook).
To do so, the Python environment must include SOFA Python modules, located in `site-packages/` repositories. The path to these libraries should be added to the `PYTHONPATH`.
The [Installation section](https://sofapython3.readthedocs.io/en/latest/content/Installation.html#using-python3) details how to configure it.

Once your Python environment is properly configured, you will be able to import SOFA Python modules (e.g., `import Sofa`).
When running your simulation from a Python interpreter, you are responsible for:
- Creating the root node.
- Calling the `createScene()` function.
- Initializing the graph.

The Python environment does not pre-generate a root node as the runSofa executable does.
To run from a Python environment, any Python script should look like this:

```python
## Required import for SOFA within Python
import Sofa

def main():
    ## Call the SOFA function to create the root node
    root = Sofa.Core.Node("root")

    ## Call the createScene function, as runSofa does
    createScene(root)

    ## Once defined, initialization of the scene graph
    Sofa.Simulation.initRoot(root)

    ## Run as many simulation steps (here 10 steps are computed)
    for iteration in range(10):
        Sofa.Simulation.animate(root, root.dt.value)
        print("Computing iteration " + str(iteration + 1))

    print("Computation is done.")

## Same createScene function as in the previous case
def createScene(rootNode):
    ## Doesn't do anything yet
    return rootNode

## Function used only if this script is called from a Python environment
if __name__ == '__main__':
    main()
```

The above script can be run as follows:

```shell
python3 example.py
```

> **Note:**
> - By structuring your scripts this way, you get the advantage of having a script loadable from both runSofa and a Python 3 interpreter.
> - In the above example, the `main()` function runs 10 time steps without any graphical user interface, and the script ends.


#### Using the SOFA GUI from a Python Environment

If you want to manage the simulation from the runSofa GUI, you can call the GUI from the `main()` function as follows:

```python
def main():
    ## Call the SOFA function to create the root node
    root = Sofa.Core.Node("root")

    ## Call the createScene function, as runSofa does
    createScene(root)

    ## Once defined, initialization of the scene graph
    Sofa.Simulation.initRoot(root)

    ## Import the GUI package
    import SofaImGui
    import Sofa.Gui

    ## Launch the GUI (ImGui is now the default; to use Qt, refer to the example "basic-useQtGui.py")
    Sofa.Gui.GUIManager.Init("myscene", "imgui")
    Sofa.Gui.GUIManager.createGUI(root, __file__)
    Sofa.Gui.GUIManager.SetDimension(1080, 800)

    ## Initialization of the scene will be done here
    Sofa.Gui.GUIManager.MainLoop(root)
    Sofa.Gui.GUIManager.closeGUI()
```

So far, you can load this Python scene, but it doesn't do much. Let's enrich this scene!

A scene in SOFA is an ordered tree of nodes representing objects (e.g., a node for a hand, with child nodes for fingers). Each node has one or more components. Every node and component has a name and a few features. The main node at the top of the tree is usually called `"rootNode"` or `"root"`.
More about how to create a simulation scene can be found in the [SOFA online documentation](https://www.sofa-framework.org/community/doc/using-sofa/lexicography/).


#### Using the Old Qt GUI

Since SOFA v25.06, the SOFA GUI relies on the ImGui library. The previous Qt-based GUI is still available. To use it, ensure you:

- Add the `lib/` repository in the SOFA binaries to your `LD_LIBRARY_PATH`.
- Add the `lib/python3/site-packages/` repository to your `PYTHONPATH`.
- Ensure your SOFA install path does not include any special characters.

An example using the Qt GUI is available: [`basic-useQtGUI.py`](https://github.com/sofa-framework/SofaPython3/blob/master/examples/basic-useQtGUI.py).


## Writing Custom Binding

It is possible to write custom bindings for your own SOFA C++ components. To do this, follow the example provided in `bindings/BindingExample`. In this example, a custom C++ SOFA object named `CustomObject` is bound to Python and exposes some properties and methods.

See [this video tutorial](https://www.youtube.com/watch?v=S0QWV4Wj-vs) on how to create Python bindings with pybind11 and SofaPython3!

<iframe width="600" height="400" src="http://www.youtube.com/embed/S0QWV4Wj-vs?rel=0" frameborder="0" allowfullscreen></iframe>