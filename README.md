# install-abaqus2018-on-Ubuntu22.04
Here is a cheating sheet for installing abaqus2018 on Ubuntu 22.04 OS
# Install prerequisites
sudo apt update
sudo apt install csh tcsh ksh gcc g++ gfortran libstdc++5 build-essential make libjpeg62 libmotif-dev lsb-core
# Install the cracked licence server
将SSQ_UniversalLicenseServer_Core_<release-date>.zip中的SolidSQUAD_License_Servers文件夹解压至目标文件夹(must be root folders)，例如/opt/DassaultSystemes/SolidSQUAD_License_Servers；
将SSQ_UniversalLicenseServer_Module_DSSimulia_<release-date>.zip中的Vendors文件夹解压至刚才的SolidSQUAD_License_Servers文件夹中；
运行 SolidSQUAD_License_Servers文件夹中的install_or_update.sh，等待脚本完成；
sudo sh ./install_or_update.sh
Troubleshooting
运行install_or_update.sh时报错
...
Cannot find a source for symlink to /lib/ld-lsb.so.3! Exiting...
  
# ld-linux-x86-64.so.2 is identical to ld-lsb.so.3. A symbolic link solved this problem.
sudo ln -s /lib/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 /lib/ld-lsb.so.3
Change the contents of all linux.sh file to the following
 
  The Linux.sh file (as per 2018/08/16):

Note this file contains two changes: 1) the release version was forced to be "CentOS", and 2) checking of prerequisites was disabled.
 
 DSY_LIBPATH_VARNAME=LD_LIBRARY_PATH

which lsb_release
if [[ $? -ne 0 ]] ; then
  echo "lsb_release is not found: check in the PDIR the list of installed packages for servers validation."
  exit 12
fi

DSY_OS_Release="CentOS" #Override release setting, old: DSY_OS_Release=`lsb_release --short --id |sed 's/ //g'`
echo "DSY_OS_Release=\""${DSY_OS_Release}"\""
export DSY_OS_Release=${DSY_OS_Release}
export DSY_Skip_CheckPrereq=1 #Added to avoid prerequisite check

if [[ -n ${DSY_Force_OS} ]]; then
  DSY_OS=${DSY_Force_OS}
  echo "DSY_Force_OS=\""${DSY_Force_OS}"\", use it for DSY_OS"
  return
fi

case ${DSY_OS_Release} in
    "RedHatEnterpriseServer"|"RedHatEnterpriseClient"|"RedHatEnterpriseWorkstation"|"CentOS")
        DSY_OS=linux_a64;;
    "SUSELINUX"|"SUSE")
        DSY_OS=linux_a64;;
    *)
        echo "Unknown linux release \""${DSY_OS_Release}"\""
        echo "exit 8"
        exit 8;;
esac
  
  
  
 in folder 1 of the installation file, we run
  sudo ./StartGUI.sh
  
  在安装Abaqus/CAE时， Extended Product Documentation的安装目录：/opt/DassaultSystemes/SIMULIA2019doc

Abaqus Simulation Services及Abaqus Simulation Services CAA API的安装目录：/opt/DassaultSystemes/SimulationServices/V6R2019x

Abaqus/CAE的安装目录：/opt/DassaultSystemes/SIMULIA/CAE/2019

CAE Commands的安装目录：/opt/DassaultSystemes/SIMULIA/Commands

CAE external plugins的安装目录：/opt/DassaultSystemes/SIMULIA/CAE/plugins/2019

fe-safe的安装目录：/opt/DassaultSystemes/SIMULIA/fe-safe/2019

Tosca的安装目录：/opt/DassaultSystemes/SIMULIA/Tosca/2019

Isight的安装目录：/opt/DassaultSystemes/SIMULIA/Isight/2019

可以在安装中跳过证书设置，这并不影响软件的安装。在完成安装之后，将/opt/DassaultSystemes/SimulationServices/V6R2019x/linux_a64/SMA/site/custom_v6.env中的
  
  license_server_type=DSLS
dsls_license_config="/var/DassaultSystemes/Licenses/DSLicSrv.txt"
  
  改为

license_server_type=FLEXNET
abaquslm_license_file="27800@localhost"
# license_server_type=DSLS
# dsls_license_config="/var/DassaultSystemes/Licenses/DSLicSrv.txt"

  运行Abaqus/CAE

当系统开机或重启后，证书服务器将会自动启动。手动重启服务器：

sudo ksh /opt/DassaultSystemes/SolidSQUAD_License_Servers/install_or_update.sh
以管理员身份使用以下命令来启动Abaqus/CAE：

/opt/DassaultSystemes/SIMULIA/Commands/abaqus cae

  或

/opt/DassaultSystemes/SIMULIA/Commands/abq2018 cae
  
  
  
  Make abaqus command available from any directory:

sudo ln /opt/SIMULIA/Commands/abaqus /usr/bin/abaqus

If there is any warning regarding OpenGL in the terminal during start,
simply run Abaqus with -mesa parameter:

abaqus cae -mesa
abaqus view -mesa

  
  To use gfortran overwrite:

/opt/DassaultSystemes/SimulationServices/V6R2019x/linux_a64/SMA/site/lnx86_64.env

with this file. Flag for free form Fortran is active. It works at least for gfortran 9.3.0.

  
  
  
