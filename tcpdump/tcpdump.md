Examples of traffic inspection
==============================

-   Inspect packets with that go to port 2222:

        $ tcpdump -nnXSs 0 'port 2222'

-   Get only icmp on iface eth0:

        $ tcpdump -nni eth0 icmp

-   Get only icmp echoes on eth0:

        $ tcpdump -nni eth0 icmp[icmptype] == 8

-   Get HTTP requests and headers:

        $ tcpdump -i YOUR_IFACE -A -vvs 10240 'tcp port 5000' | grep -v IP | egrep --line-buffered "..(GET |\.HTTP\/|POST |HEAD )|^[A-Za-z0-9-]+: " |sed -r 's/..(GET |HTTP\/|POST |HEAD )/\n\n\1/g'

