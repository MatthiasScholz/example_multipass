base := impish
server := backstage

# vm stats
size_cpu := 1
size_disk := 10G
size_memory := 2G
# NOTE: ONLY cpu and memory can be updated after creation:
#       /var/root/Library/Application Support/multipassd/multipassd-vm-instances.json

launch:
	multipass launch -n $(server) --cpus $(size_cpu) --mem $(size_memory) --disk $(size_disk) --cloud-init cloud-config.yml $(base)
	multipass info $(server)

stop:
	multipass stop $(server)

rebuild: stop cleanup launch shell

start:
	multipass start $(server)

cleanup:
	multipass delete $(server)
	multipass purge

list:
	multipass list

shell:
	multipass shell $(server)

multipass-stop:
	sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist

multipass-start:
	sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist
