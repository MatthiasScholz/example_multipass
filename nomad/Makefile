base := impish
server := nomad
start:
	multipass launch -n $(server) --cloud-init cloud-config.yml $(base)
	multipass info $(server)

stop:
	multipass stop $(server)

rebuild: stop cleanup start shell

cleanup:
	multipass delete $(server)
	multipass purge

list:
	multipass list

shell:
	multipass shell $(server)
