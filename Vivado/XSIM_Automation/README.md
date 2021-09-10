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
Copy the necessary files for the embedding in the exported xsim simulations folder 
```shell
cp -r pyxsi/src  <exportet_xsim_dir>/py2xsim_lib
```

#### Step 3:
Lead out of the top_tb the clock signals to get a control point. 
Start to create multiple test runs with the python pytest library.
An expamle is given in this tutorial folder.

#### Step 4:
Adjust the `<top_tb>.sh` in the `<exportet_xsim_dir>`
Source Xilinx environment variables or add it to the bash script
Extend the script with a new functions: `binding` and `simulate`

```shell 
# Source Xilinx environment variables - paste it at the top.
source <path/to/your>/Xilinx/Vivado/20xx.x/settings64.sh

# binding
binding()
{
  g++ -fPIC -std=c++17 -I/usr/include/python3.8 -I<path/to/your>/Xilinx/Vivado/20xx.x/data/xsim/include -Isrc -c -o pybind.o py2xsim_lib/pybind.cpp
  g++ -fPIC -std=c++17 -I/usr/include/python3.8 -I<path/to/your>/Xilinx/Vivado/20xx.x/data/xsim/include -Isrc -c -o xsi_loader.o py2xsim_lib/xsi_loader.cpp
  g++ -fPIC -std=c++17 -I/usr/include/python3.8 -I<path/to/your>/Xilinx/Vivado/20xx.x/data/xsim/include -Isrc -shared -o pyxsi.so pybind.o xsi_loader.o -ldl
}

# start simulation, test are discovered and executed using Python's pytest environment
simulate()
{
  LD_LIBRARY_PATH=<path/to/your>/Xilinx/Vivado/20xx.x/lib/lnx64.o python3.8 -m pytest <path/to>.py -v
}

```
Have fun with Python and Xsim!

