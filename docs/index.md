# Managing Workspace Environment Without Sudo Access

## Introduction

This guide provides strategies for managing a workspace environment and installing software without sudo access. It covers Python environment management, software installation using CMake, shared modules, and other useful techniques.

## Python Environment Management

For Python, we recommend using Conda, preferably in conjunction with Mamba for faster package resolution and installation.

### Basic Conda Commands

```bash
# Create a new environment
conda create --name myenv python=3.8

# Activate the environment
conda activate myenv

# Install packages
conda install package_name

# Deactivate the environment
conda deactivate
```

### Using Mamba with Conda

Mamba is a faster alternative to Conda's solver. Here's how to set it up and use it with Conda:

```bash
# Install Mamba in the base Conda environment
conda install mamba -n base -c conda-forge

# Set Mamba as the solver for Conda
conda config --set solver mamba
```

After setting Mamba as the solver, you can continue to use the `conda` command, which will now use Mamba's faster solver:

```bash
# Install packages using Conda (now with Mamba solver)
conda install package_name

# Create environments (now faster with Mamba solver)
conda create -n newenv python=3.9
```

If you want to use Mamba directly, you can still do so:

```bash
# Use Mamba directly
mamba install package_name
```

This setup allows you to benefit from Mamba's speed while maintaining the familiar Conda workflow.

## Installing Software with CMake

For software that uses CMake, you can install it in your user directory:

```bash
# Clone the repository
git clone https://github.com/example/software.git
cd software

# Create a build directory
mkdir build && cd build

# Configure CMake to install in your home directory
cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/.local

# Build and install
make
make install
```

## User Directory Structure

Create bin and lib directories in your home folder:

```bash
mkdir -p $HOME/.local/{bin,lib}
```

Add the following to your `.bashrc` or `.bash_profile`:

```bash
export PATH="$HOME/.local/bin:$PATH"
export LD_LIBRARY_PATH="$HOME/.local/lib:$LD_LIBRARY_PATH"
```

## Using Shared Modules

Shared modules allow you to load pre-installed software packages.

### Basic Module Commands

```bash
# List available modules
module avail

# Load a module
module load module_name

# Unload a module
module unload module_name

# List loaded modules
module list
```

### MathLab Modules

MathLab provides its own set of modules. To use them:

```bash
# Add MathLab modules to the module path
module use /opt/contrib/mathlab/modules

# Load a specific MathLab module, e.g., OpenFOAM
module load openfoam/2106
```

This allows you to access MathLab-specific software and tools that are pre-installed on the system.

## Using Spack

Spack is a flexible package manager for HPC environments. To use Spack:

```bash
# Clone Spack repository
git clone https://github.com/spack/spack.git

# Set up Spack environment
. /path/to/spack/share/spack/setup-env.sh

# Install a package
spack install package_name
```

## Using Flatpak for Non-Scientific Applications

Flatpak is a useful tool for installing and managing non-scientific applications without root access. Here's how to set it up and use it:

```bash
# Install Flatpak for your user (if not already available)
wget https://dl.flathub.org/repo/flathub.flatpakrepo
flatpak --user remote-add --if-not-exists flathub flathub.flatpakrepo

# Install an application
flatpak --user install flathub com.application.Name

# Run the installed application
flatpak run com.application.Name

# Update all installed Flatpak applications
flatpak --user update

# List installed Flatpak applications
flatpak --user list
```

Flatpak allows you to install a wide range of desktop applications, productivity tools, and development environments without requiring system-wide installation privileges.

## Extracting and Using RPM Packages

When you need to use software from an RPM package without system-wide installation, you can extract its contents and use them directly. Here's an expanded guide on working with extracted RPM packages:

```bash
# Extract the RPM package
rpm2cpio ../package.rpm | cpio -idmv
```

After extraction, you'll typically find a directory structure similar to the following:

- `usr/bin/`: Contains executable files
- `usr/lib/`: Contains library files
- `usr/share/`: Contains shared data, documentation, etc.

To use the extracted software:

```bash
# Add the bin directory to your PATH
export PATH="$PWD/usr/bin:$PATH"

# Add the lib directory to your LD_LIBRARY_PATH (if needed)
export LD_LIBRARY_PATH="$PWD/usr/lib:$LD_LIBRARY_PATH"

# Run the extracted executable
./usr/bin/executable_name
```

For a more permanent setup, you can move the extracted contents to your user's local directories:

```bash
# Move executables
mv usr/bin/* $HOME/.local/bin/

# Move libraries (if needed)
mv usr/lib/* $HOME/.local/lib/

# Move shared data (if needed)
mv usr/share/* $HOME/.local/share/
```

Remember to update your `.bashrc` or `.bash_profile` to include these directories in your PATH and LD_LIBRARY_PATH as shown in the "User Directory Structure" section.
