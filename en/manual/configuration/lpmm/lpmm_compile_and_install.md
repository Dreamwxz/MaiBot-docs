# Manual Compilation MaiMBot-LPMM

::: tip
If you can directly install quick_algo library, then please **skip this step**, directly use compiled package.
:::

## Get MaiMBot-LPMM
```bash
git clone https://github.com/MaiM-with-u/MaiMBot-LPMM.git
```

## Windows(x86_64)

### Environment Preparation

1. First, download Microsoft MSVC build tools installation package at [C++ Build Tools](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)
2. Open installation package, will automatically install Visual Studio Installer. After installation is complete, open Visual Studio Installer
3. Select above "Individual components" tab, search and select following components:
   - MSVC v143 - VS 2022 C++ x64/x86 build tools
   - Windows 10/11 SDK (according to your computer environment selection)
   - Cmake tools for Windows
4. Click "Install" button, start installing these components
5. Download MinGW that matches your computer environment at [MinGW-w64 releases](https://github.com/niXman/mingw-builds-binaries/releases) (note the name, need msvc version)
6. Extract to any directory, add extracted directory to environment variables

### MSVC Compilation

First install dependencies in root directory `requirements.txt`

```bash
pip install -i https://mirrors.aliyun.com/pypi/simple -r requirements.txt --upgrade
```

Then enter `lib/quick_algo/` directory, use **administrator permissions** to run `python build_lib.py --cleanup --cythonize --install`

```bash
cd lib/quick_algo
sudo python build_lib.py --cleanup --cythonize --install
```
::: tip
Before running please ensure enabled Sudo on windows. If not, use administrator to open terminal
:::

## Windows(Arm)

No data

## Linux
### Environment Preparation
1. First install gcc/g++ compiler

Debian-based systems (like Ubuntu, Linux Mint, etc.)

```bash
# Update software package index
sudo apt update
# Run following command to install `gcc` and `g++`:
sudo apt install build-essential
```

Red Hat-based systems (like Fedora, CentOS, RHEL, etc.)

```bash
# Update software package index
sudo dnf check-update
# Install `gcc` and `g++`
sudo dnf install gcc gcc-c++
# If using `yum`, replace `dnf` with `yum`
```

2. Verify installation

After installation is complete, can verify if `gcc` and `g++` are installed successfully through following commands:

```bash
gcc --version
g++ --version
# If installation is successful, will display `gcc` and `g++` version information.
```

### GCC Compilation
1. Install dependencies

Enter LPMM root directory, install corresponding dependencies

```bash
pip install -i https://mirrors.aliyun.com/pypi/simple -r requirements.txt --upgrade
```

2. Compilation
Enter `lib/quick_algo/` directory, run command `python build_lib.py --cleanup --cythonize --install`

```bash
cd lib/quick_algo
python build_lib.py --cleanup --cythonize --install
```

## MacOS
### Environment Preparation
1. First install Clang compilation environment

Homebrew-based recommended method:

```bash
# Install Homebrew (if not yet installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# Install clang
brew install llvm
```

Or directly use Xcode command line tools:

```bash
xcode-select --install
```

2. After installation is complete, can verify if clang is installed successfully through following command:

```bash
clang --version
```

### Clang Compilation
1. Install dependencies
Enter LPMM root directory, install corresponding dependencies

```bash
pip install -i https://mirrors.aliyun.com/pypi/simple -r requirements.txt --upgrade
```

2. Enter `lib/quick_algo/` directory, run command `python build_lib.py --cleanup --cythonize --install`

```bash
cd lib/quick_algo
python build_lib.py --cleanup --cythonize --install
```