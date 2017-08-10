# unity-connection-tester
A modified version of Unity's Connection Tester, original can be found at https://unity3d.com/master-server.

Motivation
--
I needed to host my own "NAT Type Detection" service on Amazon AWS using an EC2 instance without 4 seperate public NICs. The standard Connection Tester by Unity doesn't provide any kind of IP aliasing.
Using this version I was able to set up a `t2.small` instance with two NICs, each two private IPs and 4 Elastic IPs assigned to the private ones.

Changes
--
- Fixed various syntax errors
- Added optional IP alias for the third external IP parameter
- Upgraded Visual Studio project to VS 2015
- Extended the service file functionality a bit

Usage
-- 

You can run the Connection Tester without any arguments, this will use the first ethernet adapter as the main listen address and it try to find 3 other IPs available for external binding.
```
./ConnTester
```

If you want to be more in control as to which IP will be used, you can use the `-h` and `-b` parameters like so:

```
./ConnTester -h 86.91.40.13 -b 86.91.100.50 86.91.100.51 86.91.100.52
```

If your NICs are behind a NAT like mine, you can simply bind the private IPs, and alias the external IP address. You still need to assign the public IP's to your private ones in your AWS console.

```
./ConnTester -h 172.50.60.70 -b 172.50.60.90 172.50.60.91 86.91.100.51 172.50.60.92
```


Accepted parameters are:
```
        -p      Listen port (1-65535)
        -d      Daemon mode, run in the background
        -l      Use given log file
        -e      Debug level (0=OnlyErrors, 1=Warnings, 2=Informational(default), 2=FullDebug)
        -c      Connection count
        -h      Bind to listen address
        -b      Bind to three external test addresses. Note: if the external IPs are behind a NAT, please use the optional publicIpAddress parameter on the second IP address.
                Usage: -b IPAddress1 IPAddress2 <publicIPAddress2> IPAddress3
```

NOTE
--
If you are using AWS, make sure you add all 'allow all UDP traffic' to the security group.
