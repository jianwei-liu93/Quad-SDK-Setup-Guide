# Quad-SDK-Setup-Guide

## Building the package
1. Create catkin workspace.
```bash
mkdir -p catkin_ws/src
cd ~/catkin_ws/src
```
2. Download the HSL solver for IPOPT and unzip
 - Both can be found on this repo: [Academic Version](./coinhsl/Academic/) (faster but with license limitations) or [Personal version](./coinhsl/Personal/) 
 - Alternatively the latest version can be found from http://hsl.rl.ac.uk/ipoptand 
3. Build HSL using the coin-or-tools (command below assumes that you unziped the HSL solver in the src folder): 
- clone the coin-or-tools:
```bash
git clone git@github.com:coin-or-tools/ThirdParty-HSL.git
cd ThirdParty-HSL
```
- For academic version:
```bash 
sudo cp -r ../coinhsl-2021.05.05/ ./coinhsl/
```
- For Personal version:
```bash 
sudo cp -r ../coinhsl-archive-2021.05.05/ ./coinhsl/
```
- compile and install the HSL library:
```bash
sudo ./configure
sudo make install
cd ..
```
**Note:** that this step might be redundant as the setup script in the QuadSDK repo should do this step for you, however as of 2022-10-11, this step is necessary for the building of the package.  

4. clone and setup the quad-sdk repo:
- setting up the repo with dependacies:
```bash
git clone git@github.com:robomechanics/quad-sdk.git -b noetic_devel
cd ./quad-sdk
```
- Copy in the HSL solver:
  - For Academic Version of HSL:
  ```bash
  cp -r ../coinhsl-2021.05.05/ ./external/ipopt/coinhsl
  ```

  - For Free Version of HSL:
  ```bash
  cp -r ../coinhsl-archive-2021.05.05/ ./external/ipopt/coinhsl
  ```
- Build dependancies:
```bash
chmod +x setup.sh
sudo ./setup.sh
cd ../..
```
- Check that the correct version of the ```linear_solver``` is selected:
Go to line 208 of ```nmpc_controller/src/nmpc_controller.cpp``` and change the ```linear_solver``` to:
  - ```ma57``` if using the Academic version of HSL
  - ```ma27``` if using the Free version of HSL
- Build the ROS packages:
```bash
catkin build
```
5. If you are getting the "cannot find ipopt" error, try building the library manually by:
```bash
cd ~/catkin_ws/src/quad-sdk/external/ipopt/coinbrew/Ipopt/
sudo ./configure
sudo make install
```
then ```catkin build``` again
