Bootstrap: shub
From: willgpaik/centos7_aci

%environment
  PATH="/usr/local/bin/:$PATH:/usr/lib64/openmpi/bin/"
	LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib64/openmpi/lib/"
	MPI_ROOT=/usr/lib64/openmpi/
	export PATH
	export LD_LIBRARY_PATH
	export MPI_ROOT
	export BOOST_ROOT=/usr/local/
	source /opt/rh/devtoolset-8/enable
  
  export R_LIBS=/usr/R:$R_LIBS
  export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
  export PATH=/usr/local/bin:$PATH
  export PATH=/usr/abin:$PATH
  export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH
  export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH

%post
	PATH="/usr/local/bin/:$PATH:/usr/lib64/openmpi/bin/"
	LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib64/openmpi/lib/"
	MPI_ROOT=/usr/lib64/openmpi/
	export PATH
	export LD_LIBRARY_PATH
	export MPI_ROOT
	export BOOST_ROOT=/usr/local/
	source /opt/rh/devtoolset-8/enable

	yum -y update
	yum -y install tcsh libXp openmotif gsl xorg-x11-fonts-misc \
		PyQt4 R-devel netpbm-progs gnome-tweak-tool ed \
		libpng12 xorg-x11-server-Xvfb firefox \
    mesa-libGLw-devel

	mkdir -p /usr/abin

	export HOME=/usr
	export PATH=/usr/abin:$PATH

	curl -O https://afni.nimh.nih.gov/pub/dist/bin/misc/@update.afni.binaries
	tcsh @update.afni.binaries -package linux_centos_7_64 -do_extras

	cp $HOME/abin/AFNI.afnirc $HOME/.afnirc
	suma -update_env

	export R_LIBS=$HOME/R
	mkdir $R_LIBS
	echo 'export R_LIBS=$HOME/R' >> ~/.bashrc
	echo  'setenv R_LIBS ~/R'     >> ~/.cshrc
	source ~/.bashrc

	rPkgsInstall -pkgs ALL

	cp $HOME/abin/AFNI.afnirc $HOME/.afnirc
	suma -update_env

	curl -O https://afni.nimh.nih.gov/pub/dist/edu/data/CD.tgz
	tar xvzf CD.tgz
	cd CD
	tcsh s2.cp.files . ~
	cd ..
	rm CD.tgz

	afni_system_check.py -check_all

	@afni_R_package_install -shiny -circos

	@update.afni.binaries -d
