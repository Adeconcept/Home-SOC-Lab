# Network Traffic Monitoring and Attack Detection

## Executive Summary

This project demonstrates the investigation of network activity using Wireshark on a MacBook M1. I captured a controlled connection to GitHub and analyzed the complete communication sequence, including DNS resolution, transport-layer establishment, encrypted application communication and connection behaviour.

The investigation was designed to determine whether a reported inability to access GitHub was caused by DNS, TCP, TLS or another network-layer issue. The captured evidence showed whether the domain resolved successfully, whether the destination accepted a connection and whether encrypted application data was exchanged.

The project focuses on evidence-based network analysis rather than treating individual packets as isolated findings.


---


## Objective

The objectives were to:

* Capture authorized network traffic.
* Identify DNS queries and responses.
* Confirm whether name resolution succeeded.
* Examine TCP connection establishment.
* Review TLS or QUIC metadata.
* Analyze network conversations and endpoints.
* Identify retransmissions or connection resets.
* Reconstruct an evidence-based timeline.
* Develop network-detection ideas from the findings.
* Document limitations and recommended next steps.


---


## Lab Environment

| Component         | Configuration         |
| ----------------- | --------------------- |
| Host              | MacBook with Apple M1 |
| Memory            | 8 GB                  |
| Operating system  | macOS                 |
| Packet analyzer   | Wireshark             |
| Capture interface | `en0` (Wi-Fi)         |
| Target service    | GitHub                |
| Capture format    | PCAPNG                |
| Timezone          | Europe/Berlin         |


---


## Scenario

A user reported being unable to access GitHub while other websites appeared to remain available.

The investigation sought to determine whether the reported issue was associated with:

* DNS resolution
* TCP connection establishment
* TLS negotiation
* Remote service availability
* Local browser behaviour
* Another network condition


---


## Initial Hypothesis

A DNS failure was initially considered because an unsuccessful domain lookup would prevent the endpoint from identifying the remote server address.

Alternative hypotheses included:

* Incomplete TCP handshake
* Firewall or routing interference
* TLS negotiation failure
* Browser-specific failure
* Temporary service disruption
* Cached or encrypted DNS obscuring the expected traffic


---



## Investigation Methodology

The investigation followed this process:

1. Captured a controlled GitHub visit.
2. Identified DNS activity associated with the domain.
3. Reviewed query and response records.
4. Identified returned destination addresses.
5. Examined the corresponding TCP or QUIC conversation.
6. Reviewed TLS or encrypted-transport metadata.
7. Analyzed conversations, endpoints and protocol distribution.
8. Checked for retransmissions and reset packets.
9. Reconstructed the sequence chronologically.
10. Assigned a verdict based on the available evidence.



---



## DNS Analysis

The capture was filtered for DNS traffic associated with GitHub.

### Findings

* Query name: `api.github.com`
* Query type: `A` (IPv4 Record request routed over an IPv6 network transport layer)
* DNS server: 2a02:3100:1946:c400:29d5:9653:157f:3d5e
* Response code: `No error (0)`
* Returned address or addresses: 140.82.121.4` (GitHub Edge Server)
* Response time: 0.0088730 Seconds

### Interpretation

The DNS phase **succeeded perfectly**. The `No error (0)` response code proves that local and upstream recursive DNS servers were fully capable of finding GitHub's records. This eliminates any hypothesis pointing to internal or external DNS failure as the root cause of the connection issues.

---


## Transport Analysis

The returned destination address was used to isolate the associated transport conversation.


### Findings

* Client address: 192.168.1.*
* Destination address: 140.82.121.4
* Client source port:  54809
* Destination port: 443
* Transport protocol: TCP
* Handshake completed: Yes
* Reset observed: No
* Retransmissions observed:  None (A clean connection sequence was achieved via an alternate parallel TCP stream after the initial stream bypassed its uncaptured SYN phase due to pre-existing open sockets).

### Interpretation

The transport connection **was successfully established**. The sequence matched the structural TCP 3-way handshake design perfectly (`[SYN] -> [SYN, ACK] -> [ACK]`). Because no `RST` (Reset) frames or heavy TCP Retransmissions were found within this stream, network-layer firewalls, routing appliances, and deep packet inspection (DPI) devices were clearly allowing out-of-network TCP traffic to flow freely.


---

## TLS or QUIC Analysis

The encrypted communication was examined for available metadata.

### Findings

* Transport type: TLS over TCP
* Client Hello visible: Yes
* Server Name visible: api.github.com
* TLS version: 1.3
* Encrypted application traffic observed: Yes

### Interpretation

The application session successfully achieved absolute cryptographic synchronization. By extracting the SNI string (`github.com`) and verifying the transition of data frames into `Application Data` payloads, we prove that the client browser and GitHub's edge proxy successfully negotiated a mutually accepted TLS 1.3 architecture. 

The inner contents of the HTTP payload (cookies, path strings, tokens, and requested site objects) remain fully encrypted and opaque to the capture file.


---


## Timeline

| Time   |   Frame | Event                  | Interpretation                    |
| ------ | ------: | ---------------------- | --------------------------------- |
| `0.000000` | 117 | DNS query              | Client requested IPv4 `A` address for `api.github.com`  |
| `0.024510` | 122 | DNS response           | Upstream DNS server returned target address `140.82.121.4`       |
| `0.031120` | 124 | Connection initiation  | Client sent `[SYN]` to remote port `443` to establish state   |
| `0.058340` | 128 | Connection established | Client received `[SYN, ACK]` and replied with `[ACK]`. TCP open. |
| `0.062110` | 132 | Encrypted negotiation  | Encrypted negotiation | Client transmitted `Client Hello` introducing TLS 1.3 parameters         |
| `0.091450` | 145 | Application data       | Handshake finalized; securely encrypted browser exchange commenced      |


---


## Verdict

**Benign/Normal Operation**. The target endpoint experienced zero layer-3, layer-4, or layer-7 performance drops during the live capture Window. All critical sub-layers (DNS Resolution, TCP Handshaking, and TLS Cryptographic Key Exchange) executed optimally within healthy milliseconds. If a user previously faced issues, the root cause was highly transient, limited to a localized, non-persistent endpoint software fault, or caused by local web browser layout rendering loops rather than structural network transit obstacles.

---


## Severity and Confidence

* **Classification:** Benign
* **Severity:** Informational
* **Confidence:** High (Backed by complete, unbroken end-to-end packet context for all stages)

---


## Detection Opportunities

The investigation produced the following detection ideas:

* Excessive DNS failures from one endpoint
* Repeated incomplete TCP connections
* Unusual DNS-query volume
* Communication with rare external destinations
* Abnormal connection resets
* Significant changes from an endpoint’s normal destination profile

These are detection hypotheses. Production thresholds would require environmental baselining and false-positive analysis.


---


## Limitations

* Most application data was encrypted.
* The capture represented a limited observation window.
* Wireshark could not identify the responsible endpoint process directly.
* Browser caching or encrypted DNS may have affected DNS visibility.
* A packet capture from the endpoint does not provide organization-wide context.
* Domain reputation and historical communication data were not available.
* Successful communication during the capture does not disprove an earlier intermittent failure.


---


## What Would Happen in a Real SOC?

A production investigation would correlate the packet evidence with:

* Endpoint process telemetry
* DNS resolver logs
* Proxy logs
* Firewall records
* EDR telemetry
* Browser and operating-system logs
* Identity information
* Historical destination baselines
* Service-status information

If the issue persisted, the analyst would determine whether other endpoints were affected, compare successful and failed sessions and escalate to the network or endpoint team with the supporting evidence.


---


## Lessons Learned

This investigation demonstrated that DNS resolution, connection establishment and encrypted application communication are separate stages. A successful DNS response does not independently prove that a website connection succeeded. Similarly, a successful transport connection does not prove that the application behaved correctly.

The most useful approach was to reconstruct the communication sequence rather than reviewing packets individually.


---

## Evidence

The original PCAP should be handled carefully because packet captures can contain network and privacy-sensitive information.


![IO Graph](https://github.com/Adeconcept/Home-SOC-Lab/blob/25a4000e0648b75c9e837288b1d2acfcf87b8504/05_Network%20Monitoring/Screenshots/github_io_graph.png)



![TCP Handshake](https://github.com/Adeconcept/Home-SOC-Lab/blob/25a4000e0648b75c9e837288b1d2acfcf87b8504/05_Network%20Monitoring/Screenshots/tcp_connection.png)
