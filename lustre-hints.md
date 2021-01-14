To reset lnet (defined in /etc/lnet.conf):

    (umount -t lustre --all)
    lctl network down
    lustre_rmmod
    modprobe lnet -v
    lctl network up
