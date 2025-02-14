Installation Guide
===================

.. image:: https://img.shields.io/badge/IsaacSim-2023.1.1-silver.svg
   :target: https://developer.nvidia.com/isaac-sim
   :alt: IsaacSim 2023.1.1

.. image:: https://img.shields.io/badge/python-3.10-blue.svg
   :target: https://www.python.org/downloads/release/python-31013/
   :alt: Python 3.10

.. image:: https://img.shields.io/badge/platform-linux--64-orange.svg
   :target: https://releases.ubuntu.com/20.04/
   :alt: Ubuntu 20.04


Installing Isaac Sim
--------------------


.. caution::

   We have dropped support for Isaac Sim versions 2023.1.0 and below. We recommend using the latest
   Isaac Sim 2023.1.1 release.

   For more information, please refer to the
   `Isaac Sim release notes <https://docs.omniverse.nvidia.com/isaacsim/latest/release_notes.html>`__.

Downloading pre-built binaries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please follow the Isaac Sim
`documentation <https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_workstation.html>`__
to install the latest Isaac Sim release.

To check the minimum system requirements,refer to the documentation
`here <https://docs.omniverse.nvidia.com/isaacsim/latest/installation/requirements.html>`__.

.. note::
	We have tested Isaac Lab with Isaac Sim 2023.1.1 release on Ubuntu
	20.04LTS with NVIDIA driver 525.147.

Configuring the environment variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Isaac Sim is shipped with its own Python interpreter which bundles in
the extensions released with it. To simplify the setup, we recommend
using the same Python interpreter. Alternately, it is possible to setup
a virtual environment following the instructions
`here <https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/install_python.html>`__.

Please locate the `Python executable in Isaac
Sim <https://docs.omniverse.nvidia.com/isaacsim/latest/manual_standalone_python.html#isaac-sim-python-environment>`__
by navigating to Isaac Sim root folder. In the remaining of the
documentation, we will refer to its path as ``ISAACSIM_PYTHON_EXE``.

.. note::

	On Linux systems, by default, this should be the executable ``python.sh`` in the directory
	``${HOME}/.local/share/ov/pkg/isaac_sim-*``, with ``*`` corresponding to the Isaac Sim version.

To avoid the overhead of finding and locating the Isaac Sim installation
directory every time, we recommend exporting the following environment
variables to your terminal for the remaining of the installation instructions:

.. code:: bash

   # Isaac Sim root directory
   export ISAACSIM_PATH="${HOME}/.local/share/ov/pkg/isaac_sim-2023.1.0-hotfix.1"
   # Isaac Sim python executable
   export ISAACSIM_PYTHON_EXE="${ISAACSIM_PATH}/python.sh"

For more information on common paths, please check the Isaac Sim
`documentation <https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_faq.html#common-path-locations>`__.

Running the simulator
~~~~~~~~~~~~~~~~~~~~~

Once Isaac Sim is installed successfully, make sure that the simulator runs on your
system. For this, we encourage the user to try some of the introductory
tutorials on their `website <https://docs.omniverse.nvidia.com/isaacsim/latest/introductory_tutorials/index.html>`__.

For completeness, we specify the commands here to check that everything is configured correctly.
On a new terminal (**Ctrl+Alt+T**), run the following:

-  Check that the simulator runs as expected:

   .. code:: bash

      # note: you can pass the argument "--help" to see all arguments possible.
      ${ISAACSIM_PATH}/isaac-sim.sh

-  Check that the simulator runs from a standalone python script:

   .. code:: bash

      # checks that python path is set correctly
      ${ISAACSIM_PYTHON_EXE} -c "print('Isaac Sim configuration is now complete.')"
      # checks that Isaac Sim can be launched from python
      ${ISAACSIM_PYTHON_EXE} ${ISAACSIM_PATH}/standalone_examples/api/omni.isaac.core/add_cubes.py

.. attention::

	If you have been using a previous version of Isaac Sim, you
	need to run the following command for the *first* time after
	installation to remove all the old user data and cached variables:

	.. code:: bash

		${ISAACSIM_PATH}/isaac-sim.sh --reset-user

If the simulator does not run or crashes while following the above
instructions, it means that something is incorrectly configured. To
debug and troubleshoot, please check Isaac Sim
`documentation <https://docs.omniverse.nvidia.com/dev-guide/latest/linux-troubleshooting.html>`__
and the
`forums <https://docs.omniverse.nvidia.com/isaacsim/latest/isaac_sim_forums.html>`__.


Installing Isaac Lab
--------------------

Organizing the workspace
~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

   We recommend making a `fork <https://github.com/isaac-sim/IsaacLab/fork>`_ of the ``Isaac Lab`` repository to contribute
   to the project. This is not mandatory to use the framework. If you
   make a fork, please replace ``isaac-sim`` with your username
   in the following instructions.

   If you are not familiar with git, we recommend following the `git
   tutorial <https://git-scm.com/book/en/v2/Getting-Started-Git-Basics>`__.

-  Clone the ``Isaac Lab`` repository into your workspace:

   .. code:: bash

      # Option 1: With SSH
      git clone git@github.com:isaac-sim/IsaacLab.git
      # Option 2: With HTTPS
      git clone https://github.com/isaac-sim/IsaacLab.git

-  Set up a symbolic link between the installed Isaac Sim root folder
   and ``_isaac_sim`` in the ``IsaacLab``` directory. This makes it convenient
   to index the python modules and look for extensions shipped with
   Isaac Sim.

   .. code:: bash

      # enter the cloned repository
      cd IsaacLab
      # create a symbolic link
      ln -s ${ISAACSIM_PATH} _isaac_sim

We provide a helper executable `isaaclab.sh <https://github.com/isaac-sim/IsaacLab/blob/main/isaaclab.sh>`_ that provides
utilities to manage extensions:

.. code:: text

   ./isaaclab.sh --help

   usage: isaaclab.sh [-h] [-i] [-e] [-f] [-p] [-s] [-t] [-o] [-v] [-d] [-c] -- Utility to manage Isaac Lab.

   optional arguments:
      -h, --help           Display the help content.
      -i, --install        Install the extensions inside Isaac Lab.
      -e, --extra [LIB]    Install learning frameworks (rl_games, rsl_rl, sb3) as extra dependencies. Default is 'all'.
      -f, --format         Run pre-commit to format the code and check lints.
      -p, --python         Run the python executable provided by Isaac Sim or virtual environment (if active).
      -s, --sim            Run the simulator executable (isaac-sim.sh) provided by Isaac Sim.
      -t, --test           Run all python unittest tests.
      -o, --docker         Run the docker container helper script (docker/container.sh).
      -v, --vscode         Generate the VSCode settings file from template.
      -d, --docs           Build the documentation from source using sphinx.
      -c, --conda [NAME]   Create the conda environment for Isaac Lab. Default name is 'isaaclab'.

Setting up the environment
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. attention::
   This step is optional. If you are using the bundled python with Isaac Sim, you can skip this step.

The executable ``isaaclab.sh`` automatically fetches the python bundled with Isaac
Sim, using ``./isaaclab.sh -p`` command (unless inside a virtual environment). This executable
behaves like a python executable, and can be used to run any python script or
module with the simulator. For more information, please refer to the
`documentation <https://docs.omniverse.nvidia.com/isaacsim/latest/manual_standalone_python.html#isaac-sim-python-environment>`__.

Although using a virtual environment is optional, we recommend using ``conda``. To install
``conda``, please follow the instructions `here <https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html>`__.
In case you want to use ``conda`` to create a virtual environment, you can
use the following command:

.. code:: bash

   # Option 1: Default name for conda environment is 'isaaclab'
   ./isaaclab.sh --conda  # or "./isaaclab.sh -c"
   # Option 2: Custom name for conda environment
   ./isaaclab.sh --conda my_env  # or "./isaaclab.sh -c my_env"

If you are using ``conda`` to create a virtual environment, make sure to
activate the environment before running any scripts. For example:

.. code:: bash

   conda activate isaaclab  # or "conda activate my_env"

Once you are in the virtual environment, you do not need to use ``./isaaclab.sh -p``
to run python scripts. You can use the default python executable in your environment
by running ``python`` or ``python3``. However, for the rest of the documentation,
we will assume that you are using ``./isaaclab.sh -p`` to run python scripts. This command
is equivalent to running ``python`` or ``python3`` in your virtual environment.

Building extensions
~~~~~~~~~~~~~~~~~~~

To build all the extensions, run the following commands:

-  Install dependencies using ``apt`` (on Ubuntu):

   .. code:: bash

      sudo apt install cmake build-essential

-  Run the install command that iterates over all the extensions in
   ``source/extensions`` directory and installs them using pip
   (with ``--editable`` flag):

   .. code:: bash

      ./isaaclab.sh --install  # or "./isaaclab.sh -i"

-  For installing all other dependencies (such as learning
   frameworks), execute:

   .. code:: bash

      # Option 1: Install all dependencies
      ./isaaclab.sh --extra  # or "./isaaclab.sh -e"
      # Option 2: Install only a subset of dependencies
      # note: valid options are 'rl_games', 'rsl_rl', 'sb3', 'robomimic', 'all'
      ./isaaclab.sh --extra rsl_rl  # or "./isaaclab.sh -e rsl_r"


Verifying the installation
~~~~~~~~~~~~~~~~~~~~~~~~~~

To verify that the installation was successful, run the following command from the
top of the repository:

.. code:: bash

   # Option 1: Using the isaaclab.sh executable
   # note: this works for both the bundled python and the virtual environment
   ./isaaclab.sh -p source/standalone/tutorials/00_sim/create_empty.py

   # Option 2: Using python in your virtual environment
   python source/standalone/tutorials/00_sim/create_empty.py

The above command should launch the simulator and display a window with a black
ground plane. You can exit the script by pressing ``Ctrl+C`` on your terminal or
by pressing the ``STOP`` button on the simulator window.

If you see this, then the installation was successful! |:tada:|
