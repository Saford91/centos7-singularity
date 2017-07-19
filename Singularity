# Bootstrap definition to install minimal CentOS 7 with Python virtaulenv
 
BootStrap: docker
From: centos:latest
 
%files
 
%post
################################################################################
# Make sure the umask is 0022
################################################################################
umask 0022

################################################################################
# Equal to Base package group
################################################################################
#yum -y install \
#  acl \
#  at \
#  attr \
#  authconfig \
#  bc \
#  bind-utils \
#  centos-indexhtml \
#  cpio \
#  crda \
#  crontabs \
#  cyrus-sasl-plain \
#  dbus \
#  ed \
#  file \
#  logrotate \
#  lsof \
#  man-db \
#  net-tools \
#  ntsysv \
#  pciutils \
#  psacct \
#  quota \
#  setserial \
#  traceroute \
#  usb_modeswitch \
#  abrt-addon-ccpp \
#  abrt-addon-python \
#  abrt-cli \
#  abrt-console-notification \
#  bash-completion \
#  blktrace \
#  bridge-utils \
#  bzip2 \
#  chrony \
#  cryptsetup \
#  dmraid \
#  dosfstools \
#  ethtool \
#  fprintd-pam \
#  gnupg2 \
#  hunspell \
#  hunspell-en \
#  kpatch \
#  ledmon \
#  libaio \
#  libreport-plugin-mailx \
#  libstoragemgmt \
#  lvm2 \
#  man-pages \
#  man-pages-overrides \
#  mdadm \
#  mlocate \
#  mtr \
#  nano \
#  ntpdate \
#  pinfo \
#  plymouth \
#  pm-utils \
#  rdate \
#  rfkill \
#  rng-tools \
#  rsync \
#  scl-utils \
#  setuptool \
#  smartmontools \
#  sos \
#  sssd-client \
#  strace \
#  sysstat \
#  systemtap-runtime \
#  tcpdump \
#  tcsh \
#  teamd \
#  time \
#  unzip \
#  usbutils \
#  vim-enhanced \
#  virt-what \
#  wget \
#  which \
#  words \
#  xfsdump \
#  xz \
#  yum-langpacks \
#  yum-utils \
#  zip
 
################################################################################
# Tools for development
################################################################################
#yum -y groupinstall 'Development Tools'

################################################################################
# Install additional packages to support development. The hostname package is
# normally installed with the 'Core' package group, however, because most
# packages in 'Core' are not useful in a container (rsyslog, sudo, systemd,
# etc.) , it is installed here. The zsh shell is installed for the hand full
# of user that use it.
################################################################################
#yum -y install \
#  hostname \
#  tcl \
#  tk \
#  perl \
#  gcc-gfortran \
#  vim \
#  zsh
 
# Install the same version of Nvidia drivers that is in use on compute nodes.
#yum -y install xorg-x11-drv-nvidia-libs

#rpm -ivh https://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/cuda-repo-rhel7-8.0.61-1.x86_64.rpm
 
################################################################################
# Install PIP for Python 2.7 and 3.4
################################################################################
yum -y install python34-pip python-pip

################################################################################
# Upgrade PIP to the latest version and intall virtualenv
################################################################################
pip  install --upgrade pip
pip3 install --upgrade pip
pip  install --upgrade virtualenv
 
################################################################################
# Create directories to enable accessing common HPCC mount points
################################################################################
for dir in /cvmfs /mnt/{home,research,{d,f}fs17,ls15,local} /opt/software; do
  test -d $dir || mkdir $dir
done
 
################################################################################
# Change the command prompt to show that we are in a container
################################################################################
cat > /etc/profile.d/z_container_prompt.sh <<EOF
export PS1="(CentOS7.3)\$PS1"
EOF
 
cat > /etc/profile.d/z_container_prompt.csh <<EOF
setenv PS1 "(CentOS7.3)\$PS1"
EOF
 

################################################################################
# Run the user's login shell, or a user specified command
################################################################################
%runscript
SHELL="$(getent passwd $USER | awk -F: '{print $NF}')"
SHELL=${SHELL:-/bin/bash}
if [[ "$@" == "" ]]; then 
  exec env -i TERM="$TERM" HOME="$HOME" $SHELL -l
else
  exec env -i TERM="$TERM" HOME="$HOME" $@
fi
