qemu_x86
Use apt to install Python venv package:

sudo apt install python3-venv

Create a new virtual environment:

python3 -m venv ~/zephyrproject/.venv

Activate the virtual environment:

source ~/zephyrproject/.venv/bin/activate

Once activated your shell will be prefixed with (.venv). The virtual environment can be deactivated at any time by running deactivate.

Note

# Remember to activate the virtual environment every time you start working.

## Install west:

pip install west

## Get the Zephyr source code:

west init ~/zephyrproject
cd ~/zephyrproject
west update

## Export a Zephyr CMake package. This allows CMake to automatically load boilerplate code required for building Zephyr applications.

west zephyr-export

## Zephyr’s scripts/requirements.txt file declares additional Python dependencies. Install them with pip.

pip install -r ~/zephyrproject/zephyr/scripts/requirements.txt

### If using an Ubuntu version older than 22.04, it is necessary to add extra repositories to meet the minimum required versions for the main dependencies listed above. In that case, download, inspect and execute the Kitware archive script to add the Kitware APT repository to your sources list. A detailed explanation of kitware-archive.sh can be found here kitware third-party apt repository:

wget https://apt.kitware.com/kitware-archive.sh
sudo bash kitware-archive.sh

### Use apt to install the required dependencies:

sudo apt install --no-install-recommends git cmake ninja-build gperf \
  ccache dfu-util device-tree-compiler wget \
  python3-dev python3-pip python3-setuptools python3-tk python3-wheel xz-utils file \
  make gcc gcc-multilib g++-multilib libsdl2-dev libmagic1

### Verify the versions of the main dependencies installed on your system by entering:

cmake --version
python3 --version
dtc --version

# Install the Zephyr SDK¶

### The Zephyr Software Development Kit (SDK) contains toolchains for each of Zephyr’s supported architectures, which include a compiler, assembler, linker and other programs required to build Zephyr applications.

### It also contains additional host tools, such as custom QEMU and OpenOCD builds that are used to emulate, flash and debug Zephyr applications.

# Note

You can change 0.16.3 to another version in the instructions below if needed; the Zephyr SDK Releases page contains all available SDK releases.

# Note

## If you want to uninstall the SDK, you may simply remove the directory where you installed it.

    Download and verify the Zephyr SDK bundle:

    cd ~
    wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.3/zephyr-sdk-0.16.3_linux-x86_64.tar.xz
    wget -O - https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.3/sha256.sum | shasum --check --ignore-missing

If your host architecture is 64-bit ARM (for example, Raspberry Pi), replace x86_64 with aarch64 in order to download the 64-bit ARM Linux SDK.

## Extract the Zephyr SDK bundle archive:

tar xvf zephyr-sdk-0.16.3_linux-x86_64.tar.xz

# Note

## It is recommended to extract the Zephyr SDK bundle at one of the following locations:

    $HOME

    $HOME/.local

    $HOME/.local/opt

    $HOME/bin

    /opt

    /usr/local

## The Zephyr SDK bundle archive contains the zephyr-sdk-0.16.3 directory and, when extracted under $HOME, the resulting installation path will be $HOME/zephyr-sdk-0.16.3.

## Run the Zephyr SDK bundle setup script:

cd zephyr-sdk-0.16.3
./setup.sh

# Note

## You only need to run the setup script once after extracting the Zephyr SDK bundle.

You must rerun the setup script if you relocate the Zephyr SDK bundle directory after the initial setup.

## Install udev rules, which allow you to flash most Zephyr boards as a regular user:

sudo cp ~/zephyr-sdk-0.16.3/sysroots/x86_64-pokysdk-linux/usr/share/openocd/contrib/60-openocd.rules /etc/udev/rules.d
sudo udevadm control --reload




# clean the build directory.
(~/zephyrproject/zephyr$) rm -rf build

# Use the west build command to create a new application named morse_code_app based on the hello_world 
(~/zephyrproject/zephyr$) west build -b qemu_x86 samples/hello_world

# Additionally, create a new directory for your application and copy the hello_world sample code into it.
(~/zephyrproject/zephyr$) cp -r samples/hello_world morse_code_app

# Simulate the application using QEMU.
(~/zephyrproject/zephyr$)  west build -t run
