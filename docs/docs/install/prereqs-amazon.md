# Creating security groups

Two security groups are needed. We are naming them `titusapp` and `titusmaster-mainvpc`:

<img src="../../images/secgroups.png" />

## Infrastructure security group

This is for the `titusmaster-mainvpc` security group

For inbound
- From titusmaster-mainvpc security group, ALL TCP, All ICMP
- From anywhere (including Internet), SSH

<img src="../../images/titusmaster-mainvpc-secgroup-inbound.png" />

For outbound
- All traffic

<img src="../../images/titusmaster-mainvpc-secgroup-outbound.png" />

## App security group

This is for the `titusapp` 

For inbound and outbound
- Up to your application needs

# Creating IAM Roles



