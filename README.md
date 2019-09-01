# EC2 Management Script

A script to manage EC2 instances, in particular, start/stop an instance, update the instance's SG ingress rules
with your IP, and get the instance's state.

## Configuration File

In order to use the script, you must populate a configuration file called `.servers`.

```
[rdp_server]
instance_id = <instance id>
region = ca-central-1
sg_id = <sg id>
range_description = Christopher home

[vpn_server]
instance_id = <instance id>
region = us-east-2
sg_id = <sg id>
range_description = Christopher
```
 
## Actions
 
### `start`
 
Start the instance, wait for it to go into `running` state, and then output
the public DNS name.
 
```bash
./server start rdp_server
ec2-<ip>.ca-central-1.compute.amazonaws.com
```
 
### `stop`
 
Stop the instance.

```bash
./server stop rdp_server
```
 
### `get_state`
 
Output the instance's current state.
 
```bash
./server get_state rdp_server
stopped
```
 
### `update_sg_ingress`

Scope all IP ranges with a description matching the description in the configuration file
to your current public IP address.
 
```bash
./server update_sg_ingress vpn_server
[
    {
        "FromPort": 1701,
        "IpProtocol": "udp",
        "IpRanges": [
            {
                "CidrIp": "<some other ip>/32",
                "Description": "Amy"
            },
            {
                "CidrIp": "<my ip>/32",
                "Description": "Christopher"
            }
        ],
        "Ipv6Ranges": [],
        "PrefixListIds": [],
        "ToPort": 1701,
        "UserIdGroupPairs": []
    },
    {
        "FromPort": 4500,
        "IpProtocol": "udp",
        "IpRanges": [
            {
                "CidrIp": "<some other ip>/32",
                "Description": "Amy"
            },
            {
                "CidrIp": "<my ip>/32",
                "Description": "Christopher"
            }
        ],
        "Ipv6Ranges": [],
        "PrefixListIds": [],
        "ToPort": 4500,
        "UserIdGroupPairs": []
    },
    {
        "FromPort": 500,
        "IpProtocol": "udp",
        "IpRanges": [
            {
                "CidrIp": "<some other ip>/32",
                "Description": "Amy"
            },
            {
                "CidrIp": "<my ip>/32",
                "Description": "Christopher"
            }
        ],
        "Ipv6Ranges": [],
        "PrefixListIds": [],
        "ToPort": 500,
        "UserIdGroupPairs": []
    }
]
```
 