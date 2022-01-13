# a12s-twrp
TWRP for a12s
=============



git config --global user.email "your email"
git config --global user.name "your name"



Basic Device Tree
==================

mkdir TWRP
cd TWRP
git init
git clone https://github.com/twrpdtgen/twrpdtgen
pip3 install twrpdtgen
sudo apt install cpio
copy stock recovery.img to ~/TWRP/twrpdtgen

cd twrpdtgen
python3 -m twrpdtgen recovery.img

rename ~/TWRP/twrpdtgen/output/samsung/a12s/omni_a12s.mk to twrp_a12s.mk

delete vendorsetup.sh
Make a copy of recovery.fstab and rename to twrp.flags
Move both files recovery.fstab and twrp.flags to recovery/root/system/etc
(create these folders)

open twrp_a12s.mk and 
1. delete the 3 lines below # Inherit from those products, Most specific first.
replace those 3 lines with this
$(call inherit-product, $(SRC_TARGET_DIR)/product/aosp_base.mk)

2. delete $(call inherit-product, vendor/omni/config/gsm.mk)
3. change omni to twrp (2 places)

open AndroidProducts.mk
change omni to twrp (4 spots)

Optional: add extra items to BoardConfig.mk


cd ~
sudo apt install git aria2 -y
git clone https://gitlab.com/OrangeFox/misc/scripts
cd scripts
sudo bash setup/android_build_env.sh
sudo bash setup/install_android_sdk.sh


• Make a directory called ~/a12s-twrp
cd ~/a12s-twrp

repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-11


repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j1

copy ~/TWRP/twrpdtgen/output/samsung to /a12s-twrp/device


Building
========


. build/envsetup.sh
lunch twrp_a12s-eng
mka recoveryimage –j1

Recovery.img at out/Samsung/a12s/...img



