# Quad-SDK-Setup-Guide

## Building the package
1. Create catkin workspace.
```bash
mkdir -p catkin_ws/src
cd ~/catkin_ws/src
```
2. Get HSL solver for IPOPT from http://hsl.rl.ac.uk/ipopt and unzip (**This guide used Personal version release 2021.05.05 included in this repo, the faster academic version needs to be tested**)
3. Build HSL using the coin-or-tools (command below assumes that you unziped the HSL solver in the src folder): 
```bash
git clone git@github.com:coin-or-tools/ThirdParty-HSL.git
cd ThirdParty-HSL
sudo cp -r ../coinhsl-archive-2021.05.05/ ./coinhsl/
sudo ./configure
sudo make install
cd ..
```
**Note:** that this step might be redundant as the setup script in the QuadSDK repo should do this step for you, however as of 2022-10-11, this step is necessary for the building of the package.  
4. clone and setup the quad-sdk repo:
```bash
git clone git@github.com:robomechanics/quad-sdk.git -b noetic_devel
cd ./quad-sdk
cp -r ../coinhsl-archive-2021.05.05/ ./external/ipopt/coinhsl
chmod +x setup.sh
sudo ./setup.sh
cd ../..
catkin build
```
5. If you are getting the "cannot find ipopt" error, try building the library manually by:#
```bash
cd ~/catkin_ws/src/quad-sdk/external/ipopt/coinbrew/Ipopt/
sudo ./configure
sudo make install
```
then ```catkin build``` again
