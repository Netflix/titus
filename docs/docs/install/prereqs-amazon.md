# Creating security groups

Two security groups are needed. We are naming them `titusapp` and `titusmaster-mainvpc`:

<img src="../../images/secgroups.png" />

# For the infrastructure security group (titusmaster-mainvpc)

For inbound
- From titusmaster-mainvpc security group, ALL TCP, All ICMP
- From anywhere (including Internet), SSH

<img src="../../images/titusmaster-mainvpc-secgroup-inbound.png" />

For outbound
- All traffic

<img src="../../images/titusmaster-mainvpc-secgroup-outbound.png" />





