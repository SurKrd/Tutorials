Tutorial
--------
In this tutorial we will look at how RTL simulations can interact with higher level description languages to run complex test scenarios. 
We will create a shared library with the Xilinx tools, which can embed a RTL simulation kernel into a C/C++ program. 
The [pybind11 library](https://github.com/pybind/pybind11) library even allows to define testroutines easily with python. 
For the binding and embedding of the shared library we access the preliminary work of [Graeme Smecher's pyxsi](https://github.com/gsmecher/pyxsi).

#### Step 0:
Preparing and Downloading pyxsi
```shell
pip3 install pybind11
pip3 install pytest
git clone https://github.com/gsmecher/pyxsi.git
```

#### Step 1:
We need a `.prj` file of our project, unfurtunatly it is not the same as the project file `.xpr` created by the Vivado.
To avoid to create the `.prj` manualy, we will export the simulation in Vivado.
Fortunately, we get all the necessary files.

#### Step 2:
```shell
cp -r pyxsi/src  <exportet_xsim_dir>/py2xsim_lib

```

