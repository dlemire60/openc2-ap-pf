## Schema
|                . | .                                                                                            |
|-----------------:|:---------------------------------------------------------------------------------------------|
|     **package:** | https://github.com/oasis-tcs/openc2-ap-pf                                                    |
|     **version:** | 0-wd02                                                                                       |
|       **title:** | Packet Filtering Actuator Profile                                                            |
| **description:** | Data definitions for Packet Filtering (PF) functions. Formal definition of IM for the PF AP. |
|  **namespaces:** | **ls**: http://oasis-open.org/openc2/oc2ls-types/v1.1                                        |
|     **exports:** | PF-Target, PF-Args                                                                           |

**_Type: Actions (Enumerated)_**

| ID | Name       | Description                                                                                                                        |
|---:|:-----------|:-----------------------------------------------------------------------------------------------------------------------------------|
|  3 | **query**  | Initiate a request for information. Used to communicate the supported options and determine the state or settings of the Actuator. |
|  6 | **deny**   | Prevent traffic or access                                                                                                          |
|  8 | **allow**  | Permit traffic or access                                                                                                           |
| 16 | **update** | Instruct the Actuator to update its configuration by retrieving and processing a configuration file                                |
| 20 | **delete** | Remove an access rule.                                                                                                             |

**_Type: Target (Choice)_**

|   ID | Name                | Type               | # | Description                                                                                                                                                                                  |
|-----:|:--------------------|:-------------------|--:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    9 | **features**        | ls:Feature         | 1 | A set of items such as Action/Target pairs, profiles versions, options that are supported by the Actuator. The Target is used with the query Action to determine an Actuator's capabilities. |
|   10 | **file**            | ls:File            | 1 | Properties of a file.                                                                                                                                                                        |
|   13 | **ipv4_net**        | ls:IPv4-Net        | 1 | The representation of one or a block of IPv4 addresses expressed using CIDR notation.                                                                                                        |
|   14 | **ipv6_net**        | ls:IPv6-Net        | 1 | The representation of one or a block of IPv6 addresses expressed using CIDR notation.                                                                                                        |
|   15 | **ipv4_connection** | ls:IPv4-Connection | 1 | A network connection as specified by a five-tuple (IPv4).                                                                                                                                    |
|   16 | **ipv6_connection** | ls:IPv6-Connection | 1 | A network connection as specified by a five-tuple (IPv6).                                                                                                                                    |
|   17 | **domain_name**     | ls:Domain-Name     | 1 | A domain name as defined in [RFC1034].                                                                                                                                                       |
| 1034 | **pf**              | String             | 1 |                                                                                                                                                                                              |

**_Type: Pairs (Enumerated)_**

| ID | Name                                                                                              | Description |
|---:|:--------------------------------------------------------------------------------------------------|:------------|
|  3 | **query: features, /rule_number**                                                                 |             |
|  6 | **deny: ipv4_connection, ipv6_connection, ipv4_net, ipv6_new, domain_name, /advanced_connection** |             |
|  8 | **allow:**                                                                                        |             |
| 16 | **update: file**                                                                                  |             |
| 30 | **delete: /rule_number**                                                                          |             |

**_Type: Results (Map{1..*})_**

|   ID | Name           | Type              | # | Description                                                          |
|-----:|:---------------|:------------------|--:|:---------------------------------------------------------------------|
|    1 | **versions**   | ls:Version        | 1 | List of OpenC2 versions supported by this actuator                   |
|    2 | **profiles**   | ArrayOf(Nsid)     | 1 | List of profiles supported by this Actuator.                         |
|    3 | **pairs**      | ls:Action-Targets | 1 | List of targets applicable to each supported Action.                 |
|    4 | **rate_limit** | Number{0..*}      | 1 | Maximum number of requests per minute supported by design or policy. |
| 1034 | **pf**         | PR-Results        | 1 |                                                                      |

**_Type: PF-Target (Choice)_**

|   ID | Name               | Type                | # | Description                                                                                                                                           |
|-----:|:-------------------|:--------------------|--:|:------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1024 | **rule_number**    | Rule-ID             | 1 | Immutable identifier assigned when a packet filtering rule is created. Identifies the rule to be deleted or used to request information about a rule. |
| 1025 | **adv_connection** | Advanced-Connection | 1 | Advanced connection type to support application layer firewalls                                                                                       |


| Type Name   | Type Definition | Description |
|:------------|:----------------|:------------|
| **Rule-ID** | Integer{0..*}   |             |

**_Type: Advanced-Connection (Record)_**

| ID | Name            | Type              | # | Description                                                                              |
|---:|:----------------|:------------------|--:|:-----------------------------------------------------------------------------------------|
|  1 | **src_addr**    | Adv-Addr          | 1 | Source address range, one of IPv4, IPv6, or network tag                                  |
|  2 | **src_port**    | Integer{0..65536} | 1 | Source service per [[RFC6335]](#rfc6335)                                                 |
|  3 | **dst_addr**    | Adv-Addr          | 1 | Destination address range, one of IPv4, IPv6, or network tag                             |
|  4 | **dst_port**    | Integer{0..65535} | 1 | Destination service per [[RFC6335]](#rfc6335)                                            |
|  5 | **protocol**    | ls:L4-Protocol    | 1 | Layer 4 protocol (e.g., TCP) - see Section 3.4.2.11 of the OpenC2 Language Specification |
|  6 | **network**     | String            | 1 | Reference to the name (also known as tag) of logical network to which the rule applies   |
|  7 | **application** | String            | 1 | Reference to the name of the application to which the rule applies                       |

**_Type: Adv-Addr (Choice)_**

| ID | Name        | Type        | # | Description                                                        |
|---:|:------------|:------------|--:|:-------------------------------------------------------------------|
|  1 | **v4addr**  | ls:IPv4-Net | 1 | IPv4 CIDR block address as defined in the OpenC2 LS                |
|  2 | **v6addr**  | ls:IPv6-Net | 1 | IPv6 "CIDR block" address as defined in the OpenC2 LS              |
|  3 | **net_tag** | String      | 1 | A network  name, e.g., as used in cloud system network definitions |

**_Type: Args (Map{1..*})_**

|   ID | Name                   | Type             | # | Description                                       |
|-----:|:-----------------------|:-----------------|--:|:--------------------------------------------------|
|    1 | **start_time**         | ls:Date-Time     | 1 | The specific date/time to initiate the Command.   |
|    2 | **stop_time**          | ls:Date-Time     | 1 |     The specific date/time to terminate the Command. |
|    3 | **duration**           | ls:Duration      | 1 |                                                   |
|    4 | **response_requested** | ls:Response-Type | 1 |                                                   |
| 1034 | **pf**                 | PF-ARgs          | 1 |                                                   |

**_Type: PF-Args (Map{1..*})_**

|   ID | Name             | Type          | # | Description                                                                                                                                                                                                   |
|-----:|:-----------------|:--------------|--:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1024 | **drop_process** | Drop-Process  | 1 | Specifies how to handle denied packets                                                                                                                                                                        |
| 1025 | **persistent**   | Boolean       | 1 | Normal operations assume any changes to a device are to be implemented persistently. Setting the persistent modifier to FALSE results in a change that is not persistent in the event of a reboot or restart. |
| 1026 | **direction**    | Direction     | 1 | Specifies whether to apply rules to incoming or outgoing traffic. If omitted, rules are applied to ingress packets.                                                                                           |
| 1027 | **insert_rule**  | Rule-ID       | 1 | Specifies the identifier of the rule within a list, typically used in a top-down rule list.                                                                                                                   |
| 1028 | **logged**       | Boolean       | 1 | Specifies if a log entry should be recorded as traffic matches the rule. The manner and mechanism for recording these entries is implementation specific and not defined by this specification.               |
| 1029 | **description**  | String        | 1 | A note to annotate or provide information regarding the rule.                                                                                                                                                 |
| 1030 | **stateful**     | Boolean       | 1 | Specifies if the actuator should treat the request using state tables or connection state.                                                                                                                    |
| 1031 | **priority**     | Integer{0..*} | 1 | Specifies the priority of a specific firewall rule for firewalls that assign a numeric priority. It is used to determine which firewall rule takes precedence.                                                |

**_Type: Drop-Process (Enumerated)_**

| ID | Name          | Description                                                                                    |
|---:|:--------------|:-----------------------------------------------------------------------------------------------|
|  1 | **none**      | Drop the packet and do not send a notification to the source of the packet.                    |
|  2 | **reject**    | Drop the packet and send an ICMP host unreachable (or equivalent) to the source of the packet. |
|  3 | **false_ack** | Drop the traffic and send a false acknowledgment.                                              |

**_Type: Direction (Enumerated)_**

| ID | Name        | Description                           |
|---:|:------------|:--------------------------------------|
|  1 | **both**    | Apply rules to all traffic.           |
|  2 | **ingress** | Apply rules to incoming traffic only. |
|  3 | **egress**  | Apply rules to outgoing traffic only. |

**_Type: PF-Results (Map{1..*})_**

| ID | Name            | Type    | # | Description                                          |
|---:|:----------------|:--------|--:|:-----------------------------------------------------|
|  1 | **rule_number** | Rule-ID | 1 | Rule identifier returned from allow or deny Command. |


| Type Name | Type Definition | Description |
|:----------|:----------------|:------------|
| **Nsid**  | String{1..16}   |             |
