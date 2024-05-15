STATUS!=vagrant status --machine-readable | grep ",state," | cut -d',' -f4
USERNAME?=shell whoami

install: customize
	vagrant up --no-provision
	vagrant provision --provision-with basic-root-operation
	vagrant reload
	vagrant provision --provision-with needed-package-manager
	vagrant provision --provision-with grow-partition
	vagrant provision --provision-with install-all-package

destroy:
	vagrant destroy -f
	vagrant box update

fresh-install: destroy install

start:
ifeq ($(STATUS), not_created)
	@echo "Use make install before"
else
	vagrant up
endif

pause-unpause:
ifeq ($(STATUS), running)
	@echo "Suspending your VM"
	vagrant suspend
else ifeq ($(STATUS), saved)
	@echo "Resuming your VM"
	vagrant resume
else
	@echo "You must start or create your VM before using 'pause-unpause'"
endif

customize:
	@sed -i "s/^    export LOCAL_USER=.*$$/    export LOCAL_USER=$(USERNAME)/g" Vagrantfile
