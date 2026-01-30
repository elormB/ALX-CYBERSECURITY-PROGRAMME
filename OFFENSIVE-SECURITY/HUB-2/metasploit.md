msf6 > search auxiliary
Matching Modules:
  auxiliary/scanner/portscan/tcp
  auxiliary/scanner/ftp/ftp_version
  auxiliary/scanner/http/http_version
  auxiliary/scanner/ssh/ssh_version
  auxiliary/dos/tcp/synflood
msf6 > use auxiliary/scanner/ftp/ftp_version
Loaded module: auxiliary/scanner/ftp/ftp_version
msf6 > set RHOSTS 192.168.56.100-105
Set RHOSTS => 192.168.56.100-105
msf6 > run
No FTP service detected or connection failed.